---
name: Track Lead C Report
scope: C1, C2, C3, C4 (Track C, Interview Craft)
reviewer: TRACK-LEAD-C
date: 2026-10-05
status: Review complete, pending boss sign-off
---

# Track Lead C Report (2026-10-05)

Inputs read: boss matrix (including section 10), syllabus template, tag registry, consistency report, VER-5 log, boss review, all six integration documents (targeted to every Track C reference), and all four Track C syllabi in full. Only C1-C4 were edited.

## 1. Verdicts

| Course | Verdict | Reason |
|---|---|---|
| C1 Behavioural and Leadership | **Approve** | Strongest rubric in the set (L1-L4 anchors, cap rule, scale mapping). Hours, weights and tags verified. Fixed founder-specific content and two seed gaps. |
| C2 Project Deep-Dive and Resume Defence | **Approve with conditions** | Method is sound and honest. Conditions: claim-risk calibration (30 labelled transcripts, human labellers named), canvas free-form mode date (P-19), and Profile Facts Ledger onboarding. Archetypes ARC-3 and ARC-4 remain close to real project types; boss to confirm they count as generic. |
| C3 Coding Interview Execution | **Approve with conditions** | Process-evidence design is right for the senior audience. Conditions: spike S-5 (hidden-test categories), A5.M10 supplying the six `lld` seed IDs, per-category thresholds tuned after the pilot. |
| C4 Offer and Career Positioning | **Approve with conditions (M1-M4 publishable; M5-M9 blocked)** | Ground rules are exemplary and no figures are asserted as fact. M5-M9 may not publish until a compensation/tax professional and employment-law counsel sign off (R-C4-3). Hours-floor waiver needs boss ratification. |

## 2. Resolutions requested in the brief

**C4 module numbering inconsistency.** The C4 file itself is internally consistent (9 modules; gated M5-M9; timing-flag unlock M6-M8; C1 pointer target M6-M9). The inconsistency is in boss text that was left read-only: O-104, O-107, O-108 and boss section 8 (C4 item 1) still use the old 10-module numbers, and boss section 10.2 uses three different ranges (M5-M9 for O-108, M6-M9 for C1 pointers, M6-M8 for unlock) without saying why. Resolution: the ranges are correct and distinct (M5 levels is gated but is not a C1 pointer target; M6-M8 are the timing-flag modules). A "Reading older boss text" paragraph was added to C4 section 6 with the exact old-to-new map. The matrix itself needs the corrections in section 4 below.

**C4 M3-M5 below the 0.75 h floor.** Arithmetic: 9 modules x 0.75 h = 6.75 h, which is 12.5 percent over the 6 h boss total, outside the 10 percent tolerance (6.6 h). Raising to 6.6 h would still leave M3-M5 at 0.7 h, so it fixes nothing and would ripple through Learner Journey (C4 4.6 h, route totals), Content Data Model (section 9 table) and the 294.5 h total. Decision: keep 9.0 S / 6.0 D, formalise the exception as **waiver W-C4-1** in the C4 header and open item 1, and ask the boss to ratify. The fallback, if the boss declines, is 6.75 h (total moves to 295.25 h). The importer already tolerates it (V-005 warning with waiver flag).

## 3. Changes made

### C1
- Removed founder-specific material from shared content (R-C2-5): real project names (Aero Lounge, SPIN, NextPro, Routes-Pro) deleted; section 5 reframed as an illustrative archetype persona; "aviation, IoT, sports technology" and "PHP and Laravel" generalised in M9, M9-Q2, LF header, LF-3 and LF-1.
- Added seeds M7-Q4 (gaps and job-hopping, `hr.gaps`) and M7-Q5 (relocation and work mode, `hr.relocation`): two owned tags had no seed. Seed count 30 -> 32.
- Aligned placement skip with the Placement Blueprint: spoken probe adjusts M1, M2, M8 only (was "skip M1, shorten M3"); M3 is never shortened.
- Removed the unsourced "about 18 US states" count from M7; kept the tendency with a not-legal-advice caveat.
- Seed IDs stated as `Q-C1-M<n>-<nn>` and difficulty-word to 1-5 mapping stated (data model requires integers).

### C2
- LF-5 dependency line contradicted the header (said C1 "recommended in parallel"); corrected to C1.M1-M3 required.
- Q7.1 was tagged only `behav.ownership` (C1-owned, no credit); retagged to `project.narrative` with the secondary noted. Outcome 6 fixed the same way.
- MQTT remediation pointer pointed to A5.M9 (protocols overview); owner lesson is A6.M9, now cited first. M8 remediation A5.M1 corrected to A5.M10.
- Difficulty scale and goal-tag location stated.

### C3
- Seed goal tags contradicted their modules: Q2.3 (B:S in an M module), Q6.1 and Q6.3 (S in M6, which the boss ruled M); aligned.
- Softened "typically detected" for AI-assistant misuse (unsourced claim) and removed an assumption about the reference learner's interview history.
- M3 remediation now includes A1.M5 (closures). Seed ID and difficulty mapping stated.

### C4
- Waiver W-C4-1 and numbering reading guide (section 2 above).
- Added seed Q-C4-M7-04 (employer pays a fixed uplift on current CTC): India-specific, senior, truthfulness-centred, routes the legality question to counsel (R6).
- M3 gets a 60-second recruiter pitch (C4-owned per R-C1-1) and M2 a single-take read-aloud (length and clarity only) so the Interview Master drills DR-C4-M3 and DR-C4-M2 have matching content without breaching "no spoken narrative drills".
- Statutory numbers, section numbers and form numbers (V1-V4) are now explicitly reviewer reference only; gated lessons may not print them as settled before sign-off. Form 122 flagged as resting on one class of secondary source.
- Removed two unsupported generalisations ("services-firm AI Engineer JDs often mean integration"; HR rounds "in nearly every loop").
- Difficulty scale stated.

## 4. Discrepancies in integration and boss documents (to fix there, not in C1-C4)

| File | Location | Correction needed |
|---|---|---|
| `_Boss_Decisions_and_Ownership_Matrix.md` | O-104 (line ~455) | "C4.M8-M10" -> "C4.M7-M9" |
| same | O-107 (~458) | "C4.M1-M5" -> "C4.M1-M4" |
| same | O-108 (~459) | "C4.M6-M10" -> "C4.M5-M9" |
| same | Section 8, C4 item 1 | "M4-M5 `S`, M7-M9 `M`" -> "M3-M4 `S`, M5 `S`, M6-M8 `M`, M9 `S`" |
| same | Section 10.1 last paragraph / 10.2 | Add that C4 Deeper floor exception is waiver W-C4-1 pending ratification; note M5 levels is gated but not a C1 pointer target (explains M5-M9 vs M6-M9) |
| `Placement_Test_Blueprint.md` | Pool rule 2 (line ~47) | Eligible types are MCQ, multi-select, code-output, quantitative. Track C banks use "identify-the-flaw", "classify-the-level", "spot-the-weakness" and "predict-the-outcome" MCQ-form items in the predict bucket (G-10 relabel). State these are eligible, otherwise C1, C2 and C4 fall below the 10-item parallel-form minimum. |
| `Content_Data_Model_and_Import_Schema.md` | Section 4 question schema (difficulty integer 1-5) and ID rules (line ~71) | C1, C3, C4 seeds use easy/medium/hard words and C1 `Mn-Qk` labels; add an import mapping (easy 1, easy-medium 2, medium 3, medium-hard 4, hard 5; `M1-Q1` -> `Q-C1-M1-01`). |
| same | V-005 (line ~1157) | Reference waiver ID W-C4-1 so the warning is traceable. |
| same | Section 9 table (C1 seeds etc.) and seed totals (~576) | C1 seeds now 32 (was 30), C4 30 (was 29); recount at import. |
| `Interview_Master_Curriculum_Mapping.md` | Line 86 (known drift 5) | "`python.concurrency-basics` is an alias of `concurrency.model-selection`" was removed by INT-A (registry: python.concurrency-basics owned by A1). Delete the claim. |
| same | Section 7, C4 rows | DR-C4-M2 "thesis aloud" and DR-C4-M3 "Recruiter pitch in 60 seconds" are now backed by C4.M2 (read-aloud check) and C4.M3 (recruiter pitch) respectively; keep delivery scoring structure-only and do not score narrative delivery (C1.M9). |
| same | Section 4.3 weights | C4 weight 0 is consistent with gating; no change, but note C3 CW (B 5 / A 4 / H 4) sits low against C3 module tags `M`; calibrate after pilot. |
| `Company_Pack_to_Curriculum_Map.md` | Section 4 Global-product themes | Theme labels ("bias for action", "dive deep", "raising the bar") echo a single company's published principle titles. Keep neutral wording per R-C1-3 and route to legal review (S6). |
| `Learner_Journey_and_Study_Routes.md` | Table 7.3 | C4 4.6 h baseline assumes C4 Deeper 6.0 h; if the boss declines W-C4-1 (6.75 h), recompute C4, hybrid totals and the 294.5 h grand total. |

## 5. Unresolved items needing humans

1. **Boss:** ratify waiver W-C4-1 (or approve 6.75 h).
2. **Named professionals (blocking M5-M9):** compensation/tax professional and employment-law counsel. Items: R1 wage/gratuity computation under the Labour Codes; R2 slabs/regime; R3 EPF share and ceiling; R4 ESOP/RSU tax; R5 non-compete, bond and moonlighting enforceability; R6 notice, buyout and current-pay disclosure legality (also M7-Q4); V4 Income-tax Act 2025 section and form numbers against primary text (never opened).
3. **Qualified reviewer, C1:** HR practitioner and counsel for M7 market-practice guidance (counter-offer, hybrid, relocation) and the global-HR awareness lesson; legal review of every company pack's principle titles with primary-source URLs (only Amazon's 16 titles were checked, via search listings; the page itself was not rendered).
4. **C2:** two SME labellers and 30 transcripts for claim-risk calibration; confirm ARC-3 (Django MQTT access control) and ARC-4 (field-service/pest-control) are acceptable as generic given how close they sit to real project types; confirm Ledger onboarding ships in onboarding.
5. **C3:** spike S-5 outcome; A5.M10 to publish the six `lld` seed IDs (C3 placeholders until then); pilot calibration of the seven-step time budgets and the 75 percent skip threshold.
6. **All Track C:** SME calibration of scorer on Indian-English transcripts; 3-learner pilot; named reviewers (still "to be assigned" in all four headers).
