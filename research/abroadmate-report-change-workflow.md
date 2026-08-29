# AbroadMate / ThailandMate — Report-a-Change Workflow

**Status:** Pre-launch operational specification (Part 1 of launch operations).
**Created:** 28 August 2026. **Applies to:** thailandmate.bangkok matcher and any future AbroadMate city pages.
**Owner:** Content lead (single named person at launch; currently an unassigned role — [ASSUMPTION] the founder fills this until delegated).

> **Scope note [FACT]:** This document defines a *content correction* intake. It is not a legal reporting channel, a visa/tax/medical helpline, or a customer-support ticketing system for paid products. It must never invite or accept sensitive personal documents.

---

## 1. Purpose and principles

- Visitors know the ground truth first (a rent sign, a closed cafe, a new BTS exit). A low-friction correction channel keeps the matcher honest between quarterly source refreshes.
- A report is **a lead for an editor**, never an automatic change. Nothing reported by a visitor edits the published data set until it is independently sourced and dated.
- **Minimal data by design.** We ask for the smallest amount of information needed to verify a correction.
- **No sensitive data, ever.** The form, its instructions, and its confirmation must make clear that visitors must NOT submit: passport numbers or scans, visa documents or application numbers, bank/account/card details, medical or insurance information, passwords or ID documents, or any third person's personal data. If such data arrives, it is deleted immediately under the sensitive-data deletion rule (Section 7).
- This workflow feeds the existing publication gate: every accepted correction re-opens the affected record and sets `publish_status` per `bangkok-neighbourhood-publish-audit.md`.

---

## 2. Report categories

| report_category | What it covers | Feeds which record |
|---|---|---|
| `rent_outdated` | A displayed rent range (studio/1BR/2BR) that looks wrong or stale; a building that has changed price band | `bangkok-neighbourhood-data.json` rent fields |
| `transport_fact` | Wrong station name, station count, line, interchange, opening status, or exit info | rail/transit facts in the data pack |
| `venue_closed_or_changed` | Closed or relocated coworking space, service provider, or listed venue; venue wrongly listed as open | venue records (currently `needs_verification`, default-off) |
| `broken_official_link` | A link to an official source (BTS/MRT, immigration, RD, OIC, BoT, NBTC) that 404s, redirects, or moved | `bangkok-neighbourhood-sources.csv` / `bangkok-price-sources.csv` |
| `wording_inaccurate` | Neighbourhood wording that is misleading, overstated, factually wrong, or reads as a guarantee/endorsement | content records / matcher copy |
| `disclaimer_missing_or_misleading` | A missing or unclear disclaimer on rent, commute, deposit, visa/tax/legal, insurance, or affiliate matters; affiliate disclosure not shown where required | go-live checklist sections B & H |

**Categories must not be added casually.** Any new category needs a defined target record and an owner.

---

## 3. Field specification

### 3.1 Field dictionary

| Field | Required / Optional / Allowed | Notes |
|---|---|---|
| `report_category` | **required** | One of the six categories above (single-select radio, not free text). |
| `affected_area_or_page` | **required** | Which neighbourhood page or which displayed item (e.g. "On Nut — 1BR rent"; free text, short). |
| `what_is_wrong` | **required** | Free text, capped ~800 characters. What the visitor saw and what they believe is correct. |
| `correction_detail` | optional | Suggested correction or source (e.g. "the Soi X coworking space closed in June 2026"). |
| `source_url_or_ref` | optional | A link, listing, or dated reference the visitor found helpful. Editors treat visitor-supplied URLs as **untrusted leads**, not sources. |
| `contact_email` | optional | Only if the visitor wants a reply. Labelled clearly: optional, used once to respond, not added to any mailing list. |
| `name` | optional, **not collected at v1** | We do not need it. Do not add a name field unless a specific need is documented. |
| Passport / visa / bank / medical / ID fields | **NEVER PRESENTED** | No such field may exist anywhere in the form. |

- **required_fields:** report_category, affected_area_or_page, what_is_wrong.
- **optional_fields:** correction_detail, source_url_or_ref, contact_email.
- **personal_data_allowed:** **yes — minimal only.** One optional contact email for reply purposes. No names at v1, no documents, no attachments at v1, no sensitive-data categories. An optional honeypot anti-spam field may be used (hidden, never labelled with personal data).
- **storage_location:** At v1, submissions land in a single mailbox on the chosen email provider (e.g. a dedicated `corrections@` address), and are logged by a human into a private row-per-report register (a simple spreadsheet/markdown table NOT committed to the public repo), recording: report id, date, category, area, claimed issue, triage decision, linked source record, status, date closed. [REC] Do not install a database or ticketing system until volume justifies it. If a form backend is used later, it must be a provider listed in `abroadmate-launch-stack-verification.csv` with a reviewed DPA, and the privacy notice beside the form must name it.
- **triage_owner:** Content lead. Deputy named before launch so reports are never unowned during absence.
- **response_target:**
  - Acknowledge (autoresponder if email-based, or on-screen confirmation for a form): immediate.
  - Human triage decision (accept for verification / need more info / reject as out of scope): **within 7 calendar days**.
  - Corrections affecting safety-adjacent facts (transport, official links, disclaimers) get priority and an interim action (e.g. take the item offline / revert to `needs_verification`) **within 2 working days** while verification proceeds.
  - If the visitor left an email and the correction is accepted, one confirmation reply when the fix is published (no marketing, no list signup).
- **source_record_updated:** The exact data file + field (e.g. `bangkok-neighbourhood-data.json → on_nut.rent_1br_band_thb`) plus the new/added source id in the sources CSV, with access date. A report is "accepted" only once an independent, dated source is attached.
- **review_status:** See Section 5.

### 3.2 Anti-spam and data hygiene

- Honeypot field + time-to-submit check; no CAPTCHA that sets third-party cookies without consent.
- Strip/ignore any content that looks like card numbers or ID numbers on receipt (defensive pattern-matching); if a visitor pastes sensitive data anyway, delete the message per Section 7 and do not quote it in the register.
- The public report channel must never accept file uploads at v1 (attachments are a sensitive-data vector).

---

## 4. Exact user-facing copy

All copy below is ready to publish. It is deliberately plain, sets no expectation of a personalised reply or payment, and carries no guarantee.

### 4.1 "Report a change" button

> **Spotted something out of date?**
> Button label: **"Report a change"**
> (Placed beside the methodology/source note on each data panel. Never styled as an emergency or official-government link.)

### 4.2 Report form — title and intro

> **Help us keep this accurate**
> We rely on people on the ground. If a rent figure, station detail, listed venue, link, or disclaimer looks wrong, tell us below.
>
> **Please do not include** passport details, visa documents, bank or card information, medical information, ID numbers, or anyone else's personal details. We can't act on those and we delete them on receipt. This form is for content corrections only — it isn't a visa, tax, legal, or medical advice channel, and we can't help with individual applications.

**Fields (in order):**

1. **What are you reporting?** *(required)* — radio buttons:
   - Outdated rent information
   - Incorrect station or transport information
   - A coworking space or service provider that has closed or moved
   - A broken or moved official link
   - Inaccurate neighbourhood wording
   - A missing or unclear disclaimer
2. **Where did you see it?** *(required)* — short text: neighbourhood / page / the specific figure or wording.
3. **What looks wrong?** *(required)* — text box, ~800 character limit. "What you saw, and what you believe is correct."
4. **A link or reference (optional)** — "If a listing, sign, or page helped, paste the URL. We check every suggestion ourselves; a link isn't required."
5. **Your email (optional)** — "Only if you'd like us to reply about this. We use it once to respond, and won't add you to any mailing list."

**Submit button:** **"Send correction"**

### 4.3 Confirmation message (on-screen after submit)

> **Thanks — we've got it.**
> Our editor reviews every report (usually within 7 days). We verify against independent sources before changing anything, so we may not use your suggestion exactly as sent, and we can't reply to every report. Nothing here is confirmed as an error until we've checked it.
>
> If you spot something that could mislead someone about transport, costs, visas, tax, or insurance, we prioritise it and may remove the item while we check.

### 4.4 Privacy notice beside the form (short, shown above the submit button)

> **Your privacy:** This form asks only for the correction and, optionally, an email if you want a reply. We don't ask for or accept passport, visa, bank, medical, or ID details — anything like that is deleted on receipt. Submissions are read by our editor and stored only to verify and fix the content. We never sell reports or add reporters to a mailing list. See our full [privacy notice] for how long we keep submissions and how to ask us to delete them.

### 4.5 Autoresponder (if an email channel is used)

> Subject: **We received your correction — ThailandMate**
>
> Thanks for helping us keep the Bangkok guide accurate. Our editor reviews reports within about 7 days and verifies everything against independent sources before changing the site, so a report isn't a confirmation of an error.
>
> If you included personal documents or financial details in this email, please disregard — we delete such messages and can't use them. This channel is for content corrections only; for visa, tax, legal, or insurance questions, please use the official sources linked on our disclaimer pages.

---

## 5. Internal review statuses

A report moves through these states. The wording in brackets is the internal label; the meaning is the contract.

| Status | Internal label | Meaning | SLA |
|---|---|---|---|
| 1 | `received` | Submission landed; auto-acknowledged; not yet read. | Read within 2 working days. |
| 2 | `triaging` | Editor has read it; checking it isn't spam/duplicate/sensitive data. | Decide within 7 calendar days of receipt. |
| 3a | `needs_info` | Plausible but too vague to locate/verify; if reporter left email, one clarifying reply sent. Reporter reply awaited up to 14 days, then auto-close. | 1 reply; close after 14 days' silence. |
| 3b | `rejected_out_of_scope` | Spam, duplicate, opinion/preference (not a fact), or a request for individual visa/tax/legal/medical advice. Logged with reason; no site change. Reporter not promised a reply. | At triage. |
| 4 | `verifying` | Accepted for investigation; editor seeking an independent dated source. Safety-adjacent items have the published cell reverted to `needs_verification` / hidden in the meantime. | Independent source within one refresh cycle (rents quarterly; transit/disclaimers expedited ≤ 2 working days interim action). |
| 5a | `accepted_fixed` | Independent source found; the data record and source CSV are updated with access date; `publish_status` re-set; change shipped; register closed. Reporter emailed once if they opted in. | Next content deploy. |
| 5b | `not_confirmed` | Checked; could not confirm with a reliable source. Item left unchanged (or left at its existing gate status); reporter given a short "we couldn't verify this yet" reply if they opted in. Logged; a repeat report from a different source re-opens at `verifying`. | At end of verification. |
| — | `sensitive_deleted` | Submission contained passport/visa/bank/medical/ID data or third-party personal data; message deleted immediately, logged as a deletion event (date + category, no content), no action taken on it. | Same day. |

---

## 6. Triage rules (decision aids)

1. **Duplicates:** merge with the existing open report; count corroboration but never treat multiple reports as a source.
2. **Visitor links are leads, not sources.** A correction needs the same sourcing standard as the original record: ≥2 dated sources for price where possible; official operator for transit; never a single anonymous claim.
3. **"Sounds wrong" about an estimate:** estimates are labelled as ranges with a date. A report of "I pay a different rent" is valid signal — it triggers a fresh basket check, not an immediate number change.
4. **Wording reports about disclaimers or affiliate disclosure:** treated as high priority; if a required disclaimer or affiliate disclosure is genuinely missing, that is a launch-gate blocker (see `abroadmate-launch-signoff-checklist.md`) and the affected page is corrected before further promotion.
5. **Requests for individual advice** (e.g. "what visa should I get?", "is this insurance OK?"): `rejected_out_of_scope` with a pointer to the official source page; never answered personally.

## 7. Retention and deletion (summary; full policy in production documents)

- Corrections register: retain while the affected record is live plus one refresh cycle, then anonymise (keep category + area + outcome, delete email/free text). [REC] max 24 months for raw submissions unless needed for an ongoing issue.
- Optional reporter emails: used once for the reply, then deleted from the mailbox; never imported to marketing lists.
- Sensitive data: deleted on sight, same day, logged as a `sensitive_deleted` event without copying the content.
- A reporter may email to ask what was stored about them and request deletion; handled under the user deletion/request process (see `abroadmate-production-documents.md`), target 30 days.

---

## 8. Tags

- **[FACT]:** The six report categories, the publish-gate integration, the minimal-data fields, and the no-sensitive-data rule are operational facts/decisions documented here. Provider-derived channel facts are cross-referenced to `abroadmate-launch-stack-verification.csv` with their own verification statuses.
- **[ASSUMPTION]:** Founder acts as content lead until a deputy is named; v1 uses a dedicated mailbox + human register rather than a ticketing system; no name field and no attachments at launch.
- **[REC]:** Use a dedicated corrections address; log reports in a private register outside the public repo; revert safety-adjacent cells to `needs_verification` within 2 working days while verifying; anonymise the register after each refresh cycle.
