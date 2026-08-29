# AbroadMate — Global Product Architecture

**Date:** 28 August 2026 · **Platform brand:** **AbroadMate** (global) · **First edition:** **ThailandMate** (Thailand; Bangkok-first launch).
**Goal:** launch Bangkok-first, but build a **destination-agnostic** platform so Bangkok/Thailand content later becomes just one "destination pack" among many. This is an implementation-ready architecture brief.
**Tags:** [REC] recommended structure · [ASSUMPTION] schema choices to confirm in build · [FACT] Thailand-specific anchors. Nothing here is legal advice.

**Brand model (decided):** the platform, accounts, tools, packs and provider directory live under **AbroadMate**. Thailand is the first **destination edition**, marketed as **ThailandMate — the Thailand edition of AbroadMate**. Future countries get their own edition labels under the same parent. (Rationale and alternatives in `global-brand-options.md`.)

---

## 1. Design principles

1. **Country is data, not code.** Every page, tool and pack is generated from structured records keyed by `destination` → `country` → `city`. Adding a country is adding data, not new templates.
2. **Separate stable evergreen content from volatile rule content.** Visa/tax/banking/insurance facts live in a versioned, source-linked, dated "rules" layer; city lifestyle content lives in an evergreen layer.
3. **Global templates, local facts.** Checklists, calculators, comparators and trackers are reusable; the figures and source links are destination-specific.
4. **Every claim has a source, a status and a review date.** The site never presents an unverified/expired rule as current.
5. **Affiliate/lead routing is provider-agnostic and tagged by destination** so monetisation works the day a destination has partners, and degrades to "no partner yet" where it doesn't.

---

## 2. URL & navigation structure

```
/                                  Global home (what is this; "pick a destination")
/destinations/                     All destinations index (country cards)
/thailand/                         Country hub (overview, cities, topics, packs)
/thailand/bangkok/                 City hub
/thailand/bangkok/live/            City topic hubs (live / work / retire / family)
/thailand/visas/                   Country topic hubs (visas, tax, banking, healthcare, telecoms, housing, transport)
/thailand/visas/dtv/               Rule/topic detail page (country-level)
/tools/                            Global tools index
/tools/visa-comparator/?country=TH&city=bangkok      Tool, parameterised by destination
/tools/tax-residency-tracker/?country=TH
/packs/bangkok-first-90-days/      Paid digital product (destination pack)
/p/[provider-slug]/                Global provider directory profiles
/legal/  /about/  /methodology/  /disclaimer/   Global trust pages
```

**Rules [REC]:**
- **Country vs city in the path:** national rules (visa, tax, banking, insurance) live at `/thailand/visas/...`; city reality (neighborhoods, rents, transit, clinics) lives at `/thailand/bangkok/...`.
- Tools are **global components** that load country/city parameters (`?country=TH&city=bangkok`); the same component serves Lisbon later with different data.
- Audience pages are cross-cutting filters/tags (remote-worker, retiree, family, founder), not separate trees: `/thailand/bangkok/?audience=remote-worker`.
- i18n path segment reserved for later (`/th/...`).

**Top nav:** Destinations · Tools · Packs · Providers · Methodology. Contextual sub-nav per country/topic.

---

## 3. Data model (destination / country / city)

All entities carry `id`, timestamps, `status` (draft/published/stale/archived), `owner`, and `next_review_at`.

```
Destination
  id (slug), name, type(country|city), parent_id (country),
  status, currency(s), language_tags[],
  region, geo{lat,lng}, public_launch_at

Country
  destination_id → Destination
  visa_rule_ids[], tax_rule_ids[], banking_rule_ids[],
  healthcare_rule_ids[], telecom_rule_ids[], housing_rule_ids[], transport_rule_ids[],
  official_source_ids[], default_currency, emergency_info{}

City
  destination_id → Destination
  country_id → Country
  neighborhoods[] : { name, persona_tags[], commute_refs[], rent_band{}, notes, sources[] }
  rent_bands{} : { by_audience, by_bedrooms, currency, last_verified_at }
  cost_basket{}            # local prices feeding calculators
  transit_modes[]          # e.g. BTS/MRT/ride-app; with links
  providers_by_category{} # provider availability per city
  airports[]
```

**Key point [ASSUMPTION]:** rule sets (visa/tax/banking/healthcare/telecom/housing/transport) attach to the **country**, because they are national; city records attach lifestyle/price/provider data. This is why Bangkok pages reuse Thailand rules — and why adding Chiang Mai later needs mostly city data, not new rules.

---

## 4. Audience & plan data model

```
Audience
  id (remote-worker|retiree|family|founder), name, description,
  priorities[], risk_profile, default_pack_id

Pack (paid product)                       # e.g. "Bangkok First 90 Days"
  id, slug, destination_id (country/city), audience_id,
  price_points_test[]{ currency, amount, label },
  modules[] : { module_type, tool_id?, checklist_id?, content_refs[] }
  status (presale|live|archived)

PlanJourney (the "first 90 days" engine)
  id, audience_id, destination_id,
  phases[] : { phase(pre_arrival|week_1|month_1|day_90),
               tasks[] : { task_id, required, rule_ref?, tool_ref?,
                           provider_category?, legal_risk_level } }

Checklist / Task / Tool
  id, type, destination_scope (global|country|city),
  rule_refs[], source_ids[], next_review_at
```

The **plan/journey model is global**; the **tasks, rule_refs and provider categories are destination-specific**. A new country = new task list + rule links, same engine, same UI.

---

## 5. Content & source model (the trust layer)

```
Content
  id, destination_id, audience_tags[], topic,
  title, body_blocks[], tool_refs[],
  legal_risk_level (none|medium|high),
  review_policy_id, next_review_at, last_verified_at,
  status

Claim            # every factual/legal statement in body blocks
  id, content_id, statement,
  source_id, source_tier (official|primary_secondary|secondary|anecdotal),
  valid_from, valid_to?, confidence, last_checked_at,
  publish_status (ok|needs_verification|do_not_publish)

Source
  id, name, publisher_type (government|official_industry|commercial|community),
  url(s), is_official(bool), topic, country,
  access_dates[], change_frequency, notes
```

**Rules [REC]:**
- Rule content renders with an inline **"verified <date> · source: <official> · rules change"** marker; high-risk pages link the primary official source directly.
- Body blocks are authored so each volatile claim maps to a **Claim**; if a claim's `publish_status ≠ ok`, the block is auto-flagged and, for legal/tax/insurance, hidden or replaced by a "verify with the official source / a licensed professional" callout.
- A `/methodology` page explains tiers and review cadence (builds trust; distinguishes from scraped blogs).

---

## 6. Freshness & review-date system

| Content class | Example | Default review interval | Behaviour when due |
|---|---|---|---|
| High-risk rules | visa eligibility/fees, tax residency, bank-account eligibility, insurance minimums | **Quarterly** (+ immediate on announced change) | Banner "due for review"; legal claims suppressed until re-verified |
| Mid-risk practical | arrival steps, SIM/transit, neighborhoods | Semi-annual | Re-check; update |
| Low-risk evergreen | cultural/general overviews | Annual | Light review |
| Live data | AQI, FX, prices | Real-time/quarterly feed | Automated where possible |

**Mechanisms [REC]:**
- `next_review_at` per content/claim; a review dashboard lists overdue items; CI blocks publish of a high-risk page with an expired primary-source link.
- Subscribe to change signals: official portals + reputable secondary trackers; a "rule-change log" per country doubles as a future **subscription/alert** product.
- Every page shows `last_verified_at`; users can flag outdated content ("report a change") feeding the review queue.

---

## 7. Provider & affiliate model

```
Provider
  id, slug, name, category (insurance|esim|vpn|housing|money-transfer|visa-agent|
       school|bank|telecom|health|movers|coworking|...),
  destination_availability[] { country, cities[] },
  partnership_type (affiliate|lead_gen|sponsored|none),
  affiliate_program_url?, commission_known(bool)  # rates never published; stored privately
  vetting_status (unverified|vetted|trusted), legal_licence_ref?,
  disclosure_text, source_ids[]

ReferralEvent
  provider_id, destination_id, audience_id, type(click|lead|sale),
  disclosed(bool), created_at   # analytics; no rates in content
```

**Rules [REC]:**
- Global directory with **per-destination availability**; categories repeat worldwide; partners slot in market by market.
- Monetisation degrades gracefully: with no partner, show the category + general guidance ("choose an OIC-approved insurer") rather than a link.
- Always label affiliate/sponsored/lead relationships; high-risk categories (visa/legal, health) link only **vetted/licensed** providers and are never presented as advice. Commission rates are internal data, never published (avoids inventing/staleness).

---

## 8. How Bangkok becomes "just another destination pack"

Bangkok launch content is authored **into the global model from day one**, so:
1. **Thailand rules** already sit at country level (`/thailand/visas/...`, `tax_rule_ids[]`, etc.) — reusable by future Thai cities (Chiang Mai, Hua Hin, Phuket, Pattaya) as city packs that add only neighborhoods/prices/providers.
2. The **"Bangkok First 90 Days" pack** is a `Pack` record bound to `audience=remote-worker` and `destination=/thailand/bangkok`. A future "Lisbon First 90 Days" is the same `PlanJourney` engine + same tools with Portuguese rule/source data and different providers.
3. **Tools are parameterised components** (visa comparator, day-count tracker, cost calculator, banking checker, commute matcher) — new destination = new data + sources, not new code.
4. **Reuse ladder:** global platform/brand → country rules → city packs. Bangkok proves the template; each new destination gets faster because the templates, review system and provider schema already exist.

---

## 9. Global vs destination-specific features

| Global (built once, reused) | Destination-specific (data per market) |
|---|---|
| Platform/brand, nav, design system | Visa categories, fees, application portals |
| Packs/journey/checklist engine | Tax residency rules & day thresholds |
| All tools (comparator, day tracker, cost calc, banking checker, commute matcher) | Bank account eligibility by visa |
| Content & source model, claim-tier tagging, review/staleness system | Insurance minimums & approved-provider lists |
| Provider directory schema & affiliate/lead routing logic | Providers, insurers, agents, schools, coworking |
| Email capture, presale, accounts, payments | Neighborhoods, rent bands, transit, clinics |
| Disclaimers, methodology, disclosure framework | Local prices, currency, emergency info, language |
| Search & community-distribution playbooks | Local laws, official source URLs, telecom plans |

---

## 10. Implementation note for the current Bangkok build
- Even though only Bangkok ships, author Thailand **rule pages at country level** and Bangkok **lifestyle pages at city level**; tag every claim with source tier + `next_review_at` (cadence in §6).
- Build the four highest-value tools as **parameterised components now** (visa comparator, 180-day tracker, cost calculator, banking checker) so they aren't rewritten for the second destination.
- Store providers/affiliate links in the directory with destination availability and vetting status — no hard-coded links in content.
- Seed data structure is provided in `bangkok-content-seed.csv` (it already separates `country`-level rule rows from `city`-level lifestyle rows).
