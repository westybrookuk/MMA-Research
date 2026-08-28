# Bangkok Neighbourhood Matcher — Specification

**Date:** 28 August 2026 · Platform AbroadMate / edition ThailandMate · Destination `thailand-bangkok`
**Audience:** primary English-speaking remote workers/digital nomads; secondary self-funded relocating families.
**Companion data:** `bangkok-neighbourhood-data.json`, `bangkok-neighbourhood-sources.csv`.
**Tags:** [REC] recommendation · [ASSUMPTION] scoring default to confirm. Core principle: the matcher **ranks areas by the user's own priorities from sourced data; it never recommends or guarantees** a neighbourhood, price or commute.

---

## 1. How user priorities are collected

A short, optional questionnaire. Answers set **weights (0–3)** and filters; all inputs are coarse and reversible.

**Remote-worker path:**
1. Budget band for rent (slider in THB, or "unknown").
2. Must-have rail access? (`BTS/MRT essential` / `nice to have` / `fine without`).
3. Airport frequency? (`fly monthly+` / `a few times a year` / `rarely`) — weights Airport Rail Link access.
4. Work style? (`coworking` / `home/quiet` / `cafe-hopping`) — weights work suitability and coworking.
5. Pace preference? (`busy/central` / `local/calm` / `balanced`).
6. Nightlife/social? (`important` / `some` / `quiet please`).

**Family path (adds/replaces):**
1. Total household rent band.
2. School(s) or district in consideration (free-entry or pick) — **the single biggest family weight**; commute to school is computed (or shown as "add your school").
3. Need for space (studio/1BR vs 2BR+/house).
4. Rail vs car tolerance; grocery/healthcare convenience importance.
5. Calm/safety priority (normally high for families).

**Filters (not weights):** audience, min bedrooms, max rent (hard cut), rail-required toggle.

Weights map to the data dimensions: `work`, `transport.rail`, `transport.airport`, `convenience`, `calmness` (inverse of noise/pace), `family`, `coworking`, `rentFit` (does the area's range overlap the user's band).

## 2. How scores are calculated

[ASSUMPTION defaults — tune after validation.]

1. **Only verified data contributes.** For each dimension the matcher uses a value *only* if the underlying field/record has `confidence` in {medium, high} and `publishStatus = ok`. Missing/unverified → dimension is excluded from that area's score (not scored as zero), and shown as "verify".
2. **Per-dimension score (0–5):**
   - `rentFit`: overlap between the area's **low–high rent range** and the user's band → 5 if typical sits within band, scaled down; `needs_verification` rent → not scored, flagged.
   - `transport.rail`: railLines/stations present + stations documented → scored from a verified rail-access table (BTS/MRT/ARL count and interchange), **not** from the word "near."
   - `transport.airport`: presence/quality of ARL/road airport access.
   - `work` / `coworking`: only once coworking/connectivity is source-confirmed (initially unscored or low weight).
   - `calmness`: from sourced noise/pace notes, normalised 1–5.
   - `convenience`, `family`: sourced notes normalised 1–5; family dimensions are near-unscored until verified (confidence low/unverified).
3. **Weighted sum** = Σ (dimension score × user weight) / Σ (applicable weights), producing a 0–100 match **with a confidence penalty**: `displayScore = weightedScore × coverage`, where `coverage` = fraction of weighted dimensions that were actually verified for that area. This prevents a thin-data area looking like a strong match.
4. **Audience weighting presets** (overridable): remote-worker preset emphasizes work/rail/airport/rent/coworking; family preset emphasizes family/space/school-commute/convenience/calm.

## 3. How missing data is displayed

- Unknown dimensions render as a grey **"Verify — not yet source-backed"** chip, **never** as a value or a guessed score.
- Areas with low `coverage` are shown lower and labelled **"Limited verified data — research candidate"** (this currently applies to qualitative scores and some 2BR/studio records).
- Every rent figure links to the source listing(s) and shows `lastChecked` + `confidence`; ranges are labelled **"estimated range from sampled listings, not a quote."**
- Commute times carry a **"planning estimate — confirm door-to-door in a maps app at rush hour"** note; until then, only documented **station counts** are shown as fact.
- `publishStatus = needs_verification` records are visible in the UI only as bracketed estimates with a clear flag, or hidden behind a "show unverified data" toggle (default off).

## 4. How remote-worker and family results differ

- **Remote workers:** ranked primarily by rail access, airport ease, rent fit, coworking/connectivity and pace preference; the workspace filter reshuffles (coworking → central Sukhumvit/Asok; home/quiet → Ari/On Nut/Phra Khanong; budget → On Nut/Bang Na).
- **Families:** the **school/commute dimension dominates**; results emphasise 2BR+ availability, space/value (On Nut/Bang Na/Ari/Sathorn), calmness and convenience, and de-emphasise nightlife. If a school is entered, the matcher orders by sourced door-to-door commute band; if not, it prompts for it and shows area-level trade-offs only.
- Both see the same underlying data; only weights, default filters and highlighted attributes differ. Family results carry an explicit reminder to verify school catchment/placement directly with schools (link ISAT).

## 5. How source confidence appears

Every figure/score shows a confidence marker:
- **✓ Verified (dated)** — sourced, `ok`, medium/high confidence, with a `lastChecked` date and source link.
- **◐ Estimated** — multi-source but guide-level or weakly corroborated (low confidence) — shown as a range with caution.
- **✗ Verify** — single/anecdotal source or `needs_verification` — suppressed by default or clearly flagged.
A global line states data date and the review cadence (rents/transit **quarterly/semi-annual**) and links `/methodology`.

## 6. Avoiding recommendations-as-guarantees (UI rules)

- Language is comparative and conditional: "areas that match your priorities," not "the best area" or "you should live here."
- Always show a **shortlist (3 areas)** with top pros *and* trade-offs/risks, plus "what to verify yourself" (visit at rush hour, check exact building's BTS walk distance, confirm rent and lease).
- Rents: "estimated ranges, not quotes — confirm on live listings"; leases/deposits: "common market practice, not a legal rule — read your contract."
- Commutes: "station counts are documented; door-to-door times are estimates — verify at peak."
- No legal/visa/tax guidance anywhere in the matcher; regulated needs route to the existing verify-notices/official sources.
- No claim of partner endorsements or "vetted" status unless independently verified (per partner-readiness rules).

## 7. Facts / assumptions / recommendations
- **Facts [FACT]** (2026-08-28, sourced in JSON/CSV): BTS Sukhumvit-line station order and station codes (On Nut E9, Phra Khanong E8, Bang Na/Udom Suk/Bearing east); Phaya Thai = BTS/ARL interchange; Asok = BTS/MRT interchange; Sathorn served by Silom-line BTS (Chong Nonsi etc.) + MRT; sampled rent ranges per area; common 1-year / 2-month-deposit market practice as reported on listings.
- **Assumptions [ASSUMPTION]:** the scoring weights, coverage penalty, dimension normalisation, and commute-time bands; qualitative area character is cautiously sourced and must be editorially verified before any score is populated (all scores ship as `null`).
- **Recommendations [REC]:** ship with scores null/unverified and only documented rail facts + flagged rent ranges; populate qualitative scores via a documented editorial verification pass before enabling ranking; default-unverified off; never present outputs as guarantees; re-verify rents/transit quarterly.
