---
name: Integration Consistency Report
version: '2.0'
status: 'Final candidate — pending boss approval and human SME review'
date: 2026-10-05
integration_pass: INT-A 2026-10-05
---

# Integration Consistency Report (INT-A, 2026-10-05)

Scope: all 19 syllabi (A1-A8, B1-B7, C1-C4) audited against the Boss Decisions and Ownership Matrix, the Authoring Standard, the Boss Consolidated Review and the five verification logs. Only INT-A edited the syllabi. No new scope was added; content was shortened only where it duplicated an owner course.

## 1. Summary

| Check class | Result |
|---|---|
| 1 Cross-references | Every module reference is in range and every lesson-level pointer resolves (the only external lesson ID is A8.M9.4 for X1). 8 wrong or stale pointer entries fixed (section 3). |
| 2 Ownership | 25 ownership entries covering about 30 duplicated seed questions and 6 duplicated lesson, lab or ownership-row passages, re-angled to the owner's pointer or to a distinct angle (section 3). |
| 3 Effort | Every course header equals the boss table; module hours now sum to the header in all 19 courses. Totals 366 S / 247.5 D / 47 assessment, Deeper plus assessment 294.5 h against 300 h. Corrected table appended as boss matrix section 10. |
| 4 Tag taxonomy | `_Competency_Tag_Registry.md` created (234 tags, one owner each). 36 tag-defect entries fixed (inconsistent, ownerless or non-existent tags). Goal tags now use `B:x A:x H:x` everywhere. |
| 5 Prerequisites | 13 prerequisite entries across 11 courses aligned with the boss dependency graph; no cycles; the one forward hand-off (C4.M2 to C1.M9) is stated on both sides. |
| 6 Version pins and platform | Python 3.14 default (3.13/3.15 selectable, 3.14t only A1.M3/A2.M1), PostgreSQL 18.x, MySQL 8.4 LTS, no GPU, no live K8s or cloud, no RabbitMQ/Kafka, no BYO keys: no deviations found. |
| 7 Canvas rule IDs | All R-IDs cited in A5, A6, A7, B1, B3, B4, C2 and others exist in the A6 registry (39 IDs). Two courses that described checks generically now cite IDs. |
| 8 Terminology | Simpler/Deeper, mastery check, Interview Master consistent; learner levels L1-L4 separated from score anchors (section 3, B7 and C1). |
| 9 Front matter | All 19 files carry the same YAML block with version 2.0 and the standard status. |

## 2. Front matter (class 9, all 19 files)

Each file now starts with YAML: name, course_id, track, version '2.0', status 'Final candidate — pending boss approval and human SME review', date 2026-10-05, last_reviewed 2026-10-05, integration_pass. Files that previously had no YAML: A1-A5, A8, B1, B3-B7, C1, C3, C4. Files that had YAML without the standard status (A6 'draft for pilot', C2 'Draft for review', A7 and B2 with no status) were rewritten and extra keys kept (C2's boss-wins sentence moved to a `precedence` key). Conflicting body stamps fixed: A3 (Version 1.1), B1 (v1.1 and 'draft for review'), C3 ('wave-2 v2.0'). B7 had no stamp at all.

## 3. Defect log (file, class, issue, fix)

| File | Class | Issue | Fix |
|---|---|---|---|
| A2 | thresholds | Badge capstone/canvas rubric stated as 3.0 (header, M10 extras, Q10.3); G-7 sets canvas rubric at 3.5 with no dimension below 3 | Set to overall >= 3.5, no dimension below 3, no critical rule findings |
| A2 | pointers | '(G-9, section 7)' mis-cites the boss matrix section; open item 2 (rule IDs 'reserved') stale now the registry is published | Cite 'boss matrix section 7'; open item 2 marked resolved |
| A2 | prereq | A1 is a solid prerequisite of A2 in the boss graph but A2 said 'A1.M4 recommended' | Stated A1 as required (placement-waivable) |
| A2 | tags | Secondary tag `python.gil` does not exist in A1, and an incorrect 'alias' claim existed (1x) | Replaced with `python.concurrency-basics` and removed the alias claim |
| A2 | tags | Secondary tag `python.gil` does not exist in A1 (3x) | Replaced with {new} |
| A2 | ownership | Q1.1 (GIL and web service) and Q1.3 (free-threaded status) duplicated A1.M3-Q1 and A1.M3-Q2 | Q1.1 reduced to a labelled ref to A1.M3-Q1; Q1.3 re-angled to a 3.14t adoption decision and benchmark design (status stays in A1) |
| A2 | prerequisites | M9 lab reused A4.M6's SKIP LOCKED worker although A4 comes later in the route (undeclared forward dependency) | Marked as a self-contained preview per G-1, A4.M6 recommended; open item 3 resolved |
| A2 | terminology | 'mastery-check' hyphenated as a noun | 'mastery check' |
| A3 | prereq | Dotted dependency A1 to A3 not stated | Added recommendation |
| A3 | front-matter | Body stamp said Version 1.1 | 2.0 |
| A3 | tags | Unlisted tag `dsa.coding-fluency` not in owned list (1x) | Folded into an owned tag {new} |
| A3 | tags | Unlisted tag `dsa.graphs-trees` not in owned list (1x) | Folded into owned {new} |
| A3 | tags | Secondary tag `dsa.communication` is not a registry tag (owner is C3 coding.communication) (4x) | Replaced with {new} |
| A3 | ownership | M10-Q3 compared fixed window, sliding log and token bucket with Redis atomics (A5.M4 / Q-A5-M4-02 content); archetype row, lesson and lab repeated the comparison | Narrowed M10-Q3, the archetype row, the topics list and lab names to the sliding-window-log deque data structure with an injected clock; A5.M4 pointer kept |
| A4 | pointers | Pointer to A7.M4 (AWS compute) for deploy pipelines and blue-green; those live in A7.M8 (CI/CD and deployment strategies) | Changed to A7.M8 |
| A4 | tags | Secondary tag `api.pagination` (A5) does not exist (1x) | Replaced with {new} |
| A4 | tags | Secondary tag `design.data-partitioning` (A6) does not exist (1x) | Replaced with {new} |
| A5 | ownership | M11 listed 'performance diagnosis', load testing and a load-test lab as owned, duplicating A8.M10 (O-63) and A1.M11; Q-A5-M11-03 was the same p99 diagnosis question as A8.Q10.x | Reduced to API-layer hooks plus a pointer to A8.M10/A1.M11; re-angled Q-A5-M11-03 to per-tenant API-layer causes |
| A5 | canvas-rules | A5 notifications canvas task named no registry rule IDs | Cited R-IDEMP-1, R-DUAL-1, R-RATE-1, R-QUEUE-2 and the G-7 bar |
| A5 | prereq | Boss graph dependencies for A5 (recommended A2, A4) not framed as such | Clarified required vs recommended |
| A5 | tags | Non-normalised tag `security.api` (G-9) (1x) | Respelled to {new} |
| A5 | ownership | Q-A5-M3-01 and the M3 practice task re-taught keyset SQL (tuple comparison, composite index), which A4.M4-Q3 / O-30 owns; Simpler task 'implement offset then keyset' repeated the A4.M4 lab | Re-angled to the API cursor contract; lab reuses the A4.M4 query and owns only the contract |
| A6 | prereq | A4 and A5 are solid prerequisites of A6 but were 'strongly recommended'; the forward dependency of A6.M8 on Track B was unstated | Made A4/A5 required and stated the A6.M8 gate on B2/B3/B4/B6 |
| A6 | tags | Secondary tag `api.rate-limit` (owner A5) has no owner tag (1x) | Replaced with {new} |
| A6 | ownership | M2-Q3 (cache stampede), M4-Q2 (idempotency key mechanism) and M4-Q3 (rate-limiter algorithms) duplicated Q-A5-M5-01, Q-A5-M4-01 and Q-A5-M4-02 | Re-angled to tier-level cold-start protection, per-hop idempotency placement with reconciliation, and rate-limit service placement/failure policy; mechanisms referenced to A5 |
| A6 | ownership | A6.M4 lessons 2 and 3 re-taught backoff with jitter (X2) and the idempotency-key mechanism, both owned by A5.M4 | Reframed to system-level design with pointers to A5.M4 |
| A6 | ownership | A6 outbox poller task could be read as duplicating the A5.M8 outbox lab | Added a note distinguishing the relay/poller exercise from the A5.M8 lab |
| A7 | canvas-rules | Canvas task described checks generically without registry IDs | Cited R-SPOF-1, R-QUEUE-2, R-IDEMP-1, R-AUTH-1, R-OBS-1 and the G-7 canvas bar |
| A7 | prereq | Dotted dependency A5 to A7 and A7 to A6 not stated | Added |
| A7 | tags | `cloud.aws.<x>` used a third dotted segment, breaking the namespace.topic convention (18x) | Renamed to cloud.aws-compute / aws-data / aws-messaging / aws-security |
| A7 | ownership | M5 row, lesson A7.M5.3 and the idempotent-consumer lab duplicated A5.M4/A5.M8 idempotency and outbox depth | Reframed as AWS primitives plus an A5.M8 pointer; lab reuses the A5.M8 consumer; lesson retitled to DLQ/redrive/replay operations |
| A7 | ownership | M8-Q3 duplicated A4.M8-Q2 (expand-contract migration) and M10-Q2 duplicated Q-A5-M4-03 / X2 (retries, backoff, jitter) | Re-angled M8-Q3 to pipeline stage sequencing with an A4.M8 ref; re-angled M10-Q2 to infrastructure-level resilience with an A5.M4 ref |
| A8 | effort | Module hours in 8.1 summed to 30.0 S / 18.5 D but header and boss table state 27 / 17.5 (the table total was labelled 'rounded ... ~30 raw') | Re-based module hours so they sum exactly to 27.0 S / 17.5 D (M2,M3,M4,M6,M8,M10 -0.5 S; M2,M6 -0.5 D) |
| A8 | tags | Goal tags in 8.1 were split into three letter columns (B / A / H) instead of the platform format B:x A:x H:x; legend said 'Tags per ruling' | Merged into one 'Goal tags' column in the compact format and added the seed-question inheritance sentence (G-10) |
| A8 | bands | 8.2 placement bands used 90 / 75-89 / 50-74 instead of the G-5 defaults | Skip >= 85, Skim 70-84, Study 50-69, Deep-study below 50 |
| A8 | thresholds | Badge row omitted the voice mock 'Meets' bar although the final assessment includes a voice mock | Added the G-7 voice 'Meets' criterion |
| A8 | prereq | Dotted dependencies A1/A2/A5 to A8 loosely worded; placement skip threshold 80% vs G-5 85 | Aligned with boss graph and G-5 bands |
| A8 | tags | Tag `sec.llm-app` is not owned anywhere (LLM-app security is owned by B5 as llm.security; A8 keeps a pointer) (3x) | Replaced with {new} (secondary) |
| A8 | tags | A8 header lacked an owned tag for the X1 compliance primer (sec.compliance used by B3 and C4) and listed LLM-app security as owned | Added `sec.compliance` as owned (X1, A8.M9.4); `llm.security` demoted to secondary (owner B5) |
| A8 | ownership | Q7.2 (JWT) overlapped Q-A5-M6-02; Q9.2 was a full LLM-security answer duplicating B5.M4 instead of a short pointer; Q10.3 (async endpoint slow, CPU low) duplicated A2.Q4.3 and A2.Q10.2 | Q7.2 re-angled to exploit-and-fix on a seeded snippet; Q9.2 marked as the single ref item and shortened; Q10.3 re-angled to throughput-plateau attribution with Little's Law and load-test data |
| B1 | front-matter | Body stamp said v1.1 and 'Status: draft for review' | v2.0 and the standard status |
| B1 | tags | Pseudo-tag `llm.tokenisation-pointer` use only via B2 (1x) | Replaced with owner tag |
| B2 | tags | Goal tags written as slash triples (M/S/N) in LF-2 tables (12 places), not the G-3 format | Rewritten as B:x A:x H:x (order B/A/H) |
| B2 | tags | B2 LF-2 M1 goal tag unresolved ('M/M/M? see below') with a correction note elsewhere | Set M1 to B:S A:M H:M (boss ruling) and removed the correction note |
| B2 | prereq | A2 was only implied for B2 ('async from A2') | Stated A2 as required |
| B2 | tags | Secondary tag `sec.llm-top10` does not exist (1x) | Replaced with {new} |
| B2 | tags | Secondary tag `rag.decision` does not exist (1x) | Replaced with {new} |
| B2 | tags | Secondary tag `agents.mcp` does not exist (1x) | Replaced with {new} |
| B2 | tags | Secondary tag `eval.methodology` does not exist (1x) | Replaced with {new} |
| B2 | ownership | M10-Q1 (full injection defence answer) broke R-B5-1 (B2 attack-class items must be MCQ-assessed only) and duplicated B5.M4 | Re-angled to the basic pre-launch controls; depth referenced to B5.M4 |
| B2 | ownership | M11-Q1 (full logging/tracing answer) overlapped B5.Q7.1 monitoring | Trimmed to the minimal trace record; monitoring referenced to B5.M7 |
| B2 | ownership | M11-Q3 (build a first eval set) duplicated B5.Q1.1 | Converted to a single ref item pointing to B5.Q1.1 / B5.M2 |
| B2 | ownership | M12-Q2 (voice assistant latency) duplicated B6.M6-Q1 | Re-angled to the LLM feature architecture when adding a voice channel; latency budget referenced to B6 |
| B3 | effort | B3.M10 Deeper 0.5 h is below the G-2 floor of 0.75 h | M10 Deeper set to 0.75 and M4 Deeper reduced to 1.25 (course Deeper total unchanged at 10.5) |
| B3 | tags | Goal tags written as slash triples (M/S/N) in LF-2 tables (10 places), not the G-3 format | Rewritten as B:x A:x H:x (order B/A/H) |
| B3 | tags | Secondary tag `llm.evaluation-method` does not exist (1x) | Replaced with {new} |
| B3 | ownership | Q1.3 (RAG and hallucinations: measure and reduce) overlapped B5.Q5.2 | Re-angled to retrieval-vs-generation triage and retrieval-side fixes; added ref to B5.M5/Q5.2 for measurement |
| B3 | ownership | Q10.3 (enterprise assistant architecture on AWS) duplicated A6 library #15 / M8-Q1 | Re-angled to a design review against R-AI-3, R-AI-4 and R-TENANT-2 (all in the A6 registry); full design referenced to A6 |
| B4 | pointers | Remediation link A2.M3 (multiprocessing) for task-group concurrency; TaskGroup lives in A2.M5 | Changed to A2.M5 |
| B4 | pointers | Pointer 'A2' with no module | A2.M5 |
| B4 | tags | Secondary tag `eval.llm-judge` does not exist (1x) | Replaced with {new} |
| B4 | tags | Secondary tag `sec.llm-injection` does not exist (1x) | Replaced with {new} |
| B4 | ownership | M9-Q1 (indirect prompt injection definition and defences) overlapped B5.Q4.1 | Re-angled to agent-specific injection via tool output and MCP tool descriptions with runtime controls; general concept referenced to B5 |
| B5 | tags | Goal tags written as slash triples (M/S/N) in LF-2 tables (9 places), not the G-3 format | Rewritten as B:x A:x H:x (order B/A/H) |
| B5 | pointers | OWASP API pointed at A8.M9 (threat modelling); OWASP API is A8.M7 | Split into threat modelling -> A8.M9 and OWASP API -> A8.M7 |
| B5 | prereq | B2 is a solid prerequisite of B5 but was 'strongly recommended' | Made required |
| B5 | tags | Legacy tag `sd.ai-feature` (G-9) (2x) | Replaced with {new} (secondary, owner A6) |
| B5 | ownership | Q4.3 (agent tool-permission design for injection safety) duplicated B4.M9-Q2 | Re-angled to a red-team test plan with pass/fail criteria; permission design referenced to B4.M9 |
| B6 | prereq | B2 is a solid prerequisite of B6; B6 listed only B2.M8 as 'basics' | Stated A2 and B2 as required |
| B6 | tags | Secondary tag `project.defence` has no owner in C2 (2x) | Mapped to C2-owned {new} |
| B7 | effort | B7.M4 (0.7 h) and M9 (0.8 h) odd Deeper hours; M4 below 0.75 floor | M4 0.75 and M9 0.75; total Deeper unchanged at 11.0 |
| B7 | tags | 8.3 split goal tags into separate B / A / H columns | Merged into a single 'Goal tags' column in B:x A:x H:x format |
| B7 | bands | 8.1 placement bands used 90/75/50 for most modules, 70/45 for M4 and M9, and 85/55 for M1 and M6 | Normalised every module to the G-5 defaults (85 / 70-84 / 50-69 / below 50); M1 and M6 keep 'skip not allowed' with 85+ treated as Skim |
| B7 | thresholds/terminology | Readiness used 'at L3'/'L3.5' (L1-L4 are learner levels only, G-4) and 'design canvas at Meets 3.0' (G-7 canvas bar is 3.5, no dimension below 3) | Replaced with rubric scores and the G-7 canvas bar |
| B7 | prereq | B1 and B2 solid prerequisites of B7 not marked required | Marked required |
| B7 | ownership | Q1.3 (long context vs RAG vs fine-tune) duplicated B3.Q1.2 | Re-angled to the fine-tune escalation decision after a RAG baseline; comparison referenced to B3 |
| B7 | ownership | Q5.2 (LLM-as-judge pitfalls) overlapped B5.Q3.1 | Re-angled to tuned-model-specific judge risks with a ref to B5 |
| B7 | ownership | Q5.3 (live rollout design) overlapped B5.M8 rollout depth | Re-angled to promotion criteria; rollout mechanics referenced to B5.M8 |
| B7 | ownership | Q7.1 (KV cache explanation) duplicated B1.Q9.2 | Re-angled to applying the formula to a serving config; concept referenced to B1 |
| B7 | ownership | Q9.3 (production LLM monitoring) duplicated B5.Q7.1 | Re-angled to classical drift detection and retraining triggers; LLM monitoring referenced to B5 |
| C1 | tags | Goal tags written as slash triples (M/S/N) in LF-2 tables (9 places), not the G-3 format | Rewritten as B:x A:x H:x (order B/A/H) |
| C1 | tags | C1 LF-2 M7 slash triple with embedded qualifier | Rewritten as B:M (S outside India) A:M H:M |
| C1 | pointers | Three pointers to 'C4.M6-M10' but C4 v2.0 has nine modules (compensation, negotiation, offers, notice/BGV are M6-M9) | Changed to C4.M6-M9 |
| C1 | tags | Secondary tags `proj.narrative` and `career.positioning` do not exist (owners use `project.narrative` and `career.positioning-thesis`) | Respelled to the owner tags |
| C1 | terminology | C1 uses L1-L4 for answer-level anchors while G-4 reserves L1-L4 for learner levels | Boss-sanctioned anchors kept; added a terminology note mapping them to the 5-point rubric (cannot be fully eliminated without rewriting all rubrics) |
| C2 | pointers | Pointer 'C4.M1-M5' uses the old C4 numbering (thesis is now merged into C4.M2) | Changed to C4.M2 |
| C2 | prereq | C1 is a solid prerequisite of C2 but C2 said 'recommended in parallel' | Required C1.M1-M3; C1 continues as a thread |
| C2 | tags | Wildcard secondary tag `design.*` is not a registry tag | Replaced by an explicit statement that A6 rule IDs are cited with no tag credit |
| C3 | effort | M10 Deeper 0.5 h below G-2 floor and an open item asking the boss to approve it | M10 0.75 h, M4 1.0 h, total 12.0 unchanged; text and open item 1 updated |
| C3 | tags | Goal tags written as slash triples (M/S/N) in LF-2 tables (10 places), not the G-3 format | Rewritten as B:x A:x H:x (order B/A/H) |
| C3 | front-matter | Body status said 'wave-2 v2.0' | Standard status |
| C4 | effort | Header claims module hours sum to 9 / 6 but no per-module hours table existed | Added section 1.1 module hours table summing to 9.0 S / 6.0 D (M3-M5 at 0.5 h Deeper, already open item 1) |
| C4 | tags | LF-2 M6-M8 goal tags written as 'S->M' arrows | Use B:S A:S H:S with an explicit promotion note (G-3 route weight) |
| C4 | pointers | LF-5 says M6-M9 are gated but the publication gate and module table gate M5-M9 | Changed to M5-M9 (timing-flag unlock stays M6-M8) |
| C4 | prereq | C1, C2, C3 are solid prerequisites of C4 but C4 said 'C1 and C2 recommended in parallel, not blocking' (and omitted C3); C4.M2 to C1.M9 forward hand-off not framed | Required C1-C3 for M5-M9, allowed M1-M2 after C1.M1-M3, documented the one forward hand-off |
| C4 | tags | Tag `hr.why-switch` does not exist (C1 owns hr.switch-reason) (1x) | Respelled |
| C4 | tags | Tag `compliance.x1` outside the namespace set (1x) | Replaced with {new} (X1 owner A8.M9.4) |
| C4 | ownership | Q-C4-M2-02 (why recently moved to Python) duplicated C1.M9-Q1 | Converted to a labelled ref item to C1.M9-Q1; kept only the written thesis sentence |
| C4 | ownership | Q-C4-M2-03 (domains: thread or drift) duplicated C1.M9-Q2 | Re-angled to the written through-line sentence and its falsifiability test; spoken answer stays in C1.M9-Q2 |

## 4. Accepted overlaps (kept, with a pointer, not defects)

| Overlap | Why it is accepted |
|---|---|
| B3.Q7.3 and B5.Q5.3 (erasure) | Different scope: B3 covers erasure in a vector index, B5 covers erasure law and process; each carries a pointer. |
| C1.M7 and C4.M7/M8 (counter-offer) | C1 scores answer integrity in the HR round; C4 owns the decision framework. Pointers in both. |
| Rate limiter in A2.M6, A3.M10, C3.M7 LLD seeds, A6 #2 | Boss-authorised when in-process, small or `ref`; A5.M4 is the owner (algorithms, Redis, canonical task). |
| A6.M4 outbox poller task | Allowed by R-A6-2 (distinct relay exercise); A5.M8 stays the outbox owner. Note added. |
| A1 rubric anchors 'Level 1 / 3 / 5' and C2 'Level 1-5' why-ladder | 1-5 rubric and probing-depth labels, not learner levels; no learner-level meaning. |

## 5. Defects that could not be fully fixed

1. **Boss matrix rows with old C4 numbering (O-104, O-107, O-108).** The matrix is read-only for INT-A except for the appended addendum; the mapping is recorded in boss matrix section 10.2. The matrix text itself still shows the old numbers.
2. **C4 module-floor exception.** C4 M3, M4 and M5 have 0.5 h Deeper (below the G-2 floor of 0.75 h). Nine modules at 0.75 h would be 6.75 h and exceed the 6 h boss total, and INT-A may not change course totals. Needs a boss decision (accept, or raise C4 Deeper to about 6.6 h). Already C4 open item 1.
3. **A6 outbox poller versus A5.M8 outbox owner.** R-A6-2 permits the poller while O-rows make A5.M8 the outbox owner. A mild conceptual overlap remains; only a clarifying note could be added.
4. **C1 answer-level anchors L1-L4.** The boss sanctioned them but G-4 reserves L1-L4 for learner levels. Disambiguated with a terminology note and a mapping to the 5-point scale; the wording itself ('scores L3 or higher') could not be removed without rewriting every C1 rubric.
5. **A2 to A4 route order.** A2.M9 uses a `SKIP LOCKED` lab whose primitive A4.M6 owns, while A4 comes later in the default route. Handled by a self-contained preview and a recommendation, but an engine rule (offer A4.M6 before A2.M9 when A4 not yet taken) is still needed from the platform side.
6. **Seed-question counts.** Re-angling kept the item counts per module, but seed-question counts quoted in a few LF-2 tables and final-assessment blurbs were not re-audited item by item against the question banks, which are authored later from these seeds.
7. **Placement thresholds that are owned elsewhere.** C3's default skip threshold (75 percent of the process rubric) is set by the placement spec (R-C3-1), not by G-5; left unchanged and flagged here.

## 6. Verification performed after the edits

- Hours: header equals the sum of per-module hours for all 19 courses; the totals equal boss section 5 (366 / 247.5 / 47).
- Regex scan: no goal-tag slash triples remain in any table row; no arrow-style tags remain.
- Rule IDs: every R-ID cited in any syllabus exists in the A6 section 1.3 registry (39 IDs).
- Tags: every backticked tag in the 19 syllabi maps to exactly one owner in the registry; only legacy spellings quoted inside normalisation notes remain.
- Pins: no occurrence of Python 3.12 or earlier as default, PostgreSQL 17 or earlier as baseline, or MySQL 9.x image.
