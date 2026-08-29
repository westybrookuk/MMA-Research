# Brand Migration Checklist (replace AbroadMate / ThailandMate)

**Purpose:** Operational checklist to swap the old internal names for the new parent brand once a finalist clears professional trademark review.
**Prepared:** 2026-08-29. **Do not start until:** a name is chosen from `global-brand-shortlist-v2.md` and the search in `global-brand-search-brief.md` returns clear/caution.
**Note on scope:** website code changes (index.html/styles.css/app.js) are made by the implementation team in a separate task — this document defines *what* must change and *where*, in order. Reachable placeholders shown as `{BRAND}` (parent), `{CITY}{BRAND}` (edition), e.g. **LocalFooting** / **Bangkok Footing**.

---

## Phase 0 — Before touching anything (gates)
- [ ] Trademark clearance returned for the finalist (UK IPO, EUIPO/TMview, WIPO, Thailand DIP incl. Thai script, USPTO) — result documented.
- [ ] Final domains confirmed at a registrar (recheck `.com`, `.io`, `.app`, `.co`, `.co.uk`; recheck within 7 days of buying).
- [ ] Social handles confirmed manually **logged out** on YouTube, X, Instagram, TikTok, LinkedIn, Facebook; list the exact @ to reserve.
- [ ] Final name + tagline + Thai transliteration signed off; decision recorded.
- [ ] No accounts/registrations created before clearance (standing rule).

## Phase 1 — Assets to obtain (after clearance)
- [ ] Register primary domain + defensive TLDs (`.com` primary; `.io/.app/.co/.co.uk`).
- [ ] Reserve handles on platforms actually used + defensively on the big six (consistent spelling; `{brand}` or `{brand}app` if bare is taken; never squat with impersonation).
- [ ] Domain-based role emails updated/created (corrections@, privacy@, support@, partnerships@) on the new domain; set forwards from old addresses during transition.
- [ ] Logo/wordmark lockup (and a version for long edition names like "Mexico City Footing"); favicon, social avatars/headers.
- [ ] Trademark filing(s) instructed (UK base; consider EU/Madrid and **Thailand early — first-to-file**).

## Phase 2 — Code & site content (implementation task)
Replace every occurrence of the old names; keep the same content/legal safeguards:
- [ ] Site title, `<title>`, meta tags, logo alt text, footer copyright.
- [ ] Product names map:
  - "Bangkok First 7 Days" → "**{BRAND} First 7 Days: Bangkok**"
  - "Bangkok First 90 Days" → "**{BRAND} First 90 Days: Bangkok**" (pack)
  - "Budget planner" / "neighbourhood matcher" → "{BRAND} Budget Planner" / "{BRAND} Neighbourhood Matcher" (product terms stay descriptive).
- [ ] Edition references: "ThailandMate" → "{CITY}{BRAND}" pattern (e.g. "Bangkok Footing"); any future "…Mate" phrasing removed.
- [ ] Email signup copy, welcome/confirmation emails, unsubscribe page — new brand, same consent wording (consent_copy_version bumped and dated).
- [ ] Conversion page headlines/promise/objection FAQ — swap name; preserve the no-advice (visa/tax/legal/medical/insurance/property) disclaimers.
- [ ] Privacy notice, affiliate disclosure, terms, cookie/consent strings — update controller/brand name and any addresses; processor names unchanged (Kit/Brevo, Cloudflare/Umami, Gumroad/Lemon Squeezy).
- [ ] Report-a-change form and confirmations — brand + mailbox addresses.
- [ ] Analytics: property/site names updated in the analytics tool; the **event schema and enums in `abroadmate-integration-contract.json` stay the same** (events are brand-agnostic); only labels/`campaign` tags change. Do not introduce PII.

## Phase 3 — Research / internal docs (this repository)
- [ ] Add a short "brand change note" at the top of the research docs stating old name → new name and effective date, so historical records remain readable (do not erase provenance).
- [ ] Update forward-looking docs (page spec, funnel, integration contract metadata, onboarding, partner outreach, launch checklist) to `{BRAND}`; keep file names stable or add redirect/alias notes.
- [ ] Partner outreach templates (`abroadmate-partner-outreach-templates.md`) — sender name/brand and signature updated.
- [ ] Search brief / brand DD docs kept as historical record of *why* the name changed (useful for future clearances).

## Phase 4 — Provider & data setup
- [ ] Email provider: account/forms/automations renamed or recreated on the new domain; re-authenticate DKIM/SPF/DMARC for the new sending domain; double opt-in and unsubscribe re-tested (re-run TC-01…TC-06).
- [ ] Analytics: new site property, production-only beacon; re-run TC-07…TC-09 (allowed enums / PII block / reports not in analytics).
- [ ] Payment (future only): seller/store name set to the new brand; MoR checkout reflects it; refund/presale copy updated (still reviewer-approved).
- [ ] Any existing consent/suppression lists migrated lawfully (same consent basis); export/import via the provider; suppression preserved.

## Phase 5 — Launch & cutover
- [ ] 301/redirect strategy: old domains (if ever owned) → new; internal links checked.
- [ ] Search/social: profiles published with consistent bio/tagline; update any directory or community mentions *you control*; do not claim associations you don't have.
- [ ] Update page "last updated" dates; add a short visible note if users may have seen the old name.
- [ ] Post-cutover: re-run the launch sign-off (`abroadmate-launch-signoff-checklist.md`) A–C at minimum; confirm privacy/disclosure pages show the new name.
- [ ] Post-launch checks at 7/30 days: broken links, email deliverability, analytics receiving counts only, corrections/privacy mailboxes working.

## Rollback
- Keep the old build deployable; DNS/handle changes are reversible early. If a late conflict appears on the chosen name, revert to the next finalist (CityOrienteer/NewHereCo) using the same checklist rather than forcing a blocked name.

## Tags
- **[FACT]:** The domain/handle and register requirements reflect the prior diligence screens; Thailand's first-to-file rule and provider (DKIM/suppression) mechanics are established facts.
- **[ASSUMPTION]:** One root brand plus `{city}+{root}` editions; consent records can be migrated within the same provider on the same lawful basis (confirm with reviewer).
- **[REC]:** Complete trademark clearance before any purchase; bump `consent_copy_version` on rename; keep event schemas unchanged; reserve all six social handles consistently; file in Thailand early; preserve old docs as a provenance record rather than rewriting history.
