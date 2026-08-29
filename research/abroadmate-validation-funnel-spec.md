# AbroadMate / ThailandMate — Validation Funnel Specification

**Purpose:** Define the visitor journey from free tool to (optional, later) refundable presale, the **minimum data** collected at each step, and a hard analytics data-redaction contract.
**Created:** 28 August 2026. Companion documents: page spec (`abroadmate-bangkok-pack-page-spec.md`), data contract (`abroadmate-integration-contract.json`), stack recommendation (`abroadmate-final-stack-recommendation.md`).
**No website code is changed by this document.** No conversion rates are assumed; success thresholds are to be set from real data.

---

## 1. Funnel map

```text
Visitor (organic / referred)
  → Free tool: Bangkok First 7 Days checklist OR budget planner (no email required)
  → Audience / destination selection (remote-worker | family; Bangkok | "where else?")
  → Email consent (explicit opt-in; double opt-in recommended)
  → Pack-interest action (joins "Bangkok First 90 Days" early-access list / clicks reservation)
  → [LATER, separate decision] Optional refundable presale via checkout provider
```

Two gates, deliberately separated:
- **Consent gate** (email) — must never be bundled with using a free tool or with checkout.
- **Payment gate** (presale) — a distinct, later choice; the funnel is valuable (audience signal + list) even if payment is never switched on.

## 2. Step-by-step minimum data

| Step | What the visitor does | Data the system NEEDS | Where it is stored | Analytics sees |
|---|---|---|---|---|
| **0. Visit** | Lands on a content/free-tool page | Nothing personal. Aggregate page view | Analytics (cookieless) | `page_view`, path anonymised; **no PII** |
| **1. Free tool use** | Uses checklist / budget planner in browser | Inputs stay **in the visitor's browser** (client-side only). No account, no server save | None (localStorage at most) | Optional aggregate events: `free_tool_started`, `free_tool_completed`, tool id — with **no free-text and no entered numbers** |
| **2. Audience / destination selection** | Picks "remote worker/family" and "Bangkok / other city" (or skips) | Two enum choices, if volunteered | Email-provider custom fields **only if they later consent**; otherwise a one-off analytics tag | `audience_selected` with enum value (`remote_worker`/`family`/`skipped`), `destination` enum (`bangkok`/`other`). Enums only — never free text |
| **3. Email consent** | Submits email + ticked consent on the signup form | Email address; consent timestamp; consent copy version; source page; campaign tag; double-opt-in status | **Email provider** (system of record) | `email_signup_submitted` / `email_signup_confirmed` as **counts only** — the email string MUST NOT be sent to analytics (hashed or not, at launch) |
| **4. Pack-interest action** | Clicks "Reserve early access" / joins pack list | Interest flag + price-variant label shown + checkout intent | Email-provider tag/field; analytics event | `pack_interest_click`, `price_variant` (e.g. `early_access_v1`), `checkout_started` — enum/boolean, no PII |
| **5. Optional refundable presale (later)** | Checks out with the payment provider | Data collected **by the payment provider** (card, billing); the site receives only order reference, email-for-delivery, status, price variant | **Payment provider**; minimal record in email provider for delivery | `checkout_status` enum (`initiated`/`completed`/`refunded`/`failed`) and amount band label only — **never card data, never billing address, never full email** |
| **6. Unsubscribe / rights** | Click unsubscribe or email privacy request | Suppression record | Email provider (suppression); private corrections register for rights requests | Nothing PII; `unsubscribe` may be a count event |

## 3. Analytics NEVER receives (hard contract)

The analytics tool (Cloudflare Web Analytics by default; Umami if event-level goals are needed — see stack recommendation) is configured so that **none of the following can ever appear** in an event name, property, URL parameter, or identifier:

- ❌ **Email addresses** (and no hashed emails at launch)
- ❌ **Passport or visa information** (numbers, scan references, statuses, application IDs)
- ❌ **Bank information** (account numbers, card numbers, IBANs, payment tokens)
- ❌ **Medical information** (conditions, insurers, claims)
- ❌ **Exact income** (budget-planner figures stay client-side; only an *enum band* chosen in step 2 may ever be tagged, and only if the visitor selects it explicitly)
- ❌ **Employer details** (company names, job titles entered in free text)
- ❌ **Free-text sensitive data of any kind** (names, addresses, personal notes, report-a-change text)
- ❌ Report-a-change submissions (they route to the private corrections mailbox/register, never to analytics)

**Enforcement rules:**
1. **Allow-list properties only.** Analytics calls accept a fixed schema (page, event id, the enum fields in §2). Anything not on the schema is dropped before sending.
2. **No free-text fields are ever wired to analytics.** The budget planner and checklist compute locally; no input value is transmitted.
3. **URL hygiene:** strip query strings that could contain emails/tokens (`?email=`, `?ref=` tokens) before sending; keep only campaign *labels* (`campaign` enum).
4. **Cookieless default.** Prefer tools that set no visitor cookies (Cloudflare Web Analytics / Umami both state on their official pages that they use no cookies and collect no personal data — verified 2026-08-28). If any cookie-setting tool is ever introduced, it goes behind the consent gate (`abroadmate-launch-signoff-checklist.md` block F).
5. **Checkout events are status-only.** The payment provider's webhook may inform a count of completed/refunded orders; the email address and payment details never enter analytics.
6. **Minimum PII measurement rule** (standing): track events with the least identifiable data that answers the question; if a count answers it, send only a count.

## 4. Consent and email capture specifics

- The email form collects **email + explicit consent tick** (not pre-checked); optional `audience` and `destination` enums only.
- Record `consent_timestamp`, `consent_copy_version` (e.g. `privacy-copy-2026-08-28`), `double_opt_in_status` (`pending`/`confirmed`/`not_required`), `source_page`, `campaign`.
- One-click unsubscribe in every email; suppression is permanent; signup is **never** a side effect of using a free tool, reporting a change, or checking out (a buyer email is transactional unless separately, explicitly opted into marketing).
- Sensitive-data rule mirrored from the report workflow: any email/message that includes passport/visa/bank/medical content is deleted and logged as a sensitive-deletion event, never imported.

## 5. Funnel events (names the data contract uses)

`page_view` · `free_tool_started` · `free_tool_completed` · `audience_selected` · `email_signup_submitted` · `email_signup_confirmed` · `pack_interest_click` · `checkout_started` · `checkout_status` · `unsubscribe` · `report_change_submitted` (count only; content goes to the corrections register).

Measurement questions these answer (no targets invented here): where visitors start, which free tool converts to consent, which audience/destination segment shows most interest, whether interest reaches checkout once that exists, and where drop-off happens.

## 6. Tags

- **[FACT]:** Cloudflare Web Analytics and Umami officially state no cookies / no personal data collected (official pages checked 2026-08-28); payment providers (Gumroad, Lemon Squeezy, Stripe) collect card/billing data on their own domains — the merchant site must never touch card data.
- **[ASSUMPTION]:** Free tools can operate fully client-side; audience/destination choice can be an optional enum; a small static site can enforce an analytics allow-list; the email provider is the system of record for consent.
- **[REC]:** Double opt-in; enum-only tagging; client-side budget math; cookieless analytics default; keep consent and payment as separate gates; define success thresholds after collecting real data rather than inventing conversion benchmarks.
