---
name: Skillora Competency Tag Registry
version: '2.0'
status: 'Final candidate — pending boss approval and human SME review'
date: 2026-10-05
last_reviewed: 2026-10-05
integration_pass: INT-A 2026-10-05
---

# Competency Tag Registry (Release 1.0)

Single master list of every competency tag used in the 19 syllabi (A1-A8, B1-B7, C1-C4). Built by INT-A from the course headers after the integration pass. Boss ruling G-9 applies.

## Conventions

1. Format: `<domain>.<topic>`, lowercase, hyphen-separated words, exactly one dot (no third segment, no underscores).
2. One owner per tag. Only the owner issues credit. Any other course may attach the tag as **secondary (no credit)**.
3. Goal tags are a separate scheme: `B:x A:x H:x` (B = Senior Python Backend, A = AI Engineer, H = hybrid; x = M, S or N), G-3.
4. Pointer courses never invent a tag. If a topic has no owner tag here, it is mapped to the closest owned tag and the gap is logged in the Integration Consistency Report.

## Normalisation map (legacy spelling to registry tag)

| Legacy or invalid spelling | Registry tag | Where it was found |
|---|---|---|
| `security.api` | `sec.api` | A5 |
| `sd.ai-feature`, `system-design.ai-systems` | `design.ai-systems` | A6, B5 |
| `proj.narrative` | `project.narrative` | C1 |
| `career.positioning` | `career.positioning-thesis` | C1 |
| `compliance.x1` | `sec.compliance` | C4 |
| `hr.why-switch` | `hr.switch-reason` | C4 |
| `sec.llm-app`, `sec.llm-top10`, `sec.llm-injection` | `llm.security` | A8, B2, B4 |
| `eval.methodology`, `llm.evaluation-method` | `llm.eval-design` | B2, B3 |
| `eval.llm-judge` | `llm.judge` | B4 |
| `rag.decision` | `rag.architecture` | B2 |
| `agents.mcp` | `agents.protocols-mcp` | B2 |
| `cloud.aws.compute`, `.data`, `.messaging`, `.security` | `cloud.aws-compute`, `cloud.aws-data`, `cloud.aws-messaging`, `cloud.aws-security` | A7 |
| `python.gil` | `python.concurrency-basics` | A2 |
| `dsa.communication` | `coding.communication` | A3 |
| `dsa.coding-fluency` | `dsa.arrays-hashing` | A3 |
| `dsa.graphs-trees` | `dsa.graphs` | A3 |
| `api.rate-limit` | `api.reliability` | A6 |
| `api.pagination` | `api.design` | A4 |
| `design.data-partitioning` | `design.scaling` | A4 |
| `project.defence` | `project.tradeoffs` | B6 |
| `llm.tokenisation-pointer` | `llm.fundamentals` | B1 |

## Registry

### A1

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `python.concurrency-basics` | A1 | A2 |
| `python.core` | A1 | none |
| `python.exceptions-logging` | A1 | none |
| `python.import-packaging` | A1 | none |
| `python.memory` | A1 | none |
| `python.modern` | A1 | none |
| `python.oop-datamodel` | A1 | none |
| `python.performance` | A1 | none |
| `python.serialization` | A1 | none |

### A2

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `api.fastapi-async` | A2 | A1 |
| `concurrency.background-jobs` | A2 | none |
| `concurrency.backpressure` | A2 | none |
| `concurrency.event-loop` | A2 | none |
| `concurrency.model-selection` | A2 | none |
| `concurrency.structured` | A2 | none |
| `concurrency.sync-primitives` | A2 | none |
| `concurrency.testing-debugging` | A2 | none |
| `db.async-drivers` | A2 | none |

### A3

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `dsa.applied-reasoning` | A3 | none |
| `dsa.arrays-hashing` | A3 | none |
| `dsa.backtracking` | A3 | none |
| `dsa.binary-search` | A3 | none |
| `dsa.bits-design` | A3 | none |
| `dsa.complexity` | A3 | none |
| `dsa.dp` | A3 | none |
| `dsa.graphs` | A3 | none |
| `dsa.greedy-intervals` | A3 | none |
| `dsa.linked-lists-trees` | A3 | none |
| `dsa.pattern-recognition` | A3 | none |
| `dsa.python-idioms` | A3 | none |
| `dsa.stacks-heaps` | A3 | none |
| `dsa.two-pointers-window` | A3 | none |

### A4

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `db.indexing` | A4 | A5 |
| `db.locking` | A4 | none |
| `db.operations` | A4 | none |
| `db.orm` | A4 | A5 |
| `db.scaling` | A4 | none |
| `db.schema` | A4 | none |
| `db.sql` | A4 | A5 |
| `db.transactions` | A4 | none |
| `db.tuning` | A4 | none |

### A5

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `api.authn-authz` | A5 | none |
| `api.caching` | A5 | none |
| `api.design` | A5 | A4 |
| `api.evolution` | A5 | none |
| `api.reliability` | A5 | A6 |
| `backend.architecture` | A5 | none |
| `backend.async-events` | A5 | none |
| `backend.frameworks` | A5 | none |
| `backend.patterns` | A5 | none |
| `backend.production` | A5 | none |

### A6

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `design.ai-systems` | A6 | A7, B4, B5, B6 |
| `design.caching` | A6 | none |
| `design.canvas` | A6 | none |
| `design.classic` | A6 | none |
| `design.consistency` | A6 | none |
| `design.cost` | A6 | none |
| `design.estimation` | A6 | none |
| `design.framework` | A6 | none |
| `design.messaging` | A6 | none |
| `design.observability` | A6 | none |
| `design.probing` | A6 | none |
| `design.reliability` | A6 | none |
| `design.scaling` | A6 | A4 |
| `design.security` | A6 | none |

### A7

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `cloud.aws-compute` | A7 | none |
| `cloud.aws-data` | A7 | none |
| `cloud.aws-messaging` | A7 | none |
| `cloud.aws-security` | A7 | none |
| `cloud.containers` | A7 | none |
| `cloud.cost` | A7 | none |
| `cloud.k8s` | A7 | none |
| `cloud.multicloud` | A7 | none |
| `cloud.networking` | A7 | none |
| `devops.cicd` | A7 | none |
| `devops.deploy` | A7 | none |
| `devops.iac` | A7 | none |
| `ops.incident` | A7 | none |
| `ops.linux-triage` | A7 | none |
| `ops.observability` | A7 | none |
| `ops.slo` | A7 | none |

### A8

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `eng.code-quality` | A8 | none |
| `eng.code-review` | A8 | none |
| `eng.contract-testing` | A8 | none |
| `eng.integration-testing` | A8 | none |
| `eng.pytest` | A8 | none |
| `eng.test-doubles` | A8 | none |
| `eng.test-quality` | A8 | none |
| `eng.test-strategy` | A8 | none |
| `perf.load-testing` | A8 | none |
| `sec.api` | A8 | A5, A6 |
| `sec.authn-authz` | A8 | none |
| `sec.compliance` | A8 | B3, C4 |
| `sec.injection` | A8 | none |
| `sec.secrets` | A8 | A7 |
| `sec.ssrf` | A8 | none |
| `sec.supply-chain` | A8 | A1 |
| `sec.threat-modelling` | A8 | none |

### B1

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `data.numpy-pandas` | B1 | none |
| `ml.classical-models` | B1 | none |
| `ml.data-quality` | B1 | none |
| `ml.deep-learning-basics` | B1 | none |
| `ml.embeddings` | B1 | none |
| `ml.experiment-design` | B1 | B5 |
| `ml.framing` | B1 | none |
| `ml.generalisation` | B1 | none |
| `ml.metrics` | B1 | none |
| `ml.model-choice` | B1 | none |
| `ml.transformers` | B1 | none |

### B2

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `llm.architecture` | B2 | none |
| `llm.context` | B2 | none |
| `llm.fundamentals` | B2 | B1 |
| `llm.model-selection` | B2 | B7 |
| `llm.multimodal` | B2 | none |
| `llm.production` | B2 | none |
| `llm.prompting` | B2 | none |
| `llm.safety-basics` | B2 | none |
| `llm.structured` | B2 | none |
| `llm.tools` | B2 | B4 |

### B3

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `rag.agentic` | B3 | none |
| `rag.architecture` | B3 | B2 |
| `rag.debugging` | B3 | none |
| `rag.embeddings` | B3 | none |
| `rag.evaluation` | B3 | none |
| `rag.grounding` | B3 | none |
| `rag.ingestion` | B3 | none |
| `rag.production` | B3 | none |
| `rag.retrieval` | B3 | none |
| `rag.security` | B3 | none |
| `rag.vector-db` | B3 | none |

### B4

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `agents.architecture-choice` | B4 | none |
| `agents.cost` | B4 | none |
| `agents.eval-observability` | B4 | none |
| `agents.frameworks` | B4 | none |
| `agents.hitl` | B4 | none |
| `agents.loop` | B4 | none |
| `agents.memory-state` | B4 | none |
| `agents.multi-agent` | B4 | none |
| `agents.orchestration` | B4 | none |
| `agents.protocols-mcp` | B4 | B2 |
| `agents.reliability` | B4 | none |
| `agents.security` | B4 | none |
| `agents.tool-design` | B4 | none |

### B5

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `llm.eval-design` | B5 | B2, B3 |
| `llm.governance` | B5 | none |
| `llm.grounding` | B5 | none |
| `llm.guardrails` | B5 | none |
| `llm.incident` | B5 | none |
| `llm.judge` | B5 | B4 |
| `llm.llmops` | B5 | none |
| `llm.metrics` | B5 | none |
| `llm.privacy` | B5 | none |
| `llm.responsible-ai` | B5 | none |
| `llm.security` | B5 | A8, B2, B3, B4 |

### B6

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `voice.architecture` | B6 | none |
| `voice.cost-privacy` | B6 | none |
| `voice.eval` | B6 | none |
| `voice.latency` | B6 | none |
| `voice.reliability` | B6 | none |
| `voice.stt-quality` | B6 | none |
| `voice.telephony` | B6 | none |
| `voice.transport` | B6 | none |
| `voice.tts` | B6 | none |
| `voice.turn-taking` | B6 | none |

### B7

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `llm.eval-tuned` | B7 | none |
| `llm.ft-vs-rag` | B7 | none |
| `llm.model-routing` | B7 | B2 |
| `ml.alignment-distill` | B7 | none |
| `ml.data-prep` | B7 | none |
| `ml.finetune-concepts` | B7 | none |
| `ml.gpu-cost` | B7 | none |
| `ml.serving` | B7 | none |
| `mlops.compliance` | B7 | none |
| `mlops.lifecycle` | B7 | none |

### C1

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `behav.ambiguity` | C1 | none |
| `behav.conflict` | C1 | none |
| `behav.curveball` | C1 | none |
| `behav.failure` | C1 | none |
| `behav.global-teams` | C1 | none |
| `behav.influence` | C1 | none |
| `behav.mentoring` | C1 | none |
| `behav.narrative` | C1 | none |
| `behav.ownership` | C1 | C2 |
| `behav.prioritisation` | C1 | none |
| `behav.quality-ethics` | C1 | none |
| `behav.stakeholder` | C1 | none |
| `behav.structure` | C1 | none |
| `behav.values-fit` | C1 | none |
| `hr.counter-offer` | C1 | none |
| `hr.expectations` | C1 | C4 |
| `hr.gaps` | C1 | none |
| `hr.notice-period` | C1 | C4 |
| `hr.relocation` | C1 | none |
| `hr.switch-reason` | C1 | C4 |

### C2

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `comm.multi-depth` | C2 | none |
| `design.own-architecture` | C2 | none |
| `project.confidentiality` | C2 | none |
| `project.failure` | C2 | none |
| `project.impact` | C2 | none |
| `project.narrative` | C2 | C1 |
| `project.tradeoffs` | C2 | B6 |
| `resume.claim-audit` | C2 | none |
| `resume.tech-probe` | C2 | none |

### C3

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `coding.ai-policy` | C3 | none |
| `coding.code-review` | C3 | none |
| `coding.communication` | C3 | A3 |
| `coding.complexity` | C3 | none |
| `coding.data-manipulation` | C3 | none |
| `coding.debugging` | C3 | none |
| `coding.hints` | C3 | none |
| `coding.live-coding` | C3 | none |
| `coding.lld` | C3 | none |
| `coding.pair` | C3 | none |
| `coding.practical-api` | C3 | none |
| `coding.practical-sql` | C3 | none |
| `coding.problem-solving` | C3 | none |
| `coding.process` | C3 | none |
| `coding.python-idioms` | C3 | none |
| `coding.remote-logistics` | C3 | none |
| `coding.take-home` | C3 | none |
| `coding.testing` | C3 | none |
| `coding.time-management` | C3 | none |
| `coding.trade-offs` | C3 | none |

### C4

| Tag | Owner | Secondary (no credit) |
|---|---|---|
| `career.bgv` | C4 | none |
| `career.domain-switch` | C4 | none |
| `career.positioning-thesis` | C4 | C1 |
| `career.profile` | C4 | none |
| `career.role-targeting` | C4 | none |
| `career.search-strategy` | C4 | none |
| `career.transition` | C4 | none |
| `offer.evaluation` | C4 | none |
| `offer.levels` | C4 | none |
| `offer.negotiation` | C4 | none |
| `offer.structure` | C4 | none |

Total registered tags: 234.

Notes: (a) `sec.compliance` is owned by A8 (X1 primer, A8.M9.4); B3 attaches it as secondary. (b) `mlops.compliance` (B7) covers licence and model-governance literacy only, not the X1 primer. (c) `llm.model-selection` (B2, per-request choice) and `llm.model-routing` (B7, portfolio-level routing and TCO) are distinct tags. (d) `ml.metrics` (B1, classical metrics) and `llm.metrics` (B5, LLM-eval usage) are distinct tags. (e) `hr.*` is owned by C1 (G-9); C4 attaches it as secondary only. (f) `behav.ownership` is owned by C1; C2 attaches it as secondary.
