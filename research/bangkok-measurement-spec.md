# Bangkok Measurement Specification — ThailandMate (AbroadMate)

**Date:** 28 August 2026
**Purpose:** define the events to instrument in the Bangkok MVP (First 7 Days checklist, monthly planning worksheet, First 90 Days preview, global data model) so we can validate audience and product interest **without collecting unnecessary personal information**.

**Tags:** [REC] recommended spec · [ASSUMPTION] retention/window defaults to confirm.
**Privacy principles [REC]:**
- Collect the **minimum** needed: anonymous event names, timestamps, destination/city, audience, and coarse inputs (ranges/sliders, not free-text PII).
- **No** names, passport/visa numbers, contact details in analytics; email is stored separately in the mailing list (not in the event stream).
- Use a random anonymous `session_id` (first-party cookie/localStorage); do not cross-device fingerprint; honour Do-Not-Track; provide an analytics-opt-out.
- Send only generic category/labels (e.g. `category=accommodation`, never addresses or employers). Regulated inputs (visa type, income, insurance) are coarse self-declared selections, not documents.
- Document events in a data dictionary; review against applicable privacy requirements before launch.

All events are reusable across destinations via the global model: events carry `destination`, `country`, `city`, `audience`, `product` (e.g. `bangkok-first-90-days`), `variant` (A/B), and `anon_session_id`.

---

## Event dictionary

### 1. `planner_completed` — monthly planning worksheet finished
- **Fires when:** user reaches the budget results screen (all required inputs present).
- **Track:** audience (`remote_worker`/`family`), destination/city, inputs as **ranges/bands** (stay length band, housing tier, work-style, transport mode, food style — all enumerated), whether insurance quote was self-entered (boolean only), result total **band** (low/typical/high bucket, not exact), plan variant.
- **Do NOT track:** exact salary/income, employer, bank, address, document images, free-text notes.
- **Why:** confirms the tool is completed and which lifestyle profile users match — drives basket accuracy and persona validation.

### 2. `checklist_item_completed` — First 7 Days checklist item ticked
- **Fires when:** a checklist item is checked off (debounced; include `item_id` and category).
- **Track:** `item_id`, `category` (arrival/transport/connectivity/housing/work/money/health/regulated-signpost), audience, city, day-phase (pre-arrival/week-1), cumulative completion %, whether item opened an official-source link.
- **Do NOT track:** any data entered for regulated steps (visa/banking/insurance are *links*, not form fields).
- **Why:** reveals which tasks matter and where users stall (esp. whether they click the regulated signposts).

### 3. `budget_estimate_entered` — user inputs budget assumptions (mid-funnel)
- **Fires when:** a planner field is set/changed (throttled) **and** on "calculate."
- **Track:** field names changed (categories), selected bands/toggles (e.g. coworking on/off, family size bucket), currency selected.
- **Do NOT track:** precise income or insurance premium amounts beyond coarse bands; no free text.
- **Why:** shows which categories users customise most (informs basket research priority) before they even finish.

### 4. `budget_saved` — planner result saved / exported
- **Fires when:** user saves, emails, downloads or bookmarks their budget.
- **Track:** save method (email/PDF/copy-link), audience, city, result band, whether email capture used (linked to mailing list consent, not stored in analytics).
- **Do NOT track:** the saved budget's free-text labels; store the email only in the consented list.
- **Why:** saving is a strong value signal and the primary email-list conversion for the free tool.

### 5. `early_access_signup` — joined the waitlist / email list
- **Fires when:** user submits the signup form (any page).
- **Track:** entry point (`source_page`, e.g. checklist/planner/90-day preview), audience, city, campaign/ref, consent flag (boolean), variant.
- **Do NOT track:** email address in the analytics event (it lives in the ESP under consent).
- **Why:** primary top-of-funnel metric; ties signup to the page/message that converted.

### 6. `paid_pack_interest` — Bangkok First 90 Days product-preview intent
- **Fires when:** user clicks the paid-pack CTA, opens the pricing/preview modal, selects a price option (presale), or starts checkout.
- **Track:** `action` (`view_preview`/`click_cta`/`select_price`/`begin_checkout`), price point shown (the A/B hypothesis label, not the user's data), audience, city, variant, entry page.
- **Do NOT track:** payment data (handled by the payment provider); treat completed purchase separately via provider webhook (order id + amount only).
- **Why:** this is the core willingness-to-pay signal; distinguishes curiosity from checkout intent.

### 7. `provider_referral_click` — outbound partner / official-source click
- **Fires when:** user clicks an affiliate, lead-gen, sponsored, **or official-source** link.
- **Track:** `link_type` (`affiliate`/`lead_gen`/`sponsored`/`official_source`), `category` (esim/vpn/insurance/money-transfer/housing/coworking/visa-legal), provider slug, destination, audience, disclosure shown (boolean), `verification_status` shown to user.
- **Do NOT track:** personal details; no commission data in the client event.
- **Why:** measures monetisation intent and the value of regulated signposts; also confirms disclosure rendered.

---

## Cross-cutting properties (on every event)
`anon_session_id`, `event_ts`, `destination=Thailand`, `city=Bangkok`, `audience`, `product` (first-7-days / planner / first-90-days), `ab_variant`, `referrer_category` (search/community/partner/direct), `app_version`.

## Funnel the events map to
1. Tool/visitor → 2. `budget_estimate_entered` / checklist use → 3. `checklist_item_completed` → 4. `planner_completed` → 5. `budget_saved` → 6. `early_access_signup` → 7. `paid_pack_interest` (→ provider-verified purchase) → 8. `provider_referral_click`.

## Suggested validation thresholds (from `bangkok-validation-plan.md`, to wire as dashboard targets [ASSUMPTION])
- Planner completion rate, checklist engagement, save/signup conversion, paid-pack CTA→checkout, referral-click rate — all compared against the 7-day experiment targets (signups, pre-orders, affiliate/lead clicks).

## Facts / assumptions / recommendations
- **Facts:** the four live MVP surfaces are the checklist, planning worksheet, First 90 Days preview and the global data model.
- **Assumptions:** retention windows, exact conversion targets, and the events-platform choice (e.g. privacy-friendly analytics) are to be confirmed at build.
- **Recommendations [REC]:** instrument these seven events first; keep all inputs coarse/banded; keep email and payment data out of the analytics stream; log disclosure-rendered on every referral click; review the dictionary with privacy/legal before launch.
