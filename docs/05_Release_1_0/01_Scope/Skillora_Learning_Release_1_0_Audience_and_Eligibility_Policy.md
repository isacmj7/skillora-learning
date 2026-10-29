---
name: Skillora Learning — Audience and Eligibility Policy (18+ only, Higher Secondary minimum)
version: 1.0
date: 2026-10-22
status: CONFIRMED DIRECTION (founder, 2026-10-22). Authoritative for who may register and what the product assumes about its users. Counsel to confirm the wording and the reasonable-steps standard before the paid beta.
purpose: Keep Skillora Learning an adult, tech-focused platform so that no guardian-consent flows, child-data restrictions or minor-specific features are needed, while keeping the sign-up gate simple, honest and cheap to run.
related: Skillora_Learning_Release_1_0_Web_Scope_and_Founder_Curriculum.md, Skillora_Learning_Release_1_0_Billing_Lite_and_Trial_Specification.md, Skillora_Learning_Release_1_0_Public_Profile_Lite_Specification.md, ../../01_High_Level_Design/03_Market_and_Compliance/Skillora_Learning_India_Market_and_Compliance_Specification.md (§7.1), ../02_Engineering_Kickoff/Skillora_Learning_Release_1_0_ADR_Register_and_Spike_Plan.md (ADR-042)
---

# Audience and Eligibility Policy

## 1. DECISIONS (CONFIRMED 2026-10-22)

| # | Decision |
|---|---|
| 1 | **Skillora Learning is for adults only: minimum age 18.** No accounts for anyone under 18, and no guardian-managed profiles. |
| 2 | **Minimum education: completed Higher Secondary (12 years of regular schooling) or an equivalent, which includes a Diploma (after 10th) pursuing or completed**, to register. **Diploma holders are explicitly welcome (confirmed 2026-10-22), so they can take the tech path on Skillora; the 18+ rule applies to everyone without exception.** |
| 3 | **Primary audience:** tech and CS-related students (BTech, BE, BCA, MCA, BSc CS, diploma and similar; final-year students most commonly) and tech professionals (junior, mid, senior). **Also welcome:** any adult who wants to upskill for a tech role, provided they meet decisions 1 and 2. |
| 4 | **Features removed from the plan because of this:** guardian-consent and parental dashboards, minor-specific defaults and restrictions, school-student content and marketing, safe-for-minors moderation defaults. |
| 5 | **No marketing aimed at under-18s**, no school or coaching-institute partnerships for school students, and campus programmes only for students who are 18 or older. |

## 2. WHO IS IN AND OUT

| Group | Eligible | Notes |
|---|---|---|
| Final-year and pre-final-year BTech, BE, BCA, MCA, MSc, BSc CS, IT and related students aged 18+ | Yes | Core campus-placement audience |
| Diploma (after 10th) pursuing or completed, aged 18+ | **Yes (confirmed)** | Counts as an equivalent to Higher Secondary; the 18+ rule still applies |
| Junior, mid and senior tech professionals | Yes | Core paying audience |
| Career switchers and any adult upskilling for tech | Yes | Need at least Higher Secondary completed |
| 17-year-olds, including first-year students who have not turned 18 | **No** | Not eligible until the 18th birthday |
| Anyone under 18, whatever their education (12th completed, diploma pursuing) | **No** | 18+ is mandatory for everyone; education alone never qualifies |
| School students (class 9 to 12 pursuing) | **No** | Fails both rules |
| Adults without Higher Secondary completion | **No** | Fails the education rule |

## 3. WHAT THE SIGN-UP GATE DOES

**At registration (before any account or trial is created):**

1. Ask for **date of birth**. If the learner is under 18, show a clear, polite message ("Skillora is for learners aged 18 and above. You are welcome to come back after your 18th birthday.") and **do not create an account or keep the date of birth or email** from that attempt.
2. Ask for **highest education** from a fixed list: Higher Secondary (12th) or equivalent completed; Diploma pursuing or completed; Bachelor's pursuing or completed; Master's pursuing or completed; Doctorate; Other equivalent. "12th appearing or pursuing" and "10th or below" are not accepted.
3. Ask for **current status** (final-year student, other student, working professional, between jobs, career switcher) and target role. These drive recommendations only.
4. Show the **eligibility statement** with a required checkbox: "I confirm that I am 18 or older and have completed Higher Secondary education or equivalent, and that the information I gave is true."
5. Record consent and terms acceptance with a version and timestamp.

**Data kept (minimisation):** the age check uses the full date of birth once; afterwards store only **birth year**, an "18+ confirmed on {date}" flag, the education level, and the signed eligibility statement. No guardian fields exist.

**Limits we accept:** age and education are self-declared and cannot be fully verified at registration. The gate is a documented, reasonable step, backed by the detection and response steps below. Counsel to confirm that this meets the standard expected under the DPDP Act for an adult-only service.

## 4. DETECTING AND HANDLING UNDERAGE ACCOUNTS

| Signal | Response |
|---|---|
| Support message, profile text or project that indicates the learner is under 18 or a school student | Suspend the account, ask for age confirmation by a lawful document check, and delete if the learner is under 18 |
| Implausible data (for example a date of birth that conflicts with a stated graduation year) | Ask for re-confirmation at next sign-in |
| Report from a parent, guardian or school | Suspend within 24 hours, verify, delete personal data within 7 days if under 18, and confirm in writing |
| Discovery of an under-18 account by any means | Stop processing, do not market to them, delete the account and personal data within 7 days, refund any unused paid period in full, and log the incident |

An internal "underage response" runbook, owned by the founder, covers these steps; deletion follows the data-rights process already in the specification.

## 4A. AGE ASSURANCE: NO ID COLLECTION IN 1.0 (REVISED 2026-10-22, MITIGATES R28)

**Final decision: Release 1.0 collects no ID documents, selfies or video, and stores no private documents. Age is protected by declaration, free automatic checks, fast removal, no minor-targeted marketing, and counsel-reviewed wording. An optional consented digital age check is added later only if it is free of setup cost and confirmed lawful (spike S21).** Education (Higher Secondary or diploma) stays self-declared, because age is the legal risk.

(An earlier draft of this section used a manual photo-ID and selfie review with deletion after 24 hours. It was dropped on 2026-10-22 because collecting IDs creates sensitive-data duties and breach liability that the lean launch should not carry.)

| Tier | Who | What | Data stored | Cost |
|---|---|---|---|---|

### What protects us (the documented reasonable steps)

1. A clear 18+ and education rule in the terms, privacy notice, sign-up page and checkout.
2. A required, signed eligibility statement with a version and time stamp.
3. A polite refusal for under-18 attempts, with nothing stored.
4. Automatic plausibility checks that route doubtful accounts to a manual look at the account's own data (no documents).
5. A fast response: suspend within 24 hours of any report, delete personal data within 7 days, refund any unused period in full, and keep an incident log (section 4).
6. No marketing aimed at under-18s and no school channels (section 1).
7. Counsel reviews the exact wording and confirms the steps meet the reasonable-steps standard before the paid beta.

### What happens to flagged accounts in 1.0

A flagged account is reviewed only from the data it already holds (bio, project links, support messages, sign-up details). The learner is asked by email to reconfirm their date of birth and the statement. If the answers do not hold up, or the learner does not reply within 7 days, the account is suspended, personal data is deleted within 7 days, and any unused period is refunded. No ID, selfie or video is requested.

### Optional consented digital age check (later)

- **Not built in 1.0.** Spike S21 (2 to 3 days, before the paid beta or later) checks whether a licensed provider can serve a private company, what it returns (age flag only), its price per check, DPDP processor terms and data residency.
- **If switched on:** it is voluntary, used for flagged accounts and the optional "Age verified" tick, never required for certificates, and results (not documents) are the only thing stored.
- **If not available:** Tier 2 stays off and Tiers 0 and 1 remain the controls.

### What is deliberately not done

| Item | Reason |
|---|---|
| ID upload, selfie or video checks | Collects sensitive documents and creates breach and privacy liability that the lean launch should not carry |
| Reviewer console and image deletion jobs | Only needed for ID checks; removes about a week of build work |
| Certificates gated on identity verification | Adds friction and cost; certificates carry the name as entered by the learner |
| Full Aadhaar number collection | Not permitted as a casual identity check; never collected |
| KYC for all learners | Hurts sign-up and trial conversion and costs time or fees for clearly adult learners |

### Trade-off accepted

Age remains self-declared for most learners, so a determined minor could register. This is the standard approach for adult-only online services, and the steps above are the documented defence. It does not remove all legal exposure; counsel confirms the standard. Collecting IDs would add its own exposure.

### Cost and budget

| Item | Estimate |
|---|---|
| Engineering | A few days for the rules engine, the reconfirmation email and the admin flag queue, inside the agent budget; about a week less than the ID-check design |

## 5. HOW THE REST OF THE PRODUCT CHANGES

| Area | Change |
|---|---|
| DPDP design requirements | Verifiable parental consent, child-specific bans and minor-restricted features are removed; adult consent, rights, retention, security and processor rules remain |
| Accounts and consent | One sign-up path; no guardian email, no guardian consent records, no age-band restricted features |
| Public profile | No special minors mode; profile consent applies to all learners as adults |
| Credentials | Certificates and badges can be shared by all learners; no minors' private-by-default rule |
| Admin Portal | Consent management drops guardian status; adds eligibility statement records and the underage response queue |
| Marketing | No targeting of under-18s; no school channels; creator and campus partnerships only for 18+ audiences; claims rules unchanged |
| Campus channel | College programmes accept only students who are 18 or older; first-year students under 18 wait until they turn 18 |
| Payments | Purchasers are adults; the eligibility statement is repeated at checkout |
| Content | Level model (Beginner to Senior) stays for adults; no school-level content; tone and examples assume adult learners |
| Moderation | Standard adult-platform moderation; no safe-for-minors defaults |

## 6. AUDIENCE FIT FOR RELEASE 1.0 CONTENT

Release 1.0 is purpose-built for **Senior Python backend and AI engineer interview readiness** (Tracks A, B and C). The wider audience in this policy changes the plan as follows:

- **Final-year students and juniors** can use 1.0 through the placement test and the Simpler paths (DSA in A3, coding execution in C3, behavioural in C1, IT-services and product company packs). Campus-specific content (aptitude tests, fresher HR rounds, fresher resume tips, entry-level role tracks) is **not** in 1.0 and is recommended for 1.1 once beta data shows demand.
- **Messaging and landing pages** should state the real fit: strongest for backend and AI engineering roles, with placement-ready fundamentals through the Simpler path.
- **Pricing:** the 25% student code (manual, admin-issued after checking a college email or ID) stays. If students exceed about 30% of paying users in the paid beta, automate college-email verification at 1.0.1 and test a student price.
- **Revenue Model:** student and early-career segments now count as core, with lower willingness to pay than professionals; the scenarios are unchanged until beta data exists.

## 7. REQUIREMENTS AND ACCEPTANCE CRITERIA

| ID | Requirement | Acceptance criteria |
|---|---|---|
| EL-01 | Date-of-birth check at registration | Under-18 attempts see the polite message; no account, no stored DOB or email; the check cannot be bypassed by editing the form after submit |
| EL-02 | Education level selection with the fixed list | Ineligible options block registration with a clear message; the selected level is stored |
| EL-03 | Signed eligibility statement | Required checkbox; stored with terms version and time; repeated at checkout |
| EL-04 | Data minimisation after the check | Only birth year, the 18+ flag and education level are stored; full DOB is discarded |
| EL-05 | Underage response workflow | Admin queue; suspension within 24 hours of a report; deletion within 7 days; full refund of unused period; incident log |
| EL-06 | Terms and privacy notice | State the 18+ and education requirements plainly; no content addressed to children |
| EL-07 | Marketing and partnership rules | No under-18 targeting in any channel; campus offers state the 18+ rule |
| EL-08 | Eligibility changes audited | Any admin change to a learner's age or education status is logged |
| EL-09 | Counsel review | Wording of the gate, statement, terms, the verification consent and the response runbook reviewed before the paid beta |
| EL-10 | Risk signals and plausibility checks (Tier 1) | Rules flag date-of-birth and graduation-year conflicts, disposable emails, repeat sign-ups and minor-indicating text; flagged accounts enter an admin review queue that uses only data the account already holds |
| EL-11 | Reconfirmation flow for flagged accounts | Email asks the learner to reconfirm date of birth and the statement; no reply within 7 days or a failed check leads to suspension; no documents are requested |
| EL-12 | Admin flag queue | Admin sees the risk signals and account data, records the outcome, and every action is audited; no ID images exist anywhere |
| EL-13 | Failed or underage outcome | Suspension, deletion within 7 days, full refund of any unused period, incident log, one appeal by reconfirmation |
| EL-14 | Optional digital age check (Tier 2, later) | Built only after spike S21 and counsel approval; voluntary; stores only the yes or no result and a provider reference; never gates certificates |
| EL-15 | Verified tick (later, with EL-14) | Shown on the profile only after a passed Tier 2 check; otherwise absent |
| EL-16 | No-ID guarantee in 1.0 | A test confirms the product has no upload field, storage or code path for ID documents, selfies or video

## 8. COST, DELIVERY AND RISK

| Item | Effect |
|---|---|
| Engineering | Removes the guardian-consent flow and minors' restrictions (about 1 to 2 weeks of work avoided); adds the sign-up gate and the underage queue (a few days, inside the existing agent budget) |
| Legal | Counsel confirms the adult-only wording and the reasonable-steps standard inside the existing legal package; no extra budget line |
| Milestones | Sign-up gate and statement at M1 (with auth and consent); plausibility rules, reconfirmation flow, admin flag queue and runbook at M4; counsel review before M6; S21 optional |

| # | Risk | Mitigation |
|---|---|---|
| R28 | A minor registers by lying about age | Signed statement, polite refusal that stores nothing, automatic plausibility checks, reconfirmation for flagged accounts, 24-hour suspension, 7-day deletion and refund, no minor-targeted marketing, counsel-confirmed reasonable steps; accepted residual risk because no IDs are collected |
| R31 | Optional digital age check proves unavailable or too costly | Tier 2 stays off; Tiers 0 and 1 remain the controls; revisit when volume or counsel advice changes |
| R29 | Losing 17-year-old first-year and school-leaver demand | Accepted; welcome them back at 18; no data kept; revisit only if the founder changes policy |
| R30 | Final-year students find the senior-focused content too hard | Placement test, Simpler paths, a clear landing-page promise, and campus content in 1.1 if demand shows |

## 9. DEFINITION OF DONE

- [ ] EL-01 to EL-13 and EL-16 pass (EL-14 and EL-15 only if the optional digital check is built), including a test that under-18 attempts leave no stored personal data.
- [ ] Terms, privacy notice and the eligibility statement reviewed by counsel.
- [ ] Underage response runbook written and rehearsed once.
- [ ] All specifications and prompts updated to remove guardian and minor features (done 2026-10-22; see the change log in section 10).
- [ ] Marketing checklist states the 18+ rule; campus pages state it.

## 10. CHANGE LOG (DOCUMENTS UPDATED ON 2026-10-22)

Release 1.0 scope, Billing Lite and Trial specification, Public Profile Lite specification, ADR register (ADR-042), milestones and risks, India Market and Compliance Specification (§7.1 and checklist), Credentials specification, Admin Portal specification, Engineering Quality and Operations specification, Resume Studio specification, Enterprise Upskilling specification, Production Readiness Review, MVP roadmap, web and mobile build prompts, Revenue Model, technical budget and stakeholder summary.
