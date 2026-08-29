# Bangkok Tool Specifications — ThailandMate (AbroadMate edition)

**Date:** 28 August 2026 · **Scope:** MVP calculators/tools for the Bangkok launch, built on the destination-agnostic AbroadMate model (`global-product-architecture.md`).
**Tags:** [REC] specification · [ASSUMPTION] defaults to confirm · **[REGULATED — VERIFY]** signposted, not advised.
**Hard rule:** the tool outputs **directional ranges with confidence levels and sources**, never definitive figures; **no current prices are invented**. Every figure carries a `source_id`, `last_checked`, confidence and review interval (see `bangkok-content-records.json`).

---

## 1. Monthly planning tool — "Bangkok cost planner"

A reusable, parameterised tool (`?country=TH&city=bangkok&audience=…`) that builds a **monthly cost range** from a local price basket + the user's lifestyle inputs. Same engine serves future destinations with different data.

### 1.1 Categories (8 required)

| Category | Includes | Figure origin | Review |
|---|---|---|---|
| **Accommodation** | Rent/short-term serviced stay; building/utility service fees | Local listings basket (dated), 2+ sources | Quarterly |
| **Food** | Groceries + eating out split; mix of local vs Western | Local grocery/restaurant basket | Quarterly |
| **Transport** | BTS/MRT stored-value top-ups, ride-hail, occasional taxi; (car/scooter optional) | Operator fares + local ride estimates | Semi-annual |
| **Connectivity** | Mobile SIM/eSIM plan + home fiber (if long-term) | Operator published plans | Semi-annual |
| **Coworking** | Day passes / monthly membership / café spend | Coworking published rates | Semi-annual |
| **Insurance** | Travel/health premium (user-entered or linked to broker — **not quoted by us**) | User input / insurer; **[REGULATED — VERIFY]** | Quarterly |
| **Leisure** | Dining out, gym, activities, short trips | Local basket | Semi-annual |
| **Contingency** | % buffer (default 10–15% [ASSUMPTION]) on subtotal | Calculated | N/A (formula) |

*Family mode adds toggles:* school fees, school transport, domestic help, dependant healthcare — all user-entered or sourced, never assumed.

### 1.2 Inputs (user)
- Audience: remote worker / family (and later retiree); family size & children ages.
- **Length & style of stay:** first 7–14 days temporary vs long-term rental (drives accommodation/connectivity mix).
- Housing: neighbourhood (selected from framework), bedroom count, serviced vs private, budget comfort tier (lean / mid / comfortable).
- Work style: coworking membership vs home/ café.
- Transport: transit-only vs ride-hail heavy vs car/scooter.
- Food: mostly local / mix / mostly Western-grocery.
- Insurance: **user enters their own quote** (we don't price it) — optional link to regulated notice.
- Leisure frequency slider; currency display.

### 1.3 Outputs
- **Estimated monthly range** (low–high) per category and total, in THB + user's home currency (FX via **Bank of Thailand** daily rate [C017]).
- **Confidence level** per line (verified-dated-source vs estimate) with a last-checked date.
- **What to verify locally** checklist (e.g., "confirm rent & utilities from listings," "get an insurance quote").
- One-time **arrival costs** separate from monthly (deposit, flights, SIM, temporary stay).
- Breakdown chart; shareable/savable result; email-gate the saved version.

### 1.4 Figures requiring local research (do NOT publish without sources)
- Rents & deposits per neighbourhood/unit type; serviced-apartment nightly/monthly rates.
- Grocery basket (local market vs supermarket/Western); average local vs tourist meal.
- Transit: BTS/MRT fare ranges and stored-value top-up norms; typical ride-hail trip distances/costs.
- Operator SIM/eSIM and home-fiber plan prices (AIS/True/dtac).
- Coworking day-pass and monthly rates per area.
- Leisure/gym/activity costs.
- **All require ≥2 dated sources** and enter the price basket; none are hard-coded.

### 1.5 Supporting sources & cadence
- Rents: listings platforms (data, not official) — quarterly; FX: **Bank of Thailand bot.or.th** [C017] — automated.
- Transit fares: **BTS/MRT official** [C011] — semi-annual.
- Telecoms: **NBTC nbtc.go.th** + operator plans [C010][C020][C021] — semi-annual.
- Cost cross-check: Numbeo/LivingCost as **directional only** [C016] — quarterly; never sole source for a headline figure.
- Insurance: not quoted; regulator pointers **oic.or.th / longstay.tgia.org** [C006].

---

## 2. Supporting MVP tools (reuse AbroadMate components)

| Tool | Purpose | Key inputs | Outputs | Regulated? |
|---|---|---|---|---|
| **Visa comparator / signpost** | Helps users identify which route to *research*; routes to official source | Nationality, stay length, work type, family | Route names + **official links**, not eligibility verdicts | **Yes** — signpost to thaievisa.go.th / ltr.boi.go.th [C001][C002]; never advise |
| **Stay/day counter** | Calendar of physical presence; explains residency-day concepts in plain language | Entry/exit dates | Days present; *educational* prompts linking the Revenue Department [C003] | **Yes — tax**; explainer only, "not tax advice" |
| **Arrival checklist generator** | Personalises Part-1 checklist by audience/arrival date | Audience, arrival date, family? | Interactive checklist + deadline nudges for *non-regulated* steps; regulated steps = links | Mixed |
| **Neighbourhood matcher** | Scores areas against user priorities using the Part-2 framework | Priorities (work/transit/family/noise), budget, school location | Ranked shortlist **from verified table**; unknowns shown as "verify" | Low (data must be sourced) |
| **Provider directory (later)** | Lists vetted partners by category/destination | Category, city | Partner cards with disclosure; degrades to "no partner yet" | Sponsored/affiliate labelled [see global architecture §7] |

### 2.1 Rules for every tool
1. Outputs are **ranges + confidence + last-checked date**, not single guaranteed numbers.
2. Regulated results always render the **Verify-notice** and the primary official source (claims `risk_level: high`).
3. Claims with `publish_status: needs_verification` show as "verify locally" prompts, never as facts; `do_not_publish` claims never render.
4. Figures are data (a versioned price basket), not code constants — swap per destination.
5. Email-gate only the *save/export*, never the tool itself.

---

## 3. Data model fragment (ties to AbroadMate)
`cost_tool` loads: `destination → cost_basket{ category, item, local_value, currency, source_id, confidence, last_checked, review_frequency }` + user `inputs` → `outputs{ range_low, range_high, confidence, verify[] }`. Each `source_id` resolves to a record in `bangkok-content-records.json` with `source_type` and `publish_status`. Unknown/unverified items simply don't contribute a number and surface as "local research needed," so the tool never fabricates.
