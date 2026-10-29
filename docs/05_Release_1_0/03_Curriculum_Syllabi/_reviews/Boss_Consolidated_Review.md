---
name: Skillora Learning 1.0 — Boss Consolidated Review of the 19 Course Syllabi
version: 1.0
date: 2026-10-05
author: BOSS (editor-in-chief)
scope: A1-A8, B1-B7, C1-C4 plus REV-A1, REV-A4, REV-B2
companion: ../_Boss_Decisions_and_Ownership_Matrix.md (binding decisions, ownership matrix, study order, effort, platform list, wave-2 instructions)
---

# Boss Consolidated Review

## 1. Executive summary for the founder

**Where we are.** All 19 course syllabi exist and are, as a set, a strong first draft: 193 modules, about 580 seed verbal questions, consistent structure (header, module table, per-module detail, seed questions with ideal outline, weak answers and probes, final assessment, risks, open questions). Technical content is generally accurate and honest about volatility; fast-changing items are flagged `[verify]` (about 280 flags, checklist in section 5).

**What is not yet good enough.**

1. **The learner-fit layer (template section 6) is missing in all 19 syllabi** (the section was added after the agents wrote). No course has entry-diagnostic bands, L1-L4 paths, MUST/SHOULD/NICE goal tags, a senior-bar framing, adaptive hooks per module or a readiness criterion. Average score on this criterion is 2.0 of 5. This is the single biggest gap and the reason a second wave is needed.
2. **Overlap is heavy.** At least 20 topics were taught in two to four courses (OWASP API security in A5 and A8; rate limiting, idempotency and outbox in A2, A5 and A6; guardrails in B2, B4, B5 and A8; RAG evaluation in B3, B5 and B4; the career narrative in C1, C2 and C4; coding communication in A3, C1 and C3; voice design in A6 and B6). I have assigned one owner per topic (109 matrix rows) and reduced the rest to short pointers.
3. **Hours were not budgeted.** As written the courses total 368 h Simpler and 250 h Deeper (plus assessments). Deeper is shorter than Simpler because "Deeper" was used to mean "after skipping"; I fixed the semantics. After my rulings: **about 294 h for the full Deeper path (budget 300 h)**, with goal-weighted routes of **about 210 h (Senior Python Backend), 215 h (AI Engineer) and 250 h (hybrid)**. For your profile, expect about 215 h after skip-ahead.
4. **Sandbox needs were scattered.** Seven syllabi quietly assume capabilities that do not exist yet. I consolidated them into 25 platform capabilities and 7 engineering spikes. The three that matter most: a **shared LLM Lab Harness** (record/replay plus scripted mock model), a **two-session database lab mode**, and the **hidden-test engine** with categories and complexity-aware timeouts.
5. **One human action is blocking:** C4 (offer, compensation, notice period, background verification) needs review by a qualified compensation/tax professional and employment-law counsel before publication. The agent did no live verification and the area is legally sensitive.

**Key rulings (headline).**

- Python 3.14 is the default sandbox runtime (3.13 and 3.15 selectable, free-threaded 3.14t selectable); PostgreSQL 18 and MySQL 8.4 LTS are pinned; MySQL 9.7 only after its LTS status is verified.
- A5 owns rate limiting, idempotency keys, retries and the outbox implementation; A6 owns delivery-semantics theory and design; A2 keeps only in-process async mechanics; A4 owns the `SKIP LOCKED` primitive; A2 owns the queue-versus-DB decision.
- A8 owns contract testing, OWASP API depth and the compliance primer; B5 owns prompt injection, guardrails, LLM-as-judge and LLM observability; B3 owns RAG metric definitions while B5 owns general evaluation methodology.
- A6 owns the canvas vocabulary and the rule-ID registry (v0.1 with amendments) and the voice-platform design; B6 owns voice technology and the latency method.
- C3 owns the seven-step coding loop and LLD/machine-coding rounds; C1 owns the spoken career narrative; C4 owns positioning and negotiation; C2 owns project narrative and the claim ledger (a shared profile entity).
- No GPU, no live replication, no live Kubernetes or cloud, no RabbitMQ or Kafka, no bring-your-own API keys in 1.0. LLM and voice labs run on recorded fixtures; live mode is optional and never gates a badge.
- Two gap-fillers added: networking and Linux triage fundamentals (A7), and streaming-platform essentials (A6 Challenge lesson). Also added: design patterns and LLD (A5, C3) and statistics for evals (B1).

**Weakest courses (to prioritise in wave 2).** C4 Offer and Career Positioning (13 of 25: currency and legal risk, thinner depth, merged modules), then B4 Agents, B5 Evaluation and Safety and B7 Fine-tuning (15 each: volatile content, unverified framework claims, fewer seed questions, learner-fit absent), and A5, A8, B2, B3, A1, B6 (16 each: A5 and A8 for overlap, B2/B3/A1/B5 for no learner-fit layer at all). Strongest: C2 Project Deep-Dive (20), A3 DSA, A4 Databases and B1 ML Foundations (19 each).

**Decisions I need from you (short list).** (1) Confirm the 300 h Deeper budget and the three route targets. (2) Approve external professional reviewers for C4 (compensation/tax and employment law). (3) Approve the Python 3.14 default and the no-GPU, no-live-cloud, no-BYO-key stance for 1.0. (4) Confirm 20 system-design library items for the closed beta and 26 for public launch. (5) Confirm that your real projects stay private learner data and are never seeded into shared content.

---

## 2. Audit scores

Scale 1 to 5 where 5 is best. **(a)** technical accuracy and currency risk (5 = accurate and low drift risk, 1 = high risk of being wrong or stale). **(b)** depth for a senior audience. **(c)** learner-fit layer present and strong (template section 6). **(d)** question-bank quality (seed questions, rubric fields, coverage of senior probes, readiness for the 33 per module target). **(e)** uniqueness versus overlap (5 = clean boundaries, 1 = heavily duplicated). Scores reflect the syllabi as submitted, before wave 2.

| Course | (a) Accuracy/currency | (b) Senior depth | (c) Learner-fit | (d) Question bank | (e) Uniqueness | Total /25 |
|---|---|---|---|---|---|---|
| A1 Python Internals | 4 | 4 | 1 | 4 | 3 | 16 |
| A2 Concurrency and Async | 4 | 5 | 2 | 4 | 3 | 18 |
| A3 DSA | 5 | 4 | 2 | 4 | 4 | 19 |
| A4 Databases | 4 | 5 | 2 | 4 | 4 | 19 |
| A5 API and Backend Architecture | 4 | 4 | 2 | 4 | 2 | 16 |
| A6 System Design | 4 | 5 | 2 | 4 | 3 | 18 |
| A7 Cloud DevOps Observability | 3 | 4 | 3 | 4 | 3 | 17 |
| A8 Testing Quality Security | 4 | 4 | 2 | 4 | 2 | 16 |
| B1 ML and Data Foundations | 4 | 4 | 3 | 4 | 4 | 19 |
| B2 LLM Application Engineering | 4 | 4 | 1 | 4 | 3 | 16 |
| B3 RAG and Vector Search | 3 | 5 | 1 | 4 | 3 | 16 |
| B4 Agents and Workflows | 3 | 4 | 2 | 3 | 3 | 15 |
| B5 Eval Safety LLMOps | 3 | 4 | 1 | 4 | 3 | 15 |
| B6 Real-time and Voice AI | 3 | 4 | 2 | 4 | 3 | 16 |
| B7 Finetuning and MLOps Basics | 3 | 4 | 2 | 3 | 3 | 15 |
| C1 Behavioural and Leadership | 4 | 4 | 3 | 4 | 3 | 18 |
| C2 Project Deep-Dive | 5 | 4 | 3 | 4 | 4 | 20 |
| C3 Coding Interview Execution | 4 | 4 | 2 | 4 | 3 | 17 |
| C4 Offer and Career Positioning | 2 | 3 | 2 | 3 | 3 | 13 |
| **Average** | **3.6** | **4.1** | **2.0** | **3.8** | **3.1** | **16.7** |

**Scoring rubric used for (c):** 1 = only a placement-skip rule; 2 = skip rule plus one or two other elements (for example interview-weighted note, never-skip rationale, personalised audience statement); 3 = three or more elements of the eleven (diagnostics, level paths, goal tags, experience bridge, senior bar, interview-weighted time, adaptive hooks, applied labs, rehearsal link, readiness criterion, quality bar). No course reached 4 because none has L1-L4 paths or MUST/SHOULD/NICE tags.

### Why each score (evidence)

| Course | Notes |
|---|---|
| A1 | Reviewed and corrected once (REV-A1); 11 modules, 33 seeds; strongest on topic coverage after review; 24 `[verify]` items on 3.14/3.15 facts, several only from secondary sources; GIL decision guide, logging and container internals duplicated elsewhere; (c)=1 because only a placement-skip rule exists. |
| A2 | Excellent senior depth (cancellation, backpressure, pool budgets, incident-style debugging); (e) hurt by rate limiting, retry, outbox, idempotency and generic GIL content that others own; IDE note says 3.13 default (conflicts with 3.14). |
| A3 | Patterns-first design, original problem archetypes, complexity-aware hidden tests; low currency risk; 36 h Simpler is heavy; communication protocol (nine steps) conflicts with C3's seven steps; (c)=2 for the placement-set skip rule and senior calibration principle. |
| A4 | Best-corrected syllabus (engine versions, per-engine MVCC, online DDL); 22 h Deeper; lesson IDs `L1`..`L5` collide with learner levels; two-session and replication labs need platform support; MySQL 9.7 and labelling unresolved. |
| A5 | Strong on "why and trade-offs"; (e)=2: M7 duplicates A8.M7 (OWASP API Top 10), M2 contract testing duplicates A8.M3, M4 and M8 overlap A2/A6, M5 overlaps A6.M2; 15 `[verify]`. |
| A6 | Best artefact in the set: typed-graph vocabulary, rubric D1-D11 and a 36-rule catalogue that anchors the evaluator; 20 designs (scope asks 25+); voice and RAG designs overlap B-track; rule IDs must become a governed registry. |
| A7 | Only course with per-module SKIP-AHEAD/GAP flags and diagnostics (best partial learner-fit); (a)=3 because Kubernetes, Lambda, Terraform and OpenTelemetry facts and the emulator choice are volatile; no networking/Linux fundamentals; LLM observability depth duplicates B5. |
| A8 | Wide and practical (pytest, mocking, mutation, OWASP, threat modelling, load testing); (e)=2 for OWASP overlap with A5, supply chain overlap with A7, LLM security overlap with B5; vulnerable lab needs legal/security review. |
| B1 | Good learner-aware framing (backend-engineer bridges, L1-L2 ML assumption, diagnostics); owns tokenisation and transformers; embeddings/ANN boundary with B3 needed; no stats for evals; library pins (3.12/3.13) inconsistent. |
| B2 | Reviewed once; 12 modules; reasoning models and caching covered; (c)=1; M7, M10, M11, M12 overlap B3, B5, A6; provider-neutral approach was open. |
| B3 | Deepest RAG syllabus (hybrid, rerank, ACLs, pgvector); 21 `[verify]` (pgvector limits, BM25, services); (c)=1; evaluation overlap with B5; fixture-first sandbox needed. |
| B4 | Good loop-first, framework-neutral approach and durable-execution focus; **explicitly not verified** (frameworks, MCP revision, token-multiplication figures, "lethal trifecta" attribution); only 27 seeds; judge/red-team overlap with B5. |
| B5 | Complete eval, safety and LLMOps; regulatory items (EU AI Act dates, DPDP rules) volatile; (c)=1; owns more after rulings, so needs the shared harness and calibration datasets. |
| B6 | Turns lived experience into structured knowledge; harness-only labs; (a)=3 for vendor/model/regulation churn; M11 contains unreleased Interview Master design and duplicates A6 #17. |
| B7 | Right concept depth for interviews; CPU-only constraint; overlap with B5 (M5, M9), B3 and B2 on decision frameworks; 27 seeds; (c)=2 only through never-skip rationale. |
| C1 | Excellent rubric anchors (L1-L4), worked 11-year career example, truthfulness stance; narrative duplicated in C4 and C2; coding bucket mismatch; HR content overlaps C4. |
| C2 | Most distinctive course (claim ledger, 5-level why, archetypes, claim-risk); low drift risk; depends on canvas free-form mode and the shared ledger; calibration protocol needed. |
| C3 | Clear execution loop and scoring table tied to Interview Master; idioms overlap A3 and A1; LLD/machine-coding rounds missing (common in Indian-product loops); hidden-test display needs IDE decisions. |
| C4 | Ground rules are exemplary (no invented numbers, truthfulness) but the agent did no live verification and 14 statutory/platform items remain `[verify]`; positioning content overlaps C1; thinnest depth in Track C; legal review is required. |

---

## 3. Cross-cutting findings

Severity: **High** blocks consistent delivery, **Medium** causes rework, **Low** polish.

| # | Severity | Finding | Resolution (see decisions doc) |
|---|---|---|---|
| F1 | High | Learner-fit layer absent in all 19 syllabi | G-11 Learner-Fit Appendix mandatory; wave 2 |
| F2 | High | Effort semantics inconsistent; totals 368/250 h unbudgeted; "Deeper shorter than Simpler" unexplained | G-2, section 5: budget about 300 h, route targets 210/215/250 h |
| F3 | High | 20+ topics duplicated across courses | Ownership matrix O-1..O-111, pointer rule G-1 |
| F4 | High | Sandbox needs scattered and unspecified (two-session DB, mock-LLM harness, AWS emulation, K8s, GPU, voice, vulnerable lab, hidden-test display) | Platform capabilities P-01..P-25, spikes S-1..S-7 |
| F5 | High | C4 compensation, tax and legal content unverified and legally sensitive | R-C4-3 human gate; no statutory engine in 1.0 |
| F6 | Medium | Version baseline inconsistent (Python 3.12/3.13/3.14; MySQL 8.4 vs 9.7; PostgreSQL 18 vs 19 beta) | G-8 pins |
| F7 | Medium | Competency tag namespace inconsistent (`security.` vs `sec.`, underscores in A8, duplicate `hr.*` in C1/C4, `sd.`/`system-design.` vs `design.`) | G-9 registry and normalisations |
| F8 | Medium | Thresholds inconsistent (75/80/85 percent; voice 3.0 vs 3.5 vs L3) | G-6, G-7: Meets 3.0, Senior bar 3.5 |
| F9 | Medium | Lesson ID `L1`..`L5` in A4 collides with learner levels L1-L4 | Lesson IDs `.n`; levels reserved |
| F10 | Medium | Question banks: 27-36 seeds per course vs the template's 33 items per module (about 6,400 items across 193 modules not yet authored); seed formats vary; no goal tags | G-10: IDs, owner/`ref`, goal tags; authoring plan below |
| F11 | Medium | Canvas vocabulary and rule IDs lived inside A6 only; other courses want canvas tasks | Section 7 of decisions doc: governed registry, v0.1 amendments, six new rules |
| F12 | Medium | Coverage gaps: LLD/machine-coding and design patterns; networking/OS fundamentals; streaming-platform essentials; statistics for evals; compliance primer | O-101, O-110, O-111, B1.M10 lesson, X1 |
| F13 | Medium | Track C coding bucket (C1, C2, C4) not meaningful | G-10 relabel to applied artefact tasks |
| F14 | Medium | Mastery of fast-moving AI content depends on unverified vendor/framework facts (B4, B5, B6, B7, B2, B3) | Tier language, provider notes, verify checklist, 3-month cadence |
| F15 | Low | A3 problem ladder and A6 design library volumes make "Core vs Extended" tagging necessary for SHOULD/NICE routes | Instructions in A3 and A6 upgrade items |
| F16 | Low | C3 section 7 and C1 section 4 define Interview Master scoring inside course files | Treat as source for the platform rubric registry (P-21); courses keep dimension names, thresholds in admin |
| F17 | Low | Some syllabi name specific vendors/tools as defaults (LocalStack, Pipecat, LangGraph-like frameworks) | Dated examples only; engineering spikes decide |

### Overlap resolution at a glance (examples of what moved)

| Topic | Was in | Now owned by |
|---|---|---|
| Rate-limit algorithms, Redis token bucket | A2.M6, A5.M4, A6.M4 | A5.M4 (A6 owns the service design) |
| Idempotency keys, retries, circuit breaker | A2.M6, A2.M9, A5.M4, A6.M4, A7.M10 | A5.M4 (shared lesson X2) |
| Outbox | A2.M9, A5.M8, A6.M4 | A5.M8 implementation, A6.M4 theory |
| `SKIP LOCKED` queues | A4.M6, A2.M9 | A4.M6 primitive, A2.M9 decision |
| OWASP API Top 10, SSRF, BOLA | A5.M7, A8.M7 | A8.M7 |
| Contract testing | A5.M2, A8.M3 | A8.M3 |
| Prompt injection, guardrails, OWASP LLM | B2.M10, B4.M9, B5.M4-M6, A8.M9 | B5 (B4.M9 keeps agent tool permissions) |
| LLM-as-judge | B3.M8, B4.M8, B5.M3 | B5.M3 |
| RAG metrics versus eval methodology | B3.M8, B5.M1-M3 | B3.M8 definitions, B5 methodology |
| Voice platform design | A6 #17, B6.M11 | A6 #17 |
| Coding communication protocol | A3.M1, C1, C3.M1-M2 | C3.M1 (seven steps) |
| Career narrative | C1.M9, C2.M2, C4.M2-M3 | C1.M9 spoken, C4 thesis, C2 project |
| Embeddings and tokenisation | B1.M8, B2.M1, B3.M3 | B1.M8 concepts, B3.M3 selection |
| Fine-tune versus RAG versus prompt | B2.M7, B3.M1, B7.M1 | B7.M1 (B3.M1 owns RAG vs long context) |
| Compliance (DPDP, GDPR) | A8.M9, B3.M7, B5.M9, B6.M9, B7.M6, C4 | X1 in A8.M9.4; B5 owns AI governance |
| Observability for LLM and voice | A7.M9, B4.M8, B5.M7, B6 | B5.M7 (LLM), B6 (voice), A7.M9 (generic) |

---

## 4. Per-course findings (summary; actionable items are in the decisions doc section 8)

| Course | Top findings | Biggest upgrade lever |
|---|---|---|
| A1 | No learner-fit; GIL guide and logging overlap; 3.15 facts partly secondary-sourced; effort semantics | Appendix with L4 skip bands for decorators/context managers; verify What's New; pointers to A2/A7 |
| A2 | Overlap on rate limiting, retry, outbox; IDE default conflict; infra assumptions (Celery/RabbitMQ) | Narrow M6/M9, adopt P-13, add incident-style scenarios |
| A3 | Nine-step protocol conflicts with C3; heavy effort; strict-library variant undecided | Seven-step alignment, Core vs Extended, strict from-scratch items, calibrated time bands |
| A4 | Lesson-ID collision; labs need P-05/P-06; MySQL labelling unresolved | Version pins, simulators for replication/PITR, owner of keyset SQL and RLS data model |
| A5 | M7 and M2 overlap A8; M4/M8 overlap A2/A6; missing design patterns | Shrink M7, make M4/M8 the canonical owners, add patterns lesson and `lld` seeds |
| A6 | Rule registry governance; library 20 vs 25+; voice/RAG overlap | Registry with permanent IDs, six new rules, 26-design plan, Kafka essentials, cost card |
| A7 | Emulator uncertainty; LLM observability overlap; no networking fundamentals | P-11 simulators, networking and Linux triage lesson, pointer to B5/B6 |
| A8 | OWASP overlap with A5; compliance primer home; lab review | Own contract testing, OWASP depth, X1; pointer-only LLM security; S-6 gate |
| B1 | Embeddings/ANN boundary; no statistics lesson; pins | Own tokenisation, transformers/KV formula; add stats lesson; platform pins |
| B2 | No learner-fit; overlaps in M7, M10, M11, M12; provider approach | `LLMClient` adapter, trims, owner of caching/streaming/cost template |
| B3 | No learner-fit; eval overlap; fixtures | Own RAG metrics, fixtures default, canvas capstone with registry rules |
| B4 | Unverified frameworks and figures; low seed count; judge/red-team overlap | Verify or delete claims; code-reading framework lab; crash-injection lab; more probes |
| B5 | No learner-fit; owns most depth now; datasets owner | Harness orientation (X5), calibration set specs, statistics link, governance only in M9 |
| B6 | M11 duplication and unreleased details; vendor drift; harness-only | Latency workbook, Challenge WebRTC lesson, voice compliance awareness, defend-your-numbers mode |
| B7 | Overlap with B5/B3/B2; CPU-only; 27 seeds | Own decision framework, tuned-model eval, serving/GPU arithmetic via calculators, fixture dry-run |
| C1 | Narrative overlap; cap rule; coding bucket | Own spoken narrative, cap rule, principle-title policy, readiness criterion |
| C2 | Dependency on canvas mode and ledger; calibration | Ledger-based workshop, consistency rubric, qualitative claim-risk until calibrated |
| C3 | Idiom overlap; LLD gap; hidden-test display | Own seven-step loop, add LLD lesson, adopt display policy, trim M3 |
| C4 | Currency and legal risk; overlap; thin depth | Merge M2/M3, professional review gate, remove duplicated HR answers, date or flag every number |

---

## 5. Integration of the three reviews

| Review | Status | Boss action |
|---|---|---|
| REV-A1 | A1 rewritten to the brief; 5 unresolved questions | All five ruled (R-A1-1..5, R-A1-6): container internals owner A1, Python 3.14 default with 3.14t, A2 owns the concurrency guide, logging split A1/A7, 11 modules accepted |
| REV-A4 | A4 rewritten (v1.1); 5 unresolved questions | All five ruled (R-A4-1..5): MySQL 8.4 LTS primary, PostgreSQL 18, two-session mandatory, replication and PITR simulated, boundaries fixed, effort 30/22 approved, skip scope limited |
| REV-B2 | B2 rewritten (v1.1); 4 unresolved questions | All four ruled (R-B2-1..4) plus R-B2-5 on trimming to 16/11.5 h; provider adapter, B3/B4/B5 ownership, recorded fixtures only for graded work |

The three reviews raised the structure of the syllabi to the brief but predate template section 6. Their `[verify]` items are included below.

### Method note and limits of this audit

I read every file's header, module table, final assessment, risks and open questions, grep-audited each file for learner-fit elements, `[verify]` markers and seed-question structure, and read the three review logs, the template and the scope document. I read a sample of per-module blocks and seed questions in several courses but **not every seed question in full**; question-level accuracy (answer keys) still needs the SME review gate. I did not run web verification (the checklist below is for humans or wave-2 agents). Scores are judgement against the stated rubric and should be recalibrated after the 3-learner pilot.

---

## 6. `[verify]` checklist (grouped by course)

Legend: **W** = verifiable on the web from official sources; **H** = needs a human or professional; **E** = needs an engineering decision or spike; **P** = needs product/legal.

### A1 Python Internals
- [ ] W: Python 3.15 final release date and exact patch version; 3.10 and 3.9 EOL dates (python.org status page)
- [ ] W: Free-threaded status in 3.15 (PEP 703/779 phase III) and default-build status
- [ ] W: JIT status and gains in 3.14/3.15 (PEP 744); tail-call interpreter status
- [ ] W: Incremental GC behaviour in 3.14.x and 3.15
- [ ] W: Deferred annotation evaluation in 3.14 (PEP 649/749) and effect on decorators
- [ ] W: PEP 758 (except without parentheses) in 3.14
- [ ] W: PEP 810 lazy imports final status in 3.15; built-in `frozendict` in 3.15; built-in sampling profiler name and status
- [ ] W: PEP 751 lockfile (`pylock.toml`) adoption; tooling landscape (pip, uv, Poetry, pip-tools)
- [ ] W: Memory profiling tools currency (tracemalloc, objgraph, memray); vectorisation tool preferences (NumPy, pandas, Polars)
- [ ] W: Single-thread slowdown figure for free-threaded builds per version

### A2 Concurrency and Async
- [ ] E: Free-threaded 3.14t availability in the sandbox image
- [ ] W: Linux default multiprocessing start method change in 3.14 (forkserver)
- [ ] W: asyncio loop API and child-watcher changes in 3.14; `python -m asyncio ps/pstree` introspection CLI
- [ ] W: AnyIO default threadpool limiter (40) and FastAPI behaviour
- [ ] W: Arq, Dramatiq, Taskiq maintenance status
- [ ] W: pytest-asyncio loop-scope configuration options (0.2x to 1.x)
- [ ] W: Celery 5.x, FastAPI/Starlette, SQLAlchemy 2.x async pinned versions

### A3 DSA
- [ ] W/E: `heapq` max-heap helper functions and version availability in the pinned Python
- [ ] E: Pinned-Python support for `itertools.batched`, `bisect` key, `int.bit_count`, `graphlib`
- [ ] W: Current public guidance on AI-tool policies in coding rounds
- [ ] E: Operation-count proxies and runtime calibration per sandbox hardware

### A4 Databases
- [ ] W: PostgreSQL 19 GA status and feature list; PostgreSQL 18 facts (skip scan scope, `uuidv7()`, virtual generated columns, temporal-constraint syntax, async I/O, checksum default)
- [ ] W: MySQL 9.7 designation (LTS or innovation), version labels ("26.x"), 8.0 EOL date (April 2026)
- [ ] W: PgBouncer protocol-level prepared-statement support version
- [ ] W: MySQL skip-scan, row-value index use, hash join coverage, `INSTANT` DDL limits, `VECTOR` type maturity
- [ ] W: `pg_upgrade` statistics retention behaviour

### A5 API and Backend Architecture
- [ ] W: IETF RateLimit header and Idempotency-Key drafts status; DPoP adoption; OAuth 2.1 status
- [ ] W: OWASP API Security Top 10 edition (2023 vs newer)
- [ ] W: FastAPI, Pydantic v2, Django 5.2 LTS and 6.x, Flask 3.x, SQLAlchemy 2.x versions; Django async ORM coverage
- [ ] W: Redis licensing and Valkey position; Redlock critique wording
- [ ] W: Maintenance of Celery, RQ, Arq, Dramatiq; GraphQL Python libraries (Strawberry, Ariadne, Graphene)
- [ ] W: Policy engines (OPA, Cedar, OpenFGA) current status; OpenAPI 3.1 vs later minor versions

### A6 System Design
- [ ] W: Kafka-style platform version claims (coordination-dependency removal, queue features) and exactly-once scope per platform
- [ ] W: MQTT 3.1.1 vs 5 platform support; managed IoT broker limits
- [ ] W: Redis licensing/forks wording
- [ ] W: "How far does one Postgres go" and other single-node limit numbers (re-state as orders of magnitude)
- [ ] H: Voice p95 latency target wording (provider-dependent)
- [ ] E: Canvas vocabulary v0.1 and rule IDs accepted by the canvas spec

### A7 Cloud, DevOps and Observability
- [ ] E/P: AWS emulator licensing and feature tier (LocalStack-style vs moto-style) (spike S-1)
- [ ] W: ingress-nginx retirement status and Gateway API migration path
- [ ] W: Lambda supported Python runtimes, init-phase billing, SnapStart for Python, 15-minute and payload limits
- [ ] W: App Runner status; ElastiCache Valkey option; X-Ray and OTel direction; RDS Multi-AZ cluster options
- [ ] W: Terraform native S3 state locking vs DynamoDB; OpenTofu licensing context; `terraform test` mocks; Terragrunt/Atlantis/Terraform Cloud licensing
- [ ] W: OpenTelemetry signal stability (logs, metrics, traces, profiles) and Python instrumentation coverage
- [ ] W: GitHub Actions and Bitbucket Pipelines features and pricing; SLSA level definitions; Docker Scout and scanner tool names
- [ ] W: gVisor/Firecracker wording for isolation; Azure and GCP service names

### A8 Testing, Quality and Security
- [ ] W: OWASP API Top 10 (2023 vs newer), OWASP Web Top 10 2025 status, OWASP Top 10 for LLM Applications current edition
- [ ] W: pytest-asyncio 1.x semantics; pytest 8.x/9.x changes
- [ ] W: mutmut and Cosmic Ray maintenance; Locust, k6, wrk/hey versions
- [ ] W: ty and pyrefly maturity; PyPI attestations and trusted publishing status
- [ ] H/P: DPDP Act rules status and commencement; GDPR basics wording
- [ ] E/P: Lab Service legal and security review (spike S-6)

### B1 ML and Data Foundations
- [ ] W: NumPy 2.x, pandas 2.x/3.x copy-on-write defaults, scikit-learn 1.x, PyTorch 2.x, XGBoost/LightGBM/CatBoost versions
- [ ] W: CatBoost/LightGBM native categorical handling; Polars/DuckDB positioning
- [ ] W: Experiment-tracking and data-validation tools (MLflow, W&B, Great Expectations, Pandera, Evidently)
- [ ] W: Adam-memory and compute rule-of-thumb multipliers (state assumptions or remove)
- [ ] W: Multilingual token cost ratios (generate live; do not hard-code); "lost in the middle" evidence
- [ ] E/P: Offline tokeniser availability and licence

### B2 LLM Application Engineering
- [ ] W: Prompt caching: TTL options, minimum lengths, read/write pricing semantics per provider
- [ ] W: Batch API discount (about 50 percent claim) and turnaround
- [ ] W: Structured-output supported schema subsets per provider; strict tool use
- [ ] W: Reasoning controls (effort levels, thinking budgets, reasoning-state handling across tool turns)
- [ ] W: Context-management features (editing, compaction); provider data-use and zero-retention options
- [ ] W: Media tokenisation rules per provider for multimodal inputs

### B3 RAG and Vector Search
- [ ] W: pgvector current release, `halfvec`/`sparsevec`/binary quantisation, index dimension limits, iterative scans, defaults (m 16, ef_construction 64, ef_search 40)
- [ ] W: BM25 options in Postgres and managed-Postgres support; pgvectorscale/DiskANN-style options
- [ ] W: Managed services features (Bedrock Knowledge Bases, Azure AI Search, Vertex AI Search)
- [ ] W: Provider citation features; rerank latency ranges; embedding and rerank prices (use configurable calculator inputs)
- [ ] W: OCR and layout-aware parser choices; RAPTOR-style naming; evaluation tool APIs (Ragas, DeepEval, TruLens, Phoenix, LangSmith, Langfuse)
- [ ] H/P: DPDP and GDPR erasure and residency statements reviewed by counsel

### B4 Agents and Workflows
- [ ] W: MCP specification revision, transports, authorisation model; A2A status
- [ ] W: Frameworks status (LangGraph, LlamaIndex, CrewAI, AutoGen/Microsoft Agent Framework, Pydantic AI, Google ADK, OpenAI Agents SDK, Claude Agent SDK)
- [ ] W: Tool-search/dynamic tool-loading provider support; extended thinking and effort controls
- [ ] W: OpenTelemetry GenAI semantic conventions stability; tracing tools (LangSmith, Langfuse)
- [ ] W: Durable-execution engine capabilities (Temporal-style)
- [ ] W: Multi-agent token-multiplication figures (source or remove); "lethal trifecta" attribution
- [ ] W: Prompt caching semantics and pricing for stable prefixes

### B5 LLM Evaluation, Safety and LLMOps
- [ ] W: OWASP Top 10 for LLM Applications edition and numbering; agent-specific risk guidance and MCP server trust
- [ ] W: EU AI Act application dates and amendment or delay proposals; NIST AI RMF; ISO/IEC 42001
- [ ] H/P: DPDP Rules phasing, data fiduciary notions, sector regulator guidance (RBI, SEBI, IRDAI)
- [ ] W: PII tooling state (Presidio-style); moderation endpoints; Llama Guard family versions; NeMo Guardrails and Guardrails AI maintenance
- [ ] W: Tracing tool landscape (Langfuse, LangSmith, Phoenix, Helicone, Weave) and OTel GenAI conventions
- [ ] W: Provider zero-retention and regional processing terms; batch discounts
- [ ] W: Drift-detection suitability claims (PSI/KL on embeddings); local GitHub Actions runner approach in the sandbox
- [ ] H: India PII format specifics (Aadhaar, PAN, GSTIN-like patterns) handling rules

### B6 Real-time and Voice AI Systems
- [ ] W: Provider/model/framework names and versions (Pipecat, LiveKit Agents); speech-to-speech pricing; Indic-language provider leaders
- [ ] W: Browser support for WebTransport/QUIC; Safari/iOS audio and AudioWorklet quirks
- [ ] W: G.711 regional variants and Indian carrier/CPaaS details
- [ ] H/P: TRAI call-recording, telemarketing and DLT rules; DPDP consent for audio; provider data-processing and zero-retention terms
- [ ] E: Indian-English technical speech fixtures consent and licence (spike S-4)

### B7 Fine-tuning, Model Selection and MLOps Basics
- [ ] W: Hugging Face PEFT/TRL APIs and versions; preference-method landscape (DPO, IPO, KTO, ORPO and newer)
- [ ] W: vLLM and alternatives (TGI, TensorRT-LLM, SGLang, llama.cpp) feature sets; chunked prefill
- [ ] W: Quantisation formats and GPU support (FP8/FP4); GPU availability and pricing in Indian regions
- [ ] W: Open-weight licence terms; provider terms on training from outputs and fine-tuning data retention
- [ ] H/P: DPDP and sector regulator guidance on AI/model risk
- [ ] W: Experiment-tracking tools (MLflow, W&B) category claims

### C1 Behavioural and Leadership
- [ ] P: Legal/policy statements on compensation disclosure and background verification (HR/legal review)
- [ ] W: Any named company's published principles before quoting titles (primary source links)
- [ ] H: Notice-period buyout, counter-offer and hybrid/relocation norms (market-practice review every 6 months)

### C2 Project Deep-Dive and Resume Defence
- [ ] E/H: Claim-risk score weights and thresholds (calibration with at least 30 SME-labelled transcripts)
- [ ] W: MQTT QoS semantics and MQTT 5 features re-checked against the specification (archetype ARC-3)

### C3 Coding Interview Execution
- [ ] W: AI-assistant policies in interviews (principles only; no company table); shared-editor vendor features and proctoring signals
- [ ] W: Python version features used in interviews (`match`, `itertools.batched`, free-threaded status)
- [ ] W: MySQL 5.7 EOL date and relevance of no-window-function question
- [ ] E: Hidden-test category catch rate availability in the IDE

### C4 Offer and Career Positioning (all require professional sign-off)
- [ ] H/P: EPF employer share, gratuity eligibility, labour-code transition, CTC definitions
- [ ] H/P: Tax regime choice, TDS, ESOP/RSU taxation; non-compete, non-solicit, bond and training-cost recovery enforceability; moonlighting clauses
- [ ] H/P: Notice-period norms and buyout; legality of disclosing or declining current pay; offer-letter clauses
- [ ] W/H: Job portals (Naukri, LinkedIn Jobs, Indeed) and LinkedIn feature claims; recruiter payment models; level maps across companies (approximate and sourced)
- [ ] H: Named external reviewers appointed (compensation/tax and employment law) before M6-M10 publish

---

## 7. Wave-2 plan and risks

### Recommended order of wave 2 (so dependencies resolve first)

| Batch | Courses | Why first |
|---|---|---|
| 1 | A6, A5, A8, A4 | Own the canvas registry, the reliability/security/data boundaries that most others point to |
| 2 | A2, A1, A7, A3, C3 | Python and ops boundaries, seven-step loop, LLD lesson |
| 3 | B2, B5, B3, B4, B1 | LLM boundaries and the shared harness conventions |
| 4 | B6, B7, C1, C2, C4 | Depend on A6 #17, B2/B5, C1 narrative ownership; C4 last because of the professional-review gate |

Run each batch with reviewers (human senior SME per course) in parallel with batch N+1 drafting. Pilot with at least 3 learners per course after wave 2 (quality gate 5).

### Authoring-load note

193 modules at the template's 33 items per module is about 6,400 bank items. Seeds cover about 580 verbal items. Recommend a **staged bank**: wave 3 delivers per module 12 MCQ, 4 code-output, 2 coding, 5 verbal, 2 scenario (about 65 percent) for the closed beta, with full targets before public launch; prioritise modules tagged MUST for the hybrid route and the placement diagnostics (5-8 per module, about 1,000 items, a subset of the bank).

### Top risks

| Risk | Impact | Mitigation |
|---|---|---|
| Shared harnesses (LLM Lab Harness, two-session DB, hidden-test engine) slip | B2-B6, A4, A3, C3 labs cannot launch | Schedule spikes S-2, S-3, S-5 before wave 3; fallback to guided step-throughs |
| AI-content drift between authoring and launch (B2-B7, A6.M8) | Wrong or stale claims at launch | Tier language, provider notes, dated data file, quarterly review, verify checklist |
| C4 legal exposure | Harm to learners and the company | Professional reviewers; no statutory calculators; opt-in pay data; disclaimers |
| Learner-fit layer adds authoring volume | Wave 2 bloats files | Table-driven appendix (G-11), 250-line cap, 20 percent net growth cap |
| Evaluator-vs-SME agreement not met | Canvas and behavioural scoring not credible | Labelled sets (30 designs, 30 transcripts), qualitative feedback until calibrated |
| Overstuffed Deeper path for the reference learner | Burnout, slipped timeline | Goal-weighted routes, adaptive skip-ahead, budget 300 h with route targets |
| Lab Service security | Sandbox escape or misuse | Isolation (P-24), legal/security review (S-6), static-analysis fallback |

### What is confirmed good and should not be changed

A6's rubric (D1-D11) and rule catalogue structure, C1's L1-L4 rubric anchors, C2's honesty-as-criterion stance, A4's per-engine isolation coverage, A2's incident-style framing, B1's backend-engineer bridges, C4's ground rules on no invented numbers, and the consistent seed-question format (ideal outline, weak answers, probes, difficulty, competency, cluster).
