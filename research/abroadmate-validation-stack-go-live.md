# AbroadMate / ThailandMate — Minimum Validation-Launch Stack (Go-Live Decision)

**Purpose:** The smallest, lowest-cost set of infrastructure for a real Bangkok validation launch, with a clear separation of what stays local/demo, what needs an account, what needs privacy/legal review, what needs business/tax review, and what waits for first results.
**Created:** 28 August 2026. Companion runbooks: `abroadmate-provider-onboarding.md`; sign-off: `abroadmate-launch-signoff-checklist.md`; terms: `abroadmate-launch-stack-verification.csv` and `abroadmate-final-stack-recommendation.md`.
**No accounts have been opened and no terms accepted in producing this document.** Provider facts are from official pages checked 2026-08-28.

---

## 1. The minimum stack for a Bangkok validation launch

| Layer | Choice (default) | Fallback | Monthly cost at validation scale |
|---|---|---|---|
| Site / content | Existing static preview site (content + free tools) | — | Already built; hosting only |
| Measurement | **Cloudflare Web Analytics** (free, cookieless) | **Umami Cloud Hobby** if custom funnel events are needed | $0 (both have free tiers: Cloudflare all plans; Umami 100k events/1 site) |
| Email capture | **Kit free plan** (up to 10k subscribers) | **Brevo free** (~300 emails/day, EU-hosted per provider) | $0 within free tiers |
| Consent tooling | **None** — cookieless + strictly-necessary only at launch | Klaro self-host / CookieYes or iubenda only if a cookie-setting tool is added | $0 by avoiding non-essential cookies |
| Payments / presale | **Off for validation** — demand captured by the email list | Gumroad (10% + $0.50 direct, MoR) or Lemon Squeezy (5% + 50¢, MoR) later | $0 until presale is switched on |
| Corrections | Dedicated mailbox + private register (already specced) | — | $0 (existing mail) |
| Partners | Plain outbound links + disclosure only; outreach per templates | Affiliate pixels only after review | $0 |

**Validation can run entirely on free tiers with zero payment processing.** The goal of this stage is to learn (which free tool drives consent, which audience/destination segment shows intent), and that requires no checkout.

---

## 2. What can remain local / demo-only (do NOT provision yet)

- **Payment providers (Gumroad, Lemon Squeezy, Stripe):** stay in runbook/test-mode thinking only. No account, no product, no live checkout. Demand signal = email list + `pack_interest_action`, not revenue.
- **Custom/funnel analytics beyond page counts:** Cloudflare gives traffic/referrer/country. If the team isn't acting on funnel events yet, **don't** add Umami/PostHog — stay on Cloudflare.
- **Consent-management platform (CookieYes/iubenda/Termly/Klaro hosted):** not needed if the site sets no non-essential cookies. Klaro self-host is a future option only.
- **Session replay, heatmaps, marketing pixels, affiliate tracking scripts:** all off; none are needed for validation and each adds consent/PII burden.
- **CRM / ticketing / automation beyond basic welcome email:** corrections use a mailbox + register; email automation stays at a welcome message.
- **Paid email tiers:** free plans cover thousands of subscribers; no paid plan until volume or branding removal justifies it.
- **Budget-planner/checklist inputs:** always client-side; no backend storage is built (these never leave the browser).

## 3. What requires an account (provision after checklist, before traffic)

1. **Cloudflare account** → add the production hostname → Web Analytics beacon (automatic if proxied, else manual snippet). ⚙
2. **Kit account** (or Brevo as fallback) → domain authentication (DKIM/SPF), consent form, double opt-in on, welcome email, tags for the enum fields. ⚙
3. **Role-based mailboxes** (e.g. corrections@, privacy@, support@) on the AbroadMate domain. ⚙
4. (Only if Umami chosen) **Umami Cloud account** → add site → tracking code, production-gated. ⚙
5. Secret/token handling: any beacon token or future webhook secret goes in the hosting platform's secret store, **never** in the repo or pages.

Each account signup accepts that provider's terms — do it only after the relevant sign-off block is complete (`abroadmate-launch-signoff-checklist.md` A–C).

## 4. What requires privacy / legal review (⚖ — before the switch)

- **Privacy notice** published and linked site-wide, naming the exact tools switched on (Cloudflare and/or Umami, Kit or Brevo, and the mail/corrections process).
- **DPA / data-processing terms** reviewed for each live processor (Kit/Brevo; Cloudflare; Umami if used) — sub-processors, data locations (Brevo states EU hosting; Umami US/EU; Kit/Cloudflare US-headquartered), transfer safeguards for EU/UK/Thailand visitors.
- **Consent position:** documented sign-off that the cookieless analytics + strictly-necessary setup needs no banner (provider statements support this; legal applicability per jurisdiction is not assumed); analytics still named in the notice. Any future cookie-setting script flips this to "consent tool required, fail-closed."
- **Email consent mechanics:** explicit opt-in (no pre-check), double opt-in confirmed, one-click unsubscribe tested, consent copy versioned/dated, suppression permanent.
- **Affiliate/partner disclosure** live wherever outbound partner links appear; no "vetted/approved" wording; no commission rates published.
- **Report-a-change workflow** live with the no-sensitive-data rule and deletion path.
- **User rights/deletion process** with a named owner and 30-day fulfilment; retention schedule configured.
- **Regulated content:** visa/tax/banking/medical/insurance pages remain signposts to official sources — no personalised advice; reviewed.

## 5. What requires business / tax review (🌍💲 — before payments or partner revenue)

- **Operator business jurisdiction:** where AbroadMate/the operator is established/resident determines business registration, income tax, and which consumer-protection rules apply. This is **unresolved** and blocks payments.
- **Merchant-of-record choice:** Gumroad and Lemon Squeezy state they handle sales tax/VAT as MoR — confirm this covers the operator's sales and that income-tax/business-registration duties are still met. Direct Stripe (not recommended now) makes the operator the MoR with full tax responsibility.
- **Refund policy legality:** the voluntary refund promise must sit *alongside* statutory rights (EU/UK digital-content & distance-sales rules; Thailand consumer protection) — final wording reviewed before any presale.
- **Fee confirmation 💲:** Gumroad **10% + $0.50 direct / 30% Discover** (official; refund fee treatment to confirm in a live test); Lemon Squeezy **5% + 50¢** plus possible surcharges and the 2026 Stripe-Managed-Payments change; email paid-tier pricing if free tiers are exceeded.
- **Partner/affiliate revenue and lead-gen:** any data-passing partner arrangement needs contracts/DPA and may have tax/regulatory implications; insurance and visa/legal referrals additionally need licence verification.
- Payout availability/timing for the operator's country for any future payment provider.

## 6. What should wait until after the first validation results

- **Paid presale entirely.** Turn on Gumroad/Lemon Squeezy only after the list shows concrete pack interest (`pack_interest_action = reserve_clicked` at meaningful volume) and blocks E (and ⚖/🌍 sign-off) are complete.
- **Funnel analytics (Umami/PostHog):** add only if Cloudflare's page/referrer counts can't answer the key questions; don't instrument events nobody is reading yet.
- **Paid email plan / branding removal / automations beyond welcome:** wait until list size or deliverability needs justify cost.
- **Consent platform and affiliate pixels:** only when a cookie-setting or paid-partner tracking need actually exists.
- **Self-hosting (Umami/Klaro):** only if traffic, cost or data-control needs justify the ops burden.
- **Multiple products, subscriptions, memberships, CRM:** after the single pack validates.
- **Expanding partner links beyond a few plain outbound references:** after outreach responses and review.

---

## 7. Go-live gate for the validation launch

The validation launch is a GO when **all** are true:
1. Content gate satisfied (`do_not_publish` items absent; scores null; dated sources; disclaimers present).
2. Privacy notice + affiliate disclosure + report-a-change live; ⚖ sign-off recorded for the analytics no-banner position and email consent flow.
3. Cloudflare (or Umami) analytics live in production only, privacy notice names it, and negative tests TC-08/TC-09 pass (no email/free text/PII can reach analytics).
4. Kit (or Brevo) account provisioned, domain authenticated, double opt-in and unsubscribe tested (TC-01…TC-06 pass).
5. Corrections mailbox/register owned; no sensitive data collected anywhere.
6. **No payment, no cookie-setting scripts, no affiliate pixels** switched on.

Then run the post-launch review (7-day, 30-day, quarterly) on `abroadmate-launch-signoff-checklist.md` G6, and let the data decide whether payments and deeper analytics are warranted.

## 8. Tags
- **[FACT]:** Provider free tiers/fees/cookie positions are as verified on official pages 2026-08-28 (Cloudflare free + no cookies/PII; Umami Hobby $0 100k events/1 site, US/EU; Kit free to 10k subs; Brevo free ~300 emails/day with EU hosting stated; Gumroad 10%+$0.50/30% MoR; Lemon Squeezy 5%+50¢ MoR). No conversion rates, approvals or legal conclusions are asserted.
- **[ASSUMPTION]:** Validation traffic fits within free tiers; the operator's business jurisdiction is not yet fixed (blocks 🌍 items); a static site can gate scripts to production and enforce the analytics allow-list.
- **[REC]:** Launch measurement + email only on free tiers; keep payments, funnel analytics, consent tooling and tracking pixels OFF until evidence/review warrants them; sequence reviews (⚖ before email/analytics, ⚖+🌍 before money); make every later switch fail-closed.
