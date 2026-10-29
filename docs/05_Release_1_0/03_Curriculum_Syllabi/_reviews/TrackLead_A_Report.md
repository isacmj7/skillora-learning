---
name: Track Lead A Report
scope: A1 to A8 (Track A, Senior Python Backend Mastery)
reviewer: TRACK-LEAD-A (resumed run)
date: 2026-10-06
status: Review complete, pending boss sign-off
---

# Track Lead A Report (2026-10-06)

Inputs read: boss matrix (G-1 to G-13, rulings, ownership rows, effort table, section 8 items for A1 to A8, section 10 addendum), syllabus template, tag registry, Integration Consistency Report, Verification Log A5-A8 (and the A1-A4 entries for A4), Boss Consolidated Review, all six integration documents (every Track A reference), A7 and A8 in full, A2 in full, and risk-based passes over A1, A3, A4, A5 and A6.

State found on resume: no A-file carried a track-lead log line. File timestamps and in-text "Track A lead review" resolutions showed the interrupted run had edited A1 to A6 and never reached A7 and A8 (their last edit predated the review). A1 to A6 therefore got a sanity pass (hours, weights, tags, ownership, seeds, facts); A7 and A8 got the full review. A one-line "Track A lead review" log was appended to all eight files.

Mechanical checks passed for all eight courses: module hours sum exactly to the header and to boss section 5 (A1 22/15, A2 15.5/10.5, A3 35/23.5, A4 30/22, A5 21.5/13.5, A6 26/18, A7 24/15, A8 27/17.5); no module Deeper hours below 0.75 h or above 3.0 h; every LF-2 weight column sums to 100 (all three targets); every backticked competency tag exists in the registry with the right owner; goal tags match boss section 8 and 4.5; no model names; Python 3.14 default, PostgreSQL 18.x, MySQL 8.4 LTS, no live cloud/cluster/Kafka pins respected.

## 1. Verdicts

| Course | Verdict | Reason |
|---|---|---|
| A1 Python Internals | **Approve** | Strongest dated-status handling in the set (GC revert, free-threading phases, JIT status never overstated). Seeds are senior, with measurement or failure-story requirements. One route-multiplier contradiction fixed. |
| A2 Concurrency and Async | **Approve with conditions** | Best incident-style framing and seed outlines. Conditions: Celery 5.6 officially lists CPython 3.9-3.13 while the sandbox default is 3.14, so the M9 labs need an engineering check or a pinned 3.13 lab header; 3.15 final What's New re-read after 2026-10-09. |
| A3 DSA | **Approve** | Pattern-first design, original problem ladder, strict from-scratch variants, calibrated time bands, clean pointers to C3 and A5.M4. Only open human item is the AI-tool policy primary-source check. |
| A4 Databases | **Approve with conditions** | Exemplary per-engine accuracy. Conditions: MySQL 9.7 driver/ORM support check before any 9.7 image; PostgreSQL 19 GA review within 30 days of GA; two-session mode (S-2) is the critical platform dependency for M5-M6. |
| A5 API and Backend Architecture | **Approve with conditions** | Owner depth in M4, M5, M6, M8, M10 is strong. Two ownership leaks fixed (job-worker semantics, performance diagnosis). Conditions: Django 6.1 async DB claim (primary docs unread), mocked issuer controls (P-09), C3.M7 acceptance of the six `lld` seeds. |
| A6 System Design | **Approve with conditions** | Rubric, rule registry and library are the platform's backbone. Conditions: canvas spec must mirror section 1.1 and 1.3 before visuals are authored; two independent SME scorers for the 30-design calibration set; M8 needs the 3-month AI review cadence honoured. |
| A7 Cloud, DevOps and Observability | **Approve with conditions** | Substantively sound and honest about simulators (every lab states what it does not model). Conditions: spike S-1 (emulator and licence) must close before M4 and M5 labs are authored; five narrow `[verify]` items stay at each review (SnapStart exclusions and Python 3.15 runtime date, Fargate wording, LocalStack licence text); named reviewer. |
| A8 Testing, Quality and Security | **Approve with conditions** | Technically strong and correctly scoped (OWASP depth owner, X1 owner). Needed real conformity work (learner-fit layer lacked weights, diagnostics counts and a compliance log). Conditions: S-6 Lab Service legal and security review; legal read of the X1 primer (DPDP dates) before publication; named reviewer. |

## 2. Changes made (A1 to A8 only)

### A1
- LF-5 route multipliers: "M6 at 0.25" contradicted the module tags (M5, M6, M8, M9, M10 are `S` for the AI route, multiplier 0.6 per G-3). Corrected.
- Difficulty-word to 1-5 integer mapping stated (data model requires integers).
- Log line appended.

### A2
- Compliance log restated the open `[verify]` count (it was 3; the Celery-on-3.14 compatibility risk makes it 4).
- Difficulty mapping stated; log line appended. No content defects found in seeds, hours (15.5/10.5), weights, ownership or pointer rules.

### A3
- Lesson IDs `A3.Mn.k` stated (lessons were only numbered inline; the data model needs stable IDs); difficulty mapping stated; log line.

### A4
- Difficulty mapping stated; log line. Hours, version pins (PostgreSQL 18, MySQL 8.4 LTS primary, 9.7 as dated LTS fact only, no 9.7 image), `.n` lesson IDs and ownership rows verified.

### A5
- **Ownership leak, M8.3.** The lesson, the ownership row and a seed treated worker-level job semantics (Celery/RQ/Arq choice, acks, visibility timeout) as A5-owned. Boss O-15 gives that to A2.M9. Lesson A5.M8.3 is now an API-side pointer (202 plus job resource, enqueue in transaction; worker semantics -> A2.M9, 20 minutes, no lab); the ownership row says so.
- **Duplicate seed.** Q-A5-M8-03 (choose Celery, RQ, Arq, Dramatiq or cloud queue) duplicated A2 Q9.2. Replaced with an owner-content question on event contract design and evolution (A5.M8.5, previously without a seed); tag row in 7.1 updated.
- **M11.4** "Performance diagnosis and tuning" retitled to API-layer hooks (diagnosis method is A8.M10 and A1.M11 per O-63); mastery focus reworded.
- Q-A5-M7-03: credit tag was the secondary `sec.api` first; now `backend.async-events` (credit) with `sec.api` as secondary.
- Difficulty mapping stated; log line.

### A6
- Library numbering (#1 to #20), the solution-architecture profile weights (D1 15, D4 15, D10 10, D2 5, D5 10, sums to 100) and the O-111 Challenge checked against the Company Pack and Interview Master. Difficulty mapping stated; log line. No content change needed.

### A7
- Header outcome 4 still said "`ops.linux-triage` [new tag proposed to registry]"; it is registered (A7-owned). Removed; compliance log wording fixed.
- M5-Q1 ideal answer said the SQS visibility timeout should be "commonly several times" the function timeout; made precise (AWS guidance for Lambda consumers is at least six times the function timeout plus the batching window; flagged for re-read at review).
- Seed label vs canonical ID: 4.1 said "IDs are shorthand for `Q-A7-Mn-V1..V3`", which does not match the data model pattern. Replaced with the importer rule (canonical number assigned, label kept as legacy ID).
- LF-1 L4 hours ("9-11 h after skip") did not reconcile with the module hours; restated as 11-13 h at full weight, with the hybrid 0.6 SHOULD multiplier on top (ties to Learner Journey baseline 10.8 h and expected 7.5 h).
- Open item 2 resolved: the A6.M4 Challenge is platform-neutral and names no AWS service; A7.M5 owns Kinesis vs SQS vs MSK selection criteria at overview level (O-23). No A6 change needed.
- Difficulty mapping stated; log line.

### A8
- **Outcomes** were nine (template: five to eight) and credited `llm.security` (owner B5) while omitting owned `sec.compliance`. Reduced to eight; X1 and `sec.compliance` added; `llm.security` marked secondary, pointer level.
- **Learner-fit layer not conformant with G-11.** Added section 8.1a: interview-weight percent per module for B, A and H (each sums to 100, `[judgement]`) and diagnostic counts per module (63 in total, IDs `Q-A8-Mn-D1..Dk`); added the LF-1 to LF-7 mapping so the importer can find each part.
- **Readiness** used 80 percent figures as if they were readiness. Rewritten to G-6 (85 percent retake after 7 days, two Senior-bar voice sessions 48 hours apart); module rows 8.9 M7 and M9 aligned.
- **Wrong tag guidance.** Section 8.10 told authors to write `eng-code-review`, `sec-supply-chain`, `perf-load-testing` (hyphen for dot), which would fail the registry and the data-model Tag pattern. Corrected to `<domain>.<topic>` and stated that dotted-to-hyphen forms are invalid.
- **Missing Boss compliance log** (boss section 0.1 requires one in every wave-2 syllabus). Added as section 9.
- **Lab Service fallback** added to the timed practical (S-6 failure case).
- Generic "still [verify] at authoring" list replaced by a precise engineering item (record pins at image build; no course claim depends on an unverified version).
- Lesson ID rule stated (X1 is `A8.M9.4`); seed label to canonical ID rule and difficulty mapping stated; log line.

## 3. Discrepancies in integration documents (fix there, not in the syllabi)

| File | Location | Correction needed |
|---|---|---|
| `Interview_Master_Curriculum_Mapping.md` | Line 86, known drift 5 | Delete "`python.concurrency-basics` is an alias of `concurrency.model-selection`". The registry owns `python.concurrency-basics` (A1, secondary A2) and A2 attaches it as a secondary tag only. (Also reported by Track Lead C.) |
| same | Line 99 (A7 tag list) and rows 564 to 566 | `aws.compute`, `aws.messaging`, `aws.data`, `aws.security` and `cloud.aws.compute`, `cloud.aws.data`, `cloud.aws.messaging`, `cloud.aws.security` are invalid (third segment). Use `cloud.aws-compute`, `cloud.aws-data`, `cloud.aws-messaging`, `cloud.aws-security` (registry A7). |
| same | Line 100 (A8 tag list) and row 584 (A8 M9) | `sec.llm-app` is not a registry tag. Use `sec.compliance` (owned, X1 at A8.M9.4) for the M9 row; `llm.security` is secondary (owner B5) and gives no A8 credit. |
| same | Section 7, A2 row M3 | M3 maps to `concurrency.model-selection` (as A2 seeds Q3.1 to Q3.3 do). No change; noted because the alias claim above must not be used to justify it. |
| `Content_Data_Model_and_Import_Schema.md` | Section 3, Question row (line ~71, legacy forms) | Add legacy forms: A2 seeds `Qm.k` (map to `Q-A2-Mm-0k`), A2 placement `Q-A2-Mx-P1..P6`, A4 `Q-A4-Mn-D1..` and seeds `Mn-Qk`, A6 seeds `Mn-Qk` and placement `Q-A6-Mn-P01..`, A7 seeds `Mn-Qk` and placement `Q-A7-Mn-D1..Dk`, A8 seeds `Qn.k` and placement `Q-A8-Mn-D1..Dk`. All use "difficulty words" that map to 1-5 (easy 1, easy-medium 2, medium 3, medium-hard 4, hard 5; A8 uses seniority labels Mid 2, Mid-Senior 3, Senior 4, Senior-Staff 5; align after pilot). |
| same | Section 8, seeds row | Per-seed goal tags live in different places: A1 5.1, A2 inline per seed, A4 4.1, A5 7.1, A6 section 8 table, A7 4.1; **A8 seeds inherit the module tag (8.1) by design** (G-10 sentence in A8 8.1). Parser must handle all. A2 also carries 6 incident-style scenario items (36 seeds, not 30). |
| same | Section 12 item 5 and `Platform_Capability_Requirements.md` the open-items table, item 5 (line ~388) | Stale: A4 now uses `A4.Mn.k` lesson IDs and A1 `A1.Mn.k`; A3 and A8 now state `A3.Mn.k` and `A8.Mn.k`. Only the importer alias remains necessary. |
| same | Section 7 worked example, lesson A1.M1.1 title "Names, objects and mutation" | Syllabus title is "Names, objects and references" (A1 4, A1.M1.1). Align the example. |
| same | Section 9, validation V-017 and section 11 diagnostics (~6 per module) | A8 now has 63 diagnostics across 10 modules (5 to 8 per module); A8 modules with 7 or 8 (M7, M9) need extra MCQs for two parallel forms. Mention in the placement-eligible count. |
| `Placement_Test_Blueprint.md` | Open item 3 (line ~329) | Extend the list of diagnostic ID forms with A2 `Q-A2-Mx-P1..P6`, A4 `Q-A4-Mn-D1..`, A6 `Q-A6-Mn-P01..`, A7 and A8 `Q-A7-Mn-D1..Dk`, `Q-A8-Mn-D1..Dk`. |
| same | Line 35 (Python section) | "A1 (11), A2 (10) ... about 126" is the total for four sections combined; make the per-section count explicit (A1 5 per module, A2 6 per module, A4 and A7 about 6, A8 63 in total). |
| `Learner_Journey_and_Study_Routes.md` | Section 7.1, A8 row (line ~281) | Skim list should include M3, M6, M8, M10 (A8 8.2 reference-learner expectation: M2 skip or skim, M3 skim, M4 skim, M5 study, M6 skim, M7 study, M8 skim, M9 study, M10 skim). M8 is shown as a gap in the doc but is "skim" in A8. |
| same | Section 7.1, A2 row (line ~275) | A2 states M1 and M2 are the standing skip candidates and M3 is lesson-skippable at a measured score; the doc lists M1 as a gap. Reconcile: M1's decision-guide lesson is explain-depth (keep as gap) but M2 and M3 should appear as skim. Decide with the A2 owner. |
| same | Section 7.2/7.3 | No change; A1 arithmetic (15.0 h Deeper, M5 skip, M1 skim) and A7/A8 baselines (10.8 and 12.3 h from 0.6 x Deeper + assessment share) reconcile with the syllabi. |
| `Company_Pack_to_Curriculum_Map.md` | Lines ~85 to 90 | All A-module references resolve. Library item references (#8, #10, #11, #12, #15 to #17) match A6 section 3.1. No change. |
| `Platform_Capability_Requirements.md` | PCR-014 (Lab Service, priority "Should") | A8.M7 labs and the A8 timed practical are written around the Lab Service with a fallback; keep "Should" but record that A8's fallback is code-review and static-analysis on vulnerable snippets. Flag `lab_service.enabled` Off until S-6 passes (already there). |
| same | PCR-001 / PCR-011 | Add a compatibility check: Celery 5.6 lists official support for CPython 3.9-3.13; the default image is 3.14 (A2.M9 and A5.M8 labs). Engineering must run the Celery eager, memory-broker and Redis labs on 3.14 or pin the labs to the selectable 3.13 image and say so in the lab header. |
| `_Boss_Decisions_and_Ownership_Matrix.md` | O-15 and the A5 section 8 item 5 text | No change to rulings. Note for the boss: A5.M8 carries a 20-minute pointer lesson (job systems from the API side) to A2.M9 as a consequence of O-15; list it in the pointer inventory. |
| `_Competency_Tag_Registry.md` | Footer "Total registered tags: 234" | Correct (verified by count). No change. |

## 4. Unresolved items needing humans

1. **Named reviewers** for all eight courses ("to be assigned"); the quality gates (SME, pedagogy, visual, question bank, 3-learner pilot) are not yet run.
2. **Spike S-1** (moto-style emulator vs LocalStack, licence review): blocks authoring of A7.M4 and A7.M5 labs. Default to moto-style if the review is late.
3. **Spike S-6** (Lab Service legal and security review): gates A8.M7 labs and the A8 timed practical; also A5.M7 and A7 lab use.
4. **Spike S-2** (two-session database mode): critical for A4.M5-M6 and the A2.M9 `SKIP LOCKED` preview.
5. **Engineering image-build checks:** Celery on 3.14 (A2.M9, A5.M8); pinned versions of pytest, pytest-asyncio, Hypothesis, coverage.py, mutmut, Django 5.2 LTS, FastAPI, Pydantic v2, SQLAlchemy 2; CI import check of the A3 standard-library features on the pinned image; 3.15 selectable only after 3.15.0 final (2026-10-09) and re-read of its What's New (A1, A2).
6. **Remaining `[verify]` items (all justified, narrow):** A2 (4, see above); A3 AI-tool-policy primary company guidance (human web check before M11.3 publishes) and the import check; A4 MySQL full-prefix row-comparison optimisation (run EXPLAIN in the sandbox) and MySQL 9.7 third-party driver/ORM support; A5 manifest pins and Django 6.1 native async DB coverage (primary docs page unreadable during verification); A6 canvas-spec acceptance of vocabulary v0.1+; A7 SnapStart exclusions and Python 3.15 runtime date, Fargate "Firecracker" wording, LocalStack licence text, X-Ray and App Runner status re-checks; A8 none beyond the engineering pins.
7. **Legal and professional review:** X1 compliance primer (A8.M9.4) is awareness-only but carries DPDP Rules 2025 phase dates; a qualified reviewer should read it before publication, then every 6 months. Licence statements (Terraform BUSL, Redis tri-licence, Valkey) in A6 and A7 are vendor-risk notes only and need a counsel glance before public release.
8. **Calibration staffing:** 30 labelled designs with two independent SME scorers (A6 7.3, R-A6-4); pilot calibration of every `[judgement]` weight, band threshold and difficulty label (including A8's seniority-based labels vs A1 to A7 easy/medium/hard).
9. **A8 Q9.2** is the single `ref` item to B5.M4 but still carries an authored principles outline; replace with a pure bank-ID `ref` once B5 publishes its item IDs.
10. **Boss decisions:** (a) accept the A5.M8 pointer lesson as a consequence of O-15; (b) confirm the MySQL 9.7 image gate (A4 open item 1); (c) confirm the 3.15 image gate at 3.15.1 (A1 open item 1); (d) approve the A8 interview-weight table `[judgement]` (8.1a).
