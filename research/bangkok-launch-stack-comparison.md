# Bangkok Launch-Stack Comparison (low-cost)

**Date:** 28 August 2026
**Purpose:** identify low-cost, low-complexity options for email capture, refundable presale/payments, privacy-friendly analytics, and consent/unsubscribe.
**Critical caveat:** the figures below come from provider pricing pages / aggregator articles **checked 2026-08-28**; free tiers and fees change frequently. **Verify each provider's current public terms directly before signing up.** Nothing here is an endorsement, approval, or suitability guarantee, and no provider has been approached or approved. VAT/tax handling and data-residency should be checked for the jurisdictions you operate in.

Tags: [FACT] stated on the cited public page at the access date · [REC] suggestion.

---

## Comparison table

| Provider / category | Free tier or cost (as found 2026-08-28) | Email capture | Payment / presale | Analytics | Privacy features | Integration difficulty | Limitations / verify |
|---|---|---|---|---|---|---|---|
| **MailerLite** (email) | Free tier exists; **reduced to 250 active subscribers & 2,500 emails/mo from July 2026** (caps: 3 automations, 3 forms, 1 landing page) [FACT, aggregator] | Yes (forms + 1 landing page on free) | No native checkout (link out) | Basic campaign stats | Standard opt-in/email compliance; double opt-in | Easy | Free is now very small; verify current limit at mailerlite.com/pricing |
| **Brevo** (email) | Free tier: **unlimited contacts, ~300 emails/day** [FACT, aggregator] | Yes (forms, landing pages) | Has transactional/payment add-ons (verify) | Campaign stats; also transactional email | GDPR-oriented; verify data terms | Easy | Daily send cap; branding on free |
| **Kit / ConvertKit** (email) | Free plan for creators (aggregators cite up to ~10,000 subscribers, landing pages + broadcasts) [FACT, aggregator] | Yes (landing pages/forms) | Sells digital products/paid newsletters (verify fees) | Basic | Creator-focused; opt-in handling | Easy | Feature caps on free; verify at kit.com/pricing |
| **Buttondown** (email) | Free for very small lists (aggregator ~100 subs) [FACT, aggregator] | Yes (simple) | Stripe-based paid newsletter possible | Minimal | Minimalist, privacy-light | Very easy | Small free cap; developer-leaning |
| **Gumroad** (payment/presale) | **$0 monthly; fees ~10% + $0.50 platform + payment processing (~2.9%+$0.30 US)**; marketplace/Discover sales carry a much higher fee (~30%) [FACT, multiple] | Via Gumroad landing | **Yes** — hosted checkout, presale, refunds configurable | Sales analytics | Merchant of Record; handles sales tax/VAT collection | Very easy | Higher fee; Discover-fee caveat; refund policy set by seller |
| **Lemon Squeezy** (payment/presale) | **$0 monthly; ~5% + $0.50 (Merchant of Record; processing + tax/VAT included)** [FACT, multiple] | Via LS checkout page | **Yes** — checkout, presale, licensing, refunds | Sales/subscription analytics | MoR handles global tax; verify data terms | Easy | Fixed $0.50 per sale hurts very low-price items; terms change |
| **Stripe Payment Links** (payment) | No monthly fee; card processing fee (~per-transaction, set by Stripe per country) [FACT, generic] | No (pair with email tool) | **Yes** — payment links, no site needed | Via Stripe dashboard | You handle tax/VAT unless Stripe Tax enabled | Easy-medium | You are merchant of record; tax/compliance on you; verify local rates |
| **Cloudflare Web Analytics** (analytics) | **Free, cookieless**, no banner typically required [FACT, multiple] | No | No | Simple traffic/pages/referrers | No personal data/cookies; privacy-forward | Very easy (if using Cloudflare) | No event funnels; basic counts only |
| **Umami** (analytics) | Open-source; **Cloud free tier (~100k events/mo)** or self-host free [FACT, multiple] | No | No | Custom events, simple funnels | Cookieless, self-host data option | Easy (cloud) / medium (self-host) | Event caps; self-host needs a VPS |
| **PostHog** (analytics/events) | Generous free hosted tier (~1M events/mo) [FACT, multiple] | No | No | **Product events, funnels, feature flags, replay** | Can run cookieless/self-host; verify defaults | Medium | Overkill for MVP; review data settings carefully |
| **GoatCounter** (analytics) | Free for **non-commercial**; small commercial fee [FACT] | No | No | Simple open-source stats | Cookieless/open-source | Very easy | Commercial use terms apply; limited features |
| **Microsoft Clarity** (UX analytics) | Free (heatmaps/session recordings) [FACT] | No | No | Replays/heatmaps | Privacy-conscious; verify PII masking | Easy | Session replay = higher privacy care; mask fields |
| **CookieYes** (consent CMP) | Free plan exists (banner, auto-blocking, consent logging; traffic/scan caps) [FACT, WP/TermsFeed] | Not capture (consent) | No | N/A | GDPR/CCPA consent management; Google Consent Mode | Easy | Free caps (~15k pageviews cited); verify tier |
| **iubenda / Termly / Klaro** (consent + policies) | Free entry tiers / open-source (Klaro) for banners; policy generators often paid [FACT, aggregator] | No | No | N/A | Consent + privacy/cookie policy generation | Easy-medium | Free tiers are basic; legal text still needs review |

*Unsubscribe handling:* reputable email providers (MailerLite/Brevo/Kit/Buttondown) include one-click unsubscribe and list-management by default [FACT, generic platform feature] — do not run email capture through a tool without this. Verify each provider's unsubscribe/compliance terms.

---

## Recommended minimal stack (all to be re-verified before signup)

- **Email capture:** start on **Brevo (unlimited contacts, daily send cap)** or **Kit** if creator-flavoured; MailerLite's free tier shrank to 250 subs in July 2026, so treat it as a short trial not a home. [REC]
- **Refundable presale:** **Gumroad or Lemon Squeezy** for zero monthly cost and hosted checkout; Lemon Squeezy (MoR) if you want global tax/VAT handled and expect prices above ~$5; Gumroad if you want the simplest setup and its marketplace. Set a clear refund policy; do **not** rely on Gumroad Discover traffic (high fee). [REC]
- **Analytics:** **Cloudflare Web Analytics** for free, cookieless traffic counts (often no consent banner needed), **plus Umami or PostHog** if the MVP needs the event funnel in `bangkok-measurement-spec.md`. Avoid Google Analytics unless you add proper consent. [REC]
- **Consent/unsubscribe:** use a free CMP (**CookieYes/iubenda/Klaro**) **only if** you set non-essential cookies/tags; with cookieless analytics + email embeds you may need little/no banner — get this confirmed by someone qualified. Always include one-click unsubscribe and a privacy notice. [REC]

## Facts / assumptions / recommendations
- **Facts [FACT]** (28 Aug 2026, citations above): provider free-tier/fee figures as currently published via pricing pages and recent aggregator reviews (MailerLite July-2026 reduction; Gumroad ~10%+$0.50 + processing and ~30% Discover; Lemon Squeezy ~5%+$0.50 MoR; Cloudflare/Umami/PostHog/GoatCounter free options; CookieYes free tier).
- **Assumptions [ASSUMPTION]:** that free tiers remain available; that cookieless analytics remove banner obligations (varies by jurisdiction/other tools); that MoR tax handling fits the business.
- **Recommendations [REC]:** confirm each provider's terms on its official pricing page, check VAT/tax/data-residency for your entity, keep analytics cookieless/minimal, and have the privacy notice and refund policy reviewed before collecting any email or payment.
