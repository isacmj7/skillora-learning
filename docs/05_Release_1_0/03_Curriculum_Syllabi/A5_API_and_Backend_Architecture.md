---
name: A5 — API and Backend Architecture
course_id: A5
track: A (Backend)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-02, HAR-08 (see _reviews/Boss_Final_Approval.md)
---

# A5 — API and Backend Architecture (Track A)

Version 2.0 (wave-2 upgrade, learner-fit and ownership-compliant). Last reviewed: 2026-10-05 (October 2026). Version-sensitive facts carry an as-of date and source type, or **[verify]** with the exact check needed. Provider-neutral: no LLM model names in lessons.

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | A5 — API and Backend Architecture / Track A |
| Audience | Working backend engineers (L3-L4) preparing for senior Python backend and AI engineer interviews; L1-L2 learners use the Simpler path. The reference learner (11+ years, about 4 in Python, FastAPI/Django/Flask, Redis, Postgres, AWS, LLM APIs) already *uses* most of this; the course targets the "explain it at senior depth, with trade-offs and failure modes" layer rather than syntax |
| Prerequisites | Working HTTP/REST and one Python web framework (no required course prerequisite; dotted edges per the boss graph: A2 and A4 recommended); A4 recommended before M10 and for M3 index alignment; A2 recommended before M8. Placement skip follows G-5 bands per module: score >= 85 skips lessons but the module mastery check is still taken; M4, M6, M8 and M10 carry the highest interview weight, so their skip bands also require the optional Challenge items. No band waives the final assessment. |
| Outcomes (each tied to a competency) | 1. Design a resource model, status-code and error contract (RFC 9457 problem details) and defend it (`api.design`). 2. Choose and implement versioning, pagination and filtering strategies with stated failure modes (`api.design`, `api.evolution`). 3. Make a write endpoint safe under retries: idempotency keys, conditional requests, rate limiting, timeouts (`api.reliability`). 4. Design a layered cache (HTTP caching plus Redis) with invalidation and stampede protection (`api.caching`). 5. Specify an OAuth2/OIDC flow and a RBAC/ABAC model; name JWT pitfalls and mitigate them (`api.authn-authz`, `sec.api`). 6. Build webhook, background-job and outbox flows with at-least-once semantics (`backend.async-events`). 7. Structure a service with hexagonal boundaries and justify modular monolith vs microservices and FastAPI vs Django vs Flask for a given context (`backend.architecture`). 8. Reason about multi-tenancy, uploads, observability hooks and performance for a production API (`backend.production`). |
| Estimated effort | **21.5 h Simpler / 13.5 h Deeper** module time (table below) plus a **2.5 h assessment allowance** (final assessment, voice mock, revision loops). Simpler = L1-L2 route with full scaffolding; Deeper = L3-L4 route with foundations skimmed per placement, hours net of typical skip-ahead, so usually shorter in wall-clock but harder per hour. M7 was reduced to design-time controls (-2 S / -1.5 D) and a design-patterns lesson added to M10 (+1.5 S / +1 D) per boss ruling. |
| Interview coverage tags | **Owned (issue credit):** `api.design`, `api.evolution`, `api.reliability`, `api.caching`, `api.authn-authz`, `backend.async-events`, `backend.architecture`, `backend.frameworks`, `backend.production`, `backend.patterns`. **Secondary (no credit):** `sec.api` (owner A8), `db.indexing`, `db.orm`, `db.sql` (owner A4). Feeds Interview Master API/backend depth rounds and the API-design canvas task. |
| IDE | Python workspace (3.14 default image; versions per platform manifest) with FastAPI, Django/DRF and Flask templates, disposable PostgreSQL 18 and Redis-compatible server (P-04), an HTTP client, a request-replay and fault-injection harness (drops, duplicates, delays) and webhook receiver (P-09), a **mocked token issuer** (local JWKS signer with fixed keys, scripted claims, expiry and key-rotation controls; no full IdP in 1.0), Celery in eager/memory-broker mode (P-13), and a multi-file project workspace for the practical (P-16). |
| Badge and certificate | Module badges M1-M11 (mastery check at 80% and all practice tasks passing hidden tests). Course badge "API and Backend Architecture" requires all module badges, final mastery check at 80%, voice mock at "Meets" (average >= 3.0, none < 2), and a design artefact (API contract plus architecture diagram) scored >= 3.5 with no dimension < 3. Interview-ready status follows G-6, not the badge. |
| Authoring owner | SME agent A5 (wave 2) |
| Reviewer | To be assigned |
| Review cadence | Auth, security standards and framework/vendor content every 3-6 months (G-12); stable REST/architecture principles every 12 months. |

### Module hours (hours)

| Module | M1 | M2 | M3 | M4 | M5 | M6 | M7 | M8 | M9 | M10 | M11 | Total |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Simpler | 1.25 | 2.0 | 1.75 | 3.5 | 2.25 | 2.75 | 1.0 | 2.5 | 1.25 | 2.25 | 1.0 | **21.5** |
| Deeper | 0.75 | 1.0 | 0.75 | 2.25 | 1.5 | 1.75 | 0.75 | 1.75 | 0.75 | 1.5 | 0.75 | **13.5** |

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice (IDE or design) | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Resource modelling and HTTP semantics | Design a resource model with correct methods, status codes and safe/idempotent semantics for a given domain | Resources vs RPC verbs, nouns and sub-resources, actions as sub-resources (`POST /orders/{id}/cancel`), safe vs idempotent methods, PUT vs PATCH (JSON Merge Patch vs JSON Patch), 200/201/202/204/409/422/429 usage, 400 vs 422, HATEOAS in practice, Richardson maturity as a talking point | Method semantics matrix; request lifecycle sequence diagram | Fix a badly designed API (verbs in URLs, 200 with error body); implement a CRUD plus action resource in FastAPI | "REST means JSON over HTTP"; POST for everything; 200 with `{"error":...}`; PUT and PATCH conflated; treating DELETE idempotency as "returns same status" |
| M2 | Contracts, errors, OpenAPI and versioning | Produce an OpenAPI contract with a consistent error model and an evolution/versioning plan | OpenAPI 3.1 and schema design (3.1 is the teaching baseline; 3.2.0, released Sept 2025, is backward compatible and adds the QUERY method, streaming media types and device-flow fields; checked Oct 2026, official spec site), schema-first vs code-first, Pydantic v2 models, RFC 9457 problem details, error codes vs messages, URI vs header vs media-type versioning, additive vs breaking changes, deprecation (`Deprecation` and `Sunset` headers), lint and diff tooling in CI (spec linter, breaking-change differ), SDK generation; **pointer:** consumer-driven contracts, schemathesis and property-based API tests are owned by A8.M3 | Breaking-change decision tree; versioning strategies comparison | Write an OpenAPI spec and lint it; classify 12 changes as breaking or not; add a v2 with a compatibility shim | "Version everything from day one in the URL" without a strategy; renaming a field as non-breaking; leaking internal exceptions; inconsistent error shapes across endpoints; adding an enum value treated as safe for strict clients |
| M3 | Collections: pagination, filtering, sorting, bulk and long-running operations | Choose and implement correct list semantics at scale, including bulk and async job resources | Offset vs keyset (cursor) pagination, opaque cursors, stable ordering with tie-breakers, total counts cost, filtering/sorting grammar, field selection, sparse fieldsets, bulk endpoints and partial failure (207 style or per-item results), 202 plus job resource and polling, Link headers | Offset vs keyset scan animation; pagination under concurrent inserts step-through | Implement the keyset-paginated list endpoint on top of the A4.M4 feed query and plan (reused, not re-derived): opaque signed cursor, tie-breaker, page-size bounds; build a bulk-create with per-item errors; build a 202 job resource | Offset pagination at depth; non-unique sort keys causing skipped or duplicated rows; exposing raw DB ids as cursors; unbounded page size; filtering on unindexed columns; always returning exact `COUNT(*)` |
| M4 | Reliability: idempotency, retries, rate limiting, timeouts | Make a write API safe under retries and protect it from overload | Idempotency-Key pattern (store request fingerprint plus response, TTL, in-flight lock, conflict on different payload), natural idempotency, conditional requests (`ETag` with `If-Match`) for lost updates, retry with exponential backoff plus jitter, retry budgets, timeouts and deadlines, circuit breaker, bulkheads, rate-limit algorithms (fixed window, sliding window, token bucket, leaky bucket), Redis Lua token bucket, `429` with `Retry-After` and the IETF `RateLimit` and `RateLimit-Policy` fields (Internet-Draft version 11, May 2026, not yet an RFC, so vendors still differ; checked Oct 2026) and `Idempotency-Key` header (IETF httpapi draft -07, Oct 2025, an expired Internet-Draft and not an RFC; verified on the IETF datatracker 2026-10-05), shared lesson X2 (retry, backoff, jitter), retry-budget and retry-storm lab, worked LLM-backed endpoint example (20 min, pointer to B2.M8 and B2.M9 for token-metered limits and streaming), load shedding, per-key vs per-IP vs per-tenant limits | Idempotency state machine; token bucket vs sliding window timelines; retry storm animation | Implement Idempotency-Key middleware with PostgreSQL unique constraint; implement a Redis token bucket and test under burst; inject duplicates with the harness (LAB-A5-IDEMP, LAB-A5-TOKENBUCKET); retry-budget and storm lab (LAB-A5-RETRYSTORM); breaker and bulkhead lab (LAB-A5-BREAKER) | "Idempotent means safe"; check-then-insert race without a unique constraint; keying idempotency on URL only; retrying non-idempotent calls blindly; no jitter; rate limiting only at the gateway with no per-tenant fairness; fixed-window burst at boundaries |
| M5 | Caching: HTTP caching and Redis | Design a multi-layer cache with explicit invalidation and stampede protection | `Cache-Control`, `ETag`, `Last-Modified`, `304`, `Vary`, CDN/shared vs private caches, cache-aside, read-through, write-through, write-behind, TTL and jitter, invalidation strategies (event-driven, versioned keys, tags), stampede/dogpile (locking, probabilistic early refresh, request coalescing), negative caching, Redis data structure choices, eviction policies, persistence trade-offs (RDB, AOF), Redis cluster basics, hot keys, big keys, Redis-compatible landscape (Redis 8 added AGPLv3 as a licence option in 2025; Valkey is the BSD-3 Linux Foundation fork and the ElastiCache default; vendor-risk note only, say "Redis-compatible (Redis or Valkey)" in designs; checked Oct 2026, secondary sources), cache poisoning and per-user data leaks | Layered cache diagram; stampede timeline; cache-aside sequence | Add ETag plus conditional GET to an endpoint; implement cache-aside with single-flight lock; reproduce and fix a stampede | "Just add Redis"; caching user-specific responses on shared caches (missing `Vary`/`private`); no invalidation story; same TTL for all keys; treating Redis as a durable DB without stating persistence settings; caching errors |
| M6 | Authentication and authorization | Specify the right auth flow and permission model for a given client and defend token choices | Sessions (cookie, server-side, CSRF) vs tokens, OAuth2 grant types in current practice (authorization code with PKCE, client credentials, device code; implicit and password grants avoided), OAuth 2.1 consolidation (still an IETF Internet-Draft, draft-15 of March 2026, not an RFC at last check Oct 2026; its rules of PKCE for all clients, no implicit or password grant and exact redirect matching are already current practice), OIDC (ID token vs access token, discovery, JWKS), JWT structure and validation (`alg`, `iss`, `aud`, `exp`, `kid`), JWT pitfalls (`alg=none`, algorithm confusion, no revocation, long-lived tokens, secrets in payload, storing in localStorage), refresh token rotation and reuse detection, opaque tokens with introspection, sender-constrained tokens (DPoP, RFC 9449, used in FAPI 2.0 and several IdPs), API keys, service-to-service (mTLS, workload identity), RBAC vs ABAC vs ReBAC overview, scopes vs roles vs permissions, enforcing authorization at object level, FastAPI dependencies and Django/DRF permissions | OAuth2 authorization code plus PKCE sequence; token lifecycle with rotation; RBAC vs ABAC matrix | Build login plus refresh rotation in FastAPI against the mocked token issuer (JWKS signer, scripted claims, expiry and key-rotation controls); validate a JWT correctly (and break a vulnerable validator); implement object-level permission checks | "JWT is more secure than sessions"; "OAuth is authentication"; trusting the `alg` header; skipping `aud` validation; no revocation plan; roles encoded stale in a long-lived JWT; checking authorization only in the UI or only at route level |
| M7 | API security: design-time controls | Apply design-time API security controls and know where A8.M7 takes over | CORS correct usage and preflight, CSRF for cookie auth (SameSite, tokens), security headers, TLS and HSTS basics, secrets handling at design level, mass assignment and excessive data exposure at the schema level (request and response models, allow-lists), webhook signing (pointer to M8), audit-log fields, PII in logs; **pointer:** BOLA, BFLA, SSRF, injection, resource-consumption exploits and the OWASP API Top 10 (2023 edition, still the current list at last check Oct 2026) are owned by A8.M7 | Trust-boundary diagram; CORS preflight sequence | Configure CORS and CSRF correctly for a cookie and a token client; fix mass assignment with separate request and response schemas | Treating CORS as a security control against non-browser clients; `Access-Control-Allow-Origin: *` with credentials; returning full ORM objects; binding request JSON straight to a model; "we use HTTPS so we are secure" |
| M8 | Async APIs, webhooks, background processing and events | Build reliable async flows and choose between sync, async and event-driven designs | Sync vs async request patterns, 202 plus polling vs webhook vs SSE/WebSocket, webhook design (signing, retries with backoff, idempotent consumers, ordering, dead-letter, replay, delivery logs), background job systems (Celery 5.6 stable, Arq in maintenance-only mode (README points to issue #510, announced Oct 2025; last releases 0.27.0 Feb 2026 and 0.28.0 Apr 2026), Dramatiq 2.x active (2.2.1, Sep 2026), RQ 2.x active (2.12.0, 30 Aug 2026; a release every 1-2 months in 2026), cloud queues; verified 2026-10 via PyPI and the Arq README), the implementation consequence of at-least-once delivery (idempotent consumer; delivery-semantics theory is a pointer to A6.M4), queue-versus-DB decision is a pointer to A2 and the `SKIP LOCKED` primitive to A4, transactional outbox and inbox, CDC overview (Debezium), saga basics, queue vs log (SQS vs Kafka) at API level, poison messages, event schema evolution, AsyncAPI overview | Outbox sequence diagram; webhook delivery state machine; dual-write failure timeline | Implement transactional outbox with a relay worker and idempotent consumer; build a webhook sender with signing and retries; simulate a crash between commit and publish | Dual write to DB and broker without outbox; claiming "exactly once"; webhooks without signature or timeout; processing webhooks synchronously; job payloads containing mutable state instead of ids; no DLQ or visibility |
| M9 | Other API styles: GraphQL, gRPC, streaming | Pick REST, GraphQL, gRPC or streaming for a use case with honest trade-offs | GraphQL schema, resolvers, N+1 and DataLoader, query depth/cost limiting, persisted queries, caching difficulty, errors in GraphQL; Python options (Strawberry 0.331.x, releasing almost daily in Oct 2026; Ariadne 1.1.0, Jun 2026; Graphene 3.4.3, last release Nov 2024, so low activity; verified 2026-10 via PyPI); QUERY method and OpenAPI 3.2 as the REST answer to complex reads; pointers: WebSocket and SSE server implementation A2.M7, LLM token streaming B2.M8, audio transport B6.M3 (this module owns only the protocol-choice criteria); gRPC and Protobuf, streaming types, deadlines, gRPC-web, schema evolution rules for Protobuf, REST gateway; SSE vs WebSocket vs long polling; BFF pattern; API gateway responsibilities | Style comparison matrix; DataLoader batching step-through | Add a DataLoader to fix N+1 in a GraphQL resolver; write a Protobuf schema and evolve it safely; design a BFF | "GraphQL replaces REST"; no query cost limits; HTTP caching ignored; reusing Protobuf field numbers; gRPC for browser clients without a proxy |
| M10 | Architecture: clean/hexagonal, monolith vs microservices, frameworks | Structure a backend with explicit boundaries and justify the deployment and framework choices | Layering (routers, services, domain, repositories), ports and adapters, dependency inversion in Python (protocols, FastAPI `Depends`), domain vs persistence models, unit of work, transaction boundaries, anemic vs rich domain, DDD-lite (aggregates, bounded contexts), modular monolith vs microservices (team topology, data ownership, distributed monolith), strangler fig, FastAPI vs Django/DRF vs Flask (async model, ORM, admin, batteries, validation via Pydantic, ecosystem, team fit, hiring), framework comparison with criteria (as of Oct 2026: FastAPI on the 0.14x line with Pydantic v2; Django 5.2 LTS with 6.1 current and 6.0 already out of mainstream support; Flask 3.1; SQLAlchemy 2.x; secondary sources; PyPI latest on 2026-10: FastAPI 0.142.2, Django 6.1.1, Flask 3.1.3, SQLAlchemy 2.1.3, so SQLAlchemy 2.x covers 2.1, verified 2026-10; exact pins live in the platform manifest **[verify: manifest pins at publish]**), ASGI vs WSGI, Django async reality (async ORM methods exist; DB I/O still passes through a thread-sensitive adapter unless the driver path is natively async **[verify: Django 6.1 async docs page not readable from primary source; check docs.djangoproject.com/en/6.1/topics/async/]**), **design patterns in Python** (strategy, factory, observer, adapter, repository, dependency injection) with six `lld` seed problems supplied to C3 | Hexagonal diagram; modular monolith boundaries; framework decision matrix | Refactor a fat-view app into hexagonal layers with an in-memory adapter for tests; draw module boundaries and enforce them with an import linter | Over-engineering CRUD with five layers; microservices as a default; shared database between services; framework wars without criteria; business logic in serializers or route handlers; ORM models leaking into API schemas |
| M11 | Production concerns: multi-tenancy, uploads, observability, performance | Make an API operable and scalable: tenant isolation, safe uploads, telemetry and performance tuning | Multi-tenancy models (shared schema with `tenant_id`, schema-per-tenant, DB-per-tenant), row-level security (design-level recap; pointer to A4.M2), tenant context propagation, noisy neighbour; file uploads (presigned S3 URLs, multipart/resumable, size/type validation, virus scanning, streaming vs buffering, storing metadata); observability hooks reduced to a 20-minute pointer to A7.M9 (only request-id and trace-context propagation stays here); health vs readiness endpoints; API-layer performance hooks (N+1, connection pooling, serialization cost, compression, pagination, async pitfalls, worker tuning with Uvicorn/Gunicorn; profiling and load-testing method are a 20-minute pointer to A8.M10 and A1.M11), graceful shutdown, config and feature flags | Tenant isolation spectrum; presigned upload sequence; trace propagation diagram | Add tenant context propagation and scoping to a service (tiny lab, RLS pointer to A4.M2); implement presigned upload with validation callback; add request-id propagation (span analysis uses the A7.M9 lab); fix one seeded API-layer bottleneck (pool/worker mismatch) using the A8.M10 load-test recipe by pointer | Trusting client-supplied tenant id; filtering by tenant in some queries only; proxying large uploads through the app; trusting file extension/MIME; logging secrets/PII; blocking calls inside async handlers; "scale by adding workers" without checking the DB pool |

### Ownership notes (G-1)

| Module | OWNS | POINTS |
|---|---|---|
| M1 | REST resource modelling, HTTP method and status semantics | none |
| M2 | Contract-first OpenAPI, RFC 9457 errors, versioning, breaking-change classification, lint and diff | consumer-driven contracts, schemathesis, property-based API tests -> A8.M3 |
| M3 | Pagination contract, cursors, bulk and async job resources | keyset index and plan detail -> A4.M4 |
| M4 | Rate-limit algorithms and Redis token bucket (canonical for A6), idempotency-key middleware, retry/backoff/jitter (X2), circuit breaker, bulkhead, retry budgets | rate-limiter service design -> A6; LLM token-metered limits and non-deterministic retries -> B2.M8, B2.M9 |
| M5 | HTTP caching and Redis caching mechanics, invalidation, stampede | cache topology, sizing, hit-rate design -> A6.M2; DB-side caching -> A4 |
| M6 | OAuth2/OIDC protocols, JWT, token design, object-level authorisation design | exploit-and-fix depth -> A8.M7; secrets and IAM -> A7.M6 |
| M7 | Design-time controls: CORS, CSRF, headers, TLS, schema-level mass assignment | BOLA, BFLA, SSRF, injection, OWASP API Top 10 -> A8.M7; webhook signing -> A5.M8 |
| M8 | Webhooks, outbox and idempotent-consumer implementation, API-level job contract and job-system choice criteria, AsyncAPI | worker-level semantics (acks, visibility timeout, poison messages) -> A2.M9; delivery-semantics theory and design -> A6.M4; queue vs DB decision -> A2.M9.1; `SKIP LOCKED` primitive -> A4.M6 |
| M9 | Protocol-choice criteria, GraphQL, gRPC, BFF and gateway overview | WebSocket/SSE server -> A2.M7; LLM token streaming -> B2.M8; audio transport -> B6.M3 |
| M10 | Hexagonal architecture, patterns in Python, modular monolith, framework comparison, `lld` seeds | SOLID and clean-code principles -> A8.M6; LLD round format and process -> C3.M7 |
| M11 | Config, uploads, health endpoints, tenant context hooks, API-layer performance hooks (N+1, pooling, serialization, worker sizing) | observability depth -> A7.M9; RLS -> A4.M2; load testing, profiling and p99 bottleneck diagnosis -> A8.M10 (O-63), py-spy/cProfile -> A1.M11 |

## 3. Per-module detail blocks

Question-bank targets per module (template minimum, IDs `Q-A5-Mn-nn`): 12 MCQ/concept, 6 code-output/predict, 4 coding tasks, 8 verbal with follow-up trees, 3 scenario/design. Mastery check: 8 items, pass 80% (G-7), revision loop on misses. Module badge: mastery check passed plus all practice tasks passing hidden tests. Totals across 11 modules: 132 MCQ, 66 predict, 44 coding, 88 verbal (3 seeded below per module plus added probes), 33 scenario. M7 items that would duplicate A8.M7 are `ref` items to the A8 bank (at most one).

### M1 Resource modelling and HTTP semantics
- Lessons: A5.M1.1 Resources, URIs and when RPC style is acceptable; A5.M1.2 Methods, safety and idempotency; A5.M1.3 Status codes and bodies that clients can rely on; A5.M1.4 PUT vs PATCH and partial updates.
- Simpler: guided classification of verbs and codes. Deeper: edge cases (DELETE on missing, 409 vs 422, 202 semantics, cache implications of methods).
- Mastery focus: method semantics, status-code choice, actions modelling, partial update formats.

### M2 Contracts, errors, OpenAPI and versioning
- Lessons: A5.M2.1 Schema design and OpenAPI as a contract; A5.M2.2 Error model with problem details; A5.M2.3 Breaking vs non-breaking changes; A5.M2.4 Versioning strategies and deprecation; A5.M2.5 Lint, diff and the contract-testing pointer (20 min, `ref` to A8.M3).
- Simpler: read and write a spec. Deeper: tolerant reader, enum evolution, SDK generation pitfalls.
- Mastery focus: change classification, error shape, deprecation process.

### M3 Collections
- Lessons: A5.M3.1 Offset vs keyset pagination; A5.M3.2 Filtering and sorting design; A5.M3.3 Bulk operations and partial failure; A5.M3.4 Long-running operations as resources.
- Simpler: wrap the A4.M4 offset and keyset queries in an endpoint with an opaque cursor contract. Deeper: composite cursors, backward paging, count strategies, index alignment with sort.
- Mastery focus: ordering stability, cursor design, index alignment, partial-failure contracts.

### M4 Reliability
- Lessons: A5.M4.1 Idempotency keys end to end; A5.M4.2 Optimistic concurrency with ETags; A5.M4.3 Retries, backoff, jitter and budgets; A5.M4.4 Rate-limiting algorithms and placement; A5.M4.5 Timeouts, circuit breakers, bulkheads, load shedding; A5.M4.6 Worked example: an LLM-backed endpoint (20 min; pointer to B2.M8 and B2.M9). Lesson 3 is shared lesson X2; lessons 1, 3, 4, 5 carry the stable lab names LAB-A5-IDEMP, LAB-A5-RETRYSTORM, LAB-A5-TOKENBUCKET, LAB-A5-BREAKER.
- Simpler: use a library pattern. Deeper: in-flight request handling, atomic Redis Lua, distributed limiter accuracy trade-offs.
- Mastery focus: race conditions, algorithm selection, fairness across tenants, retry safety.

### M5 Caching
- Lessons: A5.M5.1 HTTP caching headers and validators; A5.M5.2 Cache patterns and invalidation; A5.M5.3 Redis data structures and eviction; A5.M5.4 Stampede and hot-key handling; A5.M5.5 Caching security and correctness.
- Simpler: add TTL cache-aside. Deeper: tag invalidation, probabilistic refresh, consistency windows, Redis failure behaviour.
- Mastery focus: invalidation, `Vary`/private, stampede, persistence and eviction.

### M6 Authentication and authorization
- Lessons: A5.M6.1 Sessions vs tokens; A5.M6.2 OAuth2 flows and PKCE; A5.M6.3 OIDC and JWT validation; A5.M6.4 Refresh rotation, revocation, logout; A5.M6.5 Authorization models and object-level checks.
- Simpler: end-to-end login with a library. Deeper: DPoP, token exchange, multi-audience, key rotation (JWKS), policy engines overview.
- Mastery focus: flow selection, JWT validation checklist, revocation strategies, RBAC vs ABAC.

### M7 API security
- Lessons: A5.M7.1 Trust boundaries and where design-time controls end and A8.M7 begins (pointer for BOLA, BFLA, SSRF, injection); A5.M7.2 Schema-level controls: request and response models, mass assignment; A5.M7.3 CORS, CSRF, security headers, TLS; A5.M7.4 Secrets, audit-log and PII design at API level (webhook signing pointer to M8).
- Simpler: guided CORS and schema fix. Deeper: cookie plus token dual-client design, preflight caching, header review.
- Mastery focus: CORS vs CSRF, mass assignment, what CORS does not protect.

### M8 Async APIs, webhooks, background processing, events
- Lessons: A5.M8.1 Choosing sync vs async interaction; A5.M8.2 Webhook provider and consumer design; A5.M8.3 Job systems from the API side: 202 plus job resource, enqueue inside the DB transaction, choice criteria (worker-level semantics are owned by A2.M9; 20-minute pointer, no lab); A5.M8.4 Transactional outbox and idempotent consumer (implementation owner; theory pointer to A6.M4); A5.M8.5 Event design, AsyncAPI and evolution. Labs: LAB-A5-OUTBOX, LAB-A5-WEBHOOK.
- Simpler: single worker plus retry. Deeper: ordering, partitioning keys, DLQ replay, CDC-based outbox, saga compensation.
- Mastery focus: delivery semantics, idempotent consumers, dual-write failure, observability of async flows.

### M9 Other API styles
- Lessons: A5.M9.1 GraphQL model and N+1; A5.M9.2 GraphQL operations concerns (cost, caching, auth); A5.M9.3 gRPC and Protobuf; A5.M9.4 SSE, WebSocket, long polling; A5.M9.5 Gateway and BFF.
- Simpler: compare by use case table. Deeper: federation overview, Protobuf compatibility rules, backpressure in streams.
- Mastery focus: selection criteria, GraphQL abuse controls, schema evolution rules.

### M10 Architecture and frameworks
- Lessons: A5.M10.1 Layers, ports and adapters; A5.M10.2 Transactions, unit of work and domain boundaries; A5.M10.3 Modular monolith vs microservices; A5.M10.4 FastAPI vs Django vs Flask; A5.M10.5 Migration paths (strangler, extraction); A5.M10.6 Design patterns in Python: strategy, factory, observer, adapter, repository, dependency injection, plus six `lld` seed problems for C3.
- Simpler: refactor guided. Deeper: data ownership, eventual consistency between modules, organisational fit (Conway), async in Django, ORM choice.
- Mastery focus: justify not over-engineering; recognise distributed-monolith smells; framework trade-offs with criteria.

### M11 Production concerns
- Lessons: A5.M11.1 Multi-tenancy models, isolation and tenant context propagation (RLS pointer to A4.M2); A5.M11.2 File upload architecture; A5.M11.3 Observability hooks (20-minute pointer to A7.M9; request-id and trace-context propagation only); A5.M11.4 API-layer performance hooks: N+1, pooling, serialisation, worker sizing (diagnosis method is A8.M10 and A1.M11; 20-minute pointer, no load-test lab); A5.M11.5 Operability: health checks, graceful shutdown, config.
- Simpler: add middleware for ids and logs. Deeper: RLS, per-tenant limits and quotas, tail latency, pool sizing.
- Mastery focus: isolation guarantees, upload security, RED metrics, API-layer causes of tail latency.

## 4. Seed interview questions (3 per module, 33 total)

Format per question: ideal outline; weak answers; follow-ups; difficulty; competency; cluster. All are style-of-question items, not questions attributed to any company. Difficulty words map to the data-model integer scale 1-5 at import: easy 1, easy-medium 2, medium 3, medium-hard 4, hard 5 (same mapping as Track C); calibrated after the pilot.

### M1

**Q-A5-M1-01 "What is the difference between PUT and PATCH, and when is each idempotent?"** — Difficulty: Medium. Competency `api.design`. Cluster: IT-services, global-IT-consulting.
- Ideal outline: PUT replaces the full representation at a URI; idempotent by definition (same request repeated yields the same state); PATCH applies a partial change; idempotent depends on the patch format; JSON Merge Patch (set fields) is usually idempotent, JSON Patch with "add to array" or increments is not; mention `If-Match` plus ETag for lost-update protection; creating via PUT when client chooses the id; response codes 200/204 and 404/409 handling; document the semantics in the contract.
- Weak: "PUT updates, PATCH also updates"; claims PATCH is never idempotent; ignores concurrency.
- Follow-ups: How would you make a PATCH that increments a counter safe? What happens with PUT when the client omits optional fields? How do you handle null vs absent in Pydantic models?

**Q-A5-M1-02 "A client creates an order and times out. They call POST again. What goes wrong and what are your options in the design?"** — Medium. `api.design`, `api.reliability`. Cluster: global-product, Big4.
- Outline: POST is neither safe nor idempotent; duplicate orders; options: client-generated id with PUT, Idempotency-Key header, natural unique business key with constraint, return 409 or the original; mention response replay; confirm server-side uniqueness; mention client UX (poll resource status); link to M4.
- Weak: "Tell clients not to retry"; "check if order exists by user" without race analysis.
- Follow-ups: How long do you retain keys? What if the same key arrives with a different body? How do two concurrent identical requests behave?

**Q-A5-M1-03 "When would you pick 400, 404, 409, 422 or 403 vs 404 for an unauthorized resource?"** — Easy-Medium. `api.design`. Cluster: IT-services, Indian-product.
- Outline: 400 malformed syntax or unparseable request; 422 semantically invalid (validation) — note FastAPI defaults to 422 for validation, many teams standardise on 400; 404 missing; 409 state conflict (duplicate, version mismatch); 403 authenticated but not allowed; 401 unauthenticated; 404 instead of 403 to avoid revealing existence (explicit policy choice); consistency and documentation matter more than the exact pick; machine-readable error code in the body.
- Weak: memorised list with no reasoning; always 400 or always 500.
- Follow-ups: How do you represent multiple field errors? How do you avoid leaking existence in login and reset flows? What status for an optimistic-lock failure (409 vs 412)?

### M2

**Q-A5-M2-01 "How do you evolve an API without breaking clients? What counts as a breaking change?"** — Medium. `api.evolution`. Cluster: global-product, global-IT-consulting.
- Outline: define consumers and compat promise; additive changes are safe for tolerant readers (new optional fields, new endpoints); breaking: removing/renaming fields, changing types, tightening validation, changing semantics/defaults, new required params, enum additions for strict clients; expand-and-contract; deprecation with `Deprecation`/`Sunset` headers, telemetry on usage per version/consumer, contract tests; versioning as last resort; communicate and set timelines.
- Weak: "Just use /v2"; thinks adding a field is always safe.
- Follow-ups: How do you know who still uses v1? How do you handle mobile apps that cannot be force-upgraded? Would you version per resource or per API?

**Q-A5-M2-02 "Compare URI, header and media-type versioning."** — Medium. `api.evolution`. Cluster: IT-services, Indian-product.
- Outline: URI: visible, cacheable, easy routing and docs, but resource identity changes; header/media-type: clean URIs, more REST-pure, harder to test, cache needs `Vary`, tooling friction; date-based versions (account pinned) as used by some payment APIs; version granularity; gateway routing implications; internal code strategy (shared domain, versioned schemas/adapters, not copy-pasted services); pick based on consumer types and tooling; consistency.
- Weak: declares one "correct" without trade-offs; duplicates the whole codebase per version.
- Follow-ups: How do you structure FastAPI routers and schemas for v1 and v2? How do you test both? When would you not version at all?

**Q-A5-M2-03 "Design the error model for a public API."** — Medium-Hard. `api.design`. Cluster: global-product, Big4.
- Outline: RFC 9457 `application/problem+json` (type, title, status, detail, instance, plus extensions); stable machine-readable `code` separate from human text; field-level errors array; correlation/request id; no stack traces or internals; consistent across 4xx and 5xx including gateway-generated ones; localisation strategy; documented in OpenAPI; retryability hints (`Retry-After`); logging link; FastAPI exception handlers and Django DRF `EXCEPTION_HANDLER` mapping.
- Weak: `{"error": "something went wrong"}`; leaking exception text; different shapes per service.
- Follow-ups: How do you keep error shapes consistent across microservices? How do you document errors in OpenAPI? What do you return for validation errors across multiple fields?

### M3

**Q-A5-M3-01 "Design the pagination contract for a public list endpoint: what does the client see, and what promises do you make?"** — Medium. `api.design`. Cluster: global-product, Indian-product. (Keyset SQL, the tuple comparison and the matching composite index are owned by Q-A4-M4-03: pointer, not repeated.)
- Outline: response envelope with opaque cursor and `next` link; cursor encodes sort key plus tie-breaker, signed or encoded, never a raw id; stable ordering promise under concurrent inserts and deletes; page-size default and maximum; cannot jump to page N and what the UI does instead; count strategy (estimate, omitted, or capped); backward paging; cursor invalidation when filters or sort change and the error returned; versioning of the cursor format.
- Weak: "use cursors, they are faster" with no contract; exposes the raw id; changes sort without invalidating cursors.
- Follow-ups: How would the UI show "page 5 of 20"? What happens to a saved cursor after a deploy changes the sort? How do you paginate a nullable sort column at the contract level (SQL detail: A4.M4)?

**Q-A5-M3-02 "Design filtering and sorting for a list endpoint with 10M rows."** — Medium. `api.design`. Cluster: IT-services, global-IT-consulting.
- Outline: whitelist of filterable/sortable fields mapped to indexes; query-param grammar (`status=active&created_gte=...`) or structured filter; validate and bound; never concatenate SQL; combine with keyset; composite indexes aligned to common filters; limit on `OR`/`LIKE '%x%'`, route search to a search engine; default sort; field selection; abuse protection via max page size and rate limits; document in OpenAPI.
- Weak: "allow any column"; `LIKE '%q%'` on millions of rows; no limits.
- Follow-ups: How do you support multi-value and range filters? When move to Elasticsearch/OpenSearch? How do you prevent expensive filter combinations?

**Q-A5-M3-03 "How do you design an endpoint that triggers a 2-minute report?"** — Medium. `api.design`, `backend.async-events`. Cluster: Big4, global-product.
- Outline: `POST /reports` returns `202 Accepted` with `Location` to a job resource; job states (queued, running, succeeded, failed, canceled); poll with `Retry-After` hint, or webhook/SSE for completion; result stored in object storage with expiring presigned URL; idempotency key for creation; progress and error details; retention and cleanup; authorization on job resource; worker concurrency and rate limits.
- Weak: holding the HTTP request open; raising gateway timeouts; no failure model.
- Follow-ups: How do you cancel? How do you avoid duplicate jobs? How would clients be notified at scale?

### M4

**Q-A5-M4-01 "Implement idempotency for a payment-creation endpoint."** — Hard. `api.reliability`. Cluster: global-product, Indian-product, Big4.
- Outline: client supplies `Idempotency-Key`; server stores (key, scope such as user/tenant, request hash, status, response) with a unique constraint on (scope, key); first request inserts a "processing" record atomically, then executes; duplicate while processing returns 409 Conflict (the IETF draft's choice) or waits briefly for the first request; duplicate after completion replays stored status and body; same key with different payload returns 422/409; TTL (e.g. 24h); business effect and key record committed in the same DB transaction where possible; downstream calls (PSP) get their own idempotency key; failure handling: only cache deterministic outcomes, allow retry after transient errors; cleanup job.
- Weak: in-memory dict; Redis `SETNX` only without durability reasoning; no payload fingerprint; caching 500s.
- Follow-ups: What if the process crashes after charging but before storing the response? How would this work across multiple API instances? What about GET and DELETE?

**Q-A5-M4-02 "Design a rate limiter for a multi-tenant API. Which algorithm and where does it run?"** — Medium-Hard. `api.reliability`. Cluster: global-product, AI-startup, Indian-product.
- Outline: requirements (per key, per tenant, per endpoint tier, burst tolerance); algorithms: fixed window (boundary burst), sliding window log/counter (accuracy vs memory), token bucket (burst plus steady rate, common choice), leaky bucket (smoothing); Redis with atomic Lua script (or `INCR`+`EXPIRE` pitfalls), clock source; placement: edge/gateway for coarse, app-level for tenant-aware, plus concurrency limits; response `429` with `Retry-After` and rate-limit headers; fail-open vs fail-closed on Redis outage; fairness and noisy neighbour; cost-weighted limits (e.g. LLM tokens); observability of throttling.
- Weak: names an algorithm without trade-offs; non-atomic get-then-set in Redis; only per-IP limits behind a NAT.
- Follow-ups: How do you do this across regions? How would you rate limit by LLM tokens? What happens when Redis is down?

**Q-A5-M4-03 "Your service retries failed downstream calls and an outage got worse. Why, and what do you change?"** — Medium. `api.reliability`. Cluster: global-IT-consulting, global-product.
- Outline: retry storm/amplification across layers (multiplicative retries); fix with exponential backoff plus full jitter, retry only on idempotent or keyed operations and retryable errors, retry budgets, per-call timeouts and overall deadlines, circuit breaker, bulkheads, load shedding, honour `Retry-After`, retry at one layer only; metrics for retry rate; fallback/degradation.
- Weak: "increase retries"; fixed-interval retries; no timeouts.
- Follow-ups: How do you pick timeouts? What state does a circuit breaker keep, and where? How do you test this?

### M5

**Q-A5-M5-01 "Explain cache-aside, and how you would prevent a cache stampede."** — Medium. `api.caching`. Cluster: global-product, Indian-product, IT-services.
- Outline: cache-aside read path (check, miss, load, populate with TTL) and write path (update DB then invalidate/delete); race windows (stale set after delete) and mitigations (short TTL, versioning); stampede on hot-key expiry: single-flight/mutex lock with short TTL, request coalescing, probabilistic early expiry, serve-stale-while-revalidate, TTL jitter, pre-warming; protect DB with limits; negative caching with shorter TTL; metrics (hit ratio, latency, evictions).
- Weak: "set TTL" only; delete-then-write ordering errors; locks without expiry.
- Follow-ups: What if the lock holder dies? Update vs delete on write, and why? How do you handle a hot key on one Redis shard?

**Q-A5-M5-02 "How do HTTP caching headers work, and how do you cache an authenticated API response safely?"** — Medium. `api.caching`, `sec.api`. Cluster: global-IT-consulting, Big4.
- Outline: `Cache-Control` (`max-age`, `s-maxage`, `no-store`, `no-cache` meaning revalidate, `private`, `public`, `stale-while-revalidate`); validators `ETag`/`If-None-Match`, `Last-Modified`; 304 saves bandwidth not compute unless the check is cheap; `Vary` (e.g. `Accept-Encoding`, `Authorization` handling); personalised responses `private` or `no-store`; CDN keys must include tenant/user dimension or skip caching; don't cache sensitive data in shared caches; weak vs strong ETags; versioned URLs for static assets.
- Weak: confuses `no-cache` with `no-store`; caches personalised JSON at a CDN.
- Follow-ups: How do you compute an ETag cheaply? How do you purge a CDN on update? What does `Vary: Authorization` do and why is it often a poor fit?

**Q-A5-M5-03 "When would you not use Redis as the system of record, and how do eviction and persistence settings matter?"** — Medium. `api.caching`. Cluster: Indian-product, IT-services.
- Outline: Redis is in-memory; persistence via RDB snapshots (data loss window) and AOF (fsync policy trade-offs); replication is asynchronous so failover can lose writes; eviction policies (`allkeys-lru`, `volatile-ttl`, `noeviction`) and effect when memory is full; use as cache vs queue vs lock vs rate limiter vs session store; data modelling (hashes, sorted sets, streams); Redlock critique and fencing tokens for correctness-critical locks; memory sizing; Redis-compatible naming (Redis or Valkey) and licence note as vendor-risk only (checked Oct 2026).
- Weak: "Redis is a database so it is durable"; ignores eviction in a session store.
- Follow-ups: What happens to sessions if Redis fails over? Is a Redis lock safe for a payment workflow? How do you find big keys?

### M6

**Q-A5-M6-01 "Walk me through OAuth2 authorization code flow with PKCE and say why we use it."** — Medium. `api.authn-authz`. Cluster: global-product, Big4, global-IT-consulting.
- Outline: actors (user, client, authorization server, resource server); client creates `code_verifier` and `code_challenge`; redirect with `state` (and `nonce` for OIDC); user authenticates; authorization code returned to redirect URI; client exchanges code plus verifier at token endpoint; receives access (and optionally refresh and ID) token; PKCE defends against code interception, now recommended for all clients including confidential; exact redirect URI matching; OAuth is delegated authorization, OIDC adds identity; why implicit/password grants are avoided; where tokens are stored (BFF with cookies for SPAs).
- Weak: describes login form posting credentials to the API; confuses ID and access tokens.
- Follow-ups: Where does a SPA keep tokens? What does `state` protect against? When use client credentials?

**Q-A5-M6-02 "List JWT pitfalls and how you would validate a JWT in a FastAPI service."** — Medium-Hard. `api.authn-authz`, `sec.api`. Cluster: IT-services, global-product, Indian-product.
- Outline: validate signature with a pinned allowed algorithm list (never trust header `alg`; reject `none`; algorithm confusion RS256 vs HS256); verify `iss`, `aud`, `exp`, `nbf`, and `kid` via JWKS with caching and rotation; clock skew leeway; short access-token life; revocation limits (stateless) and mitigations (short TTL, denylist by `jti`, token versioning, introspection for sensitive ops); don't store sensitive data in payload (it is only signed); avoid localStorage for browser storage; refresh token rotation with reuse detection; use a maintained library; permission data staleness; asymmetric keys for multi-service verification; FastAPI `Depends` security dependency with testable verifier.
- Weak: "JWT is encrypted"; "just decode and read the user id".
- Follow-ups: How do you log a user out everywhere? When prefer opaque tokens or sessions? How do you rotate signing keys without downtime?

**Q-A5-M6-03 "Design authorization for a multi-tenant SaaS: RBAC, ABAC or something else?"** — Hard. `api.authn-authz`, `backend.production`. Cluster: global-product, Big4, AI-startup.
- Outline: separate authentication from authorization; start with tenant-scoped RBAC (roles to permissions, permissions checked in code, not role names); add attributes/relationships (ABAC/ReBAC) for ownership and resource-level rules (e.g. "editor of this project"); enforce at object level in a central policy layer, not scattered ifs; default deny; tenant id derived from the token or path and enforced in the data layer (RLS or mandatory query scope); audit decisions; caching permission lookups with invalidation; policy engines (OPA, Cedar, OpenFGA) as concepts only (OPA is CNCF graduated, its core maintainers joined Apple in 2025 with governance staying at CNCF; Cedar is a CNCF Sandbox project, accepted Oct 2025; OpenFGA is CNCF Incubating since Oct 2025; verified 2026-10); testing matrices; avoid role explosion.
- Weak: hard-coded `if user.role == "admin"` everywhere; checks only on list endpoints.
- Follow-ups: How do you handle a user in multiple tenants? How do you test authorization exhaustively? How do you expose "what can I do" to the frontend?

### M7

**Q-A5-M7-01 "How do you prevent mass assignment and excessive data exposure at the schema layer of a FastAPI or DRF service?"** — Medium. `api.design`, `sec.api` (secondary). Cluster: global-product, IT-services, Big4.
- Outline: separate request, response and persistence models; response models as allow-lists (never return the ORM object); read-only and server-set fields excluded from input models; explicit field lists in DRF serializers (no `__all__`); `extra=forbid` in Pydantic where appropriate; role-dependent response shapes via distinct schemas; tests asserting forbidden fields are ignored or rejected; link to A8.M7 for object-level and function-level authorisation exploits.
- Weak: "validate input" only; relies on UUIDs; returns `model_dump()` of the DB row.
- Follow-ups: How do you handle admin-only fields? How do you catch a leaked field in CI? Where does BOLA start (pointer: `ref` to the A8.M7 BOLA item)?

**Q-A5-M7-02 "Explain CORS: what it protects, what it does not, and common misconfigurations."** — Medium. `api.authn-authz` (closest A5-owned tag for design-time browser trust controls; secondary `sec.api`, owner A8, no credit). Cluster: IT-services, Indian-product, global-IT-consulting.
- Outline: browser-enforced same-origin relaxation, not server authorization; preflight (`OPTIONS`) for non-simple requests; `Access-Control-Allow-Origin` must be a specific origin when credentials are used; wildcard plus credentials invalid; reflecting arbitrary `Origin` is dangerous; `Vary: Origin`; does not stop curl/server clients or CSRF by itself; CSRF defences (SameSite cookies, CSRF tokens) for cookie-auth; headers allowed/exposed; FastAPI/Django CORS middleware configuration; cache preflight with `max-age`.
- Weak: "CORS blocks attackers"; fixes by `*` everywhere.
- Follow-ups: Why does a cross-origin request with cookies fail? Is CORS relevant for server-to-server calls? How do SameSite cookies interact with CSRF?

**Q-A5-M7-03 "How do you secure outbound and inbound webhooks?"** — Medium. `backend.async-events` (credit), `sec.api` (secondary, owner A8, no credit). Cluster: global-product, AI-startup.
- Outline: inbound: verify HMAC signature over raw body using constant-time comparison, timestamp tolerance to limit replay, store event ids for dedupe, per-endpoint secrets with rotation, respond fast and process async, restrict by IP only as an additional layer; outbound: sign payloads, HTTPS only, SSRF protection when customers supply URLs (block private ranges, DNS rebinding checks, no redirects to internal hosts), timeouts, retry policy, secret rotation with dual-secret window, payloads minimal (thin events), delivery logs for customers.
- Weak: "just check a secret query param"; parses JSON before verifying signature.
- Follow-ups: How do you rotate secrets without dropping events? How do you stop customers pointing webhooks at your metadata service? How do you handle out-of-order events?

### M8

**Q-A5-M8-01 "What is the transactional outbox pattern and what problem does it solve?"** — Hard. `backend.async-events`. Cluster: global-product, Big4, global-IT-consulting.
- Outline: dual-write problem (DB commit succeeds, publish fails, or vice versa); write state change and an outbox row in the same DB transaction; relay (poller with `SELECT ... FOR UPDATE SKIP LOCKED`, or CDC with Debezium) publishes to broker, marks sent; guarantees at-least-once so consumers must be idempotent (inbox table or dedupe by event id); ordering per aggregate via keying/partitioning; cleanup/retention; monitoring outbox lag; alternatives and costs (polling publisher load, CDC operational weight); no "exactly once" claim end to end.
- Weak: "publish after commit in a try block"; "use Kafka transactions" as complete answer.
- Follow-ups: How does the relay avoid double-publishing with multiple instances? How do you preserve order? What happens when the broker is down for an hour?

**Q-A5-M8-02 "Design a webhook delivery system for customers."** — Hard. `backend.async-events`, `api.design`. Cluster: global-product, AI-startup, Indian-product.
- Outline: event creation persisted (outbox), per-endpoint subscriptions, delivery workers via queue, HTTP POST with signature and event id, timeouts (short), success = 2xx, retries with exponential backoff and jitter over hours/days, disable endpoint after sustained failure with notification, DLQ and manual replay, per-endpoint concurrency and rate limits to isolate slow consumers, ordering not guaranteed (document, include sequence/timestamp), thin vs fat payloads, delivery log UI/API, secret rotation, SSRF controls, metrics (success rate, latency, backlog), idempotent consumer guidance.
- Weak: synchronous call in the request path; unlimited retries; one queue where one slow customer blocks others.
- Follow-ups: How do you prevent a slow consumer from starving others? How do you support replay of last 7 days? How would you scale to 10k events per second?

**Q-A5-M8-03 "Several teams consume the domain events your service publishes. How do you design the event contract and evolve it without breaking anyone?"** — Hard. `backend.async-events`, `api.evolution`. Cluster: global-product, global-IT-consulting, Indian-product. (Worker-level job semantics and the choice of job library are owned by A2.M9 and Q-A2-M9-02, `ref`; delivery-semantics theory is A6.M4.)
- Outline: event as a public contract: stable type name, version field, unique event id, occurred-at, producer, correlation and causation ids, a schema registry or AsyncAPI document in the repo; thin event (ids plus changed fields) versus fat event (state snapshot) trade-offs: coupling, payload size, privacy, consumer round trips; compatibility rules: additive optional fields are safe, removing or retyping fields or changing meaning is breaking, enum widening breaks strict consumers, tolerant-reader policy for consumers; evolution paths: dual-publish old and new versions for a deprecation window, upcasters, consumer-driven checks in CI (pointer A8.M3), explicit sunset dates; ordering key per aggregate and what you promise (at-least-once, per-key order, no global order); replay and retention (consumers must tolerate old versions on replay); PII minimisation and tenant id in the envelope; ownership and review process for schema changes.
- Weak: "just publish JSON"; renames a field because "nobody uses it"; no version or id; assumes global ordering or exactly-once.
- Follow-ups: A consumer team cannot migrate for a quarter: what do you do? How do you find out who consumes an event? How do you test that a new producer version is compatible with recorded old events?

### M9

**Q-A5-M9-01 "REST vs GraphQL vs gRPC: how do you choose?"** — Medium. `backend.architecture`, `api.design`. Cluster: global-product, global-IT-consulting, AI-startup.
- Outline: REST: resource-oriented, HTTP caching, broad tooling, public APIs; GraphQL: client-driven shape, aggregation for varied clients (mobile, web), single endpoint, complexity (N+1, cost control, caching, authorization per field), good fit for BFF; gRPC: Protobuf, strong contracts, streaming, performance, internal service-to-service, poorer browser story; criteria: consumers, latency, team skills, evolution, tooling, observability; hybrid is common (public REST, internal gRPC, BFF GraphQL); WebSocket/SSE for push.
- Weak: "GraphQL is the modern way"; performance claims without basis.
- Follow-ups: How do you cache GraphQL responses? How do you secure GraphQL against expensive queries? How do you evolve Protobuf messages?

**Q-A5-M9-02 "Explain the GraphQL N+1 problem and how DataLoader fixes it."** — Medium. `api.design`, `db.orm`. Cluster: Indian-product, global-product.
- Outline: resolvers per field run per parent item causing one query per child; DataLoader batches keys within one tick of the event loop into a single `WHERE id IN (...)` and caches per request; per-request instance (avoid cross-user cache leaks); ordering results to match keys; alternatives: join/prefetch via query look-ahead; add depth and complexity limits, persisted queries; monitoring resolver counts.
- Weak: "add caching"; global DataLoader.
- Follow-ups: How do you apply this with SQLAlchemy async? How do you authorize per field? What about pagination within nested lists?

**Q-A5-M9-03 "SSE, WebSocket or polling for pushing job status or LLM token streams?"** — Medium. `api.design`. Cluster: AI-startup, global-product.
- Outline: SSE: one-way server push over HTTP, auto-reconnect with `Last-Event-ID`, proxy-friendly, simple for token streaming; WebSocket: bidirectional, stateful connections, needs sticky routing or pub/sub fan-out, auth on upgrade; long polling: broad compatibility, higher overhead; polling: simplest and cacheable; considerations: connection limits, timeouts at LB/proxy, backpressure, heartbeat, authentication refresh, horizontal scaling via Redis pub/sub or a broker, resumability; pick by directionality and scale.
- Weak: "WebSockets for everything"; ignores load balancer idle timeouts.
- Follow-ups: How do you scale WebSocket servers horizontally? How do you resume after a dropped connection? How do you authenticate and re-authenticate?

### M10

**Q-A5-M10-01 "Explain hexagonal (ports and adapters) architecture in a Python backend and what it buys you."** — Medium-Hard. `backend.architecture`. Cluster: global-IT-consulting, Big4, global-product.
- Outline: domain/core independent of frameworks and I/O; ports are interfaces (Protocols/ABCs) the core defines (repositories, payment gateway, clock, notifier); adapters implement them (SQLAlchemy repo, HTTP client, in-memory fake); inbound adapters (FastAPI routes, CLI, workers) call application services/use cases; dependency inversion with injection (`Depends`, constructor injection); benefits: testability without DB, swapping infra, clearer boundaries; costs: boilerplate, mapping between models; when it is overkill (simple CRUD); unit of work for transactions; keep domain models separate from ORM and API schemas when complexity warrants.
- Weak: describes folder names only; applies five layers to a trivial CRUD; imports ORM into domain.
- Follow-ups: Where does the transaction boundary sit? How do you test the service layer? How does this map to Django where the ORM is central?

**Q-A5-M10-02 "Modular monolith or microservices for a new product with 15 engineers?"** — Hard. `backend.architecture`. Cluster: global-product, Big4, AI-startup.
- Outline: start with modular monolith: clear module boundaries, public interfaces, no cross-module table access, enforce via import rules, per-module data ownership (schema separation) so extraction is possible; microservices cost: network failures, distributed transactions, observability, deployment tooling, versioning, data consistency; justified by independent scaling, team autonomy at larger org, different tech/compliance needs, fault isolation; distributed monolith smells (shared DB, lockstep deploys, chatty calls); extraction via strangler fig and outbox events; Conway's law; decision records; measure before splitting (e.g. one hot component such as an LLM worker could be separate).
- Weak: "microservices are scalable"; "monolith is legacy".
- Follow-ups: How do you enforce module boundaries in Python? How do you split out the first service? How do you handle a query joining data across modules?

**Q-A5-M10-03 "FastAPI vs Django (DRF) vs Flask: how do you choose for a new service?"** — Medium. `backend.frameworks`. Cluster: IT-services, Indian-product, global-IT-consulting, AI-startup.
- Outline: FastAPI: ASGI, async-first, Pydantic validation, auto OpenAPI, dependency injection, good for API-only and AI/streaming services, you choose ORM (SQLAlchemy), migrations (Alembic), auth, admin yourself; Django: batteries (ORM, migrations, admin, auth, forms), mature ecosystem, DRF or Django Ninja for APIs, great for data-heavy CRUD plus admin, async support is real, async ORM methods exist but DB I/O still passes through a thread-sensitive adapter unless the driver path is natively async **[verify: Django 6.1 async docs page not readable from primary source]**; Flask: minimal WSGI micro-framework (async support limited), flexible, suited to small services and prototypes, you assemble everything; criteria: team skills, time to market, admin needs, async/streaming needs, performance (usually DB-bound), ecosystem, long-term maintenance; mixing is possible (Django for admin/core, FastAPI for high-throughput edge).
- Weak: benchmarks only; "FastAPI is always faster so always better"; unfamiliarity with Django async limits.
- Follow-ups: What do you lose with FastAPI that Django gives for free? How does sync code behave inside FastAPI `async def` vs `def` endpoints? How do you run Django under ASGI and what changes?

### M11

**Q-A5-M11-01 "Compare multi-tenancy models and how you enforce isolation."** — Hard. `backend.production`. Cluster: global-product, Big4, Indian-product.
- Outline: shared schema with `tenant_id` (cheapest, highest leakage risk, noisy neighbours), schema-per-tenant (better isolation, migration fan-out, connection management), database-per-tenant (strongest isolation, per-tenant restore, highest cost/ops); hybrid for large customers; enforcement: derive tenant from authenticated context, never trust client input, mandatory scoping in the data layer (repository base, ORM manager), PostgreSQL row-level security with `SET LOCAL` tenant setting per transaction (pooler caution), tests that attempt cross-tenant access; per-tenant rate limits and quotas; tenant-aware caching keys; backups and data deletion per tenant; migrations strategy.
- Weak: "add tenant_id column" with no enforcement story; session-level variables leaking via pooled connections.
- Follow-ups: How do you migrate 5,000 schemas? How does PgBouncer transaction pooling interact with RLS settings? How do you move a tenant between shards?

**Q-A5-M11-02 "Design secure large-file uploads for an API."** — Medium-Hard. `backend.production`, `sec.api`. Cluster: IT-services, global-product, Big4.
- Outline: avoid streaming big files through app servers; presigned S3 URL (PUT or POST policy) with size/content-type constraints and short expiry; multipart/resumable for large files; client notifies completion or S3 event triggers processing; server validates after upload (magic bytes not extension, size, virus scan, image re-encoding), quarantine bucket before release; metadata row with status; per-user quotas and rate limits; private objects and signed downloads; checksum verification; cleanup of abandoned uploads (lifecycle rules); if proxying, stream to storage with limits (Starlette/Django upload handlers) rather than buffering.
- Weak: base64 JSON bodies; trusting `Content-Type`; unlimited size.
- Follow-ups: How do you prevent users overwriting others' objects? How do you resume a failed 5 GB upload? How do you handle post-upload processing failures?

**Q-A5-M11-03 "After a large tenant was onboarded, one endpoint's p99 doubled while the median stayed flat. Which API-layer causes do you check first?"** — Medium-Hard. `backend.production`, `db.sql`. Pointer: the load-test and bottleneck-attribution method is owned by A8.M10 (O-63); this item stays at the API layer. Cluster: global-IT-consulting, Indian-product, global-product.
- Outline: per-tenant percentiles split (tenant skew); tenant-scoped N+1 or missing tenant index; connection pool saturation vs worker count; event loop blocking in async handlers; serialization cost on large tenant payloads; noisy-neighbour quota; mitigations: per-tenant limits, pagination, pool and worker sizing; measurement method and profiling -> A8.M10 and A1.M11.
- Weak: "add more servers"; jumps to rewriting in another language.
- Follow-ups: How do you tell queueing delay from slow execution? What if only some tenants are slow? How do you size a DB pool relative to workers?

## 5. Final assessment

- **Mastery check:** 30 items sampled across M1–M11 (MCQ, predict-the-result, scenario), pass at 80%, weighted towards M4, M6, M8, M10.
- **Timed task (60 min, IDE):** implement an order-creation API in FastAPI with Idempotency-Key handling, keyset-paginated listing, problem-details errors, token-bucket rate limiting backed by Redis, and an object-level authorization check; hidden tests include duplicate and concurrent requests, a cross-tenant access attempt, and a pagination-under-insert case.
- **System-design/canvas task (35 min):** "Design the public API and backend for a multi-tenant notifications/webhook platform" covering resource model, versioning, auth, idempotency, outbox, delivery, rate limits, observability; scored against the API-and-data-model, consistency, failure-mode, security and observability rubric, with auto-evaluation against A6 registry IDs only (R-IDEMP-1, R-DUAL-1, R-RATE-1, R-QUEUE-2; overall 3.5 or higher, no dimension below 3, no critical findings; G-7).
- **Voice mock round (20 min):** adaptive interviewer, mixes M4, M6, M8 and M10 questions with follow-up trees; strict mode (no hints).
- **Evidence for course badge:** all module badges plus assessment report plus design artefact.

- Final-assessment ownership: items copied from other banks are `ref` only (at most one per pointer topic). Mastery check weighting M4, M6, M8, M10 about 50 percent of items. Readiness (G-6) additionally needs the strict voice drill on `api.reliability`, `api.authn-authz`, `backend.async-events`, `backend.architecture` at Senior bar in two sessions at least 48 h apart.

## 6. Course-level risks and freshness notes (as of October 2026)

**Verified this review (source type in brackets):**
- IETF `RateLimit` and `RateLimit-Policy` fields: Internet-Draft version 11 (May 2026), not an RFC; a 2026 directorate review was unfavourable, so syntax may still change; teach as "converging, vendor-implemented, not final" [IETF datatracker].
- `Idempotency-Key` header: IETF httpapi draft version 07 (last revision 2025-10-15), now an **expired** Internet-Draft with no RFC intended (datatracker shows "Expired", 2026-04-18); the Stripe-style pattern remains de facto, so teach the semantics and say the header name is a convention, not a standard [IETF datatracker, verified 2026-10-05]. The RateLimit draft is version 11 (updated 2026-05-23, still active; the HTTPDIR early review of version 10 was "Not ready") [IETF datatracker, verified 2026-10-05].
- OAuth 2.1: still an Internet-Draft (draft-15 of March 2026 or later; verified 2026-10); PKCE-for-all and no implicit/password grants are already best practice [IETF datatracker]. DPoP is RFC 9449 [IETF].
- OWASP API Security Top 10: the 2023 edition is still the current list; no newer edition found [OWASP project and secondary sources]. Owner is A8.M7.
- OpenAPI: 3.2.0 released September 2025, backward compatible; 3.1 stays the teaching baseline [OpenAPI Initiative].
- Redis licensing: Redis 8 added AGPLv3 as a licence option (2025); Valkey (BSD-3, Linux Foundation) is the ElastiCache default [vendor and secondary sources]. Designs say "Redis-compatible".
- Arq is maintenance-only (README, issue #510, announced Oct 2025; last releases 0.27.0 Feb 2026 and 0.28.0 Apr 2026); Celery 5.6 is stable (5.6.3, Mar 2026); Dramatiq 2.x is active; RQ 2.x is active [PyPI and project README, verified 2026-10].
- Django 6.1 current (Aug 2026), 6.0 out of mainstream support, 5.2 LTS remains the teaching baseline; FastAPI is on the 0.14x line; Flask 3.1 [secondary sources, so pins come from the platform manifest].

**Remaining [verify] (2):** (1) manifest pins for FastAPI, Django, Flask, SQLAlchemy at publish; (2) Django 6.1 native async DB coverage (primary docs page not readable during verification). Resolved 2026-10: Strawberry, Ariadne, Graphene status; OPA, Cedar, OpenFGA status; RQ cadence; Arq dates (see _reviews/Verification_Log_A5-A8.md).

**Other notes**
- Re-check the draft status of rate-limit, idempotency-key and OAuth 2.1 documents every 3 months; if any becomes an RFC, update M4 and M6 text and the dated stamps.
- Overlap boundaries are those in the Ownership notes table; the main risk is drift back into A8 exploit content in M7 and A6 theory in M8.
- Code examples run on pinned versions in CI; the LLM-backed endpoint example uses the `LLMClient` adapter and the scripted mock model, with no model names.

## 7. Question tags, senior-bar notes and added probes

### 7.1 Goal tags and senior-bar note per seed question (G-10)

| ID | Goal | Senior-bar note (mid / senior / staff) |
|---|---|---|
| Q-A5-M1-01 | B:S A:N H:S | mid: PUT replaces; senior: idempotency depends on patch format plus If-Match; staff: contract-level semantics policy |
| Q-A5-M1-02 | B:S A:N H:S | mid: dedupe by user; senior: key plus unique constraint; staff: client UX and retention policy |
| Q-A5-M1-03 | B:S A:N H:S | mid: memorised list; senior: consistency and existence-leak policy; staff: org-wide error taxonomy |
| Q-A5-M2-01 | B:S A:N H:S | mid: /v2; senior: expand and contract with usage telemetry; staff: consumer-governance and sunset policy |
| Q-A5-M2-02 | B:S A:N H:S | mid: picks one; senior: trade-offs incl. Vary and routing; staff: version granularity strategy |
| Q-A5-M2-03 | B:S A:N H:S | mid: JSON error; senior: RFC 9457 plus stable codes; staff: cross-service consistency tooling |
| Q-A5-M3-01 | B:S A:N H:S | mid: cursor faster; senior: opaque cursor, tie-breaker and invalidation rules in the contract; staff: count strategy, cursor versioning and filter changes between pages |
| Q-A5-M3-02 | B:S A:N H:S | mid: allow any column; senior: whitelist aligned to indexes; staff: when to move to a search engine |
| Q-A5-M3-03 | B:S A:S H:S | mid: poll; senior: 202 job resource with failure model; staff: scale of notifications and retention |
| Q-A5-M4-01 | B:M A:S H:M | mid: SETNX; senior: unique constraint plus fingerprint plus in-flight state; staff: crash windows and downstream keys |
| Q-A5-M4-02 | B:M A:S H:M | mid: names an algorithm; senior: atomic Lua, fail-open policy, tenant fairness; staff: cost-weighted and multi-region limits |
| Q-A5-M4-03 | B:M A:S H:M | mid: more retries; senior: jitter, budgets, one retry layer; staff: system-wide amplification analysis |
| Q-A5-M5-01 | B:M A:S H:M | mid: TTL; senior: single-flight plus jitter plus early refresh; staff: consistency windows and failure modes |
| Q-A5-M5-02 | B:M A:S H:M | mid: sets max-age; senior: private vs shared, Vary, validators; staff: CDN key and purge design |
| Q-A5-M5-03 | B:M A:S H:M | mid: Redis is durable; senior: RDB vs AOF, async replication loss, eviction; staff: fencing tokens and Redlock critique |
| Q-A5-M6-01 | B:M A:S H:M | mid: recites flow; senior: why PKCE, state, nonce; staff: BFF vs SPA token storage and DPoP |
| Q-A5-M6-02 | B:M A:S H:M | mid: decode and trust; senior: pinned alg, iss, aud, JWKS rotation; staff: revocation and key-rotation operations |
| Q-A5-M6-03 | B:M A:S H:M | mid: role checks; senior: central object-level policy, default deny; staff: ReBAC, audit, permission caching |
| Q-A5-M7-01 | B:S A:N H:S | mid: validate input; senior: separate request and response models; staff: CI guard against leaked fields |
| Q-A5-M7-02 | B:S A:N H:S | mid: CORS blocks attackers; senior: browser-only, credentials rules; staff: cookie plus token dual-client design |
| Q-A5-M7-03 | B:S A:S H:S | mid: shared secret; senior: HMAC over raw body, timestamp, dedupe; staff: rotation with dual secrets, SSRF on outbound |
| Q-A5-M8-01 | B:M A:S H:M | mid: publish after commit; senior: outbox plus idempotent consumer; staff: relay scaling, ordering, lag SLOs |
| Q-A5-M8-02 | B:M A:S H:M | mid: sync POST; senior: queue, backoff, DLQ, replay; staff: per-endpoint isolation and 10k events per second |
| Q-A5-M8-03 | B:M A:S H:M | mid: publishes JSON and hopes; senior: versioned envelope, compatibility rules, deprecation window; staff: schema governance, consumer discovery, replay-safe evolution |
| Q-A5-M9-01 | B:S A:N H:S | mid: GraphQL is modern; senior: criteria and hybrid; staff: org and governance fit |
| Q-A5-M9-02 | B:S A:N H:S | mid: add caching; senior: DataLoader per request; staff: field-level auth and cost limits |
| Q-A5-M9-03 | B:S A:S H:S | mid: WebSocket for all; senior: SSE vs WS by direction and proxies; staff: fan-out scaling and resume |
| Q-A5-M10-01 | B:M A:S H:M | mid: folder names; senior: ports, adapters, UoW; staff: when it is overkill and mapping costs |
| Q-A5-M10-02 | B:M A:S H:M | mid: microservices scale; senior: modular monolith with enforced boundaries; staff: extraction criteria, Conway |
| Q-A5-M10-03 | B:M A:S H:M | mid: benchmarks; senior: criteria and async reality; staff: hybrid and team topology |
| Q-A5-M11-01 | B:S A:N H:S | mid: tenant_id column; senior: enforcement layer and RLS caveats; staff: migration fan-out and tenant moves |
| Q-A5-M11-02 | B:S A:N H:S | mid: proxy upload; senior: presigned plus validation and quarantine; staff: resumable and cleanup lifecycle |
| Q-A5-M11-03 | B:S A:S H:S | mid: add servers; senior: pool, worker and event-loop checks per tenant; staff: tenant skew, quotas and noisy-neighbour response (method -> A8.M10) |

### 7.2 Added probes (two failure-scenario, two senior-bar upgrade per module)

| Module | Failure-scenario probes | Staff-level probes |
|---|---|---|
| M1 | Two clients PATCH the same order at once: what is lost and how do you detect it? A proxy retries a POST after a timeout: what duplicates? | How would you set org-wide method and status conventions and enforce them? When is RPC style the better contract? |
| M2 | A strict client breaks when you add an enum value: who is at fault and how do you prevent it? A cached v1 SDK sends removed fields: what happens? | How do you run a 12-month deprecation across 40 consumers? How do you govern error codes across teams? |
| M3 | Rows inserted during paging: show the skip or duplicate. A bulk request half-fails: what does the client retry? | How do you design cursors that survive a sort-order change? How do you cap cost of arbitrary filters? |
| M4 | Duplicate request race: two identical POSTs arrive 2 ms apart, walk the state machine. Redis is down: fail-open or closed per endpoint, and why? Token replay on the idempotency key by another user: how is scope enforced? | Design retry budgets across three service layers. How would you rate limit by cost with fairness across tenants and regions? |
| M5 | Cache poisoning: an unkeyed header changes the cached body for everyone. Stale write after delete: show the interleaving. | Design invalidation for a page assembled from five entities. How do you reason about consistency windows with a Redis-compatible failover? |
| M6 | Token replay: a stolen access token is reused, what limits the blast radius? A refresh token is used twice, what does the server do? | Design token revocation for 10 million sessions. When would you adopt sender-constrained tokens or an external policy engine? |
| M7 | A preflight cached with a wildcard origin and credentials: what leaks? A mass-assignment field is added by a new developer: how does CI catch it? | Define the design-time security checklist for every new API. Where do you draw the line to hand off to AppSec? |
| M8 | Crash between commit and publish: show recovery. A consumer sees the same event twice, and an old event after a new one: what breaks? | Design outbox lag SLOs and backpressure. When would you move from a polling relay to CDC? |
| M9 | A client sends a deeply nested GraphQL query: what protects you? A Protobuf field number is reused: what is the failure? | How would you govern a federated graph across teams? When do you reject GraphQL outright? |
| M10 | A repository method silently opens its own transaction inside a use case: what breaks? Two modules read each other's tables: what is the first symptom? | How do you decide the first service to extract and prove the boundary is real? How do you steer a team from five layers back to two? |
| M11 | A pooled connection keeps the previous tenant setting: show the leak. A presigned URL is reused by another user: what limits it? | Design per-tenant quotas and a noisy-neighbour response. How do you drive a p99 regression from alert to root cause across teams? |

## 8. Learner-Fit Appendix

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 (new) | Simpler, all lessons, 21.5 h; prerequisite links to HTTP and one framework primer | Guided labs, library patterns, 70 percent guided / 30 percent open | nothing | none until M1-M4 mastery passes |
| L2 (junior-mid) | Simpler, skim known lessons, about 16-19 h | Guided then open labs; the M4 and M8 labs unscaffolded | lessons scored 85+ | one Challenge per module after pass |
| L3 (experienced) | Deeper, about 12-14 h; study "explain at depth" lessons (A5.M4.1, M4.4, M5.2, M6.3, M8.4, M10.3) | Open labs with hidden tests; failure probes | foundation lessons in M1, M3, M9, M11 | Challenge on M4, M6, M8, M10 |
| L4 (senior) | Deeper plus Challenge, about 9-12 h net of skips; mastery check first, study only misses | Open labs and canvas; staff probes in voice drills | any module at 85+ (checks still taken) | all staff probes (7.2) and the notifications-platform canvas |

### LF-2 Module matrix

Diagnostic items are `placement` titles drawn from the MCQ/predict buckets (IDs `Q-A5-Mn-P1..Pk`, aligned with the data-model ID pattern; legacy form `P-A5-Mn-k`). Bands (G-5): >= 85 skip lessons and take the check; 70-84 skim (Deeper lessons and check); 50-69 study; < 50 deep-study (Simpler path plus remediation links in LF-5). Tags `B:x A:x H:x`: M4, M6, M8, M10 per boss; M2 and M5 chosen by author. Weights are percent of interview time per target, summing to 100 `[judgement]`: heaviest where senior backend and AI-platform interviews probe failure modes (reliability, auth, async, architecture).

| Module | Diagnostics (5-8) | Tags | Weight B / A / H | Rehearsal modes | Module readiness criterion (G-6 specialised) |
|---|---|---|---|---|---|
| M1 | 5: PUT vs PATCH idempotency; 409 vs 422; DELETE repeat; action as sub-resource; safe vs idempotent | B:S A:N H:S | 6 / 5 / 6 | voice drill | check >= 85 on retake; names method and status choices unprompted |
| M2 | 6: breaking-change classification x3; RFC 9457 fields; Sunset header; enum addition | B:S A:N H:S | 6 / 5 / 5 | voice, canvas | classifies 12 changes with >= 11 correct; voice Senior bar on `api.evolution` |
| M3 | 6: offset cost; tie-breaker; cursor opacity; count cost; 202 job; bulk partial failure | B:S A:N H:S | 6 / 4 / 5 | voice, coding | keyset task passes hidden tests incl. insert-during-paging |
| M4 | 8: idempotency state machine; token bucket vs fixed window; Lua atomicity; jitter; retry storm; Retry-After; breaker states; ETag If-Match | B:M A:S H:M | 16 / 18 / 17 | voice, coding, canvas | timed idempotency plus token-bucket task passes; storm lab reproduced and fixed; voice Senior bar on `api.reliability` x2 sessions |
| M5 | 7: cache-aside race; no-cache vs no-store; Vary; stampede fixes; eviction; RDB vs AOF; negative caching | B:M A:S H:M | 10 / 8 / 9 | voice, coding | single-flight lab passes; explains stale-write race unprompted |
| M6 | 8: PKCE purpose; ID vs access token; alg confusion; aud check; refresh rotation; JWKS rotation; RBAC vs ABAC; revocation | B:M A:S H:M | 12 / 11 / 12 | voice, canvas | validates JWT checklist with no omissions; voice Senior bar on `api.authn-authz` x2 |
| M7 | 5: CORS credentials rule; CSRF vs CORS; mass assignment; security headers; TLS termination | B:S A:N H:S | 5 / 5 / 5 | voice | check >= 85; hand-off line to A8.M7 stated correctly |
| M8 | 8: dual write; outbox relay; idempotent consumer; HMAC raw body; DLQ replay; ack timing; ordering key; poison message | B:M A:S H:M | 12 / 10 / 11 | voice, coding, canvas | outbox crash-recovery lab passes; webhook design on canvas >= 3.5 |
| M9 | 5: N+1 and DataLoader; gRPC field numbers; SSE vs WebSocket; GraphQL cost limit; BFF role | B:S A:N H:S | 4 / 9 / 6 | voice | selection matrix justified for three scenarios |
| M10 | 7: ports and adapters; UoW boundary; distributed-monolith smell; Depends injection; strategy vs factory; framework criteria; Django async | B:M A:S H:M | 15 / 15 / 15 | voice, canvas, coding (lld) | refactor lab passes with in-memory adapter; voice Senior bar on `backend.architecture`; 2 of 6 `lld` seeds solved cleanly |
| M11 | 6: tenant leak via pool; presigned constraints; magic-byte check; pool vs workers; per-tenant p99 API-layer causes; graceful shutdown | B:S A:N H:S | 8 / 10 / 9 | voice, canvas | per-tenant p99 API-layer cause list given in under 4 minutes; method pointer to A8.M10 |

### LF-3 Experience bridge ("you already know" to "gap at senior depth")

| Module | You already know | Gap between using and explaining at senior depth |
|---|---|---|
| M1 | FastAPI/DRF routes and status codes | Stating safe/idempotent semantics, PUT vs PATCH formats and 409 vs 422 with reasons |
| M2 | Pydantic models, auto OpenAPI | Breaking-change taxonomy, tolerant reader, deprecation process, RFC 9457 |
| M3 | LIMIT/OFFSET, DRF pagination classes | Why offset breaks, tuple keyset plus index, count cost, partial failure |
| M4 | Redis INCR, retry decorators, Stripe-style keys, LLM API 429 handling | Race windows, crash points, retry amplification, atomic Lua, fail-open policy |
| M5 | Redis cache, Django cache framework, CDN | Invalidation, stampede, Vary/private, persistence and failover loss |
| M6 | JWT with FastAPI, Django auth, OAuth login via a provider | Validation checklist, rotation and revocation, PKCE reasoning, object-level policy |
| M7 | CORS middleware, CSRF in Django | What CORS does not do, schema-level exposure control, hand-off to security course |
| M8 | Celery tasks, webhooks from Stripe-like APIs, AWS Lambda triggers, MQTT | Outbox, idempotent consumers, ack semantics, DLQ replay, delivery logs |
| M9 | REST mostly; SSE for LLM streaming | Criteria-based selection, DataLoader, Protobuf evolution, proxy limits |
| M10 | Django/FastAPI project layout | Ports and adapters, UoW, modular monolith enforcement, framework criteria, patterns |
| M11 | Docker, Gunicorn/Uvicorn, S3 uploads, voice-agent latency work | Isolation guarantees, presigned security, pool sizing, tail-latency method |

### LF-4 Senior-bar framing

| Module | Mid | Senior | Staff |
|---|---|---|---|
| M1-M3 | Knows rules | Gives reasons, edge cases, consistency policy | Sets conventions across teams, governs evolution |
| M4 | Names patterns | Walks race and crash windows, picks fail modes | Designs system-wide budgets, fairness, cost-weighted limits |
| M5 | Adds TTL | Handles invalidation, stampede, Vary | Quantifies consistency windows and failover behaviour |
| M6 | Uses a library | Validates tokens fully, designs rotation and revocation | Designs policy architecture and key operations at scale |
| M7 | Enables CORS | Separates schemas, explains limits of CORS | Owns the design-time checklist and AppSec hand-off |
| M8 | Runs a worker | Outbox plus idempotent consumer | Lag SLOs, relay scaling, CDC trade-off, replay tooling |
| M9 | Prefers a favourite | Selects by criteria, controls GraphQL cost | Governance of graph or schema across teams |
| M10 | Applies layers everywhere | Right-sizes structure, enforces boundaries | Chooses extraction order, aligns to team topology |
| M11 | Adds tenant_id | Enforcement layer, secure uploads | Quotas, noisy neighbours, tail-latency culture |

### LF-5 Adaptive hooks

| Hook | Rule |
|---|---|
| Mastery | Module check 80 percent; retake spaced >= 7 days for readiness (G-6) |
| Remediation links | M3 -> A4.M4 (indexes); M4 -> A2 (async basics, in-process queues); M5 -> A4 (DB caching side); M6 -> M1 and M2 (HTTP and contracts); M8 -> A2 and A4 (SKIP LOCKED, transactions); M10 -> A8.M6 (SOLID); M11 -> A4.M2 (RLS), A7.M9 (observability) |
| Challenge extensions | M4 cost-weighted limiter with fairness; M6 sender-constrained token variant; M8 ordered outbox with partition keys; M10 extract-a-service plan |
| Reorderable by the engine | M1, M3, M7, M9, M11 may be deferred or reordered; M4, M6, M8, M10 stay in sequence; M8 needs M4 first (idempotency) |
| Route weights (G-3) | Backend route: M4, M6, M8, M10 at 1.0, others 0.6 or less per tag; AI route: those four SHOULD (0.6), M1, M2, M3, M7, M9, M11 skim primers (0.25) except M9 streaming choice and M11 uploads kept as short lessons |

### LF-6 Applied labs (production-framed)

| Module | Lab |
|---|---|
| M1 | Repair a SaaS billing-style API with verbs in URLs and 200-with-error bodies |
| M2 | Evolve a mobile-app API to v2 with a Sunset plan and a compatibility shim |
| M3 | Paginate an IoT telemetry feed under continuous inserts with a composite cursor |
| M4 | LAB-A5-IDEMP (payment create), LAB-A5-TOKENBUCKET (multi-tenant limiter), LAB-A5-RETRYSTORM (retry amplification and budget fix), LAB-A5-BREAKER (breaker and bulkhead for a slow downstream) |
| M5 | Cache an AI-assistant conversation-summary endpoint with single-flight and private/Vary correctness |
| M6 | Login, refresh rotation and object-level checks against the mocked token issuer, then break a vulnerable validator |
| M7 | Dual-client (SPA cookie, mobile token) CORS and CSRF configuration plus schema allow-lists |
| M8 | LAB-A5-OUTBOX (order events with crash between commit and publish), LAB-A5-WEBHOOK (signed sender with retries and DLQ), voice-call-completed webhook receiver |
| M9 | Fix N+1 in a GraphQL resolver; choose a protocol for MQTT-to-cloud device telemetry and an LLM stream |
| M10 | Refactor a fat Django or FastAPI app to hexagonal layers; solve two `lld` seeds (rate limiter, notification dispatcher) with patterns |
| M11 | Tenant scoping plus presigned upload for a multi-tenant document-AI SaaS; per-tenant p99 API-layer cause hunt (load-test method by pointer to A8.M10) |

### LF-7 Currency and quality bar

| Module | Memorable visual or analogy | Top 3 misconceptions handled | Stamp |
|---|---|---|---|
| M1 | Method semantics matrix as a "lift buttons" panel (read-only, same-result, state-changing) | REST = JSON over HTTP; PATCH never idempotent; 200 with error body | Oct 2026 |
| M2 | Breaking-change decision tree; "bridge with traffic still on it" | adding a field is always safe; /v2 is a strategy; enum additions are safe | Oct 2026 |
| M3 | Scan animation: offset re-reads the whole book, keyset uses a bookmark | cursor is faster by magic; exact counts are free; ids are safe cursors | Oct 2026 |
| M4 | Retry-storm animation: a "stampede of echoes"; token bucket as a bucket with a tap | idempotent = safe; retries are always good; gateway limit is enough | Oct 2026 (header drafts dated) |
| M5 | Layered cache "sandwich"; stampede timeline | Redis is a database; no-cache = no-store; one TTL for all | Oct 2026 |
| M6 | Passport and visa analogy for ID vs access tokens; PKCE sequence | JWT is encrypted; OAuth is authentication; stateless means no revocation | Oct 2026 |
| M7 | Trust-boundary diagram: "CORS is the browser's bouncer, not your lock" | CORS protects the server; HTTPS = secure; ORM objects are fine to return | Oct 2026 |
| M8 | Dual-write failure timeline; outbox as a "postbox inside the same house" | exactly once end to end; publish after commit is safe; webhooks need no signature | Oct 2026 |
| M9 | Comparison matrix; DataLoader as "one waiter, one trip, all orders" | GraphQL replaces REST; gRPC works in browsers directly; WebSocket for everything | Oct 2026 |
| M10 | Hexagon with plugs and sockets; "how many layers does CRUD deserve" | microservices by default; more layers is better design; shared DB is fine | Oct 2026 |
| M11 | Tenant isolation spectrum as apartment vs house vs gated estate | tenant_id column is isolation; scale by adding workers; trust file extension | Oct 2026 |

## 9. Open items for boss

1. Resolved (Track A lead review): the queue-versus-DB decision lesson is A2.M9.1 (A2 module table and M9 lesson list); A5.M8 points to it by that ID.
2. P-09 should expose key-rotation and expiry controls on the mocked issuer and a replay-with-modified-claims switch (needed by M6 labs and the A8 validator-breaking exercise).
3. Confirm C3.M7 accepts the six `lld` seeds as tagged items (strategy, factory, observer, adapter, repository, dependency injection scenarios) and the lab names LAB-A5-TOKENBUCKET and LAB-A5-IDEMP as canonical references for A2 and A6.

## 10. Boss compliance log

- G-1: Ownership notes table added; M2 contract testing, M7 exploits, M8 delivery theory, M9 transports, M10 SOLID/LLD format, M11 observability and RLS converted to pointers.
- G-2, section 5: header 21.5 S / 13.5 D plus 2.5 assessment, S/D semantics sentence, module hours table summing to header.
- G-3, G-11: Learner-Fit Appendix LF-1 to LF-7; tags B/A/H per module and per seed question (7.1).
- G-5 to G-7: placement bands, readiness criterion, thresholds 80 percent and "Meets" voice mock.
- G-8: Python 3.14 image, PostgreSQL 18, Redis-compatible wording, framework pins via manifest.
- G-9: `security.api` renamed `sec.api` (secondary); owned and secondary tags listed.
- G-10: Question IDs `Q-A5-Mn-nn`; goal tag and senior-bar note per question; two failure probes and two staff probes per module.
- G-12: dated October 2026 stamps; no model names; verified items cited by source type.
- G-13: X2 retry, backoff, jitter owned at A5.M4.3.
- R-A5-1: contract testing to A8.M3. R-A5-2: GraphQL/gRPC overview only in M9. R-A5-3: design-patterns lesson A5.M10.6 and six `lld` seeds to C3. R-A5-4: mocked token issuer only. R-A5-5: LLM-backed endpoint example (20 min) in A5.M4.6 with pointer to B2.M8 and B2.M9.
- Upgrade items 1-11: M7 reduced; M4 owns limiter, idempotency, retries, breaker, bulkhead, retry-budget lab and stable lab names; M8 owns outbox and idempotent consumer; M5 owns caching; M6 owns auth; M11 pointer; `[verify]` items attempted (2 remain); failure-scenario probes added.

- Track A lead review (2026-10-05, TRACK-LEAD-A): A5.M8.3 reduced to an API-side pointer (worker semantics owned by A2.M9) and Q-A5-M8-03 re-angled to event-contract design (it duplicated A2 Q9.2); A5.M11.4 retitled to API-layer hooks (diagnosis owned by A8.M10); Q-A5-M7-03 credit tag fixed. Verdict: Approve with conditions. See _reviews/TrackLead_A_Report.md.
