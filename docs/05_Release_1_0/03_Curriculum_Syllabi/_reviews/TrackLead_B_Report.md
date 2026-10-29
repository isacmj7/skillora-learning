---
name: Track Lead B Report
scope: B1-B7 (Track B, AI Engineer)
reviewer: TRACK-LEAD-B
date: 2026-10-06
status: Review complete, pending boss sign-off
---

# Track Lead B Report (2026-10-06)

Method: the interrupted earlier run left no "track lead" log line in any B file, so all seven syllabi were treated as unreviewed. Each course was reviewed in full by a dedicated pass against the boss matrix (including section 10), the template, the tag registry, the consistency report, the verification logs and every integration-doc mention of the course. Only B1-B7 were edited. Each file now ends with a "Track lead B review (2026-10-06)" line.

Positive confirmations (all seven): module hours sum to the boss header (B1 19.5/13, B2 16/11.5, B3 15.5/10.5, B4 15.5/10.5, B5 16/11, B6 15.5/10.5, B7 16/11); every module is at least 0.75 h Deeper; all tags used are registered; interview weights sum to 100; module names, counts and hours match the integration docs.

## 1. Verdicts

| Course | Verdict | Reason and conditions |
|---|---|---|
| B1 ML and Data Foundations | Approve with conditions | Sound and template-conformant. Conditions: tokeniser vocabulary redistribution licence (engineering and legal, P-01/S9); Great Expectations install check on Python 3.14; reference-learner hours (about 15 h plus assessment, not 8.5 h) calibrated in pilot. |
| B2 LLM Application Engineering | Approve with conditions | Current and senior-level. Conditions: legal or privacy read of data-use, ZDR and residency wording; interleaved-thinking header and GA status check; owner for the Provider Notes data file; SME reviewer unassigned. |
| B3 RAG and Vector Search | Approve with conditions | pgvector facts match the 2026-10 log; ownership now clean. Conditions: AWS extension-list checks (pg_search, pgvectorscale, pgvector versions), counsel on DPDP/GDPR erasure, P-04 image pinning pgvector 0.8.2 or later; 30 seeds against an 80-verbal target need authoring. |
| B4 Agents and Workflows | Approve with conditions | Strong; badge thresholds and currency lines fixed. Conditions: engineering picks the single graph-style framework and fixtures (P-07, P-12); dated vendor facts (MCP 2026-07-28 revision, A2A v1.0, OTel GenAI) not re-fetched in this pass; SME reviewer unassigned. |
| B5 LLM Evaluation, Safety and LLMOps | Approve with conditions | Complete and conformant. Conditions: human legal review of M5 and M9 (EU AI Act, DPDP, RBI/SEBI/IRDAI, Aadhaar handling); OWASP LLM 2026 numbering from primary source; assessment-hours decision (see section 4); named calibration labellers CAL-1 to CAL-3. |
| B6 Realtime and Voice AI | Approve with conditions | Accurate and senior depth. Conditions: named compliance reviewer for lesson 7.5 (TRAI recording, DLT, AI-call disclosure) before publish; five `[verify]` items with owners; P-08 harness must expose per-hop timestamps and seeded jitter. |
| B7 Fine-tuning, Model Selection and MLOps | Approve with conditions | Complete and ownership-clean after fixes. Conditions: three remaining `[verify]` items (provider terms on training from outputs, DPDP/regulator status, GPU price data file); boss to accept the M1/M6 "skip not allowed" exception to G-5. |

No course needs Rework.

## 2. Changes made

### B1
- Added a worked-example table (section 3.1) for all 10 modules (template requirement was unmet); noted per-lesson visuals.
- M10 lessons 7 -> 6 and M6 practice 8-10 -> 8 (template maxima); canvas task made optional for the badge.
- LF-1 reference-learner hours corrected from about 11 h to about 15 h plus 2.0 h assessment; placement diagnostics stated as 62 (6 per module, 8 for M6).
- Prerequisites row now states B1's place in the dependency graph.
- M9 currency: reasoning-model post-training with verifiable rewards (concept only, pointers to B7 and B2.M2), sliding-window attention and latent-KV at awareness level.
- Great Expectations `[verify]` now names an exact check with Pandera as fallback.

### B2
- Removed model names from section 6 (G-8, G-12); deleted an unsupported "rejects with HTTP 400" claim with no verification-log backing.
- Added caveat in M1 that some reasoning models fix, ignore or reject sampling parameters.
- `[verify]` markers narrowed to exact checks (legal wording; interleaved-thinking header or GA).
- LF-4 boilerplate replaced with a 12-row mid/senior/staff table; LF-5 readiness uses the full G-6 definition; cluster emphasis aligned to the Company Pack map.
- Seed fixes: senior-bar line added to M11-Q3; M12-Q2 goal tag A:M -> A:S to match its module. Worked-example and alt-text note added.

### B3
- Outcomes 9 -> 8 (template maximum); worked-example line added to all 10 modules.
- M6 indirect-injection lesson and lab reduced to a RAG-specific pointer to B5.M4-M6 (G-1, O-81); M8 judge lab replaced by the prebuilt B5.M3 judge fixture (R-B3-1).
- Q10.3 now cites only R-AI-3, R-AI-4, R-TENANT-2 as ruled.
- Model and vendor names removed from weak-answer examples; badge and readiness text aligned to G-6; LF-1 L4 skip list fixed (pgvector in practice not skipped).
- Two `[verify]` markers given exact checks; cache-augmented generation added to M1.

### B4
- Badge thresholds made explicit (G-7: 70 percent hidden tests, canvas 3.5 with no dimension below 3, voice "Meets"); header primer list matches section 8.
- Currency: code-as-action and computer-use agents (overview, 20 min or less) in M3; context engineering and on-demand skill loading in M4.
- Corrected source label for tool search (vendor docs), removed an "R-B2-2 style" mis-citation, defined pass@k vs pass^k, qualified the "quadratic cost" claim (without caching), removed a named product from the caching note (G-8).
- M7-Q3 re-angled to agent-specific error classes with a ref to X2/A5.M4 (G-1).
- LF-1 L2 skip band corrected to 85 (G-5); LF-2 M5 gets voice drill; LF-5 A5 -> A5.M4; framework-pick `[verify]` names its check.

### B5
- Fixed the garbled data-minimisation bullet in Q5.1 (synthetic identifiers only in labs; Aadhaar/PAN handling rules reviewed by a human before publish).
- Removed OWASP 2026 renumbering claims (LLM03, LLM10, "Hidden Context Exposure") sourced only from conflicting secondary summaries; risk-class names used; `[verify]` narrowed to reading the numbering from genai.owasp.org.
- Added kappa-prevalence caveat (M3), case-level resampling (M1), EU AI Act in-force dates (prohibited practices Feb 2025, GPAI Aug 2025; Omnibus dates stay as-of dated), cautious DPDP breach-notification wording (Q9.1) with X1 as owner in Q9.2.
- Rubric anchors (1-5, senior bar 3.5) added to the seed format; LF-4 split to one row per module; LF-2 cluster sentence aligned to Company Pack map; reviewer roles named (SME plus separate legal reviewer, PCR-069).

### B6
- Learner-Fit Appendix is now section 8 and open items section 7 (G-11, and existing "section 8" citations now resolve).
- Placement diagnostics moved to `Q-B6-Mn-nn` IDs with a `placement` tag (G-10), replacing `PL-B6-`.
- M7 lessons reordered so compliance is B6.M7.5 (platform docs cite 7.5); M2-Q1 retagged `voice.stt-quality`.
- Added speech-to-speech truncation (client must report audio actually played) to M4 and M4-Q1.
- G-7 design-rubric threshold added to the case-study assessment; Challenge lesson hour cap clarified (0.5 h Deeper, 0.75 h Simpler, judgement); goal-tag wording aligned to Company Pack (SHOULD for AI-startup, NICE elsewhere); A2.M7 added to prerequisites; reviewer roles named; stray `[verify]` removed, others given checks and owners.

### B7
- Added the missing G-1 ownership-notes row (OWNS and POINTS).
- Depth: rule of thumb for mixed-precision Adam full fine-tune memory (about 16 bytes per parameter, labelled), hosted preference/reinforcement-tuning provider note (no paid-API lab), mixture-of-experts memory scales with total parameters, plus a pitfall.
- Readiness criteria aligned to G-6/G-7 (85 percent retake after 7 days, Senior bar 3.5, two voice sessions 48 h apart, badge "Meets" 3.0 with none below 2); G-5 test-out rule stated.
- LF-1 L1-L4 course table, missing fifth M8 diagnostic, placement-ID note (46 items), MQTT/IoT experience-bridge row; Q-ID mapping and G-10 inheritance note; unverifiable SGLang caveat resolved as mechanism-level only; three remaining `[verify]` items given exact checks.

## 3. Integration-doc discrepancies (fix there, not in B1-B7)

| File | Location | Correction needed |
|---|---|---|
| `Interview_Master_Curriculum_Mapping.md` | B2 table (~604-619); B3 (~625-634); B4 (~640-647); B5 (~654-662); B7 (~680-692) | Rehearsal-mode columns disagree with each syllabus's LF-2 (missing coding "C" for most modules, design canvas shown as "T"/missing, B2 M2/M7/M9/M6, B3 M1-M9, B4 M1/M5/M7/M8, B5 M1-M8, B7 M2/M5/M6/M8/M9). File line 5 says the syllabus wins: regenerate the Modes column from LF-2. |
| same | B1 rows (~101, ~593-602) | Tags written without `ml.`/`data.` prefix; use registry spelling. |
| same | B4 M3 name | "Planning and reflection" -> "Planning, reflection and reasoning control". |
| same | B6 M2 row (~669) | Add `voice.transport` (M2 Q2, Q3). |
| same | B6 M11 row (~678) | Tag `voice.latency` -> `voice.architecture` (seeds); `design.ai-systems` is a ref. |
| same | B7 tag column; line ~260 | `mlops.compliance` also tags B7 Q2.3 (M2), not only M6; the `mlops.*`/`llm.ft-vs-rag` floor row omits `llm.model-routing`, `llm.eval-tuned`, `ml.data-prep`: confirm intent. |
| `Learner_Journey_and_Study_Routes.md` | ~282 and Table 7.3 (B1) | Reference learner is deep-study on M1-M5, M7-M10 but expected B1 hours are 8.5 h (below the 9.0 h baseline) and M1 is skipped where B1 skims it. Recompute against about 15 h Deeper-path estimate. |
| same | ~283 (B2) | M7 marked "study" with no B2 band; M5 "skim" while B2 says no skip without the mastery check: align rule text. |
| same | ~284 (B3) | Shows skim M1, M2 and no skip; B3 LF-1 has L4 skipping M2.1-2.2. |
| same | ~287 (B6) | Add placements for M9 and M11; M1 listed as skip. |
| same | ~132 and ~243; matrix ~552 | Backend primers list only B4.M1; B4 NICE slice is M1.2-1.3, M2.3, M7.1-7.2. Backend NICE lists B5.M4-M5 but AI crash list names B5.M1 and M4: confirm deliberate. |
| `Content_Data_Model_and_Import_Schema.md` | ~1244 (B1 row) | Counts "about 60 / 45 / 55 / 50" -> B1 has 62 diagnostics (M6 has 8), about 50 lessons, 45 visuals, 45 practice tasks. |
| same | ~1246 (B3) | State "seeds vs target": 30 seed verbal items against an 80 target (gap about 50). B4 section 11 (54 diagnostics, 27 seeds) and B7 (46 placement items) match. |
| `Placement_Test_Blueprint.md` | B1 item counts | Confirm B1 is 62 items (6 per module, 8 for M6), not a flat 60. |
| `Company_Pack_to_Curriculum_Map.md` | lines 85-90 | B2 seeds are tagged IT-services for M6 and Big4/global-IT consulting for M6 and M10; map lists IT-services as M10 only, global-IT as M1 and M12, Big4 as M12 only. Extend map or retag. |
| same | line 90 (AI-startup) | Lists B5.M1-M4 and M7, no M8; B5 treats M8 as A:M: confirm. |
| same | line 69 (B3) | Cluster tags are `[judgement]`; Big4 syllabus probes M1, M7, M10: note which modules carry S. |
| `_Boss_Decisions_and_Ownership_Matrix.md` | section 8 B5 item 1 | "M1-M4, M8 A:M H:M" and "M3 H:S" are ambiguous; B5 follows M3 H:S. State it explicitly. |
| same | section 8 B4 item 1 | "M1-M7 A:M H:M B:N" vs syllabus per-seed B:S/B:M; add note. |
| same | section 8 B6 item 6 | Still carries `[verify]` on TRAI rules; Verification_Log records it open. Keep consistent. |
| same | section 8 B7 item 1; section 5 | B7 module H:S (M1, M6) vs course-level H NICE: add note. Section 5 B5 assessment 2.0 h is below actual parts (mastery check, 90-minute timed task, canvas, voice mock, about 3 h); likely true for other courses too. |
| `_reviews/Verification_Log_B4-B7.md` | Summary | "B4 1 open marker (LangGraph)": stale, B4 removed it. |
| `_reviews/Verification_Log_B1-B3.md` | row 18 | Source of model names stripped from B2 (acceptable in a log). |

## 4. Unresolved items needing humans

1. **Boss:** decide B5 (and possibly all courses) assessment allowance 2.0 h vs about 3 h; accept B7 M1/M6 "skip not allowed" exception to G-5; decide authority of LF-2 vs the Interview Master Modes column (recommended: LF-2 wins, regenerate).
2. **Legal and compliance reviewers (named):** B2 data-use, ZDR and residency wording; B3 DPDP/GDPR erasure; B5 M5 and M9 (EU AI Act article text, DPDP Rules breach provision, RBI/SEBI/IRDAI status, Aadhaar handling, NIST AI RMF status); B6 lesson 7.5 (TRAI, DLT, AI-call disclosure) gated for publish; B7 provider terms and DPDP.
3. **Primary-source checks:** OWASP LLM 2026 numbering (genai.owasp.org); NeMo Guardrails beta label; interleaved-thinking header/GA (B2); B4 dated vendor facts (MCP revision, A2A v1.0, OTel GenAI) not re-fetched; AWS extension lists for pg_search, pgvectorscale and pgvector versions (B3); Great Expectations on Python 3.14 (B1); tokeniser vocabulary licence (B1, P-01/S9).
4. **Engineering:** pick the one graph-style framework (B4); P-07 injected-document fixture and P-12 crash injection; P-08 voice harness with per-hop timestamps and seeded jitter (B6); P-04 image pinning pgvector 0.8.2 or later (B3); Provider Notes data file owner and quarterly slot (B2); GPU price data file and X5 tuned-model gate template (B7); rerun B1 examples on Python 3.15 once selectable.
5. **Content:** SME reviewers still unassigned for several courses; quality gates 2, 3 and 5 (pedagogy, visual, 3-learner pilot) outstanding; B3 needs about 50 more verbal seeds for the 80 target; B6 A6 #17 ref ID for M11-Q3; B5 CAL-1 to CAL-3 labellers; pilot calibration of weights, bands and reference-learner hours.
