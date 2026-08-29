# AbroadMate / ThailandMate — Final Launch Stack Recommendation

**Status:** Recommendation only. **No accounts have been opened, no terms accepted, no approval or jurisdiction-suitability claimed.** Every fee/tier below was checked on the provider's **official pricing page on 28 August 2026**; terms change, so re-check within 7 days of launch and before any signup.
**Created:** 28 August 2026. Companion: `abroadmate-launch-stack-verification.csv` (13-provider register), `abroadmate-launch-signoff-checklist.md`.

Legend: ✅ default recommendation · 🔁 fallback · ⚙ requires account signup · ⚖ requires DPA/privacy review · 💲 fees need confirmation at signup/on your region · 🌍 depends on the operator's business jurisdiction.

---

## 1. Recommendation summary

| Need | ✅ Default | 🔁 Fallback | Why |
|---|---|---|---|
| **Email capture** | **Kit (ConvertKit)** | **Brevo** | Kit's free tier covers up to 10,000 subscribers with unlimited broadcasts/forms — generous headroom for validation; creator-focused with built-in tags/automations. Brevo if EU-hosted contacts or tighter cost at tiny volume matter, or as a ready migration target. |
| **Future refundable presale** | **Gumroad** | **Lemon Squeezy** | Gumroad is the simplest hosted sale for a single digital guide and is now a Merchant of Record (tax handled). Lemon Squeezy has a lower headline rate (5%+50¢ vs 10%+50¢) and more SaaS/subscription features if the pack grows into editions/memberships. |
| **Privacy-conscious measurement** | **Cloudflare Web Analytics** | **Umami Cloud (or self-host)** | Cloudflare is free, cookieless, no personal data (official), and needs no banner — enough for traffic validation. Umami adds custom events/goals/funnels (the funnel events in the integration contract) while remaining cookieless. |

**Sequencing [REC]:** Cloudflare analytics + live privacy notice/affiliate disclosure first → Kit email capture after consent/DPA review → presale last, only after ⚖ sign-off and a reviewer-approved refund policy. Interest in the pack is captured by the **email list** (free), so payment is never needed to validate demand.

---

## 2. Terms as verified on official pages — 28 August 2026

### Email
- **Kit (ConvertKit)** — kit.com/pricing ✅/🔁-candidate
  - Free plan **$0 up to 10,000 subscribers**: unlimited landing pages & forms, unlimited broadcasts, audience tagging/segmentation, **sell digital products & subscriptions** listed on free plan; 1 automation. Creator **$33/mo billed annually** ($390/yr; ~$39 monthly) at 1,000 subs — unlimited automations/sequences, remove branding; Pro $66/mo annual. 14-day trial, no card required.
- **Brevo** — brevo.com/pricing
  - **Free forever, no credit card**: forms, basic reporting (free daily send cap ~300 emails/day per official product/transactional pages; branding present). Starter **$9/mo monthly / $8.08 annual** from 5,000 emails; lowest paid tier shows a **500-contact** marketing cap on the official plan builder (unlimited contacts at higher tiers) — confirm for your use. EU data centres (France/Germany) per official material.

### Presale / payment
- **Gumroad** — gumroad.com/pricing
  - **Official page now states: "10% + $0.50 per transaction for all sales through your profile or direct links"; "30% per transaction when new customers find and buy through our Discover marketplace." No monthly fee.** This **resolves the earlier aggregator conflict** (a Jan-2026 article claimed a flat 10% with processing included — the official page supersedes it; processing/card fees within or on top of 10%+50¢ should still be confirmed in the fee breakdown at setup 💲).
  - **Merchant of Record since 1 January 2025:** Gumroad states it handles all sales-tax/VAT collection and remittance worldwide.
- **Lemon Squeezy** — lemonsqueezy.com/pricing
  - **"5% + 50¢ per transaction," no monthly ecommerce charges**; Merchant of Record (automated sales tax/VAT), up to ~21 payment methods; footnote: some payments may incur **additional fees** (international cards/PayPal/subscriptions — see docs) 💲. 2026 page also flags "Lemon Squeezy + Stripe Managed Payments" — read that update before signup.
- **Reference — Stripe Payment Links:** ~2.9% + $0.30 domestic card (US; regional UK rate ~1.5%+20p per Stripe), **but you are the merchant of record** (tax is your responsibility) → not recommended for the first presale; noted only.

### Analytics
- **Cloudflare Web Analytics** — cloudflare.com/web-analytics
  - **Free.** Official page: does **not** use client-side state (no cookies or localStorage), does **not fingerprint** by IP/User-Agent, doesn't track across sites. JS beacon or edge collection. Gives page/URL/country/referrer/status codes and performance; **no custom events/funnels/revenue attribution**.
- **Umami** — umami.is/pricing
  - **Hobby $0**: up to **100K events/month**, **1 website**, 6-month retention, community support. Pro **$20/mo**: 1M events, up to 20 sites, 2-year retention, API, goals/funnels. Self-host free (MIT) on your own infrastructure.
  - Official FAQ: no cookies, no personal data, no cross-site tracking; GDPR/CCPA compliant; **no cookie banner required** (per provider); Cloud servers in **US and EU**; data exportable. (Legal applicability per jurisdiction still ⚖.)

---

## 3. What still requires what (explicit flags)

| Item | Flag | Detail |
|---|---|---|
| Creating any account (Kit, Brevo, Gumroad, Lemon Squeezy, Cloudflare, Umami) | ⚙ | Signup itself accepts the provider's terms — do this only after review; a Cloudflare account is also needed for Web Analytics. |
| DPA / data-processing terms | ⚖ | Review before enabling: Kit (US company), Brevo (EU-hosted per provider), Gumroad, Lemon Squeezy, Umami Cloud (US/EU), Cloudflare. Confirm sub-processors and whether EU/UK/Thailand visitor data is transferred, and with what safeguards. |
| Privacy notice naming each tool | ⚖ | Even cookieless analytics must be *named* in the privacy notice. Checkout providers and any form embeds must be disclosed. |
| Email fees | 💲 | Kit free to 10k then usage; Brevo free daily cap + paid tiers/contact caps — reconfirm the exact tier for your list size before launch. |
| Payment fees | 💲 | Gumroad **10% + $0.50 direct / 30% Discover** (official); confirm card-processing treatment and refund fee mechanics. Lemon Squeezy **5% + 50¢** plus possible surcharges; read the 2026 Stripe Managed Payments update. Refund wording must match actual mechanics. |
| Business jurisdiction / tax | 🌍 | MoR providers (Gumroad, Lemon Squeezy) state they handle sales tax/VAT — confirm this covers **your** sales and that you still meet any **income-tax/business-registration** duties where **you** are resident/established. Direct Stripe puts all tax on you. Consumer-law/refund rights (EU/UK digital & distance rules, Thailand consumer protection) depend on buyer location ⚖. |
| Consumer refund rights | 🌍⚖ | Voluntary refund promise must sit *alongside* statutory rights, not replace them; legal reviewer to confirm wording per jurisdiction. |
| Consent banner | ⚖ | Not required for the cookieless default (Cloudflare/Umami per their official statements); becomes required if any cookie-setting tool, affiliate pixel, or session replay is introduced — fail-closed. |
| Affiliate pixels/tracking | ⚖ | Default stack includes none; partner links should be plain outbound links with disclosure until reviewed. |

---

## 4. Decision notes

- **Why Kit over Brevo as default:** the 10,000-subscriber free ceiling comfortably covers the validation stage with unlimited sends and tagging for the `audience`/`destination`/`pack_interest_action` fields in the integration contract; breakeven to Brevo would come from EU-hosting preference or very low volume where Brevo's metered sending is cheaper. [REC] Keep the integration contract provider-neutral so either plugs in without site changes.
- **Why Gumroad over Lemon Squeezy for the *first* presale:** single-product digital guide, fastest hosted checkout, MoR tax handling. Choose Lemon Squeezy instead if launch shows demand for multiple editions/subscriptions, where its lower rate and subscription tooling pay off — or if Gumroad's fee treatment at checkout proves materially worse 💲.
- **Why Cloudflare first:** the validation questions (which page, which free tool, where interest drops) need counts and referrers, not user journeys. Move to Umami when the funnel events (`email_signup_confirmed`, `pack_interest_click`, `checkout_status`) need goal/funnel reporting — Umami free tier (100k events, 1 site) covers early use, and self-hosting is a later zero-license-cost option that moves data to infrastructure you control.
- **Do not** switch on presale until `abroadmate-launch-signoff-checklist.md` block E is signed; demand validation uses the email list only, so payment timing is a business choice, not an information requirement.

## 5. Tags

- **[FACT]:** All fee/free-tier figures in §2 were read from the providers' official pricing/feature pages on 2026-08-28 (Gumroad 10%+$0.50 direct / 30% Discover, MoR since 1 Jan 2025; Lemon Squeezy 5%+50¢ MoR; Kit free to 10k subs, Creator $33/mo annual; Brevo free plan with ~300 emails/day, Starter from $9/mo; Cloudflare free/no cookies/no personal data; Umami Hobby $0/100k events/1 site, US+EU cloud, no cookies per official FAQ).
- **[ASSUMPTION]:** A single digital guide is the first paid product; list volume stays under 10k subscribers during validation; traffic stays within Cloudflare/Umami free limits; the operator's business jurisdiction and residence are not yet set — 🌍 items cannot be finally resolved until that is known.
- **[REC]:** Defaults as in §1; re-verify all official pages within 7 days before signup; obtain ⚖ DPA/privacy review before email capture and ⚖+accountant review before payments; keep the data contract provider-neutral; cookieless analytics so no consent banner is needed at launch.
