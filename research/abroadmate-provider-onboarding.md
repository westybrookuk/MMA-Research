# AbroadMate / ThailandMate — Provider Onboarding Runbooks

**Status:** Pre-implementation runbooks. **No accounts have been opened and no terms accepted.** Execute a runbook only after the relevant sign-off block in `abroadmate-launch-signoff-checklist.md` is complete.
**Created:** 28 August 2026. Operational facts below were checked against official help/documentation pages on **2026-08-28** (URLs per provider).
**Rules for every runbook:** no credentials, API keys, secrets or provider-specific code are stored in this repository — any token/key a provider generates goes straight into the hosting platform's secret manager, never into git, pages or analytics. Field names follow `abroadmate-integration-contract.json`.

How to read each runbook: the 13 required headings appear in the same order for every provider.

---
---

# 1. Kit (ConvertKit) — preferred email provider

**account_setup_steps**
1. Create the account with a role-based address on the AbroadMate domain (not a personal freemail address) at kit.com; choose the Free plan (up to 10,000 subscribers per the official pricing page).
2. Complete account approval (Kit reviews new accounts before sending); set the public sender name (e.g. "ThailandMate") and support/reply address.
3. Set up sending-domain authentication (DKIM/SPF) for the AbroadMate domain — Kit requires this for reliable delivery.
4. Create the list/segment structure: one primary list; tags for `audience`, `destination`, `campaign`, `pack_interest_action`, `price_variant`; a suppression/unsubscribe state is managed automatically.
5. Build the signup form/landing page with **email + explicit consent tick only**; add the `audience` and `destination` enum fields as optional custom fields.
6. Configure the welcome/confirmation email and the first welcome broadcast (copy approved per `abroadmate-bangkok-pack-page-spec.md`).
7. Embed/host the form; wire the *count-only* analytics events per the integration contract.

**required_business_information**
- Account holder name; business/trading name if operating as an entity; country; website URL (must be a live, content-bearing site for approval); physical mailing address (required in email footers under anti-spam rules — use a genuine contact address or registered PO box).

**required_contact_information**
- Account login email (role address); public reply/support email shown to subscribers; the consent/privacy contact email.

**privacy_or_DPA_review** ⚖
- Review Kit's privacy policy, terms and data-processing agreement before signup; confirm sub-processors and where subscriber data is stored (US company) and transfer safeguards for EU/UK/Thailand visitors.
- Confirm the privacy notice names Kit and links its terms; record the `consent_copy_version` used at launch.

**consent_configuration**
- Double opt-in is **ON by default** in Kit (official help: subscribers receive a confirmation email before being added; enable via the form/landing-page builder → Settings → Confirmation Email → "Send confirmation email" checked). Keep it on unless the legal reviewer approves otherwise for a given audience.
- Consent tick box must be unticked by default; consent wording versioned and dated. Confirmation emails send once per subscriber per 12-hour window (re-test with fresh aliases).
- Send from a custom-domain address — Kit states freemail sender addresses (gmail/yahoo/hotmail) harm deliverability and confirmation emails.

**unsubscribe_or_refund_configuration**
- One-click unsubscribe is included by default in Kit emails; verify the unsubscribe link and list-unsubscribe header render in a test send; confirm unsubscribed addresses are suppressed and cannot be re-added.
- Refunds are not applicable to Kit email; Kit Commerce (digital-product sales) is NOT to be enabled at launch (payments go through Gumroad/Lemon Squeezy if presale starts).

**data_fields_to_send**
- To Kit: `contact_email`, `audience` (enum), `destination` (enum), `source_page` (path), `campaign` (controlled label), `consent_timestamp`, `consent_copy_version`, `double_opt_in_status`, `pack_interest_action` (enum), `price_variant` (enum).

**data_fields_never_to_send**
- Passport/visa data, bank/card data, medical data, exact income, employer details, free-text report content, report-a-change submissions, payment card data, and any analytics event payload containing the above. Never import buyer or reporter emails into marketing without separate consent.

**testing_steps**
1. Subscribe with a test alias; confirm the double-opt-in email arrives and the contact only appears after clicking confirm.
2. Test the no-consent path: submit without the consent tick → submission must be blocked.
3. Receive a welcome email; verify sender domain authentication, footer address, working unsubscribe and privacy link.
4. Unsubscribe; confirm suppression; attempt re-add to confirm the address stays suppressed.
5. Confirm analytics receives only `email_signup_submitted`/`email_signup_confirmed` counts with no email string.

**rollback_steps**
- Disable/remove the embedded form (site falls back to no capture); pause automations/broadcasts; export the subscriber list for records; if abandoning Kit, delete contacts per the deletion process and close the account; suppression/consent evidence retained per the retention schedule.

**official_documentation_urls**
- Pricing: https://kit.com/pricing · Help home: https://help.kit.com · Confirmation/DOI troubleshooting: https://help.kit.com/en/articles/4327425 · Privacy: https://kit.com/privacy

**last_checked:** 2026-08-28

**unresolved_questions**
- Free-plan sending limits/deliverability for non-US senders (verify in account at setup). Exact DPA/sub-processor list for EU/UK visitors (⚖). Whether account approval accepts a pre-launch preview site or requires the live domain.

---
---

# 2. Brevo — email fallback

**account_setup_steps**
1. Register at brevo.com with a role-based domain email; Free plan (no card) initially.
2. Authenticate the sending domain (DKIM/SPF/DMARC) and set the default sender (name + domain address).
3. Create the main contact list plus a temporary list if using an externally hosted form with the DOI automation.
4. Build the sign-up form: email + consent; enable **"Double confirmation" (double opt-in)** in the form's subscription-confirmation settings; select/create the DOI confirmation template and include the GDPR consent text in that template (official guidance: this is the way to prove which consent text was agreed to).
5. Add custom attributes for `audience`, `destination`, `campaign`, `pack_interest_action`, `price_variant`. Brevo auto-adds a `DOUBLE_OPT-IN` attribute (Yes/No/blank).
6. Configure the final confirmation email and redirect pages (thank-you page on the site).
7. Embed the form; verify event/transactional logs record consent evidence.

**required_business_information**
- Account holder/trading name, country, website URL, postal address for email footers; company details if invoicing for a paid tier later.

**required_contact_information**
- Login email; sender/reply-to address on the AbroadMate domain; privacy contact address.

**privacy_or_DPA_review** ⚖
- Review Brevo's DPA and privacy terms; official material states data hosting in France/Germany (EU) and ISO 27001/GDPR/CCPA/CASL claims — verify current sub-processors and the DPA before enabling.
- Confirm the privacy notice names Brevo; keep DOI event logs and transactional logs as consent proof (official: event logs record form submission; transactional logs record the DOI email content/link and confirmation time).

**consent_configuration**
- Use Double confirmation (DOI): confirmation link expires after 30 days (official); unconfirmed contacts are not added to the list. Put the consent text in the DOI email template so the agreed wording is provable.
- Consent tick unticked by default; data minimisation — only email plus optional enum fields.

**unsubscribe_or_refund_configuration**
- List-unsubscribe header and one-click unsubscribe are standard in Brevo campaigns; verify in a test send; blocklist contacts who do not confirm DOI (official recommendation) and any unsubscribes.
- Refunds not applicable (email platform); Brevo's other products (CRM/Sales/Payments) are out of scope and must not be enabled without separate review.

**data_fields_to_send**
- Same field set as Kit: `contact_email`, enum tags (`audience`, `destination`, `campaign`, `pack_interest_action`, `price_variant`), `source_page`, `consent_timestamp`/consent evidence, `double_opt_in_status` (Brevo `DOUBLE_OPT-IN`).

**data_fields_never_to_send**
- Passport/visa, bank/card, medical, exact income, employer, free-text report content, report submissions, payment data; no analytics payloads containing email or PII.

**testing_steps**
1. Submit the form with a test alias; verify the DOI email, confirmation link, final confirmation email, and that the contact shows `DOUBLE_OPT-IN = Yes` only after clicking.
2. Verify an unconfirmed contact never reaches the sendable list and is blocklisted after the wait window.
3. No-consent submission must be rejected.
4. Test unsubscribe in a campaign; verify blocklist.
5. Check event + transactional logs contain the consent record; confirm analytics sees counts only.

**rollback_steps**
- Remove/disable the form; pause automations and campaigns; export contacts and logs for evidence; delete contacts per the deletion process and downgrade/close the account if fully rolling back.

**official_documentation_urls**
- Pricing: https://www.brevo.com/pricing · DOI guide: https://help.brevo.com/hc/en-us/articles/208733449 · Create a sign-up form: https://help.brevo.com/hc/en-us/articles/208771869 · GDPR-compliant form: https://help.brevo.com/hc/en-us/articles/360000454204 · Privacy: https://www.brevo.com/legal/privacypolicy/

**last_checked:** 2026-08-28

**unresolved_questions**
- Free-plan daily send cap and branding on the current plan (confirm in account); contact cap on the lowest paid tier for list size planning; exact DPA/sub-processor list (⚖); whether Brevo's EU hosting covers all data at rest.

---
---

# 3. Cloudflare Web Analytics — preferred analytics

**account_setup_steps**
1. Create a Cloudflare account with a role-based email (free).
2. In the dashboard: **Analytics & Logs → Web Analytics → Add a site**; enter the production hostname.
3. Choose **Automatic setup** only if the site is proxied through Cloudflare (edge injection; requires no page change); otherwise use the **manual JS beacon** and place the provided snippet so it loads on production only.
4. Use one site tag per hostname/apex (official: tags are validated per apex domain; do not share across unrelated domains).
5. If the site sends a Content-Security-Policy, allow the beacon script source and the analytics endpoint host (official FAQ lists the required `script-src` entry for `static.cloudflareinsights.com` and the beacon endpoint).
6. Ensure the beacon is NOT rendered on localhost, preview or staging (gate on production environment) so test traffic is excluded.
7. For any single-page-app behaviour, confirm route changes are counted (official SPA guidance).

**required_business_information**
- None beyond the account and the hostname; no billing, tax or business registration for the free Web Analytics product.

**required_contact_information**
- Account email (role address).

**privacy_or_DPA_review** ⚖
- Official product/docs: no cookies or localStorage, no fingerprinting by IP/User-Agent, no cross-site tracking, no visitor personal data collected. Still: name Cloudflare Web Analytics in the privacy notice and have the reviewer confirm the no-banner position for the visitor jurisdictions (provider statement is not a legal ruling).
- Review Cloudflare's privacy policy/DPA for the account.

**consent_configuration**
- No consent banner is required *for this tool* on the official basis that it sets no client-side state and collects no personal data; record this decision with reviewer sign-off. If any other script sets cookies, that script is gated separately — Cloudflare's presence does not grant consent for it.

**unsubscribe_or_refund_configuration**
- Not applicable (free analytics; no end-user accounts, no payments).

**data_fields_to_send**
- Only what the beacon collects automatically: page paths (sanitised — no tokens/emails in query strings), referrer type, country (derived), device/browser class, performance metrics. No custom event payloads are supported by this product.

**data_fields_never_to_send**
- Email addresses, passport/visa, bank/card, medical, exact income, employer, names, any free text, report-a-change content, user identifiers. Cloudflare Web Analytics cannot accept custom properties, so the funnel's custom events (signup/interest/checkout) are NOT instrumented here — counts for those come from the email/payment providers, or move to Umami if event reporting is needed.

**testing_steps**
1. Load a production page; in browser DevTools Network tab confirm the beacon script loads and a beacon request is sent on page load and on leaving the page.
2. Confirm data appears in the dashboard for the correct hostname within a few minutes.
3. Confirm NO beacon loads on local/preview/staging.
4. Verify ad-blocker impact is understood (official: the JS beacon can be blocked by ad blockers; edge analytics — if proxied — cannot; document the under-count).
5. Confirm the privacy notice names the tool.

**rollback_steps**
- Remove the beacon snippet (or turn off Automatic setup); data collection stops immediately; the site is unaffected. Delete the site from the Web Analytics dashboard if fully rolling back.

**official_documentation_urls**
- Product: https://www.cloudflare.com/web-analytics/ · Docs/about: https://developers.cloudflare.com/web-analytics/about/ · FAQ (CSP, SPA, ad blockers): https://developers.cloudflare.com/web-analytics/faq/ · Privacy: https://www.cloudflare.com/privacypolicy/

**last_checked:** 2026-08-28

**unresolved_questions**
- Whether the production site is proxied through Cloudflare (determines automatic vs manual setup). CSP headers currently in use (to add the allow-list). Reviewer sign-off on the no-banner position for all visitor jurisdictions.

---
---

# 4. Umami — analytics fallback (custom events)

**account_setup_steps**
1. Choose **Umami Cloud** (Hobby free tier: 100k events/month, 1 website, 6-month retention per the official pricing page) or self-host (MIT, free, own infrastructure — ops burden). For validation, Cloud Hobby is the low-effort path.
2. Register at cloud.umami.is; in **Settings → Websites → Add website**, enter the site name and production domain.
3. Open the site's **Edit → Tracking code** and place the provided script in the page `<head>` (or the framework's script component) so it renders in production only.
4. Restrict tracking to the production domain via the tracker's domain restriction; consider enabling Do-Not-Track respect per policy decision.
5. Define the allowed custom events (only the 11 events in the integration contract) using Umami's event tracking with enum-only properties; never pass free text or identifiers.
6. Set up goals/funnels for the validation questions (free tool → consent → pack interest).
7. Exclude internal/preview visits; confirm no script on staging.

**required_business_information**
- For Cloud: account details only; no tax/billing info on the free Hobby tier. For self-host: hosting account details (operator-provided infrastructure).

**required_contact_information**
- Account email (role address).

**privacy_or_DPA_review** ⚖
- Official FAQ: no cookies, no personal data, no cross-site tracking; GDPR/CCPA compliant; Cloud servers in US and EU; data exportable. Verify the current DPA, choose the Cloud region if selectable, and name Umami in the privacy notice.
- Self-hosted: document where the instance and database run; that environment is in scope for the privacy review.

**consent_configuration**
- Per Umami's official statement no cookie banner is required (cookieless, no PII); record the reviewer's sign-off. Custom events must still carry enum-only, non-PII properties.

**unsubscribe_or_refund_configuration**
- Not applicable (analytics).

**data_fields_to_send**
- Auto page metrics plus the allow-listed events with enum properties: `free_tool_started/completed` (tool id), `audience_selected` (audience/destination enums), `email_signup_submitted/confirmed` (counts; source/campaign labels), `pack_interest_click` (`price_variant` enum), `checkout_started/status` (status enum + amount-band label only), `unsubscribe` (count), `report_change_submitted` (category enum only).

**data_fields_never_to_send**
- Emails, passport/visa, bank/card, medical, exact income (budget numbers stay client-side), employer, names, report free-text, any document, full URLs containing tokens or emails (sanitise to path).

**testing_steps**
1. Load the production site; in DevTools Network confirm the Umami script loads and page views appear immediately in the dashboard.
2. Trigger each allow-listed event in a test session; confirm it appears with the correct enum property and nothing else.
3. Deliberately attempt to send an event with an email string/free text in a test build; confirm the allow-list layer drops it and it never reaches Umami.
4. Confirm no script/events on staging; confirm domain restriction works.
5. Verify retention (6 months Hobby) matches the retention schedule.

**rollback_steps**
- Remove the tracking script (collection stops); delete the website in Umami Cloud or decommission the self-hosted instance/database; export any needed aggregate data first.

**official_documentation_urls**
- Pricing: https://umami.is/pricing · Collect data / tracking code: https://docs.umami.is/docs/collect-data · Tracker configuration / events: https://umami.is/docs/tracker-functions (and docs.umami.is) · Privacy: https://umami.is/privacy

**last_checked:** 2026-08-28

**unresolved_questions**
- Cloud vs self-host choice (ops capacity); Cloud region selection availability on Hobby; whether 100k events/1 site covers launch traffic; reviewer confirmation of no-banner position.

---
---

# 5. Gumroad — future presale option (do not enable until block E signed)

**account_setup_steps**
1. Only after legal/tax sign-off: create a Gumroad creator account with a role-based email.
2. Complete payout setup (payout method varies by country) and any required tax identity forms (Gumroad is Merchant of Record for sales tax/VAT since 1 Jan 2025 per its official pricing page, but payout/tax-identity setup is still required).
3. Set the public seller profile name ("ThailandMate"/AbroadMate entity), support email, and refund policy text (reviewer-approved version of `abroadmate-bangkok-pack-page-spec.md` §10).
4. Create the digital product for "Bangkok First 90 Days" as a **pre-order/early-access** listing with the reviewer-approved presale copy; upload the placeholder/deliverable or set delivery for launch.
5. Turn Discover/marketplace distribution OFF if direct sales are intended (official: Discover-originated sales carry a 30% fee vs 10% + $0.50 for direct/profile sales) — decide deliberately.
6. Configure the receipt/delivery email and the post-purchase download; ensure checkout asks only for what Gumroad requires.
7. Test in Gumroad's test mode before any live link; connect the order-status webhook/email notification to record `checkout_status` (status only).

**required_business_information**
- Legal seller name/entity, country, payout/banking details for payouts (entered into Gumroad, never stored by AbroadMate), tax identity information as prompted by Gumroad's onboarding.

**required_contact_information**
- Account email; public support/refund email shown to buyers; business support contact.

**privacy_or_DPA_review** ⚖
- Review Gumroad's terms and privacy policy; the privacy notice must state checkout is handled by Gumroad (Merchant of Record) under its terms and that card data never touches AbroadMate.
- Confirm what buyer data Gumroad passes to the seller (sale notification email, product, amount) and that buyer emails are treated as transactional unless separately opted into marketing.

**consent_configuration**
- No marketing consent is collected at checkout; any newsletter opt-in at/after purchase must be a separate, unticked, explicit consent wired to the email provider. Gumroad's own cookies/checkout are covered by Gumroad's policy and disclosed; AbroadMate adds no tracking pixels without consent.

**unsubscribe_or_refund_configuration**
- Refunds are issued **only from the Gumroad dashboard** (Sales → sale → customer drawer → Refund fully / partial) — official help says refunds must not be issued directly from Stripe/PayPal.
- Official fee treatment on refunds: Gumroad returns its platform fee **minus** the payment-processor fee that is not returned to Gumroad (help center wording; verify live at the time of refunds); for PayPal Connect purchases, PayPal keeps its fees and the seller is responsible. Refunds can take time to appear and FX differences may apply (Gumroad charges/refunds in USD).
- Set and publish the reviewer-approved refund window; full refund if the pack never launches.

**data_fields_to_send**
- To Gumroad: product/variant (`price_variant` enum), price, the buyer's checkout details entered on Gumroad. Back to AbroadMate: order reference, `checkout_status`, `price_variant`, buyer email for delivery (transactional).

**data_fields_never_to_send**
- Card numbers/billing data (never leave Gumroad's checkout), passport/visa/medical documents, marketing-list data into Gumroad, free-text report content; never ask buyers for ID/visa/bank documents for a digital guide.

**testing_steps**
1. In test mode: complete a test purchase; verify receipt, delivery, and the order notification.
2. Issue a full test refund and a partial test refund from the dashboard; confirm status updates and observe fee treatment.
3. Simulate a failed payment; confirm no access is granted and the status is `failed`.
4. Confirm the presale page shows reviewer-approved wording, the MoR/checkout-provider disclosure, and no document requests.
5. Confirm analytics receives only `checkout_status` enum/band — no email, card or address.

**rollback_steps**
- Set the product to unpublished/draft (checkout link stops working); disable any on-site checkout buttons; issue refunds for live sales if the launch is cancelled (per policy); keep order records for tax/accounting per advice; disconnect/close the account if abandoned.

**official_documentation_urls**
- Pricing: https://gumroad.com/pricing · Issuing a refund: https://help.gumroad.com/article/47-how-to-refund-a-customer · Fees: https://help.gumroad.com/article/66-gumroads-fees · Chargebacks: https://help.gumroad.com/article/134 · Terms: https://gumroad.com/terms · Privacy: https://gumroad.com/privacy

**last_checked:** 2026-08-28

**unresolved_questions**
- Exact card-processing treatment within/on top of 10% + $0.50 at checkout for the seller's country 💲; payout availability/timing for the operator's country 🌍; whether pre-order/early-access delivery is supported natively or via a placeholder deliverable; refund fee amounts in practice (verify in a live test); Discover opt-out mechanics.

---
---

# 6. Lemon Squeezy — payment fallback (do not enable until block E signed)

**account_setup_steps**
1. Only after legal/tax sign-off: apply for a Lemon Squeezy store (store activation/approval required).
2. Complete business verification and payout details; note Lemon Squeezy acts as Merchant of Record (official pricing page: 5% + 50¢ per transaction, automated sales tax/VAT). Read the 2026 "Lemon Squeezy + Stripe Managed Payments" update before relying on terms.
3. Set store name, support email, and reviewer-approved refund policy.
4. Create the product + variant for the pack (variant maps to `price_variant` enum); use a hosted checkout link or embed.
5. Configure webhooks for order events (created/paid/refunded) so AbroadMate records `checkout_status` (status only); store the webhook signing secret in the platform's secret manager (never in code or git).
6. Enable the customer portal/receipt; set up delivery of the digital product.
7. Test in test mode (store can run test purchases) before going live.

**required_business_information**
- Legal entity/individual name, business country/address for verification, payout account details (entered into Lemon Squeezy), tax/business details for store activation.

**required_contact_information**
- Account email; public support email (refunds are primarily the seller's responsibility per official docs — buyers contact the seller first).

**privacy_or_DPA_review** ⚖
- Review terms/privacy/DPA; privacy notice must state checkout is handled by Lemon Squeezy as Merchant of Record (billing descriptor "LEMSQZY*"), with its own terms/privacy; card data never touches AbroadMate.
- Confirm webhook payload handling: store status/order reference only; do not log full buyer PII into analytics.

**consent_configuration**
- No marketing consent at checkout; separate explicit opt-in for any newsletter; no tracking pixels without consent; Lemon Squeezy checkout cookies covered by its policy and disclosed.

**unsubscribe_or_refund_configuration**
- Refunds are issued from the **Orders page action menu** (partial or full, at any time per official docs); can take up to 10 days to appear on the buyer's statement. Lemon Squeezy encourages sellers to set their own refund policy and handles chargebacks as MoR (a dispute fee is reported by third-party sources — verify in current terms 💲).
- Publish the reviewer-approved refund window and the "full refund if not delivered" promise; buyer support is the seller's responsibility first.

**data_fields_to_send**
- To Lemon Squeezy: product/variant (`price_variant`), price. Back to AbroadMate via webhook: order reference, `checkout_status` enum, `price_variant`, refunded amount/state, buyer email for delivery (transactional).

**data_fields_never_to_send**
- Card/billing data (never leaves Lemon Squeezy), passport/visa/medical/bank documents, free-text reports, marketing list data; analytics never receives buyer email/address/name.

**testing_steps**
1. Test-mode purchase end to end: checkout, receipt, delivery, webhook fires with the expected status.
2. Full and partial refund via the Orders menu; confirm webhook `refunded` state and the up-to-10-days disclosure.
3. Failed payment test → no access, status `failed`.
4. Webhook secret verification: confirm a tampered/unsigned request is rejected (configure; no secrets in the repo).
5. Confirm analytics receives only the status enum/band.

**rollback_steps**
- Archive/disable the product and checkout links; remove on-site buttons; process refunds per policy if cancelling; retain order records for accounting per advice; deactivate the store if abandoned.

**official_documentation_urls**
- Pricing: https://www.lemonsqueezy.com/pricing · Refund an order (help): https://docs.lemonsqueezy.com/help/orders/refund-order · Issue refund (API): https://docs.lemonsqueezy.com/api/orders/issue-refund · Fees help: https://docs.lemonsqueezy.com/help/getting-started/fees · Why-charge / buyer refunds: https://www.lemonsqueezy.com/why-did-lemon-squeeezy-charge-me · Privacy: https://www.lemonsqueezy.com/privacy

**last_checked:** 2026-08-28

**unresolved_questions**
- Store-activation requirements/approval time; exact surcharges for international cards/PayPal for the expected buyer mix 💲; effects of the 2026 Stripe Managed Payments change on fees/terms; webhook payload fields and DPA specifics (⚖); dispute-fee amount in current terms.

---
---

## Cross-provider tags

- **[FACT]:** Operational facts (Kit DOI default and confirmation-email settings; Brevo double-confirmation, `DOUBLE_OPT-IN` attribute and consent logs; Cloudflare beacon/CSP/SPA setup and no-cookie/no-PII position; Umami tracking-code and event model, US/EU cloud; Gumroad 10%+$0.50 direct / 30% Discover, MoR since 2025-01-01, dashboard-only refunds and fee treatment; Lemon Squeezy 5%+50¢ MoR, Orders-menu refunds up to 10 days, seller-set policy) are from official pages/help checked 2026-08-28.
- **[ASSUMPTION]:** A role-based domain email and sending-domain authentication will be available; the production site can gate scripts to the production environment; free tiers cover validation volume; payments remain disabled until the sign-off gate.
- **[REC]:** Keep double opt-in on; use custom-domain sending; gate all tracking scripts to production; enforce the analytics allow-list; never store secrets in the repo; enable payments only after `abroadmate-launch-signoff-checklist.md` block E; keep presale refund wording reviewer-approved and matched to the provider's actual mechanics.
