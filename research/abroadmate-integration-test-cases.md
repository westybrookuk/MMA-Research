# AbroadMate / ThailandMate — Integration Test Cases

**Purpose:** Pre-launch acceptance tests for the email, analytics, report-a-change and (future) payment flows defined in `abroadmate-integration-contract.json` and `abroadmate-validation-funnel-spec.md`.
**Created:** 28 August 2026. These are behaviour tests, not code; no provider-specific implementation is included.
**Environment rule:** all tracking scripts and real provider connectors run in a production-like test environment only; localhost/preview must never send production analytics (see runbook gating). Use fresh email aliases for each run (Kit confirmation emails send once per address per 12 hours).

**Global pass condition for every analytics assertion:** the analytics provider (Cloudflare Web Analytics or Umami) receives ONLY the allow-listed events and enum properties. The following must **never** appear in any event name, property, URL or identifier —

- ❌ Email addresses (no plaintext, no hash at launch)
- ❌ Passport or visa information (numbers, statuses, application IDs, scans)
- ❌ Bank or card information (account/IBAN/card numbers, tokens, billing addresses)
- ❌ Medical information (conditions, insurers, claims)
- ❌ Exact income (budget figures stay client-side)
- ❌ Employer information (company names, job titles)
- ❌ Free-text report content (report-a-change bodies route only to the private corrections register)

---

## TC-01 — Email signup with valid consent
- **Preconditions:** signup form live; double opt-in on; consent tick unticked by default; consent copy version recorded.
- **Steps:** enter a valid email; tick the consent box; select audience = `remote_worker`, destination = `bangkok`; submit; click the confirmation link in the DOI email.
- **Expected:** contact created in the email provider **only after** confirmation; `double_opt_in_status = confirmed`; tags `audience=remote_worker`, `destination=bangkok`; `consent_timestamp` and `consent_copy_version` recorded; welcome email delivered with working unsubscribe and privacy link.
- **Analytics:** receives `email_signup_submitted` then `email_signup_confirmed` as **counts** with source/campaign labels only — **no email address**, no timestamp keyed to a person.
- **Fail if:** contact appears before confirmation; email string reaches analytics; consent was pre-ticked.

## TC-02 — Email signup without consent
- **Steps:** enter a valid email; leave the consent box unticked; submit.
- **Expected:** submission is blocked client- and server-side with a clear "please confirm you'd like updates" message; **no contact** is created in the email provider; no DOI email sent.
- **Analytics:** at most a form-validation interaction count; **no email**, no contact record.
- **Fail if:** a contact is created or an email is sent without consent.

## TC-03 — Double opt-in confirmation behaviour
- **Steps:** submit with consent; do **not** click the confirmation link; wait; then (separate run) click after the confirmation window.
- **Expected (no click):** contact remains unconfirmed/pending and cannot be mailed (Kit: not added until confirmed; Brevo: not added, link expires after 30 days, unconfirmed blocklisted per workflow). On click: contact becomes confirmed and receives the final confirmation/welcome.
- **Analytics:** `email_signup_submitted` counted; `email_signup_confirmed` appears **only** after the link is clicked.
- **Fail if:** unconfirmed addresses are emailable; confirmation link works after expiry; DOI can be bypassed by the embed.

## TC-04 — Unsubscribe
- **Steps:** as a confirmed subscriber, open a campaign and click the unsubscribe link; confirm; then attempt to re-subscribe / be re-added.
- **Expected:** one-click unsubscribe succeeds; `unsubscribe_status = unsubscribed/suppressed`; no further campaigns arrive; the address remains on the suppression list and is not re-added by imports or checkout.
- **Analytics:** an `unsubscribe` **count** only — no email.
- **Fail if:** more emails arrive; the address can be silently re-imported; the email reaches analytics.

## TC-05 — Audience and destination tagging
- **Steps:** submit two signups — one `family`/`bangkok`, one `remote_worker`/`other_thailand`; also submit one leaving both selects untouched (skipped).
- **Expected:** contacts carry the exact enum values; the untouched submission records `skipped/unknown` defaults rather than free text; tags match the contract enums (`audience`, `destination`).
- **Analytics:** `audience_selected` events carry enum values only; any non-enum/free-text input is dropped.
- **Fail if:** free text (e.g. a typed company or city) is stored as a tag or sent to analytics; unknown values are rejected with no fallback.

## TC-06 — Pack-interest click
- **Steps:** on the pack page, click "Reserve my early-access copy" while logged-in-as-subscriber and (separately) as a visitor who has not subscribed.
- **Expected:** subscriber gets tag `pack_interest_action = reserve_clicked` and `price_variant = early_access_v1`; non-subscriber is routed to consent first (interest is captured via the list, no payment). `checkout_status = none` until payments exist.
- **Analytics:** `pack_interest_click` with `price_variant` enum and source page — **no email, no name, no free text**.
- **Fail if:** the click pushes the visitor straight to checkout without the consent gate; analytics receives identity data.

## TC-07 — Analytics event with allowed enum data
- **Steps:** in the test environment, fire a page view and each allow-listed event (`free_tool_started`, `audience_selected`, `pack_interest_click`) with valid enum properties.
- **Expected:** Cloudflare shows page/referrer/country (no custom events, by design); Umami (if used) records the events with **only** the schema'd enum properties; data appears under the production hostname.
- **Analytics:** properties are enums/controlled labels and sanitised paths; no query-string tokens.
- **Fail if:** a non-schema property is accepted and transmitted; query strings containing tokens/emails are sent.

## TC-08 — Analytics event containing prohibited email or free text (negative test)
- **Steps:** in a test build, deliberately attempt to send an analytics event whose property contains (a) an email address, (b) free-text report content, (c) an income number, (d) a company name.
- **Expected:** the allow-list/redaction layer **drops or strips** the prohibited properties before transmission; nothing reaches the analytics provider; the event either sends with permitted fields only or is rejected outright; the attempt is logged internally for debugging (log contains no PII).
- **Fail if:** any email, free text, income figure or employer string reaches the analytics dashboard.

## TC-09 — Report-a-change submission does not enter analytics
- **Steps:** submit a correction through the report form, including the optional reply email and free-text description; also submit one containing a pasted passport-style string (sensitive-data case).
- **Expected:** the report (category enum, area, text, optional email) lands **only** in the corrections mailbox/private register; `review_status = received → triaging`; the sensitive-data submission is deleted same day and logged as `sensitive_deleted` (date+category only). Analytics receives at most a `report_change_submitted` **count with the category enum** — never the text body, the email, or the sensitive string.
- **Fail if:** report text or reporter email appears in analytics; sensitive data is retained; the report creates a marketing contact.

## TC-10 — Refundable presale (future; Gumroad or Lemon Squeezy test mode)
- **Preconditions:** payments enabled only after sign-off block E; provider in test mode; product is a clearly-labelled pre-order; refund policy published.
- **Steps:** complete a test purchase; verify receipt/delivery and the order notification; then request a refund within the policy window.
- **Expected:** `checkout_status` moves `initiated → completed`; AbroadMate receives order reference + status + delivery email (transactional), **never card data**; refund processed from the provider dashboard (Gumroad: Sales → Refund; Lemon Squeezy: Orders → Refund) with status `refunded`; buyer sees the refund; fee treatment matches the provider's current terms (Gumroad returns its fee minus processor costs; Lemon Squeezy refunds may take up to 10 days).
- **Analytics:** `checkout_started` and `checkout_status` carry the status enum and an amount-**band** label only.
- **Fail if:** card data touches the AbroadMate site/logs; buyer email or amount-as-free-text reaches analytics; the refund promise contradicts provider mechanics.

## TC-11 — Failed payment
- **Steps:** in test mode, force a declined card / abandoned checkout.
- **Expected:** no access/delivery granted; `checkout_status = failed` (or remains `initiated` for abandonment); no charge; the visitor can retry; no marketing consent is implied.
- **Analytics:** failed/abandoned status enum only — no card, email or address.
- **Fail if:** failed checkout grants product access; failure payload containing card/email reaches analytics.

## TC-12 — Refund request (buyer-initiated support path)
- **Steps:** buyer emails the support/refund address requesting a refund (pre-launch and within the post-delivery window).
- **Expected:** support verifies the order in the provider dashboard, issues the refund there only, replies within the published target, and records `checkout_status = refunded`; if the pack never launched, refund is full and automatic-ish per policy; statutory consumer rights are referenced ("in addition to your statutory rights"), not replaced.
- **Analytics:** no PII; at most the refunded status band.
- **Fail if:** refund is promised outside the provider's actual mechanics/timing; staff ask for passport/ID/bank details; buyer email is added to marketing.

---

## Test-result ledger (to complete at execution)

| Test | Run date | Environment | Result (pass/fail) | Tester | Notes |
|---|---|---|---|---|---|
| TC-01 … TC-12 | | | | | |

## Tags
- **[FACT]:** Provider mechanics referenced (DOI defaults and 12-hour/30-day windows; Gumroad dashboard-only refunds and fee return; Lemon Squeezy Orders-menu refunds up to 10 days; seller-first refund support) are from official help pages checked 2026-08-28.
- **[ASSUMPTION]:** The static site can enforce an analytics property allow-list and gate scripts to production; free tools compute client-side.
- **[REC]:** Run TC-08 and TC-09 as explicit gate tests before launch (they prove the redaction contract); re-run the full suite after any analytics or form change; keep test traffic out of production dashboards.
