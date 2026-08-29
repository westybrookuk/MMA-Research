# Bangkok Neighbourhood Matcher — Publication-Gate Audit

**Date:** 28 August 2026 · AbroadMate / ThailandMate · Destination `thailand-bangkok`
**Inputs audited:** `bangkok-neighbourhood-data.json`, `bangkok-neighbourhood-sources.csv`, `bangkok-neighbourhood-matcher-spec.md`.
**Purpose:** decide exactly what may be shown publicly before the matcher goes live. **No qualitative impression is converted to a score without a documented, repeatable basis — scores stay `null` until an editorial verification pass.**
**Tags:** [FACT] verified structure · [ASSUMPTION] estimate · [REC] recommendation. Statuses: **ok** (publish as labelled estimate/fact) · **needs_verification** (hold or show only behind an explicit "unverified" flag) · **do_not_publish** (suppress until sourced).

---

## 1. Area-by-area audit

### Rail facts: station presence, names, lines, counts
The BTS/MRT/ARL **structure is stable and documented** (BTS network maps, Wikivoyage Sukhumvit, Moovit route data, station references). These are **[FACT]-grade and `ok` to publish as facts**:

| Fact | Status | Basis |
|---|---|---|
| On Nut, Phra Khanong, Bang Na/Udom Suk/Bearing are on the **BTS Sukhumvit (green) line** | ok | Wikivoyage/Moovit station order; Wikipedia On Nut (E9) |
| Central Sukhumvit served by Asok/Phrom Phong/Thong Lo/Ekkamai (BTS) and **MRT Sukhumvit interchange at Asok** | ok | BTS/MRT network; Wikivoyage |
| Sathorn/Silom served by **BTS Silom line** (Chong Nonsi, Surasak, Sala Daeng) + **MRT** (Silom/Lumphini) | ok | BTS Silom line; Sathorn area references |
| **Phaya Thai = BTS + Airport Rail Link interchange**; Ari/Saphan Khwai on Sukhumvit line | ok | ARL/BTS network; DDProperty area refs |
| Station **stop counts** along the same line (derivable from station order) | ok — label "station count, not time" | station order |
| Exact **door-to-door commute minutes** at rush hour | **needs_verification** | our times are estimates; only counts are documented |
| MRT **Yellow Line** monorail serving Bang Na–Samrong | **needs_verification** | confirm stations/walk access per address before claiming |

Documented same-line counts used (rail ride only, excludes walking/waiting): **On Nut(E9)→Asok(E4) = 5 stops**; **Ari(N5)→Siam = 5 stops** (via Sanam Pao/Victory Monument/Phaya Thai/Ratchathewi); **Bang Na(E13)→Asok(E4) = 9 stops**. Publish counts; suppress minutes until map-verified.

### Per-area data points

| Area | Data point | publish_status | Reason / action |
|---|---|---|---|
| **On Nut** | BTS On Nut (E9), Sukhumvit line | ok | documented |
| | 1-bed range 12,000–22,000 | **ok** (as estimate) | thailand-property listings + two guides |
| | Studio 10,000–14,000 | **needs_verification** | inferred; gather dedicated studio listings |
| | 2-bed 24,000–41,000 | **do_not_publish** | single listing source; need 2nd |
| | Commute "~30–45 min door-to-door" | **needs_verification** | show 5-stop count only |
| | Noise/convenience/family/work descriptions | **needs_verification** | cautious editorial; scores stay null |
| | Deposit/lease market-practice note | **ok with caveat** | listings cite 1yr/2-mo deposit; label "common practice, not a legal rule" |
| **Phra Khanong** | BTS Phra Khanong (E8) | ok | documented |
| | Studio / 1-bed ranges | **needs_verification** | guide-level only; no dedicated listing source gathered |
| | 2-bed range | **do_not_publish** | extrapolated/weak |
| | Coworking venue claims | **needs_verification** | confirm venues exist/operate |
| | Qualitative descriptions | **needs_verification** | editorial pass required |
| | Commute minutes | **needs_verification** | show ~4-stop count only |
| **Ari / Phaya Thai** | BTS Ari/Saphan Khwai/Phaya Thai + ARL at Phaya Thai | ok | documented |
| | 1-bed 19,000–30,000 | **ok** (estimate) | DDProperty + thailand-property listings |
| | 2-bed 26,000–45,000 | **ok** (estimate) | multiple DDProperty/thailand-property 2BR samples |
| | Studio 14,000–22,000 | **needs_verification** | inferred band; gather studio listings |
| | ARL airport access wording | **ok** (Phaya Thai ARL interchange is a fact; total journey time = estimate) | state interchange, verify journey time |
| | Qualitative (cafe/calm/family) | **needs_verification** | scores stay null |
| **Central Sukhumvit** | BTS Asok/Phrom Phong/Thong Lo/Ekkamai + MRT at Asok | ok | documented |
| | 1-bed 20,000–35,000 | **needs_verification** | guide ranges; confirm with live listings before publish |
| | Studio 15,000–25,000 | **needs_verification** | guide-level |
| | 2-bed 40,000–70,000 | **do_not_publish** | weakly sourced in this study |
| | Coworking supply / convenience | **needs_verification** for venue names/price; general amenity density is broadly supported but scores stay null |
| | Commute to Sathorn | **needs_verification** | show rail-interchange guidance only |
| **Sathorn / Silom** | BTS Silom line (Chong Nonsi/Surasak/Sala Daeng) + MRT | ok | documented |
| | 1-bed 22,000–40,000 | **needs_verification** | Hipflat aggregator average + single-building review; wide; confirm live listings |
| | Studio 16,000–24,000 | **needs_verification** | one aggregator average |
| | 2-bed 35,000–65,000 | **needs_verification** | very wide; narrow with listings |
| | Deposit note (1yr/2-mo explicitly cited here) | **ok with caveat** | listings state it; label market practice |
| | Qualitative (CBD/park/hospitals) | **needs_verification** | scores stay null |
| **Bang Na** | BTS Bang Na(Udom Suk/Bearing) Sukhumvit line | ok | documented |
| | 1-bed 11,000–18,000 | **ok** (estimate) | many thailand-property listings + Renthub |
| | Studio 9,000–13,000 | **needs_verification** | mixed sources; confirm studio stock |
| | 2-bed 22,000–30,000 | **do_not_publish** | single aggregator average |
| | Family/space value + Mega Bangna/BITEC convenience | **needs_verification** | broadly supported but scores stay null |
| | Commute "~45–60 min to Asok" | **needs_verification** | show 9-stop count; flag the trade-off in words, not a time |
| | Yellow Line claim | **needs_verification** | confirm before naming stations |

### Cross-cutting status summary
- **Safe to show at launch (behind the labelling rules in Part 2):** rail lines/stations/interchanges; same-line station counts; the **`ok` rent ranges** (On Nut 1BR; Ari 1BR & 2BR; Bang Na 1BR) as clearly-labelled estimates; the deposit/lease **market-practice caveat**.
- **Hold behind the "unverified" toggle or suppress:** all studio ranges except none (all studios need verification); `needs_verification` rent ranges; all door-to-door minutes; all qualitative scores (**kept null**); specific coworking venue names/prices; the Yellow Line detail; `do_not_publish` 2-bed ranges (On Nut, Phra Khanong, Central Sukhumvit, Bang Na).
- **No area may display a numeric match score until qualitative dimensions pass an editorial, repeatable verification pass** (see matcher spec §2/§5). Until then the matcher ranks only on documented rail + `ok` rent-fit, with a large "limited verified data" coverage note.

---

## 2. Safe, exact user-facing wording (Part 2)

Use verbatim. `{area}`, `{low}–{high}`, `{date}`, `{pct}` are merge fields.

**Estimated rent range**
> "Estimated **{type}** rent in {area}: roughly **{low}–{high} THB/month**, based on sampled listings dated {date}. This is an indicative range, **not a quote or guarantee** — live listings change daily and vary by building, floor and fit-out. Confirm current prices on the listings linked."

**Research candidate / limited data**
> "{area} is a **research candidate with limited verified data**. We can show documented transport facts and some rent estimates, but we haven't yet verified its day-to-day character, coworking and family fit. Treat it as a starting point for your own visits — not a recommendation."

**Data point not yet verified**
> "Not yet source-backed — we're verifying this. Don't rely on it for a decision yet."

**Commute estimates**
> "By rail this is about **{n} BTS/MRT stops** from {anchor}. We don't quote door-to-door minutes: they depend on walking distance, waits and time of day. **Check the journey in a maps app at 8:00 and 18:00** before you choose where to live."

**Lease & deposit information**
> "Long-term Bangkok rentals **commonly** ask for a 1-year contract with around a **2-month security deposit plus 1 month in advance**. This is common market practice, **not a legal rule**, and terms vary per landlord and contract. Read your lease carefully — deposits, utility billing and what's included differ."

**"Verify before deciding" notice**
> "Please verify before you decide. This tool ranks areas from **documented transport facts and sampled rent estimates**; it is general information, **not financial, legal, tax, visa or property advice**, and not a recommendation or endorsement of any area, building or agent. Visit at rush hour, confirm walk distances from your exact address, and check live listings and contracts yourself."

**Signal score / coverage (only shown once editorial scoring is enabled)**
> "Match strength **{pct}%** — based on your priorities and the **{pct}% of factors we have verified data for** for this area. A high score with low coverage means thin data, not a sure bet. Scores compare areas to your answers; they are not advice or a guarantee."

**Source/freshness line (global)**
> "Data checked {date} · rent and transport figures are reviewed at least quarterly · [see how we source data](/methodology) · **Spot something wrong? [Report a change](#report)**."

**Affiliate/partner disclosure (on any provider link)**
> "Some links are partner/affiliate links and we may earn a fee, at no extra cost to you. This never changes our data or comparisons, and we don't label anything 'vetted' unless independently verified. [More on how we're funded](/disclosure)."

**Hard rules reflected in the wording:** no "best/guaranteed/you should," no legal-rule framing for leases, no quoted prices as facts, no endorsement language, and every regulated topic routes to the existing verify-notices.

---

## 3. Facts / assumptions / recommendations
- **Facts [FACT]** (2026-08-28, sourced): BTS/MRT/ARL lines, stations and interchanges above; same-line station counts; the `ok` rent ranges (On Nut 1BR; Ari 1BR/2BR; Bang Na 1BR) from dated listings; the common 1-year/2-month deposit practice as reported on listings.
- **Assumptions [ASSUMPTION]:** all rent `typical` mid-points, studio bands, door-to-door times, and every qualitative character note; Yellow Line access detail. Scores remain null.
- **Recommendations [REC]:** launch with rail facts + station counts + `ok` rent estimates + deposit caveat only; gate all qualitative scoring behind an editorial verification pass; default-hide `needs_verification`/`do_not_publish`; show coverage % with any score; use the Part 2 wording verbatim; refresh rents quarterly and rail facts semi-annually (see `bangkok-source-refresh.csv`).
