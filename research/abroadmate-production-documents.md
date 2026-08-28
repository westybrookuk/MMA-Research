# AbroadMate / ThailandMate — Minimum Production-Readiness Documents

**Status:** Pre-launch requirements specification. Nothing here is legal advice; a qualified privacy/legal reviewer must sign off before any personal data is collected or any payment is taken (see `abroadmate-launch-signoff-checklist.md`).
**Created:** 28 August 2026. Applies to the Bangkok-first launch ("Bangkok First 90 Days") and all AbroadMate city editions.

---

## 0. What turns on, and what must exist before it does

| Switch | What it collects / does | Documents required BEFORE switching on |
|---|---|---|
| **Real email capture** | Email addresses via signup form; sends newsletter/presale updates | Privacy notice (live), consent/opt-in record method, one-click unsubscribe working, data-processing agreement (DPA) with email provider reviewed, retention + deletion process, report-a-change channel |
| **Analytics** | Page views/events; possibly IP-derived location, device | Privacy notice names the tool, cookie/consent position settled (cookieless tool preferred), DPA with analytics provider reviewed, event schema limited to minimal PII, retention set |
| **Affiliate links** | Outbound links to partners; sets/receives cookies and may generate commissions | Affiliate disclosure displayed at every relevant location, privacy notice covers third-party links/cookies, no commission rates or "vetted/approved" claims, consent for any tracking pixels |
| **Provider lead generation** | Sending visitors to partner signup forms; possibly form-based lead pass-through | Same as affiliate + explicit data-sharing disclosure if any data is passed; pass-through leads need a joint/controllership review; otherwise plain outbound links with disclosure |
| **Paid presales** | Card payments for a pre-order/refundable reservation | Refund/presale terms on the page before checkout, seller identity and contact details, privacy notice covers the payment provider, MoR vs own-merchant tax position understood and reviewed, customer support/refund process staffed, confirmation + receipt flow |

**[REC] Sequencing:** launch the content site with **cookieless analytics only** (Cloudflare Web Analytics, which per official docs sets no visitor cookies) and a **live privacy notice + affiliate disclosure** first; turn on **email capture** only after the privacy notice, unsubscribe flow and DPA review are complete; turn on **paid presales last**, after the legal/privacy sign-off and refund terms are published.

---

## 1. Privacy notice — requirements

The privacy notice is a **public page** linked from the footer of every page, and a short version beside any form that collects data (copy in `abroadmate-report-change-workflow.md` §4.4 and the email signup).

**Must include (requirements — actual wording to be finalised with the reviewer):**

1. **Who operates the site** — legal entity/trading name, a working contact email, and (where applicable law requires) a mailing address or representative.
2. **What data is collected and why**, per purpose, in plain language:
   - Email list: email address (and any tag/segment); purpose = sending the updates the visitor asked for; legal basis = consent.
   - Report-a-change form: correction text, optional reply email; purpose = verifying and fixing content; legal basis = legitimate interest / consent depending on reviewer's assessment for the visitor's jurisdiction.
   - Analytics: aggregate usage; purpose = understanding what is useful; tools and cookie behaviour named.
   - Purchases/presales: handled by the payment provider; what the site itself sees (usually email + order reference, never card numbers).
3. **Each third-party processor named**, with a link to its privacy terms: email provider, analytics provider, consent-tool provider (if any), payment processor, and any form backend. Only providers present in `abroadmate-launch-stack-verification.csv` whose DPA has been reviewed.
4. **Cookies and tracking:** a table of cookies/scripts (necessary vs analytics vs affiliate/marketing), what each does, and how consent is given/refused.
5. **Visitor rights:** access, correction, deletion, objection/withdrawal of consent, and **how to exercise them** (a single contact email suffices at launch), plus the right to complain to a supervisory authority.
6. **Retention:** how long emails, reports and analytics data are kept (Section 5).
7. **International transfers:** whether data leaves the visitor's region and under what safeguards — state only what is true per the reviewed DPAs; **do not invent "EU hosting" claims**.
8. **Children:** the product is not aimed at children; state no intended collection from minors.
9. **No sensitive data:** state that the site never asks for passport/visa/bank/medical details and that such messages are deleted.
10. **Last updated** date and a change log.

**[FACT]** If the site uses Cloudflare Web Analytics as documented (no visitor cookies, no personal data collected per Cloudflare's own documentation), analytics still must be *named* in the privacy notice even though a consent banner is typically not triggered by it. **[REC] Confirm this position with the reviewer; do not rely on it as a legal conclusion across all visitor jurisdictions.**

---

## 2. Consent requirements

1. **Email capture = explicit opt-in.** No pre-ticked boxes, no bundling signup with the report form or checkout. A checkbox (or clearly labelled submit) with words the visitor acts on; record **what was consented to, when, and the wording shown** (providers timestamp subscriptions; keep the welcome/confirmation copy in version control).
2. **Double opt-in [REC]** at launch for EU/UK visitors (confirm via reviewer; some providers default to single opt-in) — it also protects list quality.
3. **Cookies/scripts:**
   - Strictly-necessary only → no banner needed.
   - Any non-necessary cookie (analytics-with-cookies, affiliate tracking pixels, embedded video/forms that set cookies) → a consent mechanism is required **before** those scripts load. Options reviewed: cookieless-first design (no banner), or Klaro (open-source, self-host), CookieYes / iubenda / Termly (hosted) — see the verification CSV for free tiers and the fact that CookieYes/Termly free tiers **suspend the banner at pageview caps**, which is a compliance failure mode to avoid.
   - Affiliate/lead-gen tracking pixels count as marketing cookies and must be gated by consent.
4. **Withdrawal must be as easy as giving consent:** one-click unsubscribe in every email (list-unsubscribe header [REC]), and a cookie-preferences link in the footer.

---

## 3. Affiliate disclosure — wording

**Placement [REC]:** a short disclosure on every page that carries an affiliate or partner link (near the link and/or site-wide in footer), plus a fuller statement on a dedicated disclosure page. Disclosure must appear **before or at the same time as** the link, not buried.

**Short (on-page) copy, ready to use:**

> **Disclosure:** ThailandMate is editorially independent. Some links on this page are partner or affiliate links — if you sign up or buy through them, we may earn a referral fee at no extra cost to you. We never mark a provider as "vetted" or "approved," we don't quote commission rates, and a link is not a recommendation. Always check the provider's current terms, fees, and whether a service fits your own situation. We list tools and official sources to help your own research, not to replace professional advice.

**Dedicated page must also state:**
- We do not guarantee partner pricing, availability, or service quality.
- No fee paid by a partner changes whether a record is published (publication gate is source-based).
- Regulated matters (visa, tax, banking, insurance, medical) are **signposts to official sources only** — partners in those spaces are not endorsed and no personalised advice is given.
- Contact/correction channel for disclosure concerns (the report-a-change category `disclaimer_missing_or_misleading`).

---

## 4. Refund / presale wording

Only switch on presales once the chosen model is set. The wording must be on the sales page **before** checkout, in the confirmation email, and on a terms page. [FACT] Fee treatment on refunds differs by provider (Stripe's refund fee treatment is region-specific; Gumroad sources conflict on whether processing fees are returned) — **do not promise refund mechanics beyond what the provider's current terms guarantee; verify on the official pricing/terms page first** (verification CSV).

**Recommended presale design [REC]:** a clearly-labelled **refundable first-reservation / early-access deposit** for "Bangkok First 90 Days," because the product is unfinished and audience trust is the asset. Suggested copy (adjust to the real policy before publishing):

> **What a presale is:** "Bangkok First 90 Days" is still in progress. A presale reserves your copy at the early price and helps us finish it. You are pre-ordering, not buying a finished product today.
>
> **Refund promise (example — confirm against payment-provider terms before publishing):** You can cancel your presale and get a full refund any time before the guide launches, and for [14] days after you receive it, by emailing [address]. Refunds go back to the original payment method. We aim to process them within [7] days; the payment provider may take a few more days to show the amount.
>
> **What presale money isn't:** It is not a payment for visa, relocation, legal, tax, or insurance services, and it doesn't reserve accommodation, a coworking desk, or any third-party service.
>
> **If delivery is delayed or cancelled:** If we don't deliver the guide, every presale is refunded. We'll email buyers at the address used at checkout.

Also required: seller/trading name, contact email, customer-support responsibility, and (if selling through a Merchant of Record such as Lemon Squeezy/Gumroad) a statement that checkout is handled by that provider under its own terms/privacy policy. If selling directly via Stripe Payment Links, **you** are the merchant of record — tax/VAT registration and remittance are your responsibility and need professional review before taking payments.

---

## 5. Data retention requirements

| Data | Keep for | Then |
|---|---|---|
| Email list subscribers | While subscribed | Delete on unsubscribe within [REC] 30 days (keep a suppression record of the address only to prevent re-add, no marketing content) |
| Welcome/consent evidence | Life of subscription + [REC] 12 months after unsubscribe | Delete |
| Report-a-change submissions (incl. optional emails) | Until the record is fixed + one refresh cycle, max [REC] 24 months | Anonymise (category + area + outcome; delete email and free text) |
| Sensitive data sent in error | **Never stored** | Delete same day; log deletion event (date + category only) |
| Analytics (cookieless aggregate) | Provider's default / [REC] shortest useful window, e.g. 12 months | Rolling expiry; no raw-IP or PII store |
| Payment records | As long as required for tax/accounting by the jurisdictions reviewed (do not guess the period — ask reviewer/accountant) | Retain per advice; provider of record stores transaction detail |

Principles: collect the minimum, set expiry before launch, and don't keep data "just in case."

## 6. User deletion / request process

- **Single channel:** a privacy contact email (e.g. `privacy@`) plus the unsubscribe link in every email and a footer link "Privacy & your choices."
- **Requests handled:** unsubscribe (immediate via link), access request ("what do you hold on me?"), correction, deletion.
- **Process [REC]:**
  1. Acknowledge within 72 hours.
  2. Locate data across: email provider, report register/mailbox, analytics (note: cookieless aggregate can't identify an individual — say so), payment provider (handled by MoR or via Stripe dashboard).
  3. Fulfil within **30 days** (GDPR-style window; reviewer to confirm applicable regimes for the visitor base — include Thailand PDPA considerations for a Bangkok product).
  4. Log the request (date, type, outcome) without copying unnecessary personal data.
- No identity-documents requirement to exercise rights; never ask for passport/ID scans — use the email match as verification.

## 7. Report-a-change process

Fully specified in **`abroadmate-report-change-workflow.md`**: six categories, minimal fields, no sensitive data, internal statuses (`received → triaging → needs_info / rejected_out_of_scope / verifying → accepted_fixed / not_confirmed`, plus `sensitive_deleted`), 7-day triage target, and 2-working-day interim action for safety-adjacent items. The privacy notice must summarise it and the deletion route.

## 8. Final owner / sign-off checklist (summary)

The operative checklist with sign-off lines is **`abroadmate-launch-signoff-checklist.md`**. Documents that must be final and **dated** before each switch:

1. Privacy notice (full) — live and linked site-wide.
2. Short privacy line beside every form (signup + report).
3. Cookie/consent position documented (cookieless only, or banner tool configured and tested with non-essential scripts blocked pre-consent).
4. Affiliate disclosure (on-page + dedicated page).
5. Presale/refund terms (only if payments) — verified against the provider's current terms.
6. Retention schedule (this §5) configured in each provider dashboard.
7. Deletion/request process with named owner and mailbox.
8. Report-a-change workflow live with named triage owner.
9. Processor list + reviewed DPAs for every tool actually switched on.
10. Legal/privacy reviewer sign-off (email capture and payments especially), recorded with name and date.

---

## 9. Tags

- **[FACT]:** Provider capabilities/cookie behaviour are as recorded in `abroadmate-launch-stack-verification.csv` with their individual verification statuses (Cloudflare official docs: no visitor cookies/personal data; consent-tool free tiers can suspend banners at caps; MoR vs direct-Stripe tax responsibility differs). This document does not assert any provider's legal suitability.
- **[ASSUMPTION]:** v1 uses one contact mailbox each for corrections and privacy, no formal ticketing/CRM; cookieless analytics is feasible as the default; visitor base includes EU/UK and Thailand users so GDPR-style + PDPA considerations both need review.
- **[REC]:** Sequencing (cookieless analytics → content/disclosure → email → payments last); double opt-in; refundable presale with simple full-refund promise; 30-day rights window; named owners and dated sign-off; qualified legal/privacy review before collecting emails or taking money.
