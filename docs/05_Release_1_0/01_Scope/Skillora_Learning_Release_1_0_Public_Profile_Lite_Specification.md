---
name: Skillora Learning — Release 1.0 Public Profile Lite (skill badges, shareable profile, embeds)
version: 1.0
date: 2026-10-22
status: CONFIRMED DIRECTION (founder, 2026-10-22). Authoritative for the public profile subset in Release 1.0. Full credential design stays in the Credentials spec; this document fixes what ships in 1.0.
purpose: Let learners showcase verified skills anywhere (LinkedIn, GitHub, résumé, portfolio, WhatsApp) through a LeetCode-style public profile link, shareable badge images and embeddable badges, at near-zero cost, as a free growth loop.
related: ../../02_Low_Level_Design/03_Credentials/Skillora_Learning_Credentials_Badges_Certificates_Specification.md (full credential design), Skillora_Learning_Release_1_0_Web_Scope_and_Founder_Curriculum.md, Skillora_Learning_Release_1_0_Billing_Lite_and_Trial_Specification.md, ../../01_High_Level_Design/03_Market_and_Compliance/Skillora_Learning_India_Market_and_Compliance_Specification.md (DPDP), ../02_Engineering_Kickoff/Skillora_Learning_Release_1_0_ADR_Register_and_Spike_Plan.md (ADR-041, S20)
---

# Release 1.0 — Public Profile Lite

## 1. DECISIONS (CONFIRMED 2026-10-22)

| # | Decision |
|---|---|
| 1 | Release 1.0 ships **Public Profile Lite**: an opt-in public profile page, per-badge and per-certificate verification pages, share images, and an embeddable badge. |
| 2 | **Free on every plan** (Free, Trial, paid). Badges and verification stay free forever; the profile is a growth channel, not a paid feature. |
| 3 | **Opt-in and privacy-first.** A profile is off until the learner turns it on, with a clear consent. Each section has its own switch. Search-engine indexing is a separate switch, off by default. |
| 4 | **Everything shown is verified.** Each badge and certificate links to a Skillora verification page; nothing on the profile can be typed in freely except the display name, short bio and links. |
| 5 | **No ranking, no leaderboards, no recruiter search, no data feed to Skillora Screen** in 1.0. The profile is the learner's own page. |
| 6 | **Out of 1.0 (1.1 or later):** Open Badges 3.0 / verifiable-credential export, LinkedIn "add to profile" polish, wallet passes, résumé integration, custom domains, follow and comment features. |

## 2. WHAT THE PROFILE SHOWS

| Section | Content | Default once the profile is on | Notes |
|---|---|---|---|
| Header | Display name (or alias), photo (optional), headline, target role, location (optional), links (GitHub, LinkedIn, portfolio) | Visible | No email, phone, date of birth or employer is ever shown |
| Badges | Skill, module and course badges, with date earned and a link to each verification page | Visible | Gallery with filters by track (A, B, C) and skill |
| Certificates | Course certificates with verification links and QR | Visible | Only on paid-plan completions (see the Billing Lite specification) |
| Skill map | Mastery per competency area as bars (Python, databases, APIs, cloud, DSA, system design, LLM/AI, interview craft) | Visible | Uses mastery bands, not raw test scores |
| Activity | Study streak, courses completed, modules completed, last active month | Visible | Streak counts meaningful activity only (finished lessons, mastery checks, mocks), never logins |
| Interview readiness | Readiness band only (Building, Almost ready, Ready) per track | **Hidden** | Learner opts in; no raw scores, no transcripts, no recordings |
| Company-pack practice | Which companies the learner practises for | **Hidden, no public option in 1.0** | Avoids revealing job-search targets |
| Projects | Learner-added project cards (title, link, short description) | Visible | Free text, subject to moderation |
| Share tools | Copy link, share to LinkedIn, X, WhatsApp, email; download image; copy embed | Visible | Section 4 |

## 3. LINKS, HANDLES AND VERIFICATION

- **Profile URL:** `/u/{handle}`. Handle: 3 to 30 characters, lowercase letters, digits and hyphens; unique; reserved and offensive words blocked; changeable once every 90 days, with the old handle redirecting for 30 days. A stable internal profile ID never changes.
- **Credential URL:** `/verify/{credential_id}` (already in the Credentials spec): issuer, credential, learner (as privacy allows), date, status (valid, expired, revoked) and criteria summary; QR on certificates resolves here; a machine-readable JSON endpoint exists for employers and applicant-tracking systems.
- **Integrity:** credential IDs are unguessable and signed; badge criteria are versioned; a badge cannot be earned in the free tier or trial without passing the same mastery check and hidden tests as a paid learner (certificates are issued only on paid plans).
- **Verified tick (later, optional):** only if the optional consented digital age check is switched on after spike S21 and counsel approval (Audience and Eligibility Policy §4A), a learner who passes can show "Age verified"; only the yes or no result is stored, never any document. Not in 1.0; certificates do not require it.
- **Revocation:** an admin can revoke a credential issued in error or through cheating; the verification page shows "revoked" within 5 minutes and the profile hides it.

## 4. SHARING AND EMBEDS

| Feature | Behaviour |
|---|---|
| Social preview image | Auto-generated 1200×630 image for the profile and for each badge or certificate, with the Skillora brand, the learner's alias, the credential and a verification line; generated on first request and cached |
| Share buttons | LinkedIn post link, X, WhatsApp, email, copy link; messages are suggested, editable, and carry no outcome claims |
| Embed | `<img>` badge (SVG) for GitHub READMEs, portfolios and email signatures: `/badge/{credential_id}.svg` for one credential, `/u/{handle}/summary.svg` for a profile summary card; read-only, cached at the edge, no personal data beyond what the profile already shows publicly |
| Downloads | Badge as PNG or SVG; certificate as PDF |
| Attribution | Each shared page and image links back to Skillora and to the verification page |

## 5. PRIVACY, CONSENT AND SAFETY

- **Consent:** a separate, versioned consent for "public profile" (DPDP Act); withdrawing it hides the profile within 5 minutes (cache purge) and removes it from search indexes on the next crawl; verification pages remain but show only issuer, credential and status when the learner's name is hidden.
- **Adults only:** the platform registers only learners aged 18 or older (Audience and Eligibility Policy), so no special minors' mode or guardian consent is needed for profiles.
- **Data minimisation:** display name may be an alias; no email, phone, age or employer shown; location optional.
- **Search engines:** profiles are `noindex` by default; the learner can allow indexing; sitemaps include only profiles that opt in.
- **Abuse and impersonation:** report button on every profile; admin takedown within 48 hours; handle and bio profanity filter; rate limits on handle changes and profile creation; verification that the profile belongs to the account holder (email-verified); impersonation of a real person is a takedown reason.
- **Scraping and misuse:** rate limiting and bot protection at the edge; the JSON endpoint is read-only and rate-limited; no bulk listing of profiles.
- **Skillora Screen separation:** no recruiter-facing search, no API, no data export from Learning to Screen; the learner may share their own link with anyone.
- **Content rules:** projects and bio must not contain confidential employer material or misleading claims; no guarantees of jobs or outcomes.

## 6. PLAN, QUOTA AND LIMIT RULES

| Item | Rule |
|---|---|
| Availability | Free, Trial and all paid plans |
| Profiles per account | 1 |
| Share-image generation | 30 a day per account; cached results are unlimited |
| Embeds | Cached at the edge; abusive hot-linking is rate-limited |
| Projects on a profile | Up to 10 |
| Bio | 280 characters |
| Links | Up to 5, with scheme and domain checks |
| Trial and free-tier badges | Badges can be earned and shown; certificates only on paid plans |

## 7. REQUIREMENTS AND ACCEPTANCE CRITERIA

| ID | Requirement | Acceptance criteria |
|---|---|---|
| PP-01 | Opt-in profile creation with handle and consent | Profile stays private until the consent is given; consent is versioned and stored |
| PP-02 | Section-level visibility switches | Each section's visibility is respected on the page, the image, the embed and the JSON endpoint |
| PP-03 | Public profile page, server-rendered and cached | Loads in under 2.5 s on 4G (largest contentful paint); cache purged within 5 minutes of any change |
| PP-04 | Verification link for every badge and certificate | Every item resolves; revoked and expired states shown correctly |
| PP-05 | Social preview image generation | Valid 1200×630 image, brand-safe, cached; no personal data beyond the visible profile |
| PP-06 | Embeddable SVG badge and summary card | Renders in GitHub README and common email clients; respects privacy switches |
| PP-07 | Search-engine indexing switch | `noindex` by default; sitemap lists only opted-in profiles |
| PP-08 | Report, takedown and impersonation handling | Report creates an admin task; takedown within 48 hours; actions audited |
| PP-09 | Handle rules, reservation and redirects | Reserved and offensive handles refused; rename limited to once per 90 days; redirects work for 30 days |
| PP-10 | Adult-only eligibility inherited | Profiles exist only for accounts that passed the 18+ and education gate; an account suspended or deleted under the underage response removes its profile and credentials from public view within 5 minutes |
| PP-11 | Admin moderation and analytics | Admin can disable a profile, revoke a credential, view reports and see profile views and share counts |
| PP-12 | Accessibility and responsive design | WCAG 2.2 AA target; badges use shape, icon and label, never colour alone; alt text for images |

## 8. ADMIN PORTAL AND LEARNER PORTAL ADDITIONS

**Admin:** profile moderation queue (reports, takedown, disable, restore); reserved-handle list; profanity list; credential revocation; badge and share-image templates; profile analytics (enabled rate, views, shares, referral sign-ups).

**Learner:** My Profile page with preview, section switches, indexing switch, handle editor, project cards, share panel, embed codes; credential gallery with "add to profile" toggles; consent and privacy settings.

## 9. DATA MODEL ADDITIONS

`PublicProfile(handle, display_name, headline, bio, links, visibility_flags, indexing_allowed, status)`, `ProfileProject`, `ProfileConsent(version, time)`, `HandleHistory`, `ProfileReport`, `ShareImage(cache_key)`, `ProfileViewStat(aggregated)`. All rows carry `tenant_id`; analytics events carry identifiers only: `profile_enabled`, `profile_viewed`, `profile_shared`, `badge_embedded`, `profile_report_created`, `profile_referral_signup`.

## 10. COST AND DELIVERY

| Item | Estimate |
|---|---|
| Moderation | Founder time in 1.0; a simple report queue |

| Milestone | Work |
|---|---|
| M4 | Profile data model, consent, handle rules, privacy switches (with the credential engine, ADR-031) |
| M5 | Profile page, verification links, share images, embeds, indexing switch, moderation tools; S20 results |
| M6 Closed beta | Profile on for beta learners; share to LinkedIn and GitHub tested; moderation drill |
| M7 Public launch | Profile open to all; referral tracking from profile links; marketing uses learner-shared links |

## 11. METRICS

| Metric | Target after public launch |
|---|---|
| Learners who enable a profile (of those with at least one badge) | At least 30% |
| Profile or badge shared (of those with a profile) | At least 40% |
| New registrations coming from profile and badge links | At least 5% |
| Abuse reports per 1,000 profiles | Under 10; takedown within 48 hours |
| Profile page LCP | Under 2.5 s on 4G |

Public profiles are a free growth loop; the Revenue Model scenarios assume no acquisition benefit from them, so any referral sign-ups are upside.

## 12. RISKS

| # | Risk | Mitigation |
|---|---|---|
| R23 | Credential gaming or fake claims damage trust | Badges only from mastery checks and hidden tests; versioned criteria; revocation; nothing free-typed except bio, links and projects |
| R24 | Privacy or DPDP breach (profile data exposed unintentionally) | Opt-in consent, per-section switches, noindex default, minimisation, cache purge, privacy review before beta |
| R25 | Abuse, impersonation, offensive handles or bios | Filters, reports, 48-hour takedown, rate limits, admin tools |
| R26 | Profile pages become a scraping target | Edge rate limits, no bulk listing, read-only JSON with limits |
| R27 | Scope creep (leaderboards, recruiter search, comments) | Explicit out-of-scope list; any recruiter-facing feature belongs to Skillora Screen under its own rules |

## 13. DEFINITION OF DONE

- [ ] ADR-041 accepted and spike S20 complete.
- [ ] PP-01 to PP-12 pass, including a privacy test that every switch is respected on page, image, embed and JSON.
- [ ] Consent text and privacy notice reviewed by counsel; moderation and takedown drill completed.
- [ ] Share images and embeds tested on LinkedIn, X, WhatsApp, GitHub and two email clients.
- [ ] Profile pages meet the web performance budget and accessibility target.
- [ ] Metrics instrumented, and the Revenue Model growth assumptions left unchanged until real data exists.

## 14. DECISIONS TAKEN ON OPEN POINTS

| Topic | Decision | Why |
|---|---|---|
| Indexing default | Off (`noindex`); learner can allow it | Privacy first; learners who want SEO can opt in |
| Interview scores and company packs on the profile | Readiness band only, opt-in; company packs never public | Avoids exposing job-search targets and gaming incentives |
| Leaderboards and ranking | None in 1.0 | Avoids comparison pressure and cheating incentives; revisit with data |
| Certificates on free tier and trial | Not issued; badges are | Keeps a reason to buy without hiding earned skill badges |
| Open Badges 3.0 | 1.1 | Needs signing keys and standards testing; not required for sharing links |
| Moderation resourcing | Founder plus a simple queue in 1.0 | Low volume; automate if reports exceed 10 per 1,000 profiles |
