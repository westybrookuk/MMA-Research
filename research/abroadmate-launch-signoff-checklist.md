# AbroadMate / ThailandMate — Launch Sign-Off Checklist

**Purpose:** No switch is turned on until every gate for that switch is checked, dated, and signed by the named owner. This is the operational gate; the content publication gate lives in `bangkok-neighbourhood-publish-audit.md`.
**Created:** 28 August 2026. One line per owner per block: **Name — Role — Date — ✔/✘ — Notes.**

> Nothing below is legal advice. Items marked ⚖ require a qualified privacy/legal reviewer; items marked 💲 require the provider's **official current terms** to be checked (see `abroadmate-launch-stack-verification.csv` — do not rely on aggregator figures).

---

## A. Foundation (must be done before ANY switch)

| # | Gate | Owner | Done (date/initials) |
|---|---|---|---|
| A1 | Named content lead and a named deputy for corrections/privacy mailboxes | | |
| A2 | Privacy contact mailbox exists (e.g. `privacy@`) and corrections mailbox exists (e.g. `corrections@`) | | |
| A3 | Full privacy notice drafted, reviewed ⚖, published, linked from every page footer, with "last updated" date | | |
| A4 | Short privacy line placed beside the email signup form and the report-a-change form | | |
| A5 | Processor list written: every tool that can receive visitor data, with DPA reviewed ⚖ and terms last-checked date recorded | | |
| A6 | Retention schedule (`abroadmate-production-documents.md` §5) configured or calendared in each tool | | |
| A7 | User rights/deletion process documented, owner assigned, 30-day fulfilment target acknowledged | | |
| A8 | Report-a-change workflow (`abroadmate-report-change-workflow.md`) live: button, form, confirmation, internal statuses, register (kept out of the public repo) | | |
| A9 | Content gate confirmed: every displayed data point carries its `publish_status`; `do_not_publish` items are absent; qualitative scores remain null until a documented editorial basis exists | | |
| A10 | Regulated-content rule confirmed live: visa/tax/banking/medical/insurance pages are signposts to official sources only, with verify-before-acting disclaimers — no personalised advice | | |

## B. Email capture

| # | Gate | Owner | Done |
|---|---|---|---|
| B1 | Provider chosen from the verification CSV; official pricing page checked today 💲 and free-tier limits confirmed (note: MailerLite free = 250 subs / 2,500 emails from 1 Jul 2026; Brevo free = 300 emails/day with branding; Kit free = up to 10k subs) | | |
| B2 | Provider DPA/data terms reviewed ⚖; data-residency position understood and reflected in the privacy notice | | |
| B3 | Signup form collects email only; explicit opt-in (no pre-check); consent wording in version control with date | | |
| B4 | Double opt-in enabled [REC] or single opt-in confirmed acceptable by reviewer ⚖ | | |
| B5 | One-click unsubscribe tested in a live email; list-unsubscribe header present; suppression list works | | |
| B6 | Signup form and any hosted landing pages' cookie behaviour checked and disclosed ⚖ | | |
| B7 | Welcome/confirmation email contains no guarantees, no regulated advice, and links to privacy notice + unsubscribe | | |
| B8 | Report form does NOT subscribe anyone; checkout emails are NOT auto-imported to marketing | | |

## C. Analytics

| # | Gate | Owner | Done |
|---|---|---|---|
| C1 | Analytics provider chosen; cookieless option (Cloudflare Web Analytics; Umami; PostHog cookieless mode) preferred and confirmed configured | | |
| C2 | Official terms checked today 💲 (Cloudflare: free, official docs state no visitor cookies/personal data; Umami Cloud Hobby ~100k events/mo; PostHog 1M events/mo free) | | |
| C3 | Privacy notice names the analytics tool and what it does | | |
| C4 | If ANY cookie-setting analytics or session replay is used (PostHog replay default can capture PII): consent banner/tool configured, non-essential scripts blocked pre-consent, replay masking on ⚖ | | |
| C5 | Event schema reviewed: minimal PII only (measurement rule) — no emails, names, passport/visa/booking details in event names or properties | | |
| C6 | Retention/expiry set to the shortest useful window; no raw-IP store in self-owned systems | | |
| C7 | Test traffic from localhost/staging excluded from production counts | | |

## D. Affiliate links & provider lead generation

| # | Gate | Owner | Done |
|---|---|---|---|
| D1 | Affiliate/partner relationship inventory: every partner link listed; none claims "vetted/approved"; no commission rates published | | |
| D2 | Short affiliate disclosure live on every page carrying partner links; full disclosure page published (copy per production docs §3) | | |
| D3 | No affiliate relationship influences a record's `publish_status` (gate remains source-based) | | |
| D4 | Any affiliate tracking pixels/cookies gated behind consent ⚖; none loaded pre-consent | | |
| D5 | For lead generation: if any visitor data is passed to a partner (not just a link), data-sharing wording and joint-controllership/transfer terms reviewed ⚖; if data is not passed, wording says the visitor deals directly with the provider | | |
| D6 | Partner rule honoured: no accounts signed up, no approval claimed, no commission rates published beyond verified public terms 💲 | | |

## E. Paid presales

| # | Gate | Owner | Done |
|---|---|---|---|
| E1 | Payment path chosen and official pricing/terms checked today 💲 (Gumroad fee/processing figures CONFLICT across sources — resolve on gumroad.com/pricing first; Lemon Squeezy 5% + $0.50 MoR with surcharges; Stripe 2.9%+$0.30 US regional, **you are MoR with direct Stripe**) | | |
| E2 | MoR vs own-merchant decision documented; if direct Stripe, tax/VAT registration and remittance position reviewed by accountant ⚖💲 | | |
| E3 | Presale/refund terms drafted per production docs §4, verified against provider refund mechanics, published on sales page BEFORE checkout ⚖ | | |
| E4 | Refund process tested end-to-end (issue a test refund; confirm timing and fee treatment) | | |
| E5 | Seller identity/trading name, contact email and support responsibility published; checkout provider named with link to its terms/privacy | | |
| E6 | Confirmation/receipt email states: pre-order of an unfinished product, refund route, and that it is NOT payment for visa/legal/tax/insurance/accommodation services | | |
| E7 | Privacy notice covers the payment flow; marketing use of buyer emails requires separate opt-in and is default off | | |
| E8 | Chargeback/dispute contact and `sensitive_deleted`-style handling for any documents buyers mistakenly send | | |

## F. Consent tooling (only if non-essential cookies exist)

| # | Gate | Owner | Done |
|---|---|---|---|
| F1 | If site is strictly-necessary + cookieless only: decision recorded with reviewer sign-off that no banner is required ⚖ — and analytics is still named in the privacy notice | | |
| F2 | If a CMP is used: provider chosen from CSV; free-tier pageview caps understood (CookieYes free banner SUSPENDS at 5k pageviews; Termly free at 10k views and lacks consent logs; iubenda free ~1k PV; Klaro open-source free self-host) 💲 | | |
| F3 | CMP configured to block non-essential scripts by default; accept/reject equally easy; preferences link in footer | | |
| F4 | Consent records logged (GDPR demonstration of consent) — free tiers lacking logs are not acceptable if logs are required ⚖ | | |
| F5 | Banner behaviour tested on mobile and after reject; scripts confirmed not loaded on reject | | |

## G. Final go/no-go gate

| # | Gate | Owner | Done |
|---|---|---|---|
| G1 | Blocks A complete (foundation) — required for everything | | |
| G2 | Per switch: B complete before email capture goes live; C before analytics; D before affiliate/lead links are promoted; E before any payment; F if any non-essential cookies | | |
| G3 | Legal/privacy reviewer sign-off ⚖ recorded: name, role, date, scope (at minimum: privacy notice, consent position, affiliate disclosure, presale terms) | | |
| G4 | Provider terms re-checked within 7 days before launch and dates recorded in the verification CSV; any aggregator-only figures replaced with official-page figures 💲 | | |
| G5 | Rollback plan: each switch can be turned off without breaking the site (forms disabled, banner fail-closed, payment links archived) | | |
| G6 | Post-launch review booked: corrections inbox + analytics + any unsubscribes/refunds checked at 7 days, 30 days, and quarterly | | |

**Sign-off to launch:**
- Content lead: ______________________ Date: ________
- Privacy/legal reviewer ⚖: ______________________ Date: ________
- (If payments) accountant/tax reviewer 💲: ______________________ Date: ________

---

## Tags

- **[FACT]:** Free-tier/fee figures quoted in gates B1, C2, E1, F2 are the 2026-08-28 figures recorded in `abroadmate-launch-stack-verification.csv` with their own verification_status; several are aggregator-sourced and flagged for official-page recheck. Gumroad fee data is conflicting and must be resolved before any payment decision.
- **[ASSUMPTION]:** A single reviewer/accountant engagement will cover privacy and tax for the small-scale launch; tooling stays email-mailbox + hosted providers rather than self-hosted infrastructure at launch (Klaro self-host and Umami self-host remain options).
- **[REC]:** Fail-closed defaults (banner disabled means no non-essential scripts, not the reverse); cookieless analytics to defer the consent banner entirely; paid presales only after ⚖ sign-off; every provider term re-checked on the official page within 7 days of launch.
