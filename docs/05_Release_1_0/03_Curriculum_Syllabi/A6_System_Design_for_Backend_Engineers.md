---
name: A6 — System Design for Backend Engineers
course_id: A6
track: A (Backend)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-10 (see _reviews/Boss_Final_Approval.md)
---

# A6 — System Design for Backend Engineers (Track A)

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | A6 — System Design for Backend Engineers / Track A (Senior Python Backend Mastery) |
| Audience | Senior Python backend engineers (L3–L4) who build services daily but need a repeatable, defensible way to design, size, and trade off systems under interview conditions, in voice and on the canvas |
| Prerequisites | A4 (databases) and A5 (API/backend architecture) are required (solid edges in the boss graph; a placement pass on their modules waives them); A2 (concurrency) and A7 (cloud) are recommended (dotted edges). M8 (AI-system design) additionally needs B2.M1-M5 plus one of B3, B4 or B6 (dotted edges B2/B3/B6 to A6); Track A learners can defer M8. Placement skip (G-5 bands apply per module): a learner who scores at or above the skip band (>= 85) on a module's diagnostic may skip that module's lessons and take its mastery check; placement system-design section draws from `placement` items in M1 (estimation) and M10 (probing). Skipping never waives M5 or M10 canvas work (canvas and evaluator craft is platform-specific) or the course final assessment |
| Level semantics | L1-L2 take the Simpler route (full scaffolding); L3-L4 take the Deeper route, whose hours are net of typical skip-ahead, so Deeper is shorter in wall-clock but harder per hour (G-2). Learner-fit rules are in section 8 |
| Outcomes (measurable, each tied to a competency) | 1. Run the 8-step framework within 45 minutes, stating requirements and scope in the first 5 (`design.framework`). 2. Produce a back-of-envelope estimate (QPS, storage, bandwidth, cache size, fleet size) within 3x of a reference and state which number drives the design (`design.estimation`). 3. Choose and justify caching, partitioning, replication, and consistency strategies, naming the failure each introduces (`design.scaling`, `design.consistency`). 4. Design async flows with queues or streams that are safe under retries, duplicates, and reordering using idempotency keys, outbox, and sagas (`design.messaging`). 5. Proactively cover failure modes, observability, security, and cost for any design (`design.reliability`, `design.security`, `design.cost`). 6. Deliver the 12 Core reference designs (of a 20-design closed-beta library; 26 at public 1.0), including the AI-system designs (RAG, LLM gateway, voice-agent platform) and multi-tenant SaaS, each with a canvas graph that passes the rule checks with zero critical findings (`design.classic`, `design.ai-systems`). 7. Defend a design against live probing ("what if this DB fails", "10x traffic") and edit the canvas to fix it (`design.probing`) |
| Estimated effort | 26 h Simpler / 18 h Deeper (module time, summing per table in section 2.1) plus a 4.0 h assessment allowance (two canvas tasks, voice mock, coding set, mastery check, revision loops). Deeper is net of typical skip-ahead (G-2). Matches the boss effort table (26 / 18 / 4.0) |
| Interview coverage tags (owned; competency credit issued here) | `design.framework`, `design.estimation`, `design.scaling`, `design.caching`, `design.consistency`, `design.messaging`, `design.reliability`, `design.observability`, `design.security`, `design.cost`, `design.classic`, `design.ai-systems`, `design.probing`, `design.canvas`. Tag note (G-9): `sd.ai-feature` and `system-design.ai-systems` are normalised to `design.ai-systems`. Secondary tags (no credit): `api.reliability` (owner A5), `sec.api` (owner A8) |
| Goal tags (G-3) | Course: B:M A:M H:M. Per module: M1-M5 and M10 `B:M A:M H:M`; M6-M7 `B:M A:S H:M`; M8 `B:S A:M H:M` (deferrable); M9 `B:S A:S H:S` |
| Company-style clusters fed | global-product (deepest), Indian-product, AI-startup, global-IT-consulting (solution architecture), Big4 (solution design discussions), IT-services (lighter, architecture-in-project questions) |
| Badge / certificate | Module badges M1–M10 (mastery check ≥ 80% plus the module's canvas task passing the rule checks with zero "critical" findings). Course badge "System Design for Backend Engineers" requires all module badges (M8 may be deferred per adaptive rule, section 8 LF-5), final assessment (section 5) with rubric overall >= 3.5 of 5, no dimension below 3 and no critical rule findings (G-7), and the 12 Core reference designs completed on the canvas (Extended designs optional). Voice mock for badge at "Meets" (avg >= 3.0, none < 2); readiness uses the Senior bar (G-6). Certificate shows rubric radar and the learner's two best designs (evidence links to canvas versions and transcript) |
| Authoring owner / reviewer | Authoring owner: SME agent A6. Reviewer: to be assigned |
| Review cadence | Core theory (M1–M7, M10): every 12 months. AI-system designs (M8) and any managed-service or version content: every 3 months. Security-related rules: every 6 months. Estimation reference numbers: every 12 months |
| IDE / canvas usage | Python IDE for small distinct coded building blocks (consistent hashing, LRU with TTL and single-flight, outbox poller, seat hold, telemetry dedupe, provider fallback). Token bucket and idempotency middleware tasks are owned by A5.M4 and used here by `ref` (R-A6-2). Canvas (platform capability P-19) for all design tasks, with the back-of-envelope calculator and structured text mode as the accessible alternative. M8 labs use the LLM Lab Harness (P-07) and voice harness (P-08) |
| Version baseline (G-8) | Redis-compatible (Redis or Valkey) in all designs; PostgreSQL 18.x for single-node limit numbers; Python 3.14 for code tasks. No model or provider names in lessons; AI numbers are labelled placeholders |

### 1.1 Canvas typed-graph vocabulary used throughout this course

The learner draws on the Skillora system-design canvas; it serialises to a typed graph, so every "what to draw" and "rule check" below refers to these types. **Status: adopted as canvas vocabulary v0.1 with amendments (R-A6-1, decisions doc section 7).** A6 owns the semantics; the canvas spec (P-19) owns implementation. Amendments applied in the tables below are marked (v0.1+). Other courses (B3.M10, B4.M9, B6, A2/A5/A7 capstones) may cite only registry rule IDs and propose new rules to A6.

| Node type | Key attributes the learner can set | Examples |
|---|---|---|
| `client` | kind (web, mobile, IoT device, partner), expected QPS | Browser, device fleet |
| `edge` | kind (CDN, DNS, WAF, load balancer L4/L7), multi-AZ flag | CloudFront-like CDN, ALB-like LB |
| `gateway` | auth mode, rate limit, routing | API gateway |
| `service` | stateless/stateful, replicas, autoscaling, sync/async; **kind (v0.1+): service, worker, scheduler, agent (tool-calling agent), stream-processor** | App service, worker, cron scheduler |
| `datastore` | engine class (relational, KV, document, wide-column, search, graph, time-series, vector), replication (none, primary-replica, multi-primary, quorum), shard key, consistency, backup; **(v0.1+) tenant-scoped (bool), pii (bool), data-class (public, internal, restricted)** | Postgres, DynamoDB-like, OpenSearch-like, pgvector |
| `cache` | scope (client, CDN, app-local, distributed), policy (cache-aside, write-through, write-behind), TTL, eviction; **(v0.1+) tenant-scoped (bool)** | Redis-compatible (Redis or Valkey) |
| `queue` | kind (queue, log/stream), partitions, ordering key, DLQ flag, retention, delivery (at-least-once, at-most-once, effectively-once); **(v0.1+) tenant-scoped (bool)** | SQS-like, Kafka-like |
| `objectstore` | durability, lifecycle, versioning | S3-like |
| `external` | third-party dependency (payment PSP, SMS, LLM provider, STT/TTS), SLA, timeout, circuit breaker flag; **kind gains `mcp-server` (v0.1+)** | PSP, LLM API, MCP server |
| `observability` | logs, metrics, traces, alerts | Collector, dashboards |
| `identity` | AuthN/AuthZ provider, token type | IdP |
| `ai` | kind (embedding model, LLM, reranker, STT, TTS, vector index), batch/stream; **(v0.1+) tools (list, each with side-effect flag), step-budget** | LLM gateway upstream |
| `boundary` | AZ, region, VPC, tenant cell | Grouping box |
| network `edge` (v0.1+) | gains `region` attribute for cross-region links | Inter-region replication link |

| Edge attributes | Values |
|---|---|
| protocol | HTTP/REST, gRPC, WebSocket, SSE, MQTT, AMQP, Kafka protocol, SQL, internal RPC |
| mode | sync request/response, async publish, async consume, replicate, cache read, cache fill |
| semantics | at-least-once, idempotent flag (yes/no), delivery, ordering-key, **timeout-ms, retry-budget, human-approval (for agent tool edges) (v0.1+)**, retry policy (none, fixed, exponential with jitter), authenticated (yes/no), TLS (yes/no), payload size/rate (for the calculator) |

### 1.2 Rubric dimensions for evaluation (used by the auto-evaluator, interview report, and final assessment)

Scored 0–5 per dimension with level anchors (0 absent, 1 named only, 3 competent and consistent, 5 senior: proactive, quantified, trade-offs and failure of own choices stated). Weights are defaults; the Admin Portal can re-weight per role or company pack.

| # | Dimension | Weight | What a "3" shows | What a "5" adds | Evidence source |
|---|---|---|---|---|---|
| D1 | Requirements and scope | 10% | Functional list, 2+ non-functionals (latency, availability, consistency), explicit out-of-scope | Prioritisation by user impact, SLO numbers, clarifying questions that change the design | Transcript, canvas notes |
| D2 | Estimation and capacity | 10% | QPS, storage, bandwidth within 3x; identifies read/write ratio | Identifies the dominant constraint, sizes cache and fleet, sanity-checks against single-node limits | Calculator panel, transcript |
| D3 | API and data model | 10% | Core endpoints, entities, keys, access patterns drive storage choice | Pagination, idempotency keys, schema evolution, hot-key and shard-key reasoning | Canvas notes, transcript |
| D4 | High-level architecture and fit | 15% | Coherent graph, sensible component choices, request path traceable | Minimal components that satisfy the requirements; cell, multi-AZ, or multi-region reasoning where justified; no needless complexity | Typed graph |
| D5 | Scaling and bottlenecks | 15% | Cache, replicas, partitioning, queues where load implies | Hot partitions, fan-out amplification, backpressure, load-shedding, capacity headroom | Graph, transcript |
| D6 | Consistency and data integrity | 10% | States consistency model per data path; knows replication lag effects | Idempotency, exactly-once illusions, outbox, sagas, conflict resolution, PACELC reasoning per decision | Graph attributes, transcript |
| D7 | Reliability and failure modes | 10% | Identifies SPOFs, retries, timeouts, DLQ | Circuit breakers, bulkheads, graceful degradation, DR (RPO/RTO), game-day thinking | Rule checks, probing deltas |
| D8 | Security and privacy | 5% | AuthN/AuthZ path, TLS, secrets | Tenant isolation, PII handling, least privilege, abuse and prompt-injection paths for AI | Graph, transcript |
| D9 | Observability and operations | 5% | Logs, metrics, traces, alerts exist | SLIs/SLOs tied to the requirements, correlation IDs, rollout and rollback plan | Graph, transcript |
| D10 | Cost and trade-offs | 5% | Names at least one cost driver and one explicit trade-off | Quantified cost per request/user/minute, cost-vs-latency levers, what would be cut first | Transcript |
| D11 | Communication and probing resilience | 5% | Structured, signposted; answers probes without collapsing | Adapts design to probes with clean canvas deltas, states assumptions, time-boxes | Transcript, canvas version deltas |

### 1.3 Auto-evaluator rule-check catalogue (deterministic checks on the typed graph; LLM rubric scoring layered on top)

**This table is the governed rule registry (v0.1+).** Owner of semantics for every rule: course A6 (canvas spec P-19 owns implementation). IDs are `R-<AREA>-<n>`, permanent, never renumbered or reused; retired rules are marked deprecated. Severities can be re-weighted in admin; "stated non-goal" suppression is mandatory (a deliberate single-AZ internal tool is not penalised). Each rule ships with a "good" and a "weak" example graph (39 rules, so 78 graphs in the platform regression suite): section 7.1 specifies the pairs for the six new rules; the pairs for the 33 original rules are authored by the A6 SME with the P-19 canvas work and are listed as an open dependency, not yet specified here. Status of all rules below: active. Other courses cite IDs only.

Severity: **C** critical (blocks module badge), **M** major, **m** minor/advisory. Each finding must link to the offending node/edge and explain the fix.

| Rule ID | Check | Sev | Notes / avoid false positives |
|---|---|---|---|
| R-SPOF-1 | Any `service`, `gateway`, `edge` LB, `datastore`, or `queue` with a single replica/AZ on the main request path | C | Not flagged if the design explicitly states a non-HA scope (e.g. internal tool with stated RTO) |
| R-SPOF-2 | A `datastore` with replication = none and no backup attribute | C | |
| R-SPOF-3 | Single `boundary` (one AZ) for the stateful tier | M | Multi-region only required when requirements say so |
| R-CACHE-1 | Read-dominant path (read:write over 10:1, or read QPS over the threshold set in the calculator) with no `cache` or CDN between service and datastore | M | Uses learner's stated numbers |
| R-CACHE-2 | `cache` with no TTL or invalidation policy | M | |
| R-CACHE-3 | Cache-aside on hot key without stampede protection (lock, request coalescing, jittered TTL) mentioned | m | |
| R-QUEUE-1 | Synchronous call chain to a slow or bursty dependency (email, PSP callback, LLM batch, transcoding) without a `queue` | M | |
| R-QUEUE-2 | `queue` with no DLQ or retry limit | M | |
| R-QUEUE-3 | Consumer count greater than partition count with strict ordering required; or no ordering key where ordering is a stated requirement | M | |
| R-FANOUT-1 | Unbounded fan-out (e.g. write fan-out to followers, broadcast to all connections) with no cap, batching, or hybrid strategy | M | |
| R-IDEMP-1 | Edge with retry policy (or at-least-once delivery) into a state-changing consumer/service with idempotent = no | C | The core check for payments, booking, and notifications |
| R-IDEMP-2 | State-changing API on a payments/orders node with no idempotency key noted | C | |
| R-DUAL-1 | Service writes to a `datastore` and publishes to a `queue` with no outbox/CDC/transactional pattern noted | M | |
| R-AUTH-1 | Path from `client` (external) to any state-changing service with no `gateway`/`identity` authentication on the path | C | |
| R-AUTH-2 | Internal service-to-service edges without authentication or mTLS noted | m | |
| R-TLS-1 | Edge across a `boundary` or from external client with TLS = no | M | |
| R-RATE-1 | Public `gateway` with no rate limit or quota attribute | M | Required for LLM-backed endpoints (cost) |
| R-OBS-1 | No `observability` node, or no logs+metrics+traces coverage of the critical path | M | |
| R-OBS-2 | No alerting on the stated SLO | m | |
| R-TIMEOUT-1 | Edge to `external` with no timeout/retry/circuit breaker noted | M | |
| R-DATA-1 | `datastore` engine class mismatch with stated access pattern (e.g. full-text search on a KV store with no search index) | m | Advisory only; LLM rubric decides |
| R-SHARD-1 | Sharded `datastore` with no shard key, or a key that is monotonic/low-cardinality (hot shard risk) | M | |
| R-SHARD-2 | No resharding/rebalancing strategy for a sharded store | m | |
| R-STORE-1 | Large binary payloads passed through a `service` and stored in a relational `datastore` instead of an `objectstore` | m | |
| R-TENANT-1 | Multi-tenant design with no tenant identifier on the data path or isolation noted | C | Used in M9 |
| R-AI-1 | LLM/STT/TTS `external` or `ai` node on the request path with no fallback provider, timeout, or degraded mode | M | |
| R-AI-2 | LLM gateway design with no per-tenant budget/quota or cost metering | M | |
| R-AI-3 | RAG design with no ingestion path (chunk/embed/index) or no staleness/refresh plan | M | |
| R-AI-4 | User-supplied content flows to an LLM with tools/side-effects and no injection guard or least-privilege tool boundary | M | |
| R-AI-5 | Real-time voice path with no stated latency budget by stage | M | Used in M8 |
| R-IOT-1 | Device fleet ingestion with no per-device authentication, no backpressure/buffering, or no handling of offline devices and duplicates | M | Used in M9 |
| R-DR-1 | No backup/PITR or RPO/RTO stated for a system of record | M | |
| R-COST-1 | No cost driver noted for a design that includes LLM calls, cross-region replication, or heavy egress | m | |
| R-AGENT-1 (new) | `service` kind agent or `ai` node with write-capable tools (side-effect flag) and no human-approval edge or scope limit | M | Cited by B4.M9 |
| R-AGENT-2 (new) | Agent loop with no step, token or time budget (`step-budget` unset) | M | Cited by B4.M9 |
| R-TENANT-2 (new) | Vector index or cache lacking tenant filter or ACL path | C in a multi-tenant context, else m | Extends R-TENANT-1 to derived stores; used in M8 and M9 |
| R-PII-1 (new) | `datastore` or flow marked pii sends data to an external model with no redaction path | M | Used in M8; legal depth is X1 (A8.M9.4) |
| R-VOICE-1 (new) | Real-time voice path with no barge-in or cancellation path | m | Companion to R-AI-5; used in design #17 |
| R-COST-2 (new) | Metered `external` or `ai` (LLM, STT, TTS) with no budget or quota | M | Merges with R-AI-2 for gateways (R-AI-2 stays active for gateway-specific wording) |

---

## 2. Module overview table

| # | Module | Module outcome | Key topics | Visuals (named diagrams and step-throughs) | Practice tasks (IDE / canvas) | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | The design framework and estimation | Run the 8-step framework in 45 minutes and produce a defensible estimate that identifies the dominant constraint | Requirements (functional, non-functional, scope cut), estimation (QPS, peak factor, storage, bandwidth, memory, fleet), API sketch, data model driven by access patterns, high-level design, deep dives, bottlenecks, trade-offs; time-boxing; thinking aloud; latency numbers every engineer should know; single-node limits (what one Postgres or Redis can do) | 8-step framework timeline; estimation waterfall; latency-numbers ladder | Calculator drills (5 problems); canvas: draw a baseline "single server to scaled" evolution; timed 10-minute requirements-and-estimation voice drill | Jumping to boxes before requirements; false precision; estimating without stating the read:write ratio; ignoring peak vs average; designing for 1B users when 1M is stated; never choosing a deep dive |
| M2 | Scaling reads and caching | Choose and justify a read-scaling stack (LB, stateless services, replicas, caches, CDN) and predict how it fails | Stateless services and sessions, L4/L7 load balancing, health checks, autoscaling, read replicas and lag, cache-aside/read-through/write-through/write-behind, TTL and invalidation, stampede/thundering herd, hot keys, negative caching, CDN and edge caching, HTTP caching headers, cache sizing; (stampede mechanism and HTTP/Redis caching mechanics: pointer to A5.M5) | Cache topologies compared; cache-stampede step-through (pointer view of A5.M5 animation); request path with hit/miss timing | Code: LRU cache with TTL and single-flight (extends A3 LRU); canvas: add caching to a read-heavy product page; estimate hit-rate vs backend load | "Just add Redis"; ignoring invalidation and consistency; cache as source of truth; no stampede plan; replica reads after write without a read-your-writes strategy |
| M3 | Data: partitioning, replication, consistency | Pick a partition key and replication/consistency model for a data path and explain what breaks | Choosing a datastore by access pattern, sharding strategies (range, hash, directory, consistent hashing with virtual nodes), shard-key selection, hot partitions, resharding, secondary indexes across shards, replication (leader-follower, multi-leader, leaderless, quorum), consistency models (linearizable, sequential, causal, read-your-writes, monotonic reads, eventual), CAP stated pragmatically, PACELC, conflict resolution (LWW, CRDT overview), distributed unique IDs (Snowflake-style, ULID/UUIDv7), time and ordering pitfalls | Consistent-hash ring with vnodes animation; quorum read/write diagram (R+W>N); replication lag anomaly timeline | Code: consistent hashing with vnodes and rebalance count; canvas: shard a user-orders store, show hot-key mitigation; consistency matrix exercise | CAP recited as "pick 2"; confusing consistency in CAP with ACID's C; ignoring latency (PACELC); auto-increment shard keys; cross-shard joins/transactions ignored; "eventual consistency" with no stated staleness bound |
| M4 | Async, messaging, idempotency, rate limiting | Design retry-safe asynchronous pipelines and protect systems with rate limiting and backpressure | Queue vs log/stream, delivery semantics, ordering and partitions, consumer groups, DLQ and poison messages, retries with exponential backoff and jitter, idempotency keys and dedupe stores, transactional outbox and CDC, sagas (orchestration vs choreography), exactly-once as an illusion (effectively-once), backpressure, load shedding, rate-limiter **service** design (placement, failure policy, per-tenant config; algorithm choice recap with pointer to A5.M4 for algorithms and Redis implementation), bulkheads at design level (implementation pointer A5.M4); Challenge lesson: streaming-platform essentials (partitions, consumer groups, offsets, rebalances, compaction, exactly-once scope; O-111) | Outbox + relay sequence diagram; saga compensation timeline; rate-limiter service placement diagram; duplicate-delivery step-through; consumer-group rebalance step-through | Code: outbox poller and consistent-hash-style partition router (distinct items); token bucket and idempotency-key middleware reused by `ref` from A5.M4; canvas: order pipeline with queue, DLQ, outbox | "Exactly-once delivery" claims; retries without idempotency; unbounded queues; one consumer group for everything; assuming FIFO globally; dual-write without outbox; ignoring poison messages |
| M5 | Reliability, observability, security, cost | Apply a checklist for failure modes, SLOs, security, and cost to any design in under 5 minutes of talk | Failure modes (node, AZ, region, dependency, slow dependency, data corruption, bad deploy, retry storm), timeouts and deadlines, circuit breakers, bulkheads, graceful degradation, multi-AZ vs multi-region, RPO/RTO and DR tiers, SLIs/SLOs/error budgets, metrics-logs-traces (RED/USE), alerting on symptoms, rollouts (canary, blue/green, feature flags), security (authN/authZ, secrets, TLS, WAF, tenant isolation, PII, audit), cost levers (egress, storage tiers, reserved vs on-demand, caching ROI, LLM token cost) | Failure-mode heat map; retry-storm timeline; SLO and error-budget burn chart; layered security diagram | Canvas: take a given design and fix all rule-check findings; calculate monthly cost of a design with the calculator; write alert definitions for an SLO | Naming "high availability" without mechanism; retries at every layer (amplification); alerts on causes not symptoms; "we'll add monitoring later"; security as a single "add auth" box; ignoring cost entirely |
| M6 | Classic designs I: read-heavy and infrastructure | Design seven classic systems end to end with estimates, data model, deep dive, and failure modes | **1 URL shortener** (ID generation, redirects, cache, analytics), **2 rate limiter service**, **3 distributed cache / KV store** (partitioning, replication, eviction), **4 search autocomplete / typeahead** (trie vs inverted prefix index, ranking, update pipeline), **5 file storage and sync (Dropbox/S3-like)** (chunking, dedupe, metadata vs blobs, resumable uploads, sync conflicts), **6 API gateway** (routing, authN, rate limits, circuit breaking, config rollout), **7 web crawler** (frontier, politeness, dedupe, robots, scale) | One reference diagram per design plus a "decision card" (key choice, rejected alternative, why); autocomplete data flow; chunked-upload sequence | Canvas: draw each design once under a timer, run the evaluator, fix findings; code: base62 ID generator with collision handling; chunk-hash dedupe | Choosing hash-and-truncate with no collision handling; storing blobs in the DB; central rate-limit single point; autocomplete querying the OLTP DB; no politeness in the crawler; skipping analytics write path decoupling |
| M7 | Classic designs II: write-heavy, real-time, transactional | Design seven systems where concurrency, ordering, fan-out, or money make correctness hard | **8 notification system** (multi-channel, templates, preferences, dedupe, rate limits, provider failover), **9 real-time chat** (WebSocket gateways, presence, delivery receipts, ordering, group fan-out, offline sync), **10 news feed / timeline** (fan-out on write vs read vs hybrid, celebrity problem, ranking hooks), **11 payment system** (ledger, idempotency, PSP integration, reconciliation, double-entry, webhooks), **12 booking / inventory (tickets, hotel, seats)** (holds, TTL, overselling, locking vs optimistic, saga), **13 distributed job scheduler** (delayed jobs, leader election or sharded timers, at-least-once, visibility timeout, cron at scale), **14 leaderboard / counters / top-K** (sorted sets, sharded counters, approximate counting, windows) | Chat message lifecycle sequence; feed fan-out hybrid decision diagram; payment state machine; seat-hold timeline | Canvas: each design timed with evaluator feedback; code: seat-hold with conditional update or Redis lock with fencing token (compare; the `SKIP LOCKED` primitive is owned by A4.M6), ledger entries for a transfer, delayed-job scheduler with heap | Double charge on retry; overselling; global ordering assumed in chat; push fan-out for celebrity accounts; locks without expiry or fencing; notification duplicates; payment status only in the PSP |
| M8 | AI-system designs | Design LLM-era systems (RAG service, LLM gateway, voice-agent platform) with cost, latency, safety, and evaluation built in | **15 RAG service** (ingestion pipeline: parse, chunk, embed, index; hybrid retrieval, rerank, citations; freshness, ACLs per document, eval loop, caching, multi-tenant indexes), **16 LLM gateway** (provider abstraction, routing and fallback, per-tenant quotas and budgets, semantic and prompt caching, streaming, retries and idempotency, PII redaction, audit, observability of tokens and cost), **17 voice-agent platform** (telephony/WebRTC ingress, VAD, streaming STT, LLM, streaming TTS, barge-in, per-stage latency budget, session state, concurrency scaling, failover, call recording consent), shared concerns (prompt injection, tool least-privilege, eval and regression sets, cost per request/minute, queueing batch embeddings) | RAG ingest and query dual-pipeline diagram; gateway request lifecycle; voice pipeline latency waterfall with budget bars | Canvas: three designs timed; calculator: token and cost model for a chat product and a voice minute; code: provider-fallback wrapper with timeout, circuit breaker, and budget check | Treating the LLM as a deterministic reliable dependency; no fallback or budget; vector DB as the whole RAG design; ignoring ingestion and freshness; no document-level ACL; no per-stage latency budget in voice; logging raw PII prompts |
| M9 | IoT ingestion and multi-tenant SaaS | Design high-volume device ingestion and a multi-tenant platform with isolation, noisy-neighbour control, and cost attribution | **18 IoT telemetry ingestion with MQTT** (device identity and certs, topic design, QoS 0/1/2 meaning, retained and last-will messages, shared subscriptions, broker clustering, bridging to a stream, time-series store, downsampling, offline buffering, duplicate handling, command/control channel), **19 multi-tenant SaaS platform** (tenancy models: pool, bridge, silo; tenant ID propagation, row-level security, per-tenant quotas and rate limits, noisy-neighbour isolation, cell-based architecture, per-tenant encryption keys, onboarding, billing metering), **20 metrics and analytics pipeline** (events to stream to OLAP, batch vs streaming, late data, exactly-once-ish aggregation) | MQTT topic tree and QoS flow; tenancy spectrum chart; cell-based architecture diagram; ingestion funnel with backpressure points | Canvas: three designs timed; code: idempotent telemetry consumer with dedupe window; calculator: 1M devices at 1 msg/min sizing | "Use MQTT QoS 2 for everything"; broker as the database; one shared topic with no auth per device; no handling of reconnect storms; tenant ID missing from cache keys; same rate limit for all tenants; one noisy tenant takes down all |
| M10 | Canvas craft, probing, and capstone | Convert reasoning into a clean typed graph, survive probing, and complete mock design rounds that the evaluator scores in line with the rubric | Drawing for the evaluator (labels, protocols, numbers, boundaries), annotating requirements and capacity on the canvas, the 45-minute and 30-minute budgets, signposting aloud, handling interviewer pushes (scale, failure, new requirement), recovering from a wrong path, comparing to a reference design, self-review with the rule catalogue, adapting to company-style (product-depth vs consulting solution-style) | Annotated "good vs weak" canvases; probing-delta replay; rubric radar | Three full mock rounds (voice plus canvas) on unseen prompts; re-draw a weak design after evaluator feedback; compare to reference overlay | Silent drawing; unlabeled boxes; no numbers on the canvas; arguing instead of adapting to a probe; refusing to commit to a choice; over-engineering for a stated small scale |

### 2.1 Module hours (sum to header: 26.0 Simpler / 18.0 Deeper; assessment allowance 4.0 h separate)

| Module | M1 | M2 | M3 | M4 | M5 | M6 | M7 | M8 | M9 | M10 | Total |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Simpler (h) | 2.5 | 2.5 | 3.0 | 3.5 | 2.5 | 3.0 | 3.0 | 2.0 | 2.0 | 2.0 | **26.0** |
| Deeper (h, net of skip-ahead) | 1.5 | 1.5 | 2.0 | 2.5 | 1.5 | 2.0 | 2.25 | 1.75 | 1.5 | 1.5 | **18.0** |
| Goal tags | B:M A:M H:M | B:M A:M H:M | B:M A:M H:M | B:M A:M H:M | B:M A:M H:M | B:M A:S H:M | B:M A:S H:M | B:S A:M H:M | B:S A:S H:S | B:M A:M H:M | |

M4 hours include the streaming-platform Challenge lesson (<= 1.5 h Deeper only, `B:N A:N H:N`, not gating). Design reps beyond the Core set are self-paced and not counted.

### 2.2 Ownership notes (final row of the module table, G-1)

| Module | OWNS | POINTS (topic -> owner) |
|---|---|---|
| M1 | Design framework, estimation method and numbers, single-node limits as orders of magnitude | Networking and Linux triage fundamentals -> A7.M6; SQL access-pattern details -> A4.M3 |
| M2 | Cache topology, sizing, hit-rate design, LB and read-scaling design | Stampede mechanism, HTTP and Redis caching mechanics -> A5.M5; LRU data structure -> A3 |
| M3 | Sharding strategy, consistent hashing, consistency models, IDs and ordering, replication design across services | DB-level partitioning, replication, pooling -> A4.M7; isolation and MVCC -> A4.M5-M6 |
| M4 | Delivery-semantics theory, saga, CDC, exactly-once reasoning, queue vs log comparison, rate-limiter service design, streaming-platform essentials (O-111) | Rate-limit algorithms and Redis token bucket, idempotency keys, retry/backoff/jitter (X2), circuit breaker and bulkhead implementation -> A5.M4; outbox implementation and idempotent consumer -> A5.M8; worker semantics -> A2.M9; `SKIP LOCKED` primitive -> A4.M6; AWS queue services -> A7.M5 |
| M5 | Failure-mode thinking, DR tiers (design level), observability-in-design, shared cost rubric card (D10) | DR implementation, restore testing, incidents, FinOps levers -> A7.M10; SLO tooling and OTel -> A7.M9; threat modelling -> A8.M9; compliance primer -> X1 (A8.M9.4) |
| M6-M7 | Classic design library (designs 1-14), decision cards, estimation sheets | Canonical rate-limiter coding task -> A5.M4; LLD and machine-coding round format -> C3.M7 |
| M8 | AI-system design (RAG service #15, LLM gateway #16, voice platform #17 canonical canvas task) | RAG architecture and retrieval depth -> B3.M1, M5; token cost and provider limits -> B2.M8-M9 (X3); agent and orchestration depth -> B4; voice technology and latency method (workbook) -> B6.M6; LLM observability -> B5.M7; injection and guardrails -> B5.M4-M6 |
| M9 | Multi-tenancy architecture patterns (pool, bridge, silo, cells, noisy neighbour), IoT ingestion design, analytics pipeline design | Tenancy data model and RLS -> A4.M2; AWS IoT and managed services operations -> A7.M5; tenant context hooks in APIs -> A5.M11 |
| M10 | Canvas craft, probing, rubric D1-D11, rule registry, company-style adaptation | Own-architecture canvas defence -> C2.M8; seven-step coding loop -> C3.M1 (X6) |

---

## 3. Per-module detail blocks

Lesson IDs: lessons numbered (1)..(5) below are `A6.Mn.1`..`A6.Mn.5` (the Challenge lesson in M4 is `A6.M4.6`). Question IDs: `Q-A6-M<n>-<nn>`; seed questions below map as `M<n>-Q<k>` = `Q-A6-M<n>-0<k>`. Each module also carries: 5-8 `placement` diagnostic items (titles in section 8 LF-2), one production-framed applied lab (LF-6), one memorable visual or analogy and the top misconceptions (LF-7).

Question-bank targets per module (template numbers): 12 MCQ / concept checks, 6 predict-the-result items (for this course: predict behaviour, capacity, or failure from a given diagram or snippet), 4 coding tasks with hidden tests, 8 verbal questions with follow-up trees, 3 scenario/design questions (canvas). Mastery check: 6–10 items at 80% pass threshold; misses loop back to the relevant lesson.

### M1 — The design framework and estimation
- **Lessons:** (1) The 8 steps and the clock (Simpler: a worked 45-minute timeline; Deeper: adapting the framework for 30- and 60-minute rounds and for consulting-style "solution" prompts). (2) Requirements and scope (Simpler: functional vs non-functional checklist; Deeper: turning vague asks into SLOs and explicit trade-offs, "what I am not building"). (3) Estimation craft (Simpler: QPS, storage, bandwidth recipe with round numbers; Deeper: peak factors, fleet sizing, cache sizing, "one box can do X" sanity checks). (4) APIs and data model from access patterns (Simpler: REST sketch plus table design; Deeper: pagination, idempotency keys, schema evolution, choosing storage class). (5) Deep dives and trade-off language (Simpler: pick-one-component routine; Deeper: structured trade-off statements: option, benefit, cost, when I would flip).
- **Mastery focus:** step order and time boxes; estimation arithmetic and units; choosing the dominant constraint; read:write ratio implications.
- **Canvas drawing:** baseline single-server diagram, then the evolved version with LB, stateless services, DB with replica; calculator panel populated; requirements and out-of-scope notes attached.
- **Rule checks to expect:** R-SPOF-1/2 on the baseline (deliberately flagged to teach); R-CACHE-1 once read-heavy numbers are entered; R-OBS-1.
- **Badge criteria:** mastery check ≥ 80%; estimation drill within 3x on 4 of 5 problems; one canvas passing zero critical findings.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### M2 — Scaling reads and caching
- **Lessons:** (1) Stateless services, LBs, and autoscaling (Simpler: why sessions break scaling; Deeper: L4 vs L7, connection draining, slow-start, health-check design). (2) Read replicas and lag (Simpler: routing reads; Deeper: read-your-writes strategies, sticky reads, lag-aware routing). (3) Caching patterns and invalidation (Simpler: cache-aside walk-through; Deeper: write-through vs write-behind risks, TTL vs event-based invalidation, versioned keys). (4) Stampede, hot keys, and negative caching (design recap only; mechanism and implementation are owned by A5.M5, link as pointer; Simpler: concept and single-flight; Deeper: hot-key splitting and cache-sizing implications at topology level). (6 min of the lesson is a "cache failure at peak" capacity calculation: origin QPS when hit rate drops from 95% to 0%.) (5) CDN and HTTP caching (Simpler: static assets; Deeper: cache keys, Vary, stale-while-revalidate, purge strategies).
- **Mastery focus:** which pattern for which workload; consequences of each pattern's failure; sizing a cache from numbers.
- **Canvas drawing:** `edge` CDN, `cache` (scope, policy, TTL), `datastore` with replicas, edges labelled "cache read" and "cache fill".
- **Rule checks:** R-CACHE-1/2/3, R-SPOF-1 on LB and cache, R-OBS-1 (hit-rate metric).
- **Badge criteria:** mastery ≥ 80%; LRU+TTL+single-flight coding task passes hidden tests; product-page design with zero critical findings.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### M3 — Data: partitioning, replication, consistency
- **Lessons:** (1) Choosing storage by access pattern (Simpler: relational vs KV vs document vs search vs time-series; Deeper: when Postgres is enough and how far it goes before sharding). (2) Sharding and consistent hashing (Simpler: hash vs range; Deeper: vnodes, rebalancing cost, hot partitions, cross-shard queries, directory-based routing). (3) Replication and quorums (Simpler: leader-follower; Deeper: multi-leader conflicts, leaderless R+W>N, hinted handoff and anti-entropy in concept). (4) Consistency models and CAP/PACELC pragmatically (Simpler: strong vs eventual with examples; Deeper: causal, read-your-writes, monotonic reads, mapping product requirements to models, linearizable use cases such as inventory counters and uniqueness). (5) IDs, time, and ordering (Simpler: UUID vs auto-increment; Deeper: Snowflake-style IDs, UUIDv7, clock skew, logical clocks and why wall-clock ordering lies).
- **Mastery focus:** shard-key selection; reasoning about anomalies from lag; stating the consistency model per path.
- **Canvas drawing:** `datastore` with shard key, replication mode, consistency; a `boundary` per AZ; notes for hot-key mitigation.
- **Rule checks:** R-SHARD-1/2, R-SPOF-2/3, R-DR-1, R-DATA-1.
- **Badge criteria:** mastery ≥ 80%; consistent-hashing task passes (minimal key movement test); sharded orders-store canvas with no critical findings.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### M4 — Async, messaging, idempotency, rate limiting
- **Lessons:** (1) Queues vs logs and delivery semantics (Simpler: when to use a queue; Deeper: partitions, consumer groups, offsets, ordering keys, retention and replay, visibility timeouts). (2) Retries, DLQs, and poison messages (system level: retry budgets, retry amplification across layers, parking-lot queues, replay tooling; backoff with jitter is the X2 shared lesson owned by A5.M4: pointer, not re-taught). (3) Idempotency and dedupe at system level (where keys are enforced across gateway, queue consumers and downstream providers, natural idempotency vs dedupe table, effectively-once processing; the idempotency-key mechanism, TTL and request-hash handling are owned by A5.M4: pointer, not re-taught). (4) Outbox, CDC, and sagas (Simpler: why dual writes fail; Deeper: orchestration vs choreography, compensations, semantic locks, saga timeouts). (5) Rate-limiter service design and backpressure (design level: where the limiter lives, local-plus-global hybrid, per-tenant overrides, fail-open vs fail-closed, load shedding, 429 with Retry-After; algorithms and Redis atomic implementation are owned by A5.M4: pointer, not re-taught). (6) **Challenge (Deeper only, <= 1.5 h, inside M4 hours, owner of O-111, not gating):** streaming-platform essentials: partitions and ordering, consumer groups and offsets, rebalances (eager vs cooperative), log compaction, retention and replay, exactly-once scope (transactional produce-consume inside one platform vs end-to-end), share-group/queue-style consumption as an emerging feature (see section 6 dated notes).
- **Mastery focus:** what each guarantee really means; designing consumers for duplicates; deciding where the limiter lives and what happens when it fails.
- **Canvas drawing:** `queue` (kind, partitions, ordering key, DLQ, delivery), edges with idempotent flag and retry policy, outbox table in the `datastore` annotation.
- **Rule checks:** R-IDEMP-1/2, R-DUAL-1, R-QUEUE-1/2/3, R-RATE-1, R-FANOUT-1.
- **Badge criteria:** mastery ≥ 80%; outbox poller task passes hidden tests (crash between write and publish, duplicate publish; allowed by R-A6-2 as a distinct, system-level poller/relay exercise, not the A5.M8 outbox-write lab, which is referenced by `ref` and not repeated); token bucket and idempotency middleware are satisfied by the A5.M4 results via `ref` (no re-test here); order pipeline canvas with zero critical findings. Timed coding set reuses A5/A3 items by `ref` or distinct items (consistent hashing, outbox poller) per R-A6-2.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### M5 — Reliability, observability, security, cost
- **Lessons:** (1) Failure-mode analysis (Simpler: the "what if this dies" walk; Deeper: gray failures, slow dependencies, retry storms, metastable failures, correlated failures). (2) Resilience patterns (Simpler: timeouts and retries; Deeper: deadlines propagation, circuit breakers, bulkheads, hedged requests, graceful degradation tiers). (3) Availability and DR (Simpler: multi-AZ; Deeper: active-passive vs active-active, RPO/RTO tiers, failover testing, data replication costs). (4) Observability and SLOs (Simpler: metrics, logs, traces; Deeper: RED/USE, SLI selection, error budgets, burn-rate alerts, high-cardinality traps, trace sampling). (5) Security and cost as design inputs (Simpler: authN/authZ, TLS, secrets; Deeper: tenant isolation, PII minimisation, audit trails, threat sketch per boundary (method owned by A8.M9; compliance by X1); **the shared cost rubric card (D10)** owned here: top-3 cost drivers, unit cost per request/user/minute, lever per driver, "what I cut first" statement; inputs supplied by A7.M10 (infrastructure levers) and B2.M9/X3 (token cost); DR is design-level only, implementation pointer A7.M10).
- **Mastery focus:** converting "make it reliable" into mechanisms; picking SLIs; spotting amplification loops; cost-aware trade-offs.
- **Canvas drawing:** `observability`, `identity`, `boundary` groupings, timeouts and retries on every edge, DR annotations.
- **Rule checks:** R-SPOF-1/2/3, R-TIMEOUT-1, R-OBS-1/2, R-AUTH-1/2, R-TLS-1, R-DR-1, R-COST-1. This is the module where the full catalogue is taught.
- **Badge criteria:** mastery ≥ 80%; "fix the flawed design" canvas clears all critical and major findings; one SLO-to-alert exercise accepted by rubric.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### M6 — Classic designs I
- **Lessons:** (1) URL shortener and ID generation (Simpler: base62 counter; Deeper: range-allocation counters, collision handling, 301 vs 302 and analytics, expiry, abuse). (2) Rate limiter and API gateway (Simpler: limiter as a middleware; Deeper: distributed limiter, local-plus-global hybrid, config propagation, gateway as a failure domain). (3) Distributed cache / KV store (Simpler: partitioned cache; Deeper: replication, eviction at scale, rebalancing, persistence trade-offs). (4) Autocomplete and file storage/sync (Simpler: trie and chunked upload; Deeper: top-K per prefix, update pipelines, content-defined chunking, dedupe, sync conflicts). (5) Web crawler (Simpler: BFS with queue; Deeper: URL frontier prioritisation, politeness per host, dedupe with Bloom filters, trap detection).
- **Mastery focus:** each design's key trade-off ("decision card"); estimates per design; what the deep dive should be.
- **Canvas drawing:** one graph per design; the learner annotates QPS/storage on the canvas and picks one deep-dive component for an expanded sub-graph.
- **Rule checks:** R-CACHE-1, R-SHARD-1, R-STORE-1, R-RATE-1, R-SPOF-1, R-OBS-1, R-QUEUE-1 (analytics decoupling, crawler frontier).
- **Badge criteria:** mastery ≥ 80%; all 7 designs completed on canvas, at least 5 with zero critical findings and rubric ≥ 3 on D4 and D5.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3 (scenario items drawn from the seven designs).

### M7 — Classic designs II
- **Lessons:** (1) Notification system (Simpler: queue plus workers per channel; Deeper: preferences, dedupe, priority lanes, provider failover, per-user rate limits, delivery tracking). (2) Real-time chat and presence (Simpler: WebSocket plus pub/sub; Deeper: gateway scaling and connection routing, per-conversation ordering, offline sync, group fan-out limits, presence cost). (3) News feed (Simpler: fan-out on read vs write; Deeper: hybrid with celebrity threshold, ranking hooks, cache layout, backfill). (4) Payment and booking systems (Simpler: idempotency and state machines; Deeper: double-entry ledger, reconciliation, webhook handling and signature verification, seat holds with TTL, overselling prevention with locking vs optimistic checks, fencing tokens, saga compensation). (5) Job scheduler and leaderboard/counters (Simpler: delayed queue; Deeper: sharded timer wheels or leader election, visibility timeouts, cron deduplication, sharded counters, approximate top-K).
- **Mastery focus:** correctness under retries and concurrency; fan-out strategy trade-offs; ordering guarantees per entity.
- **Canvas drawing:** state-machine annotations on `service` nodes, `external` PSP with timeout and circuit breaker, `queue` per channel with DLQ, WebSocket edges labelled by protocol.
- **Rule checks:** R-IDEMP-1/2, R-DUAL-1, R-FANOUT-1, R-QUEUE-1/2/3, R-TIMEOUT-1, R-SPOF-1, R-RATE-1.
- **Badge criteria:** mastery ≥ 80%; payment and booking designs both pass with zero critical findings; seat-hold coding task (no oversell under concurrency test) passes.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### M8 — AI-system designs
- **Lessons:** (1) RAG service architecture (Simpler: ingest, embed, retrieve, generate; Deeper: hybrid retrieval and reranking placement, document-level ACL filtering, freshness and re-embedding, caching layers, evaluation loop, multi-tenant index strategy; deep retrieval quality is B3). (2) LLM gateway (Simpler: single entry point for providers; Deeper: routing by cost/latency/quality, fallback chains, quotas and budgets per tenant, prompt and semantic caching, streaming pass-through, idempotent retries, redaction, audit and cost metering; application-level patterns are B2). (3) Voice-agent platform (Simpler: STT to LLM to TTS; Deeper: per-stage latency budget, streaming at every hop, barge-in cancellation, session state and reconnect, concurrency scaling and capacity per call, telephony vs WebRTC ingress, consent and recording; **this is the canonical design #17 and canvas task**; the per-stage numbers come from B6.M6's latency-budget workbook (linked as input, not duplicated); voice technology depth (VAD, barge-in internals, STT/TTS quality, telephony) is B6, and B6.M11 does not repeat this canvas task). (4) AI-specific failure modes and cost (Simpler: timeouts and fallbacks; Deeper: provider outages and rate limits, prompt injection and tool blast-radius, cost runaway, evaluation regressions on model upgrades, batch vs real-time embedding queues).
- **Mastery focus:** carrying classic patterns (cache, queue, rate limit, idempotency, observability) into AI systems; quantifying tokens and cost; latency budgeting.
- **Canvas drawing:** `ai` nodes (embedding, LLM, reranker, STT, TTS, vector index) with fallback edges, `queue` for ingestion, `gateway` with quota attributes, per-stage latency annotations on the voice path.
- **Rule checks:** R-AI-1/2/3/4/5, R-RATE-1, R-TIMEOUT-1, R-CACHE-1 (semantic/prompt cache), R-OBS-1, R-COST-1, R-TENANT-1 where multi-tenant, R-AUTH-1.
- **Badge criteria:** mastery ≥ 80%; all three designs on canvas with zero critical and at most 2 major findings; cost model from the calculator within 30% of reference; provider-fallback code task passes.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### M9 — IoT ingestion and multi-tenant SaaS
- **Lessons:** (1) MQTT and device ingestion (Simpler: pub/sub, topics, QoS 0/1/2; Deeper: topic design for ACLs and routing, retained and last-will messages, shared subscriptions, clean session vs persistent session in MQTT 3.1.1 and session expiry in MQTT 5, broker clustering and limits, bridging to Kafka-like streams, reconnect storms). (2) Telemetry pipeline (Simpler: broker to stream to time-series DB; Deeper: dedupe by device and sequence number, out-of-order and late data, downsampling and retention tiers, command/control with acks, firmware-update fan-out). (3) Multi-tenant tenancy models (Simpler: pool vs silo; Deeper: bridge model, cell-based architecture, tenant-aware cache keys; the RLS data model and schema-per-tenant mechanics are owned by A4.M2: pointer). (4) Isolation, quotas, and metering (Simpler: per-tenant rate limits; Deeper: noisy-neighbour controls, per-tenant queues, fair scheduling, per-tenant encryption keys, usage metering to billing). (5) Metrics/analytics pipeline (Simpler: events to warehouse; Deeper: stream vs batch, late data, OLAP choice, exactly-once-ish aggregation).
- **Mastery focus:** what QoS does and does not guarantee; per-device identity at scale; choosing a tenancy model by requirement; cost attribution.
- **Canvas drawing:** `client` (IoT device, with kind and fleet size), MQTT-labelled edges, broker as `queue` with stream bridge, `boundary` per tenant or cell, `identity` for device certificates.
- **Rule checks:** R-IOT-1, R-TENANT-1, R-QUEUE-1/2, R-AUTH-1, R-TLS-1, R-RATE-1, R-SHARD-1, R-OBS-1, R-DR-1.
- **Badge criteria:** mastery ≥ 80%; both designs and the metrics pipeline on canvas with zero critical findings; telemetry dedupe consumer passes hidden tests (duplicates, out-of-order).
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### M10 — Canvas craft, probing, and capstone
- **Lessons:** (1) Drawing for the evaluator and the interviewer (Simpler: label rules and templates; Deeper: levels of detail, sub-graphs for deep dives, annotating numbers). (2) Narrating while drawing (Simpler: signposting script; Deeper: pacing against a 45-minute clock, summarising trade-offs, handling silence). (3) Probing and deltas (Simpler: answering "what if the DB fails"; Deeper: making a clean canvas edit, restating new numbers, deciding when to push back). (4) Company-style adaptation (Simpler: product vs consulting expectations; Deeper: depth-first vs breadth-first emphasis, solution-architecture communication, client-risk framing). (5) Self-review with the rule catalogue (Simpler: 2-minute checklist; Deeper: anticipating the evaluator and the human interviewer's likely probes).
- **Mastery focus:** completeness under time pressure; adapting after probing; consistent use of numbers.
- **Canvas drawing:** three unseen prompts; version history shows probing deltas.
- **Rule checks:** full catalogue; deltas after probing must clear any critical finding introduced.
- **Badge criteria:** mastery ≥ 80%; three mock rounds with rubric ≥ 3.5 average and no critical findings remaining at the end of each round; evaluator-vs-self review gap (learner's self-found issues vs evaluator findings) improving across rounds.
- **Question-bank targets:** 12 / 6 / 4 / 8 / 3.

### 3.1 Reference design library (closed beta: 20 designs; public 1.0: 26; each has model solution, decision card, estimation sheet, rule-check expectations, probing tree)

**Core (12, required for the course badge and the only designs on SHOULD/NICE routes):** #1, 2, 3, 6, 8, 10, 11, 12, 15, 16, 17, 19. **Extended (8, optional depth, Deeper path and interview-company packs):** #4, 5, 7, 9, 13, 14, 18, 20. Reference learner note: #18 (IoT/MQTT) is Extended by default but is a showcase design for a learner with MQTT experience; the adaptive engine may promote it to Core for that learner (it replaces one Core design of the same module group, never adds to the badge count).

| # | Design | Module | Expected deep dive | Signature rule checks |
|---|---|---|---|---|
| 1 | URL shortener | M6 | ID generation and cache | R-CACHE-1, R-SHARD-1 |
| 2 | Rate limiter service | M6 | Distributed token bucket, atomicity | R-RATE-1, R-SPOF-1 |
| 3 | Distributed cache / KV store | M6 | Partitioning and replication | R-SHARD-1/2, R-SPOF-2 |
| 4 | Search autocomplete | M6 | Prefix index and update pipeline | R-CACHE-1, R-QUEUE-1 |
| 5 | File storage and sync | M6 | Chunking, metadata, dedupe | R-STORE-1, R-DR-1 |
| 6 | API gateway | M6 | Routing, auth, limits, config rollout | R-AUTH-1, R-RATE-1, R-SPOF-1 |
| 7 | Web crawler | M6 | Frontier and politeness | R-QUEUE-1/2, R-RATE-1 |
| 8 | Notification system | M7 | Dedupe, priorities, provider failover | R-IDEMP-1, R-QUEUE-2, R-TIMEOUT-1 |
| 9 | Real-time chat | M7 | Gateway routing, ordering, offline sync | R-FANOUT-1, R-SPOF-1 |
| 10 | News feed | M7 | Hybrid fan-out | R-FANOUT-1, R-CACHE-1 |
| 11 | Payment system | M7 | Ledger, idempotency, reconciliation | R-IDEMP-1/2, R-DUAL-1 |
| 12 | Booking / inventory | M7 | Holds and overselling | R-IDEMP-1, R-DUAL-1 |
| 13 | Job scheduler | M7 | Timers, leader election, visibility timeout | R-IDEMP-1, R-SPOF-1 |
| 14 | Leaderboard / counters / top-K | M7 | Sharded counters, windows | R-SHARD-1, R-CACHE-1 |
| 15 | RAG service | M8 | Ingestion, hybrid retrieval, ACLs | R-AI-3/4, R-TENANT-1 |
| 16 | LLM gateway | M8 | Routing, quotas, caching, fallback | R-AI-1/2, R-RATE-1 |
| 17 | Voice-agent platform | M8 | Latency budget, barge-in, scaling | R-AI-1/5, R-TIMEOUT-1 |
| 18 | IoT telemetry ingestion (MQTT) | M9 | Device identity, backpressure, dedupe | R-IOT-1, R-AUTH-1 |
| 19 | Multi-tenant SaaS platform | M9 | Isolation, noisy neighbour, metering | R-TENANT-1, R-RATE-1 |
| 20 | Metrics and analytics pipeline | M9 | Late data, aggregation | R-QUEUE-2, R-DR-1 |

Library plan (R-A6-3): closed beta ships these 20; public 1.0 ships 26 by adding six stretch designs authored in wave 3 after the pilot (tagged M6–M9, all Extended): 21 video streaming and transcoding, 22 ride matching and geo-search, 23 ad-click aggregation, 24 distributed lock and lease service, 25 collaborative document editing (OT/CRDT), 26 agent and workflow orchestration platform (seed supplied by B4; uses R-AGENT-1/2, R-COST-2, R-IDEMP-1). The B4 seed is a design brief only; durable-execution depth stays in B4.M7. Every design, including the six, carries decision card, estimation sheet, rule expectations and probing tree.

---

## 4. Seed interview questions (style-of-question items; not real questions from any named company)

Format per question: ideal outline, common weak answers, follow-up probes, difficulty, competency tag, company-style cluster. Per G-10 each seed also carries a goal tag and a senior-bar note: they are listed per question ID in section 8 LF-8 (table) to keep the seeds readable. Question IDs: `M<n>-Q<k>` = `Q-A6-M<n>-0<k>`. Each seed is a verbal item usable in voice drill and canvas probing. Items tagged `placement`: M1-Q2 and M10-Q1. Difficulty words map to the data-model integer scale 1-5 at import: easy 1, easy-medium 2, medium 3, medium-hard 4, hard 5 (same mapping as Track C); calibrated after the pilot.

### M1 — Framework and estimation

**M1-Q1. "Design a system for X. How will you structure the next 45 minutes?"**
- Ideal answer outline:
  - Spend about 5 minutes on requirements: functional list, non-functionals (latency, availability, consistency, scale), and an explicit out-of-scope list.
  - State assumptions and get confirmation, especially users, traffic, data retention, and geography.
  - Do estimation to find the dominant constraint (read QPS, write QPS, storage, or fan-out) rather than reciting numbers.
  - Sketch API and data model driven by access patterns, then draw a minimal high-level design that satisfies the stated requirements.
  - Choose one or two deep dives where the risk lies, and say why those were chosen.
  - Reserve the last 8–10 minutes for bottlenecks, failure modes, observability, security, cost, and trade-offs.
  - Narrate decisions in "option, benefit, cost, when I would switch" form and check in with the interviewer.
- Common weak answers: starts drawing databases immediately; lists technologies, not requirements; never states numbers; treats the framework as a script and ignores interviewer hints; no time management.
- Follow-up probes: "The interviewer says scale is 100x smaller than you assumed. What do you remove?" "Which assumption, if wrong, breaks your design?" "What would you cut if you only had 20 minutes?"
- Difficulty: medium. Tag: `design.framework`. Cluster: global-product.

**M1-Q2. "Estimate storage, QPS, and bandwidth for a photo-sharing service with 50 million daily active users."**
- Ideal answer outline:
  - Ask for or state: uploads per user per day, views per user per day, average photo size, retention, replication factor.
  - Convert daily to per-second with an explicit peak factor (for example 2–5x average) and say it.
  - Compute write QPS, read QPS, ingress and egress bandwidth, and 5-year storage including replication and thumbnails/variants.
  - Note that reads dominate, so CDN offload dominates the egress cost and the origin sizing.
  - Sanity check against what one node can do and conclude on fleet size and storage tier (object store with lifecycle).
  - Round sensibly, show units, and finish by stating the dominant constraint.
- Common weak answers: no units; forgets replication and variants; uses average instead of peak; no read:write ratio; false precision to many digits.
- Follow-up probes: "How does CDN hit rate change your origin numbers?" "What if 10% of photos are videos?" "Where is your estimate most likely wrong?"
- Difficulty: medium. Tag: `design.estimation`. Cluster: global-product, Indian-product.

**M1-Q3. "How do you decide the data model and API before choosing a database?"**
- Ideal answer outline:
  - List entities and relationships, then enumerate the top access patterns with their frequency and latency needs.
  - Derive keys and indexes from the queries (including sort and range needs) rather than the reverse.
  - Pick storage class per pattern: relational for transactions and joins, KV or wide-column for key access at scale, search index for text, object store for blobs.
  - Define API shapes: resource design, pagination (cursor over offset at scale), idempotency for writes, versioning.
  - Call out where one datastore is not enough and a derived store (search, cache) is fed asynchronously.
  - Discuss schema evolution and migration approach.
- Common weak answers: "NoSQL because it scales"; picks the database first; ignores pagination; one schema for all patterns.
- Follow-up probes: "A new query pattern appears that your key cannot serve. What now?" "When would you stay on Postgres?" "How would you page through 100M rows?"
- Difficulty: medium. Tag: `design.framework`. Cluster: global-product, IT-services, global-IT-consulting.

### M2 — Scaling reads and caching

**M2-Q1. "Your read-heavy product page is slow under load. Walk through how you scale it."**
- Ideal answer outline:
  - Measure first: identify whether DB, serialization, network, or a dependency dominates; name p95 and the target.
  - Make services stateless and scale horizontally behind an LB with health checks.
  - Add layers: CDN for static and cacheable responses, application cache (Redis-like) with cache-aside for hot entities, read replicas for the remainder.
  - State the TTL and invalidation approach, and the staleness the business tolerates.
  - Protect the origin: stampede protection, request coalescing, negative caching, rate limiting.
  - Instrument hit rate, evictions, replica lag, and origin QPS, and say what you would alert on.
  - Mention what degrades first if the cache fails and how you survive it (warm-up, load shedding).
- Common weak answers: "add Redis" with no invalidation; ignores replica lag; no measurement; ignores cold-cache failure.
- Follow-up probes: "The cache cluster restarts at peak. What happens?" "How do you handle a hot product that gets 10x traffic?" "How do you keep price changes from being stale?"
- Difficulty: medium. Tag: `design.caching`. Cluster: global-product, Indian-product, IT-services.

**M2-Q2. "Compare cache-aside, write-through, and write-behind. When is each wrong?"**
- Ideal answer outline:
  - Cache-aside: app reads cache, loads on miss, writes DB then invalidates; simple, tolerates cache loss, risk of stale reads and races.
  - Read-through: cache library loads on miss; moves logic out of app, same staleness characteristics.
  - Write-through: write to cache and DB synchronously; fresher reads, higher write latency, wasted caching of never-read items.
  - Write-behind: buffer writes and flush later; fast writes, risk of data loss and ordering issues; only for tolerant data (counters, view counts).
  - Race example: concurrent read-miss and update can repopulate stale data; mitigate with versioned keys, short TTL, or delete-after-write with delay.
  - Choose by consistency need and loss tolerance; state failure of each.
- Common weak answers: lists definitions only; says write-behind is "best performance" without the loss risk; missing the invalidation race.
- Follow-up probes: "Why delete the key rather than update it on write?" "What happens if the delete fails?" "How would you make the counter durable with write-behind?"
- Difficulty: medium. Tag: `design.caching`. Cluster: global-product, AI-startup.

**M2-Q3. "Your cache tier is cold after a deploy and the origin database is at risk. Design the tier-level protection."** (stampede mechanics are owned by Q-A5-M5-01; referenced, not re-taught)
- Ideal answer outline:
  - Frame it at system level: cold-start and thundering-herd risk across many app instances and several cache layers (CDN, app cache, Redis).
  - Protect the origin: admission control and per-key request coalescing at the cache tier, origin concurrency cap, load shedding with a degraded response.
  - Warm-up plan: pre-warm known hot keys before shifting traffic, staged rollout (canary then ramp), serve-stale policy per data class.
  - Failure design: what happens if the cache tier is down (R-CACHE-1/2/3 style checks on the canvas), capacity of the origin at 0 percent hit rate.
  - Observability: hit ratio, origin QPS and expiry-correlated spikes.
- Common weak answers: restates single-flight only; no origin capacity number; ignores deploy-time cold start.
- Follow-up probes: "How do you size the origin for a 0 percent hit rate?" "How do you roll back if the origin degrades mid-ramp?" For the mechanism itself, see Q-A5-M5-01.
- Difficulty: medium-hard. Tag: `design.caching`. Cluster: global-product, Indian-product.

### M3 — Data: partitioning, replication, consistency

**M3-Q1. "How do you pick a shard key for an orders table, and what goes wrong?"**
- Ideal answer outline:
  - Start from the dominant queries: by customer, by order ID, by merchant, by time.
  - Choose a high-cardinality, evenly distributed key aligned with the most frequent queries (often customer or tenant ID), and hash it to avoid skew.
  - Name the cost: queries on other keys become scatter-gather or need secondary index tables or a search/derived store.
  - Hot partitions (a large tenant or merchant): mitigate with key salting, tenant splitting, or dedicated shards.
  - Monotonic keys (time, auto-increment) create write hotspots under range sharding.
  - Resharding: consistent hashing or directory-based routing, online migration with dual writes and backfill, and verification.
  - Cross-shard transactions: avoid via design, or use sagas and idempotent steps.
- Common weak answers: shards by auto-increment ID or by created date; ignores cross-shard queries; no resharding plan.
- Follow-up probes: "A merchant has 40% of traffic. What now?" "How do you run a global report?" "How do you migrate without downtime?"
- Difficulty: hard. Tag: `design.scaling`. Cluster: global-product, Indian-product.

**M3-Q2. "Explain CAP and PACELC in practical terms. Where would you choose each trade-off?"**
- Ideal answer outline:
  - Partition tolerance is not optional in a distributed system; the real choice during a partition is consistency vs availability for a given operation.
  - Consistency in CAP means linearizability, not ACID's C.
  - PACELC adds that even without partitions there is a latency vs consistency trade-off (replication wait).
  - Per-operation choice: payments, inventory, uniqueness constraints lean consistent (CP-like); feeds, likes, presence lean available with eventual consistency.
  - Tunable systems: quorum reads/writes (R+W>N), read-your-writes sessions, bounded staleness.
  - Concrete example with the stated staleness bound and user-visible impact.
  - Caution that "CP" and "AP" labels on products are oversimplified.
- Common weak answers: "pick two of three"; labels databases without nuance; ignores latency.
- Follow-up probes: "How do you give users read-your-writes with replicas?" "What does eventual consistency mean for a shopping cart?" "How would a quorum system behave with one node down?"
- Difficulty: medium. Tag: `design.consistency`. Cluster: global-product, global-IT-consulting.

**M3-Q3. "Explain consistent hashing and why virtual nodes matter."**
- Ideal answer outline:
  - Keys and nodes hashed onto a ring; a key belongs to the next node clockwise.
  - Adding or removing a node moves only about 1/N of keys, versus nearly all with modulo hashing.
  - Without vnodes, distribution is uneven and a failing node dumps load onto one neighbour.
  - Vnodes (many points per node) smooth load, allow weighted capacity, and spread failover load.
  - Replication: next N distinct nodes on the ring hold replicas.
  - Costs: routing metadata, rebalancing traffic, hot keys still need separate handling.
  - Where used: caches, Dynamo-style stores, request routing to stateful services.
- Common weak answers: cannot explain why modulo is bad; omits vnodes; says it solves hot keys.
- Follow-up probes: "How many vnodes per node and what is the trade-off?" "How do clients discover the ring?" "How do you handle a node that is slow, not dead?"
- Difficulty: medium. Tag: `design.scaling`. Cluster: global-product, AI-startup.

### M4 — Async, idempotency, rate limiting

**M4-Q1. "Can a message queue give exactly-once delivery? How do you build safe processing?"**
- Ideal answer outline:
  - Delivery is at-most-once or at-least-once; exactly-once across independent systems is not achievable in general; some platforms offer exactly-once within a bounded scope (for example transactional produce-consume inside one streaming system; scope differs per platform: confirmed as a general principle, October 2026, check per platform before quoting a guarantee).
  - Design consumers as idempotent: natural idempotency (upsert by key) or a dedupe table keyed by message ID in the same transaction as the side effect.
  - Producer side: transactional outbox or CDC to avoid dual-write loss.
  - Acknowledge after the side effect is durable; handle redelivery after timeouts.
  - Retries with exponential backoff and jitter, retry limits, and a DLQ for poison messages with replay tooling.
  - Ordering: per-key partitions; do not assume global FIFO.
  - Monitor lag, redelivery rate, and DLQ depth.
- Common weak answers: claims the broker "guarantees exactly-once"; acks before processing; no dedupe key; ignores poison messages.
- Follow-up probes: "How long do you keep dedupe keys?" "The side effect is an external API call with no idempotency support. What now?" "How do you replay a DLQ safely?"
- Difficulty: hard. Tag: `design.messaging`. Cluster: global-product, Indian-product, global-IT-consulting.

**M4-Q2. "A payment flow spans your gateway, a PSP and your ledger. Where does idempotency live at each hop, and how do you reconcile?"** (the idempotency-key mechanism is owned by Q-A5-M4-01; referenced, not re-taught)
- Ideal answer outline:
  - System-level placement: key issued by the client, enforced at the service that owns the state change, derived keys for each PSP call, ledger entries unique per business operation.
  - What can be retried safely at each hop (R-IDEMP-1 sets the check) and what needs a deduplicating consumer.
  - Crash windows between PSP call and local commit: recovery job and reconciliation against PSP records; settlement reports.
  - Atomic write plus event publication placement (outbox is owned by A5.M8; `ref`).
  - Observability and alerting on duplicate or stuck operations.
- Common weak answers: re-describes a key table only; no cross-system reconciliation; assumes exactly-once delivery.
- Follow-up probes: "The PSP times out but charged the card. What does the ledger say?" "The client reuses a key with a different amount." "How do you prove no double charge to an auditor?"
- Difficulty: hard. Tag: `design.messaging`. Cluster: Indian-product, global-product.

**M4-Q3. "Design a rate-limiting service for a multi-tenant public API. Where does it live and how does it behave when it fails?"** (algorithms and the Redis limiter are owned by A5.M4 and Q-A5-M4-02; `ref`)
- Ideal answer outline:
  - Requirements: limit dimensions (user, API key, tenant, IP), per-tenant plans and overrides, latency budget, accuracy tolerance.
  - Algorithm choice by reference only: token bucket versus window variants (see A5.M4); this item is about the service around them.
  - Placement: gateway or edge for coarse limits, in-service for fine-grained limits, local pre-limit with global sync to cut Redis round trips; regional behaviour.
  - Failure policy: fail open or closed per endpoint (closed for expensive endpoints such as LLM calls), limiter not a SPOF (replication, local fallback), R-RATE-1 check.
  - Response contract: 429 with Retry-After; observability and per-tenant fairness; limiting by tokens consumed rather than requests for LLM-backed endpoints.
- Common weak answers: only names an algorithm; no failure policy; no per-tenant configuration.
- Follow-up probes: "Redis is down. What do you do?" "How do you rate limit across regions?" "How do you limit by tokens consumed rather than requests?"
- Difficulty: medium. Tag: `design.messaging`. Cluster: global-product, AI-startup.

### M5 — Reliability, observability, security, cost

**M5-Q1. "Take any design you have drawn. What fails first at 10x traffic, and how does it degrade?"**
- Ideal answer outline:
  - Identify the first bottleneck from the numbers (DB connections, hot partition, single queue, downstream rate limit), not generically.
  - Apply mitigation by layer: caching, replicas, partitioning, queue buffering, autoscaling, connection pooling.
  - Backpressure and load shedding: reject early with 429/503 for non-critical traffic, prioritise critical paths.
  - Graceful degradation: serve stale, disable expensive features, switch to read-only mode.
  - Avoid retry amplification: retry budgets, jitter, circuit breakers.
  - Capacity planning: headroom, load tests, autoscaling lag awareness.
  - Observability to see it coming: saturation metrics and leading indicators.
- Common weak answers: "autoscale everything"; no mention of the database or downstream limits; no degradation plan.
- Follow-up probes: "Autoscaling takes 3 minutes. What protects you in the meantime?" "What do you shed first?" "How would you test this before it happens?"
- Difficulty: hard. Tag: `design.reliability`. Cluster: global-product, Indian-product.

**M5-Q2. "How do you define and use SLOs for a user-facing service?"**
- Ideal answer outline:
  - Pick SLIs reflecting user experience: availability (successful requests ratio), latency (percentile thresholds), correctness/freshness where relevant.
  - Set SLO targets from user needs and cost, with a time window (for example 28 or 30 days); the error budget is 1 minus the SLO.
  - Alert on symptoms using burn-rate (fast and slow windows), not on every cause metric.
  - Use the error budget to govern release pace and reliability work.
  - Instrumentation: RED metrics per endpoint, traces for latency breakdown, structured logs with correlation IDs.
  - Distinguish SLO (internal target), SLA (contractual), and SLI (measurement).
  - Beware high-cardinality labels and averaging latency.
- Common weak answers: defines "99.99%" without justification; alerts on CPU only; confuses SLA with SLO; uses average latency.
- Follow-up probes: "What does 99.9% allow per month?" "What if dependencies have worse SLOs than yours?" "How do you set an SLO for an LLM-backed feature?"
- Difficulty: medium. Tag: `design.observability`. Cluster: global-product, global-IT-consulting, Big4.

**M5-Q3. "Add security and cost to your design in three minutes."**
- Ideal answer outline:
  - Security: authenticate at the edge (OIDC/JWT validation at the gateway), authorise per resource in the service, TLS in transit and encryption at rest, secrets in a manager, least-privilege IAM, network segmentation.
  - Data protection: PII classification, minimisation, retention, audit logging, tenant isolation.
  - Abuse and threats: rate limiting, WAF, input validation, dependency and supply-chain hygiene; for AI features, prompt injection and tool permission boundaries.
  - Cost: name the top 2–3 drivers (compute, egress, storage, managed DB, LLM tokens), and the lever for each (caching, tiering, right-sizing, batching, model routing).
  - Unit economics: cost per request/user/minute and how it scales.
  - State which trade-off you would make first if the budget were halved.
- Common weak answers: "use HTTPS and JWT" only; no tenant isolation; no numbers on cost; ignores egress.
- Follow-up probes: "How would you prevent one tenant from running up your LLM bill?" "Where would you put encryption keys?" "What is the cheapest change that reduces cost by 30%?"
- Difficulty: medium. Tag: `design.security`. Cluster: global-IT-consulting, Big4, AI-startup.

### M6 — Classic designs I

**M6-Q1. "Design a URL shortener that handles 100 million new URLs per month and heavy reads."**
- Ideal answer outline:
  - Requirements: create short URL, redirect, optional custom alias and expiry, analytics; latency for redirect under about 50 ms at p95; high availability.
  - Estimate: writes about 40/s average, reads 100x writes, so thousands of reads/s at peak; storage per record small, years of retention still modest.
  - ID generation: base62 of a unique counter (range allocation per app node to avoid a central hot spot) or random 7-char with collision check; avoid sequential guessability if privacy matters.
  - Data model: key-value by short code to long URL plus metadata; relational or KV store works; cache-aside with CDN/edge for popular codes.
  - Redirect semantics: 301 vs 302, trade-off with analytics and ability to change targets.
  - Analytics decoupled via queue; abuse prevention (rate limits, malicious URL scanning).
  - Failure modes: cache loss, ID allocator outage (pre-allocated ranges), expired link cleanup.
- Common weak answers: hash-and-truncate without collisions; single auto-increment DB as bottleneck; synchronous analytics writes in the redirect path.
- Follow-up probes: "How do you generate IDs across regions?" "Custom aliases collide. How?" "How would you support click analytics at 50k/s?"
- Difficulty: easy to medium. Tag: `design.classic`. Cluster: global-product, Indian-product, IT-services.

**M6-Q2. "Design search autocomplete for an e-commerce site."**
- Ideal answer outline:
  - Requirements: sub-100 ms suggestions, ranking by popularity and personalisation, freshness for trending terms, typo tolerance optional.
  - Data structure: trie or prefix index with top-K suggestions precomputed per prefix; or an inverted/edge-n-gram index in a search engine.
  - Serving path: edge/CDN and cache for short prefixes (very high hit rate), service reading a replicated in-memory index.
  - Offline/near-real-time pipeline: query logs to stream to aggregation (counts with time decay) to index build and atomic swap.
  - Handling scale: shard by prefix range or hash; replicate for reads; limit to first few characters via cache.
  - Quality: filtering unsafe terms, blocklists, personalisation as a re-rank layer.
  - Failure: fall back to the last good index; never block typing on suggestions (client debounce and cancellation).
- Common weak answers: runs `LIKE 'prefix%'` on the OLTP database; recomputes top-K at query time; no update pipeline.
- Follow-up probes: "How fresh must trending terms be and how do you achieve it?" "How do you support multiple languages?" "Memory for the trie is too large. What now?"
- Difficulty: medium. Tag: `design.classic`. Cluster: Indian-product, global-product.

**M6-Q3. "Design file storage and sync, such as a cloud drive. Focus on large files and conflicts."**
- Ideal answer outline:
  - Split metadata (relational/strongly consistent store) from blob data (object store); never route file bytes through the metadata DB.
  - Chunking (fixed or content-defined), hashing per chunk, deduplication across users where policy allows.
  - Resumable, parallel uploads with pre-signed URLs and chunk acknowledgements; finalise on a manifest commit.
  - Sync: change feed per user/device (cursor-based), long-poll or push notification, delta transfer of changed chunks.
  - Conflicts: versioning with last-writer-wins plus conflict copies, or per-file vector/version checks; state the policy.
  - Durability and cost: replication/erasure coding in the object store, lifecycle tiers, soft delete and retention.
  - Security: per-user ACLs, encryption, sharing links with expiry; virus scanning via async pipeline.
- Common weak answers: stores blobs in a SQL table; re-uploads whole file on edit; ignores conflicts and resumability.
- Follow-up probes: "Two devices edit offline. What does the user see?" "How do you dedupe without leaking whether someone else has the file?" "How do you delete data for compliance?"
- Difficulty: hard. Tag: `design.classic`. Cluster: global-product.

### M7 — Classic designs II

**M7-Q1. "Design a notification system (email, SMS, push) at 10 million notifications per day."**
- Ideal answer outline:
  - Requirements: multi-channel, priority classes (OTP vs marketing), user preferences and opt-outs, templates, scheduling, no duplicates, delivery status.
  - Flow: producer API with idempotency key to notification service to validation and preference check to per-channel queues with priority lanes to channel workers to providers.
  - Provider integration: timeouts, retries with backoff, circuit breaker, failover to a secondary provider, rate limits per provider; webhook receipts to update status.
  - Dedupe and idempotency: dedupe key per user and event; at-least-once delivery means duplicate risk is managed at the worker with a send-ledger.
  - Fan-out for broadcasts: chunked batches, throttled to protect providers and user experience.
  - Data: template store, preference store, delivery log (append-heavy, TTL), analytics async.
  - Observability: queue lag per channel, delivery success rate, provider error rate; DLQ and replay.
  - Compliance: quiet hours, consent, regional rules, PII minimisation in logs.
- Common weak answers: single queue for all priorities (marketing blocks OTP); no dedupe; synchronous provider calls in the API path; no failover.
- Follow-up probes: "OTP delivery is slow during a marketing blast. Why and how to prevent?" "The SMS provider goes down." "How do you guarantee a notification is not sent twice?"
- Difficulty: medium. Tag: `design.classic`. Cluster: Indian-product, global-product, IT-services.

**M7-Q2. "Design a seat or inventory booking system that must never oversell."**
- Ideal answer outline:
  - Requirements: browse availability (eventually consistent is fine), hold seats for a short time, pay, confirm; strong correctness at the hold/confirm step.
  - Data model: seat/inventory rows with state (available, held with expiry, sold) and version.
  - Concurrency control: conditional update (compare-and-set) or `SELECT ... FOR UPDATE` (with `SKIP LOCKED` for assignment queues) in the system of record; or Redis-based hold with TTL plus authoritative DB check; use fencing tokens if using distributed locks.
  - Hold expiry: TTL plus a sweeper job; release on payment failure or timeout.
  - Payment saga: hold, pay (idempotent), confirm, with compensation (release) on failure; idempotency keys throughout.
  - Scale hot events: queue-based admission (waiting room), per-event partitioning, caching of availability reads with short TTL.
  - Failure modes: payment succeeds after the hold expired (reconcile and refund or reassign policy), duplicate callbacks.
- Common weak answers: check-then-update without atomicity; distributed lock with no expiry/fencing; no hold timeout; availability reads hitting the primary under flash-sale load.
- Follow-up probes: "100k users want 1,000 seats at the same instant." "Payment webhook arrives after the hold expires." "How do you test for oversell?"
- Difficulty: hard. Tag: `design.classic`. Cluster: Indian-product, global-product.

**M7-Q3. "Design a news feed. How do you handle accounts with millions of followers?"**
- Ideal answer outline:
  - Requirements: post, follow, timeline read with low latency, freshness expectations, ranking optional; read-heavy.
  - Fan-out on write: precompute followers' timelines (fast reads, heavy write amplification); fan-out on read: assemble at read time (cheap writes, slow reads).
  - Hybrid: push for normal accounts, pull-and-merge for high-follower accounts above a threshold.
  - Storage: per-user timeline as a capped list/sorted set in a cache or wide-column store holding post IDs; hydrate post content from a post store/cache.
  - Async fan-out via queues with batching and priorities (active users first), backpressure and bounded work per post.
  - Pagination with cursors; ranking as a separate service with a time/engagement score; handling deletes and edits.
  - Failure/scale: lag in fan-out is acceptable but measured; backfill on follow; cold-start users.
- Common weak answers: only fan-out on write; ignores the celebrity problem; offset pagination; no mention of hydration.
- Follow-up probes: "A celebrity posts and 20M timelines need updating." "How do you unfollow or delete a post?" "How would you add ranking without slowing reads?"
- Difficulty: medium to hard. Tag: `design.classic`. Cluster: global-product, Indian-product.

### M8 — AI-system designs

**M8-Q1. "Design a multi-tenant RAG service over customer documents with per-document access control."**
- Ideal answer outline:
  - Requirements: ingest varied documents, answer with citations, tenant isolation, ACL-aware retrieval, freshness, latency target (for example time-to-first-token), cost.
  - Ingestion pipeline: upload to object store, queue, parser/extractor, chunker, embedding workers (batched, rate-limited), index write; idempotent per document version; DLQ and reprocessing.
  - Index design: vectors plus metadata (tenant, document ID, ACL principals, version) in a vector store (pgvector or a dedicated engine) with hybrid keyword search; tenant filter enforced server-side, not by the client.
  - Query path: authn, rewrite/expand query, hybrid retrieval, ACL filter, rerank, assemble prompt within a context budget, call LLM through the gateway, stream response with citations.
  - Freshness: change events trigger re-chunk/re-embed; delete propagation including vectors; model/embedding version migration plan.
  - Safety: prompt injection in documents (treat retrieved text as untrusted), no tools with side effects without guard, PII handling.
  - Evaluation and observability: retrieval metrics, answer faithfulness sampling, trace per request (retrieved chunks, tokens, latency, cost), regression set before changing models or chunking.
  - Cost levers: caching, smaller models for rewriting, truncation of context, batch embedding.
- Common weak answers: "put docs in a vector DB and call the LLM"; no ingestion/refresh plan; ACLs applied after retrieval only in the UI; no evaluation.
- Follow-up probes: "A document is deleted. When does it stop appearing?" "Retrieval returns confidential chunks from another tenant. How could that happen and how do you prevent it?" "You change embedding models. How do you migrate?"
- Difficulty: hard. Tag: `design.ai-systems`. Cluster: AI-startup, global-product, global-IT-consulting.

**M8-Q2. "Design an LLM gateway used by many internal teams and external tenants."**
- Ideal answer outline:
  - Goals: one API in front of multiple providers/models, cost control, reliability, governance, observability.
  - Request path: authn, tenant and key resolution, policy checks (allowed models, data classification), rate limits by requests and tokens, budget check, cache lookup, router, provider adapter, streaming back, usage metering.
  - Routing and fallback: by model capability, cost, latency, and health; circuit breakers; fallback chains with consistent output-schema handling; timeouts and hedging with care for cost.
  - Caching: exact prompt cache, provider prompt-caching support, optional semantic cache with safety caveats (tenant scoping, staleness).
  - Reliability: idempotent retries (a retried generation may bill twice), retry budgets, queue for batch/async jobs, concurrency limits per provider quota.
  - Governance: PII redaction/detection, audit logs with retention policy, prompt/version registry, model allowlists.
  - Observability: tokens in/out, cost per tenant/feature, latency (time to first token and total), error classes, quality sampling.
  - Single-point-of-failure risk: the gateway itself needs multi-AZ, stateless nodes, and a degraded direct-path policy only if policy allows.
- Common weak answers: thin proxy with no budgets; logs raw prompts indefinitely; fallback that changes behaviour silently; ignores streaming.
- Follow-up probes: "One tenant suddenly sends 50x traffic." "Provider A returns 429 and B is 3x the price. What does routing do?" "How do you do a safe model upgrade?"
- Difficulty: hard. Tag: `design.ai-systems`. Cluster: AI-startup, global-product.

**M8-Q3. "Design a real-time voice-agent platform. Where does latency hide and how do you scale it?"**
- Ideal answer outline:
  - Pipeline: ingress (WebRTC/WebSocket or telephony SIP/PSTN), VAD and endpointing, streaming STT, LLM with tools, streaming TTS, audio egress; all streaming, overlapped, not sequential.
  - Latency budget by stage (for example network, endpointing, STT final, LLM time to first token, TTS first audio) with a p50/p95 target on end-of-speech to first audio, and measurement per stage; exact numbers are provider-dependent and none is quoted as fact; label any worked-example number as a placeholder.
  - Barge-in: detect user speech during playback, cancel TTS and the in-flight LLM generation, flush audio buffers, update conversation state with what was actually spoken.
  - Session state: per-call state in a fast store or sticky worker; reconnect handling; transcript persistence async.
  - Scaling: capacity is concurrent calls per worker (CPU/GPU, connection limits), autoscaling on concurrency, regional placement near users and providers, warm pools; provider rate limits and quotas.
  - Reliability: fallback STT/TTS/LLM providers, degraded modes (shorter answers, text fallback), timeouts per stage, graceful call termination.
  - Compliance and cost: recording consent, retention, PII redaction; cost per minute from STT, LLM, TTS, and telephony; budgets per session.
- Common weak answers: sequential STT then LLM then TTS with no streaming; no barge-in design; one provider with no fallback; ignores concurrency as the capacity unit.
- Follow-up probes: "p95 latency doubled after a provider change. How do you find out why?" "How do you handle 5,000 concurrent calls at 9 AM?" "What do you store from the call and for how long?"
- Difficulty: hard. Tag: `design.ai-systems`. Cluster: AI-startup, Indian-product, global-product.

### M9 — IoT and multi-tenant SaaS

**M9-Q1. "Design telemetry ingestion for 1 million devices using MQTT."**
- Ideal answer outline:
  - Estimate: devices times message rate gives messages/s; add payload size, connection count (persistent connections are the limiting resource), and reconnect storm scenarios.
  - Device identity: per-device X.509 certificates or tokens, mutual TLS, topic-level ACLs so a device can publish only to its own topics.
  - Topic design: hierarchical (tenant/device/type), avoiding unbounded wildcard subscriptions on the server side.
  - QoS: 0 for lossy high-rate data, 1 for at-least-once (needs dedupe), 2 rarely worth its cost; retained messages for last-known state; last-will for presence.
  - Broker tier: clustered MQTT broker or managed IoT service (limits and pricing vary and change; teach the sizing method, quote no limits as fact; if a worked example uses one, label it a dated placeholder), shared subscriptions for consumer scaling, bridge to a durable stream (Kafka-like) for processing.
  - Processing: stream consumers validate, dedupe by device ID and sequence number, enrich, write to time-series store with retention/downsampling tiers, route alerts.
  - Reliability: device-side buffering when offline, exponential backoff with jitter on reconnect, backpressure from stream to broker, DLQ for malformed data.
  - Command and control: separate downlink topics with acks and timeouts; device shadow/state sync pattern.
  - Observability: connected devices, publish rate, consumer lag, per-fleet error rates.
- Common weak answers: broker as the database; QoS 2 everywhere; shared credentials across devices; no plan for reconnect storms or duplicates.
- Follow-up probes: "A regional outage ends and all devices reconnect at once." "How do you rotate certificates for 1M devices?" "Devices send out-of-order timestamps. What do you do?"
- Difficulty: hard. Tag: `design.classic`. Cluster: global-product, global-IT-consulting, IT-services.

**M9-Q2. "Choose a tenancy model for a B2B SaaS and explain isolation and noisy-neighbour control."**
- Ideal answer outline:
  - Models: pool (shared everything with tenant ID), bridge (shared compute, separate schema/DB), silo (dedicated stack per tenant); choose by compliance needs, tenant size distribution, cost, and operational capacity.
  - Data isolation: tenant ID on every row and cache key, enforced by row-level security or a data-access layer, with tests; per-tenant encryption keys for sensitive tiers.
  - Compute isolation: per-tenant quotas, rate limits, concurrency caps, separate queues or fair scheduling for heavy jobs, optional dedicated cells for large tenants.
  - Noisy neighbour: detect via per-tenant metrics, throttle or move to a dedicated cell.
  - Cell-based architecture: limit blast radius by routing tenants to independent cells.
  - Operations: tenant-aware migrations, onboarding automation, per-tenant backups/restore expectations.
  - Metering and cost attribution: usage events per tenant feeding billing and showing cost-to-serve.
  - Security: authz scoped by tenant, audit logs, prevention of cross-tenant leaks (a critical severity risk).
- Common weak answers: tenant ID only in the API layer; shared cache keys without tenant; same limits for all; silo for everyone without cost thinking.
- Follow-up probes: "A tenant needs data residency in India." "How would you migrate a large tenant from pool to a dedicated cell?" "How do you test for cross-tenant leaks?"
- Difficulty: hard. Tag: `design.scaling` (secondary `design.security`, same owner, no separate credit). Cluster: global-product, AI-startup, global-IT-consulting.

**M9-Q3. "Design a pipeline that turns billions of product events per day into dashboards and alerts."**
- Ideal answer outline:
  - Requirements: freshness (seconds vs hours), accuracy needs, query patterns, retention, cost.
  - Ingest: SDK/collector to a durable log (partitioned), schema registry and validation, dedupe via event IDs.
  - Processing: stream processing for near-real-time aggregates (windowing, watermarks for late data) and batch for backfill/correctness (lambda or unified/kappa style trade-off).
  - Storage: raw events in an object store (cheap, replayable); aggregates in an OLAP/columnar store or time-series store for dashboards.
  - Late and duplicate data: event time vs processing time, allowed lateness, idempotent upserts; reprocessing capability.
  - Serving: pre-aggregation and caching for dashboards, query limits per tenant/user.
  - Reliability and operations: consumer lag alerts, DLQ, schema evolution policy, data quality checks.
  - Cost levers: sampling, tiered retention, compaction.
- Common weak answers: writes events straight into the OLTP DB; ignores late data and duplicates; no raw replay path.
- Follow-up probes: "A bug miscomputed a metric for a week. How do you fix history?" "How do you pick window sizes?" "What do you do if an event schema changes?"
- Difficulty: medium to hard. Tag: `design.classic`. Cluster: global-product, Indian-product.

### M10 — Canvas craft, probing, and capstone

**M10-Q1. "Here is your diagram. What happens if this database fails?" (live probe on the learner's own graph)**
- Ideal answer outline:
  - Locate the component on the diagram and restate the user-visible blast radius before answering.
  - Say whether it is a SPOF; if yes, state the fix on the canvas (replica, automatic failover, multi-AZ) and make the edit live.
  - Quantify RPO and RTO for the chosen mechanism, and name failover time and what happens to in-flight writes.
  - Cover read-path degradation (stale cache, read-only mode) and write-path behaviour (queue and retry, fail fast).
  - Mention detection and alerting, and how failover is tested.
  - Note the new costs and consistency implications (async replica data loss, split-brain prevention).
  - Update numbers and annotations so the diagram stays internally consistent.
- Common weak answers: "it won't fail"; verbal-only answer with no canvas change; says "add replicas" without failover or data-loss reasoning.
- Follow-up probes: "Sync or async replication, and what does that cost you?" "Both AZs lose connectivity to each other." "How do you rehearse this?"
- Difficulty: medium. Tag: `design.probing`. Cluster: global-product, Indian-product.

**M10-Q2. "The interviewer now says traffic is 10x and you must add multi-region. How do you adapt?"**
- Ideal answer outline:
  - Clarify what multi-region means: latency for global users, DR, or data residency; each leads to a different design.
  - Re-run the numbers and identify what breaks first (DB writes, cache hit rate, cross-region replication bandwidth).
  - Options: active-passive DR with RPO/RTO, active-active with partitioned ownership (home region per user/tenant), or global tables with conflict resolution; state consistency consequences.
  - Traffic routing (geo DNS/anycast), regional caches, replicated object store, regional queues.
  - Cross-region failure modes: replication lag, split-brain, failover testing, data sovereignty.
  - Cost impact: inter-region transfer, duplicated infrastructure; propose a phased rollout.
  - Show the edited canvas with region boundaries and annotated replication links.
- Common weak answers: "just deploy in two regions"; ignores write conflicts; no cost or rollout.
- Follow-up probes: "Which data can tolerate eventual consistency across regions?" "How do you fail back?" "What would you not make multi-region?"
- Difficulty: hard. Tag: `design.probing`. Cluster: global-product.

**M10-Q3. "You have 30 minutes instead of 45 for a consulting-style solution design for a client's order platform. How does your approach change?"**
- Ideal answer outline:
  - Spend more time on business context, constraints, existing systems, team skills, compliance, and delivery risks than on deep scale math.
  - Present a layered architecture with clear build-versus-buy decisions and managed services where sensible.
  - Show a phased roadmap (MVP, hardening, scale) and migration path from the current system.
  - Cover non-functionals as client-facing commitments: SLAs, security, DR, cost estimate ranges, operational ownership.
  - Identify risks and mitigations and what would need discovery before committing.
  - Keep one technical deep dive (for example integration and data consistency between order, inventory, and payment) to prove depth.
  - Communicate trade-offs in stakeholder language alongside technical detail.
- Common weak answers: gives a product-company scale deep-dive only; ignores migration and team constraints; no cost or risk discussion.
- Follow-up probes: "The client's team only knows Java. Does your design change?" "How do you phase delivery to reduce risk?" "How would you explain the consistency trade-off to a business stakeholder?"
- Difficulty: medium. Tag: `design.probing`. Cluster: global-IT-consulting, Big4, IT-services.

---

## 5. Final assessment

| Component | Description | Scoring |
|---|---|---|
| Mastery check | 25 items across M1–M10 (concept, predict-the-result, estimation arithmetic), 80% pass (G-7), adaptive weighting toward weakest modules. Passing it at 85% together with a Senior-bar voice mock is the only whole-course test-out route (G-5) | Pass/fail with module-level breakdown |
| Timed coding set | 2 tasks in 45 minutes: A6-distinct items (consistent hashing with vnodes, LRU with TTL and single-flight, outbox poller) or A5/A3 items reused by `ref` (token bucket, idempotency middleware: canonical in A5.M4), hidden tests including concurrency and duplicate cases | >= 70% of hidden tests plus correct stated complexity (G-7), and code-quality rubric |
| Canvas design task 1 (45 minutes, text or voice) | Unseen classic prompt (for example a notification, booking, or chat system), full framework, estimates entered in the calculator, evaluator scoring against D1–D11 with rule-check report | Rubric overall ≥ 3.5, no dimension under 3, no critical findings at the end |
| Canvas design task 2 (45 minutes) | Unseen AI-system prompt (for example RAG for a regulated domain, LLM gateway, or voice agent) with live probing by the interviewer (two probes minimum, deltas scored) | Same thresholds plus R-AI checks clear |
| Voice mock round (20 minutes) | Verbal design discussion with adaptive follow-ups: framework narration, trade-off defence, failure and cost questions; includes one "design a system from your own project" defence (links to C2) | Rubric D1, D6, D7, D10, D11 plus delivery-structure coaching (non-judgemental, no emotion inference) |
| Report | Competency radar across the 11 dimensions, annotated diagram with missed considerations, comparison to reference overlay, study plan mapped back to modules | Feeds the Interview Master readiness trend |

Calibration note (R-A6-4): before the final assessment is published, on a labelled set of at least 30 designs the evaluator score must be within +/-0.5 of the SME mean on at least 80 percent of dimension scores with rank correlation >= 0.8 `[judgement]`, tuned after the pilot. Owner: Interview Master team; A6 supplies the labelled designs (spec in section 7.3). Rubric thresholds: overall >= 3.5, no dimension < 3, no critical findings; readiness additionally needs Senior-bar voice results in two sessions at least 48 hours apart (G-6).

---

## 6. Course-level risks and freshness notes

| Area | Risk / what changes fast | Handling |
|---|---|---|
| AI-system designs (M8) | Model families, context sizes, prices, latency, and provider features (prompt caching, realtime speech APIs, batch APIs) change quickly; cost numbers date within months | Use ranges and formulas, not price quotes; date-stamp every number; review every 3 months |
| Managed-service limits and pricing | Limits and prices of managed streaming, IoT brokers, vector databases, and serverless platforms change | Teach mechanisms and sizing method; label any limit used in a worked example as a dated placeholder |
| Streaming platform semantics | **Verified October 2026 (project pages and release coverage, secondary sources):** Apache Kafka 4.0 (released March 2025) removed ZooKeeper mode entirely; KRaft is the only metadata mode; the 4.x line continues (4.1 and later). Queues for Kafka (KIP-932, share groups) arrived as early access in 4.0 and was recommended for testing, not production, at that point; it is described as production-ready from Kafka 4.2 (released 17 February 2026), and 4.3 (4.3.1 on 25 June 2026) is the current line per kafka.apache.org (verified 2026-10; Confluent and Apache release pages). Exactly-once scope differs per platform (transactional scope inside one platform; end-to-end still needs idempotent consumers) | Keep platform-neutral in lessons; the Challenge lesson may state the dated facts above; any newer version claim needs a version stamp |
| Redis and licensing/forks | **Verified October 2026 (vendor and press sources):** Redis moved to source-available licences in 2024 and added the OSI-approved AGPLv3 as an option with Redis 8 (May 2025; tri-licence RSALv2 / SSPLv1 / AGPLv3); Valkey is a Linux Foundation BSD-3 fork; Redis Software 7.2 reached end-of-life on 28 February 2026. Command semantics (Lua, functions) stay stable | Say "Redis-compatible (Redis or Valkey)" in designs; licensing is a vendor-risk note only, not legal advice |
| MQTT versions | **Verified October 2026 (cloud vendor documentation):** at least one major managed IoT service supports shared subscriptions on both MQTT 3.1.1 and MQTT 5, queues QoS 1 messages for persistent-session groups with documented dequeue-rate limits, and caps subscriptions per SUBSCRIBE request. Exact quotas change and vary by provider: not quoted as fact in lessons | Teach protocol concepts for both versions (session expiry, shared subscriptions standardised in 5, reason codes); platform support is a check item per provider; quotas only from the dated provider docs |
| Database capabilities | Distributed SQL and serverless relational offerings change what "needs sharding" means. "How far does one Postgres go" is restated as **orders of magnitude only** (for example: tens of thousands of simple queries per second on a well-tuned large node, working set in RAM, TB-scale tables with partitioning; all workload-dependent) against PostgreSQL 18.x; treat as teaching heuristics, never as benchmarks | Keep the lesson number-based, labelled "orders of magnitude, workload-dependent", refreshed yearly |
| Estimation constants | Hardware numbers (latency, throughput, single-node limits) drift | Use round, conservative numbers and say they are orders of magnitude; review yearly |
| Evaluator quality | Rule checks can false-positive on legitimate designs (for example a deliberate single-AZ internal tool) | Severity levels, "stated non-goal" suppression, human review sampling, never present a single "right shape" |
| Typed-graph vocabulary | Adopted as v0.1 with amendments (R-A6-1); the canvas spec must mirror section 1.1 and 1.3 `[verify]` acceptance by the canvas spec before visuals are authored | Registry changes go through A6 with permanent IDs |
| Interview-style drift | Company-style expectations vary; consulting vs product loops differ | Company packs carry style notes; this course stays technically neutral and says "style" only |
| Content originality | Classic designs are widely discussed publicly | Original decision cards, estimates, and probing trees; no copied text; no real company questions |
| Pedagogy | Risk of memorising designs instead of reasoning | Unseen-prompt mocks in M10; evaluation weights trade-offs and probing resilience |

Review cadence summary: AI and managed-service content every 3 months; core theory and designs every 12 months; question-bank calibration after pilot and after each evaluator/rubric version change.

---

## 7. Registry support material (rule examples, profiles, calibration, placement, cost card)

### 7.1 Good and weak example graph for each new rule (tests for the rule engine; compact descriptions)

| Rule | Weak graph (must fire) | Good graph (must not fire) |
|---|---|---|
| R-AGENT-1 | `ai` node with tool `refund_order` (side-effect = yes) called directly from the LLM edge, no approval, no scope limit | Same tool behind an edge with `human-approval` = yes above an amount threshold and a per-session scope limit noted |
| R-AGENT-2 | Agent `service` loops LLM to tool to LLM with `step-budget` unset and no timeout | `step-budget` = 8, per-run token cap and wall-clock timeout noted |
| R-TENANT-2 | Multi-tenant RAG: `datastore` (vector) with `tenant-scoped` = no and a retrieval edge with no filter or ACL step | Vector index `tenant-scoped` = yes, tenant and ACL filter applied server-side in the retrieval service before rerank |
| R-PII-1 | `datastore` pii = yes feeds prompts to `external` LLM with no redaction node | Redaction service node on the path, audit log, retention note |
| R-VOICE-1 | Voice pipeline STT to LLM to TTS with no cancel edge from VAD/session service to TTS and LLM | Cancel edge from session service to TTS and LLM on user speech, playback buffer flush noted |
| R-COST-2 | LLM `external` reached from a public endpoint with no budget or quota attribute | Per-tenant budget and metering node on the path, hard cap and alert noted |

### 7.2 "Solution-architecture" admin re-weighting profile (R-A6-5; a profile, not a second rubric)

Default weights: D1 10, D2 10, D3 10, D4 15, D5 15, D6 10, D7 10, D8 5, D9 5, D10 5, D11 5. Solution-architecture profile (Big4 and global-IT-consulting packs): **D1 15, D4 15, D10 10, D2 5, D5 10**; the remaining dimensions keep their default weights (D3 10, D6 10, D7 10, D8 5, D9 5, D11 5), so no rescaling is needed and the total is 100. Under this profile the evaluator also asks for a phased roadmap, build-versus-buy statement and client-facing non-functional commitments (M10 lesson 4).

### 7.3 Labelled calibration set specification (R-A6-4; at least 30 designs)

- Composition: 18 classic designs (stratified across M6-M7 designs), 8 AI-system designs (RAG, gateway, voice), 4 IoT or multi-tenant; each at three quality tiers (weak, competent, strong) with a minimum of 8 per tier.
- Per design record: prompt, typed-graph export (v0.1+), calculator inputs, transcript (voice or text), probing deltas where applicable, expected rule findings (ID and severity), SME scores D1-D11 from two independent SMEs plus adjudicated mean.
- Pass criteria `[judgement]`: evaluator within +/-0.5 of SME mean on at least 80% of dimension scores; rank correlation >= 0.8; rule-engine precision and recall on expected findings reported separately (deterministic rules must match 100% on the 6 rule-example graph pairs in 7.1 and on existing rules' tests).
- Owner: Interview Master team; A6 supplies designs, rubric anchors and SME scoring guide. No real candidate data; all designs authored originally.

### 7.4 Placement item pool (tagged `placement`, drawn from MCQ/predict buckets, not extra authoring)

System-design placement section uses M1 estimation items and M10 probing items; per-module diagnostics are listed in LF-2b. Placement skip decisions use G-5 bands.

### 7.5 Shared cost rubric card (D10; A6 owns, A7.M10 and B2.M9 supply inputs)

| Line | Score 3 shows | Score 5 adds |
|---|---|---|
| Drivers | Names top 2 cost drivers (compute, egress, storage, managed DB, LLM tokens) | Ranks drivers with order-of-magnitude numbers and shows which one dominates |
| Unit cost | States per-request, per-user or per-minute cost with assumptions | Shows how unit cost scales and where it breaks |
| Levers | One lever per driver (cache, tier, right-size, batch, route) | Quantifies lever impact and its reliability or quality cost |
| Cut order | States one trade-off | States what is cut first at half budget and what is never cut |
All numbers in lessons are labelled placeholders or come from the dated data file (G-12).

---

## 8. Learner-Fit Appendix (G-11; template section 6)

Reference learner: senior engineer, 11+ years, about 4 years Python, FastAPI/Django/Flask, SQL, Redis, Docker, AWS Lambda/S3, MQTT, LLM APIs and voice agents, targeting Senior Python Backend and AI Engineer roles. The layer is expressed as rules so other learners (L1-L4) are served.

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 (new) | Simpler, all lessons, prerequisites linked (A4, A5 first); 26 h + 4 h assessment; M8-M9 deferred until M1-M7 passed | 60% guided canvas with templates and structured-text mode, 25% calculator drills, 15% voice | Nothing; Core 12 designs only, Extended optional | None |
| L2 (junior-mid) | Simpler, skim lessons already known per placement; about 22-26 h | 50% canvas, 25% calculator, 25% verbal drills | Lessons banded skim | Optional cost card extension |
| L3 (experienced) | Deeper, skim foundations, study "explain at depth" lessons (shard-key reasoning, delivery semantics, SLO burn-rate); about 16-18 h | 55% timed canvas with probing, 20% voice, 15% coding, 10% calculator | Simpler-only lessons | Streaming essentials (M4.6), Extended designs, multi-region probe |
| L4 (senior/expert) | Deeper plus Challenge; skip to mastery check where scored >= 85; about 11-15 h net on the reference learner `[judgement]` | 65% unseen-prompt mocks with probing, 25% voice at Senior bar, 10% labs | Lessons at skip band | Challenge set: staff-level probes (cell architecture, metastable failure, evaluation-gated model upgrade), peer-review of a weak design |

### LF-2 Module matrix

Bands (G-5, default, calibrate after pilot): >= 85 skip lessons and take mastery check (plus optional Challenge); 70-84 skim (Deeper-only lessons plus mastery check); 50-69 study (standard path); < 50 deep-study (Simpler path plus remediation links in LF-5). No band waives the final assessment or M5/M10 canvas work. Interview weights are percent of system-design interview emphasis per target `[judgement]`; each column sums to 100.

| Mod | Diagnostics (LF-2b) | B/A/H tag | Weight B / A / H % | Rationale | Rehearsal modes | Module readiness (G-6 specialised) |
|---|---|---|---|---|---|---|
| M1 | 7 | B:M A:M H:M | 12 / 12 / 12 | Every design round opens with it | Voice drill (10-min requirements and estimate), canvas | Estimation within 3x on 4 of 5 timed problems; dominant constraint stated unprompted in two voice sessions |
| M2 | 6 | B:M A:M H:M | 10 / 8 / 9 | Core of read-heavy prompts; services firms probe caching | Canvas, voice drill | Product-page design zero critical findings; names three cache failure modes and cold-start impact |
| M3 | 7 | B:M A:M H:M | 12 / 9 / 11 | Separates senior from mid in product loops | Voice drill, canvas | Shard key and consistency model stated per data path; consistent-hashing task passes |
| M4 | 7 | B:M A:M H:M | 14 / 10 / 12 | Most probed senior topic across clusters (payments, notifications) | Voice drill, canvas, coding | Order pipeline zero critical; explains exactly-once honestly in voice at Senior bar |
| M5 | 6 | B:M A:M H:M | 12 / 11 / 11 | Failure, SLO, cost probes follow every design | Voice drill, canvas | "Fix the flawed design" clears critical and major findings; SLO-to-alert accepted |
| M6 | 6 | B:M A:S H:M | 12 / 8 / 10 | Classic opening prompts in product loops | Canvas (timed), voice | 5 of 7 designs zero critical; D4 and D5 >= 3 |
| M7 | 6 | B:M A:S H:M | 12 / 7 / 9 | Correctness-under-concurrency probes | Canvas (timed), coding | Payment and booking zero critical; seat-hold task passes |
| M8 | 7 | B:S A:M H:M | 4 / 20 / 12 | Dominant for AI Engineer loops | Canvas, voice | Three designs <= 2 major findings; cost model within 30%; unit cost per request and per voice minute stated |
| M9 | 6 | B:S A:S H:S | 4 / 5 / 5 | Selective (IoT and SaaS roles) | Canvas, voice | Both designs zero critical; dedupe consumer passes |
| M10 | 5 | B:M A:M H:M | 8 / 10 / 9 | Probing resilience decides the outcome | Canvas with probing, voice mock | Three unseen mocks average >= 3.5, no critical remaining; final assessment thresholds |

**LF-2b Placement diagnostics (item titles, all tag `placement`; IDs `Q-A6-Mn-P01..`)**

| Mod | Items (title, competency) |
|---|---|
| M1 | P01 order the eight steps and time boxes; P02 daily-active to peak QPS with stated factor; P03 five-year storage with replication and variants; P04 pick the dominant constraint from numbers; P05 read:write ratio changes the design; P06 single-node sanity check; P07 what to cut at 1/100 scale (`design.estimation`, `design.framework`) |
| M2 | P01 L4 vs L7 balancing; P02 read-your-writes with replicas; P03 pick cache pattern for a workload; P04 TTL vs event invalidation; P05 origin QPS when hit rate collapses; P06 CDN cache key and Vary (`design.caching`) |
| M3 | P01 shard-key choice; P02 key movement modulo vs ring; P03 quorum arithmetic; P04 CAP consistency vs ACID C; P05 PACELC trade-off; P06 UUIDv7 vs Snowflake IDs; P07 hot partition mitigation (`design.scaling`, `design.consistency`) |
| M4 | P01 exactly-once claim critique; P02 ack-after-effect; P03 dedupe in same transaction; P04 outbox vs dual write; P05 saga compensation order; P06 limiter fail-open vs closed; P07 partitions and ordering (`design.messaging`) |
| M5 | P01 error budget arithmetic; P02 burn-rate alert design; P03 retry amplification; P04 RPO and RTO tiers; P05 gray failure detection; P06 cost driver ranking (`design.reliability`, `design.observability`, `design.cost`) |
| M6 | P01 base62 capacity; P02 301 vs 302; P03 chunk dedupe; P04 crawler politeness; P05 trie vs inverted prefix; P06 gateway as failure domain (`design.classic`) |
| M7 | P01 OTP priority lane; P02 fan-out threshold; P03 ledger invariants; P04 oversell prevention; P05 fencing token; P06 visibility timeout for scheduler (`design.classic`) |
| M8 | P01 RAG ingest vs query path; P02 ACL pre-filter; P03 gateway budget and quota; P04 fallback chain consistency; P05 voice latency budget sum; P06 barge-in cancellation; P07 token cost estimate (`design.ai-systems`) |
| M9 | P01 QoS 0/1/2 meaning; P02 per-device identity; P03 reconnect storm; P04 tenancy model choice; P05 noisy neighbour control; P06 watermark and late data (`design.classic`, `design.scaling`) |
| M10 | P01 live probe on database failure; P02 multi-region clarification; P03 consulting vs product emphasis; P04 canvas label and number hygiene; P05 self-review checklist (`design.probing`, `design.canvas`) |

### LF-3 Experience bridge (from what you already know)

| Mod | You already know | Gap between using it and explaining it at senior depth |
|---|---|---|
| M1 | Sizing FastAPI/Django services and Postgres informally | Stating assumptions, peak factor and the dominant constraint aloud within five minutes, with units |
| M2 | Redis cache-aside in Django/FastAPI, CDN for static assets | Sizing hit rate, cold-start collapse, replica lag and read-your-writes, cache topology rationale |
| M3 | Postgres primary-replica, UUID keys | Shard-key reasoning, consistency vocabulary per data path, consistent hashing and why vnodes |
| M4 | Celery, SQS or Lambda triggers, Redis, retries | Precise delivery-semantics language, dedupe scope, outbox and saga trade-offs, limiter placement and failure policy |
| M5 | CloudWatch alarms, Docker, Lambda deploys | SLI/SLO and burn-rate framing, retry-amplification and metastable failure, unit economics |
| M6-M7 | Built notification, upload and webhook features | End-to-end design with estimates, decision cards and the one deep dive that matters; correctness under concurrency |
| M8 | LLM APIs, chatbots and voice agents in production | Generalising to a multi-tenant platform: ingestion pipeline, ACL-aware retrieval, gateway budgets, per-stage latency budget, quantified cost (your strongest area: expect skim, then use it as showcase) |
| M9 | MQTT experience, Lambda, S3 | 1M-device sizing, per-device identity at scale, tenancy architecture (pool/bridge/silo/cells) and noisy-neighbour control |
| M10 | Whiteboard talks inside teams | Drawing for an evaluator, narrating against a clock, answering probes with clean canvas deltas |

### LF-4 Senior-bar framing (what each level adds)

| Mod | Mid | Senior | Staff |
|---|---|---|---|
| M1 | Lists requirements, rough numbers | Dominant constraint identified, trade-off statements "option, benefit, cost, when I flip" | Challenges the premise, shapes scope with business impact, sets SLOs from user need |
| M2 | Adds Redis and replicas | Predicts failures (stampede, lag, cold cache), sizes it | Designs for blast radius and invalidation ownership across teams |
| M3 | Names sharding and CAP | Picks keys, states per-path consistency, resharding plan | Migration strategy with dual writes, verification and rollback; cost of consistency |
| M4 | Uses queues and retries | Idempotent consumers, outbox, honest exactly-once, limiter failure policy | Platform-level contracts (event schemas, replay policy), organisational ownership of DLQs |
| M5 | Adds monitoring and replicas | Mechanism per failure, SLO burn-rate alerts, cost levers | Game days, metastable-failure prevention, error budgets governing roadmap |
| M6-M7 | Draws boxes for known designs | Estimates, deep dive, failure modes per design | Evolves the design over time, rejects over-engineering, mentoring structure |
| M8 | Wraps an LLM API | Gateway budgets, ACL retrieval, latency budget, fallback, evaluation gate | Governance, cost per tenant strategy, model-upgrade risk management |
| M9 | Uses MQTT or pool tenancy | Identity, backpressure, dedupe, isolation and metering | Cell strategy, residency, tenant migration, cost-to-serve policy |
| M10 | Answers probes defensively | Adapts with clean deltas, time-boxes | Steers the conversation, frames trade-offs for non-technical stakeholders |

### LF-5 Adaptive hooks

- Mastery thresholds: module check 80% (G-7); readiness 85% on a retake spaced >= 7 days (G-6).
- Remediation links: M1 -> A4.M3 (access patterns), A7.M6 (networking basics); M2 -> A5.M5 (caching mechanics); M3 -> A4.M5-M7; M4 -> A5.M4, A5.M8, A2.M9; M5 -> A7.M9, A7.M10, A8.M9; M6-M7 -> A5.M4, A4.M6, A3 (LRU); M8 -> B2.M1-M5, B3.M1, B6.M6 (latency workbook); M9 -> A4.M2 (RLS), A7.M5; M10 -> M1-M5 weakest dimension.
- Challenge extensions: M4.6 streaming essentials, Extended designs, multi-region and cell-architecture probes; staff-bar probes in LF-4 shown to learners scoring >= 90.
- Reordering: the adaptive engine may defer M8 until B2.M1-M5 and one of B3/B4/B6 are passed (M1-M7, M9-M10 do not wait); may reorder M6/M7/M9 by goal tag (AI route moves M8 earlier; backend route moves M7 earlier); may not reorder M1 before M2-M5 or M10 before M5.
- SHOULD/NICE routes: Core 12 designs only; M9 as primer (NICE multiplier 0.25).

### LF-6 Applied labs (production-framed, one per module)

| Mod | Lab |
|---|---|
| M1 | Size an event-ticket flash sale for a regional SaaS (peak factor, dominant constraint, what to cut at 1/50 scale) |
| M2 | Add caching to a Django/FastAPI product-catalogue endpoint; simulate cache restart at peak and compute origin QPS |
| M3 | Shard a multi-tenant orders store with one tenant at 40% of traffic; design mitigation and migration |
| M4 | Order-to-fulfilment pipeline for an e-commerce backend: queue, DLQ, outbox poller; explain redelivery after a worker crash |
| M5 | Take a flawed design with a retry storm; fix findings, write the SLO alert and the cost card |
| M6 | Internal URL shortener and API gateway for an enterprise intranet: estimates, deep dive on ID generation or config rollout |
| M7 | Bookings and payments for a clinic or event app: seat hold, idempotent payment, webhook after hold expiry |
| M8 | LLM gateway with per-tenant budgets for a SaaS AI assistant; voice-agent platform for a support line using B6's latency workbook numbers (harness P-07, P-08) |
| M9 | Device-telemetry ingestion for an IoT fleet with reconnect storm and per-tenant cells |
| M10 | Unseen enterprise-platform prompt in two styles (product depth and consulting solution) with scripted probes |

### LF-7 Currency and quality bar

| Mod | Stamp | Memorable visual or analogy | Top misconceptions handled |
|---|---|---|---|
| M1 | Stable; reviewed 2026-10 | "Funnel to the dominant constraint": estimation waterfall narrowing to one number | Boxes before requirements; false precision; average instead of peak |
| M2 | Stable; 2026-10 | "Library with a reading room": CDN, cache and stacks as nested rooms, a fire (cache restart) sends everyone to the stacks | "Add Redis" fixes it; cache is source of truth; replica reads are always fresh |
| M3 | Stable; 2026-10 | Consistent-hash ring as a clock face with vnode ticks | CAP "pick two"; consistency in CAP = ACID C; auto-increment shard key |
| M4 | Mechanism stable; platform facts dated 2026-10 | "Registered post with a signature slip": delivery receipts, duplicates and the dedupe ledger | Exactly-once delivery; retry without idempotency; dual write |
| M5 | Stable; 2026-10; security rules every 6 months | Heat map plus "budget burn" fuel gauge | "High availability" with no mechanism; retries everywhere; alerts on causes |
| M6 | Stable; 2026-10 | Decision cards as flash cards (key choice, rejected alternative, flip point) | Hash-truncate without collisions; blobs in DB; synchronous analytics |
| M7 | Stable; 2026-10 | Seat-hold timeline as a theatre ticket counter queue | Double charge on retry; global chat ordering; push fan-out for celebrities |
| M8 | AI content: reviewed 2026-10, next due 2027-01 (3-month cadence); volatile: model families, prices, provider features | Dual-pipeline "kitchen and dining room" (ingest vs query) and latency waterfall bars | LLM as reliable deterministic dependency; vector DB = RAG; ACLs only in UI |
| M9 | Mechanism stable; MQTT platform support dated 2026-10 | "Airport arrivals hall": reconnect storm as a flight bank landing together | QoS 2 everywhere; broker as database; tenant ID missing from cache keys |
| M10 | Stable; 2026-10 | "Good vs weak canvas" side-by-side and probing-delta replay | Silent drawing; arguing vs adapting; over-engineering for small scale |

### LF-8 Seed-question goal tags and senior-bar notes (G-10)

| ID | Goal tag | Senior-bar note (mid / senior / staff) |
|---|---|---|
| M1-Q1 | B:M A:M H:M | Mid: lists steps; senior: time-boxes and chooses deep dives with reasons; staff: reshapes scope using business impact |
| M1-Q2 | B:M A:M H:M | Mid: gives numbers; senior: peak factor, dominant constraint, sanity check; staff: ties estimate to cost and SLO |
| M1-Q3 | B:M A:M H:M | Mid: picks a DB; senior: access patterns first, derived stores; staff: schema evolution and ownership |
| M2-Q1 | B:M A:M H:M | Mid: adds Redis; senior: measures first, invalidation, cold-cache failure; staff: blast radius and ownership |
| M2-Q2 | B:M A:S H:M | Mid: definitions; senior: races and failure of each; staff: choose per consistency budget across services |
| M2-Q3 | B:M A:S H:M | Mid: use locks; senior: tier-level warm-up, origin cap and serve-stale policy; staff: origin sizing at 0 percent hit rate and rollout staging |
| M3-Q1 | B:M A:S H:M | Mid: hash user ID; senior: hot partitions, scatter-gather, resharding; staff: online migration and cost |
| M3-Q2 | B:M A:M H:M | Mid: pick two of three; senior: per-operation choice with staleness bound; staff: product-level consistency budget |
| M3-Q3 | B:M A:S H:M | Mid: ring picture; senior: vnodes, replication, discovery; staff: operational rebalancing cost |
| M4-Q1 | B:M A:M H:M | Mid: use a queue; senior: idempotent consumers, outbox, honest exactly-once; staff: platform contracts and replay policy |
| M4-Q2 | B:M A:S H:M | Mid: check for duplicates; senior: per-hop key placement across gateway, PSP and ledger; staff: reconciliation and audit evidence |
| M4-Q3 | B:M A:M H:M | Mid: fixed window; senior: service placement, failure policy and tenant plans (algorithms owned by A5.M4: reference there); staff: per-tenant fairness and cost |
| M5-Q1 | B:M A:M H:M | Mid: autoscale; senior: first bottleneck from numbers, shedding; staff: capacity governance and game days |
| M5-Q2 | B:M A:M H:M | Mid: define SLO; senior: burn-rate alerts, error budget; staff: budget-driven roadmap |
| M5-Q3 | B:M A:M H:M | Mid: HTTPS and JWT; senior: layered security plus unit cost; staff: tenant-level cost and risk policy |
| M6-Q1 | B:M A:S H:M | Mid: draws shortener; senior: ID allocation, redirect semantics, decoupled analytics; staff: abuse, multi-region ID strategy |
| M6-Q2 | B:S A:S H:S | Mid: LIKE query; senior: precomputed top-K and update pipeline; staff: freshness vs cost and multilingual |
| M6-Q3 | B:S A:N H:S | Mid: store files; senior: metadata/blob split, chunking, conflicts; staff: dedupe privacy and compliance deletion |
| M7-Q1 | B:M A:S H:M | Mid: one queue; senior: priority lanes, provider failover, dedupe ledger; staff: consent and cost at scale |
| M7-Q2 | B:M A:S H:M | Mid: check then update; senior: atomic hold, fencing, saga; staff: flash-sale admission and reconciliation policy |
| M7-Q3 | B:M A:S H:M | Mid: fan-out on write; senior: hybrid with threshold; staff: ranking and backfill economics |
| M8-Q1 | B:S A:M H:M | Mid: vector DB plus LLM; senior: ingestion, ACL filter, freshness, eval; staff: embedding migration and tenant economics |
| M8-Q2 | B:S A:M H:M | Mid: thin proxy; senior: budgets, routing, fallback, caching; staff: governance and safe model upgrades |
| M8-Q3 | B:N A:M H:M | Mid: STT to LLM to TTS; senior: streaming, barge-in, latency budget, concurrency scaling; staff: provider strategy and cost per minute |
| M9-Q1 | B:S A:S H:S | Mid: pub/sub; senior: identity, QoS, backpressure, dedupe; staff: certificate rotation and fleet economics |
| M9-Q2 | B:S A:S H:S | Mid: tenant ID column; senior: models, isolation, metering; staff: cell strategy and residency |
| M9-Q3 | B:S A:S H:S | Mid: events to DB; senior: stream plus batch, late data, replay; staff: schema governance and cost |
| M10-Q1 | B:M A:M H:M | Mid: "add a replica"; senior: edits canvas with RPO/RTO; staff: rehearsal and detection ownership |
| M10-Q2 | B:M A:M H:M | Mid: deploy second region; senior: clarifies the goal, ownership model; staff: phased rollout and cost |
| M10-Q3 | B:S A:S H:S | Mid: product deep dive only; senior: business context, build vs buy, phasing; staff: stakeholder trade-off framing |

---

## 9. Open items for boss (max 3)

1. Canvas spec acceptance: confirm P-19 mirrors vocabulary v0.1+ (section 1.1) and the registry (section 1.3 plus the six new rules and 7.1 test graphs) before visuals are authored.
2. Calibration staffing: the 30-design labelled set (section 7.3) needs two independent SME scorers; confirm who staffs this and the timeline before the final assessment is published.
3. Resolved (verification log VER-2 item 15, confirmed in this review against section 6): KIP-932 share groups are production-ready from Kafka 4.2, and the Challenge lesson may state that with the dated source. Re-check at each 3-month streaming-platform review; keep any newer version claim version-stamped.

---

## 10. Boss compliance log

| Decision ID | Applied |
|---|---|
| G-1 | Ownership notes table (2.2); pointers for stampede, rate-limit algorithms, idempotency, outbox, RLS, SKIP LOCKED, voice technology, RAG retrieval |
| G-2 | Header hours 26/18 plus 4.0 assessment; semantics sentence; module hours table sums |
| G-3, G-4 | Goal tags per module and question; L1-L4 table (LF-1) |
| G-5, G-6, G-7 | Bands in LF-2; readiness criteria; thresholds 80% module, rubric 3.5, hidden tests 70% |
| G-8 | Version baseline row; Redis-compatible wording; PostgreSQL 18.x numbers |
| G-9 | Tags normalised (`design.ai-systems`); M9 seed tags corrected; secondary tags separated |
| G-10 | Question IDs `Q-A6-Mn-nn`; goal tag and senior-bar note per seed (LF-8) |
| G-11 | Section 8 LF-1..LF-8 |
| G-12 | Dated facts (2026-10), no model names, placeholders labelled |
| R-A6-1 | Vocabulary v0.1+ amendments in 1.1, rule registry governance and six new rules in 1.3, example graphs in 7.1 |
| R-A6-2 | Rate-limiter service design only; algorithms and canonical coding task by `ref` to A5.M4; coding set reworked |
| R-A6-3 | Closed beta 20, public 26; stretch six listed incl. B4 orchestration seed |
| R-A6-4 | Placement pool (LF-2b), calibration spec (7.3) |
| R-A6-5 | Solution-architecture profile (7.2) |
| R-A6-6, O-91 | #17 canonical; B6 latency workbook linked as input |
| R-A2-1, O-17 to O-22 | M4 owns theory, saga, CDC, queue vs log; pointers to A5.M4, A5.M8, A2.M9 |
| R-A4-4, O-33, O-39, O-43 | M3 sharding and consistency; M9 tenancy patterns; M2 topology; pointers to A4.M2, A4.M7, A5.M5 |
| R-A7-5, O-56 | Cost rubric card (7.5) with A7.M10 and B2.M9 inputs |
| O-111 | M4.6 Challenge lesson, <= 1.5 h inside M4 hours |
| Upgrade items 1-13 | Applied; item 13 verified results in section 6 (Kafka, MQTT, Redis, single-node numbers), remaining open items listed there |

- Track A lead review (2026-10-05, TRACK-LEAD-A): library numbering, solution-architecture profile weights and O-111 Challenge checked against integration docs; difficulty mapping stated. Verdict: Approve with conditions (canvas spec acceptance, calibration staffing). See _reviews/TrackLead_A_Report.md.
