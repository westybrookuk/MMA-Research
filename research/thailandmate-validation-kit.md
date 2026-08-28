# ThailandMate — Low-Cost Validation Kit

**Date:** 28 August 2026 · Goal: prove (or kill) the core hypothesis in **30 days for under ~US$100** before building city/persona breadth.

**Tags:** [FACT] verified · [ASSUMPTION] judgement call (all thresholds below are starting hypotheses, not benchmarks) · [REC] recommendation.
The hypothesis being tested [REC]: *English-speaking DTV digital nomads planning/in their first 90 days in Chiang Mai will pay ~$29–49 for a current, trusted, all-in-one "First 90 Days" pack — and a subset will want referrals for insurance, connectivity and visa help.*

**Method:** customer interviews (discovery) + three community recruitment posts + three landing-page smoke tests. Use a simple stack: a one-page site (Carrd/Webflow/Notion), an email capture (MailerLite/Buttondown free tier), a presale checkout (Gumroad/Lemon Squeezy), and a calendar booking link (Calendly). No product needs to be finished — presales/waitlist validate demand; refund if you don't ship.

---

## 1. Fifteen interview questions

Target: **12–15 interviews**, ~20–30 min each (video/voice), split nomads vs retirees (~2:1). Recruit people *planning a move* and *within first 6 months in Thailand*. Ask about behaviour, not opinions. Do **not** pitch during the interview.

**A. Situation & journey**
1. Walk me through your decision to move to Thailand — what stage are you at (researching, visa in progress, arrived, first year)?
2. Which city did you choose (or are you choosing), and what made you pick it over the others?
3. What visa are you on/applying for, and why did you choose it over the alternatives?

**B. Problems & pain**
4. What was the single most frustrating or confusing part of your move so far? Tell the story.
5. How did banking go — did you manage to open a Thai account? What happened? *(Probes: which visa, which bank, refusals, workarounds.)*
6. How do you think about the 180-day tax-residency rule and bringing money into Thailand? What have you done about it?
7. How did you handle your first-week setup: SIM/internet, TM30, arrival card, transport, finding a place?
8. *(Nomads)* What do you do about the February–April air-quality period? Did it change your plans?
9. *(Retirees)* Walk me through your health-insurance situation — what you bought, what it cost, any surprises with age/pre-existing conditions or the visa requirement.

**C. Information & alternatives**
10. Where did you actually get your information? (Forums, Facebook groups, blogs, agents, friends?) Which sources did you trust — and which did you stop trusting?
11. What did you pay for during your move (visa service, agent, insurance, eSIM, accommodation, consult)? Roughly how much, and was it worth it?
12. What questions could you **not** find a reliable answer to?

**D. Value & offer**
13. If someone had handed you a single, current "first-90-days" guide with checklists, a banking playbook, a tax tracker and vetted provider recommendations — would you have bought it? What would make it worth paying for vs free blogs?
14. What would you expect a product like that to cost? *(Listen for the number unprompted; don't anchor.)*
15. If it also connected you to vetted visa agents, insurers, or housing help — would you use that? What would make you trust a recommendation?

**Interpretation note:** score each interview for (a) a painful, recurring, recently-worsened problem, (b) evidence they already spend money, (c) a specific feature they'd pay for, and (d) the city/audience pattern. Watch especially for the **banking** and **tax-tracking** stories — these are the wedge.

---

## 2. Three recruitment messages for expat communities

Rules: add value first, never spam, read group rules, ask mod permission where needed. Offer a genuinely useful free thing (the checklist/tracker) in exchange for 20 minutes.

**Message A — Digital nomad / DTV crowd (r/digitalnomad, r/Thailand, Chiang Mai Digital Nomads FB, Discord/Slack)**
> "I'm building a free, up-to-date **First-30-Days Thailand checklist** (DTV documents, the 2026 bank-account situation, SIM/eSIM, TM30/arrival card) and a simple **180-day tax-residency tracker**. I've been digging into the fact that DTV holders now get turned away by the major banks and want to get it right from people actually going through it.
> If you're planning a move or arrived in the last ~6 months, I'd love **20 minutes** to hear what tripped you up — no pitch, I'll send you the finished checklist + tracker free as a thank-you. Comment or DM and I'll set it up."

**Message B — Retiree crowd (ASEAN Now, Hua Hin/Chiang Mai/Pattaya retiree FB groups, expat clubs)**
> "I'm researching a free, practical guide for people **retiring to Thailand** — covering the Non-O vs O-A choice, the health-insurance requirement (and what it costs as you get older), the 800k-baht/65k rule, and day-to-day setup in places like Hua Hin and Chiang Mai.
> I'd really value **20 minutes** with anyone who's made the move (or is mid-way) to learn what surprised you and what you wish you'd known. No sales, just research — happy to share the finished guide with everyone who helps."

**Message C — Coworking / local spaces (Punspace/CAMP/Yellow in Chiang Mai; Coworq/Hatch/Hua Hin Workspace) and Meetup groups**
> "Hey all — I'm putting together a free **newcomer survival kit** for remote workers arriving in Thailand (banking, connectivity, visas, where to live, burning-season planning). I'm doing short **20-minute interviews** with folks in their first few months to make sure it reflects reality, not blog myths.
> If you're happy to grab a coffee (on me) or jump on a quick call, DM me. Everyone who helps gets the kit, and I'll share a summary of what I learn back to the group."

---

## 3. Three landing-page tests

All three run as **smoke tests** (real traffic to a "get notified / pre-order" button; no finished product yet). Drive traffic from the recruitment posts, your own social, and ~$0–50 of low-cost boosting only if organic is thin. Each test changes **one variable**.

**Landing test 1 — Positioning / headline.**
- Variant A (**product**): *"The First 90 Days in Thailand — Chiang Mai Edition. Everything you need to land, set up, and stay legal."*
- Variant B (**pain**): *"Thailand just stopped DTV nomads opening bank accounts. Here's exactly what to do in your first 90 days."*
- Measure: email-capture rate + click-to-preorder. Picks whether fear/pain or completeness is the stronger hook.

**Landing test 2 — Price.**
- Identical page, presale button at **US$29 vs US$49** (stated early-bird, list $49).
- Measure: pre-order conversion and revenue; a ~same conversion at $49 validates the higher price; far higher conversion at $29 signals price sensitivity.
- Note [ASSUMPTION]: deliberately priced below the cheapest verified human touchpoint (US$100 TSK consult / US$149 Baan Smile).

**Landing test 3 — Scope / city.**
- Variant A: **Chiang Mai only.** Variant B: **"Chiang Mai now — Hua Hin for the burning-season escape" (two-city bundle).**
- Measure: which drives more signups/pre-orders and whether retirees self-select into the Hua Hin variant. Validates the city-pair strategy.

**Common page elements:** one-line value prop; 5 bullet outcomes (visa/compliance, banking, connectivity, housing, healthcare/tax); the free lead-magnet opt-in (checklist + tax tracker); a "pre-order for early-bird price" button; an "I'd rather hire someone / get referred" button (captures lead-gen demand); a last-updated date and sources note (trust signals).

---

## 4. Suggested success thresholds (30 days)

**These are starting hypotheses [ASSUMPTION] — set them before launch and treat hitting/missing as the go/no-go signal. Budget ~US$100.**

| Metric | Threshold (30 days) | What it tests |
|---|---|---|
| Landing-page visitors | **1,000+** | Distribution reach; if unreachable organically, channel risk |
| Email sign-ups (lead magnet) | **300–500+** (~8–12% capture) | Problem/topic resonance |
| Customer interviews completed | **12–15** | Qualitative depth |
| **Guide pre-orders** (or paid reservations) | **15–25+** | Actual willingness-to-pay |
| Pre-order conversion (of email list) | **3–6%+** | Offer strength |
| "Hire someone / refer me" enquiries | **5+** | Lead-gen demand (insurance/visa/housing) |
| Affiliate link clicks (insurance/eSIM) | **tracked, ≥ 100** | Early monetisation intent |

**Go / no-go logic [REC]:**
- **Green (build it):** ≥300 emails **and** ≥15 pre-orders **and** ≥5 provider enquiries → build the Chiang Mai pack and the Hua Hin fast-follow.
- **Amber (tool-led, not product-led):** strong email/tool traffic but few/no pre-orders → demand is for free tools; grow the list, monetise affiliates + lead-gen, revisit the paid product later.
- **Red (pivot audience or city):** weak email *and* no pre-orders → reassess city (Hua Hin retirees?) or audience before investing further.
- Record and report: top 3 pains heard, price signal from Q14 unprompted, which cities/visas respondents chose, and every number above.

---

## 5. Guardrails
- **[LEGAL-RISK]** Do not give personalised tax, visa or insurance advice in interviews/on the page; say "this is general info; verify with the official source / a licensed professional" and link official sources.
- Never promise outcomes (approvals, account openings). All compliance content carries a **last-updated date**.
- Do not publish affiliate commission rates, traffic claims or competitor figures you have not verified. Competitor prices used in positioning are the verified TSK (US$100 / $1,500 / $2,600) and Baan Smile (US$149) figures from the opportunity report.
- Refund presales if the pack doesn't ship on the stated timeline.
