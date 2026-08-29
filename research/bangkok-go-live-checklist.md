# Bangkok Go-Live Checklist (publication gate)

**Date:** 28 August 2026 · AbroadMate / ThailandMate · Must be satisfied before the Bangkok neighbourhood matcher / planner / checklist is publicly exposed.
Tags: [REC] required actions · [FACT] established policy. Anything legal/privacy should be reviewed by a qualified person before launch; this list is an operational gate, **not legal advice**.

---

## A. Content verification
- [ ] Every **`do_not_publish`** record in `bangkok-neighbourhood-data.json` is suppressed or has been re-sourced (notably the uncorroborated 2-bed ranges and studio bands — see `bangkok-source-refresh.csv`).
- [ ] **`needs_verification`** items are hidden behind the default-off "show unverified" toggle or re-sourced to `ok`.
- [ ] All **qualitative scores remain null** until a documented, repeatable editorial verification pass; no impression converted to a number.
- [ ] Rent figures display as **estimated ranges**, link their listings, show `checkedAt`, and use the approved wording ("not a quote or guarantee").
- [ ] Commutes show **station counts**, not invented door-to-door minutes; minutes appear only after map verification.
- [ ] Rail lines/stations/interchanges re-checked against BTS/MRT/ARL official maps (add official operator URLs).
- [ ] No area described as "best" or guaranteed; comparative, conditional wording only.

## B. Affiliate disclosure
- [ ] A clear **affiliate/partner disclosure** is visible wherever any affiliate/sponsored/lead link appears (use the wording in the publish-audit Part 2).
- [ ] No provider labelled "vetted/trusted/recommended" unless independently verified (per `bangkok-partner-readiness.md`); all provider cards show their true `verification_status`.
- [ ] **No commission rates or approval claims** published; partner links only for applied/approved programs.
- [ ] Regulated categories (insurance, visa/legal) link only to licensed providers and render the not-advice notice.

## C. Privacy notice
- [ ] A plain-language **privacy notice** is published: what is collected (anonymous events only), what is not (no names, documents, payment or income detail in analytics), cookie/storage use, data retention, contact for requests.
- [ ] Email addresses stored **only** in the consented mailing list (ESP), never in the analytics event stream.
- [ ] Payment handled by the checkout provider (Gumroad/Lemon Squeezy/Stripe) — no card data on your systems; link their terms.
- [ ] Data-residency and lawful-basis reviewed for the jurisdictions of users; someone qualified signs off.

## D. Cookie / analytics consent
- [ ] Audit tags: with **cookieless analytics (Cloudflare/Umami/PostHog cookieless)** you may not need a banner for analytics; confirm this and any **non-essential** tags (payment, affiliate tracking, embeds) that *do* require consent.
- [ ] If non-essential cookies/tags exist, deploy a consent CMP (CookieYes/iubenda/Klaro-type) with Google Consent Mode and prior blocking.
- [ ] One-click **unsubscribe** and list-management confirmed in the ESP; double opt-in where appropriate.
- [ ] Honour Do-Not-Track/opt-out; document the analytics data dictionary (`bangkok-measurement-spec.md`).

## E. Refund policy
- [ ] A **refund policy** is published on the presale/checkout page and product page: refund window, conditions, process.
- [ ] Presale page states it is a **pre-order**, the ship date, and the refund promise; policy configured in the payment tool (Gumroad/Lemon Squeezy settings).
- [ ] Confirm whether platform fees are returned on refunds; communicate honestly.

## F. Source dates
- [ ] Every factual/regulated figure shows a **last-checked date** and links its source.
- [ ] A visible data-date line on data-driven pages ("Data checked {date}…").
- [ ] `review_frequency` set for each record (rents/commute **quarterly**; rail/transit **semi-annually**; legal/visa/tax/insurance **on-change + quarterly**).
- [ ] Regulated items link **primary official sources** (thaievisa.go.th, immigration.go.th, rd.go.th, oic.or.th, bot.or.th, nbtc.go.th) per `thailandmate-source-register.csv`.

## G. Report-a-change process
- [ ] A **"Report a change / spot something wrong"** link/control on every data-driven page (form or mailto).
- [ ] Submissions route to the review queue; a named owner triages and updates records/claims.
- [ ] Stale-data process: overdue `next_review` items flag automatically and high-risk pages block publish when their official link is expired (per global architecture §6).

## H. Legal / tax / visa / insurance disclaimers
- [ ] Sitewide **general-information, not-advice** disclaimer for visa, tax, banking, healthcare/insurance and property.
- [ ] Each regulated topic renders its specific notice (the five "Verify this information" notices from `bangkok-mvp-content-pack.md`) with the official link and date.
- [ ] **No personalised advice**; the product frames decisions as "points to the official source / a licensed professional."
- [ ] Nothing stated as a legal rule (e.g. lease/deposit is labelled common market practice).
- [ ] Disclaimers reviewed by a qualified professional before launch.

## I. Launch-readiness gate (final sign-off)
- [ ] All Section A content gates pass (no `do_not_publish` live; scores null or editorially verified).
- [ ] Sections B–H documents published and linked in the footer (privacy, disclosure, refund, terms, methodology).
- [ ] Analytics events verified to carry **no PII**; consent/unsubscribe tested end-to-end.
- [ ] One legal/qualified review completed; the 7-day validation (`bangkok-validation-plan.md`) results reviewed against thresholds before scaling spend.

---

## Facts / assumptions / recommendations
- **Facts:** the data states in `bangkok-neighbourhood-data.json`/`-sources.csv`, the partner statuses in `bangkok-partner-readiness.md`, and the stack free-tiers in `bangkok-launch-stack-comparison.md` (as at 28 Aug 2026).
- **Assumptions:** that the chosen analytics are genuinely cookieless in their configured mode and that free-tier limits remain as published — both to confirm.
- **Recommendations [REC]:** do not expose the matcher publicly until Sections A, C, D and H are complete; launch in "rail facts + labelled rent estimates only" mode with qualitative scores off; complete a qualified legal/privacy review before collecting emails or payments.
