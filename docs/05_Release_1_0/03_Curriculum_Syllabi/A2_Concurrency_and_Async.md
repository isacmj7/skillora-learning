---
name: A2 — Concurrency and Async (Track A) — v2.0 (wave 2, as of October 2026)
course_id: A2
track: A (Backend)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-02, HAR-03 (see _reviews/Boss_Final_Approval.md)
---

# A2 — Concurrency and Async (Track A) — v2.0 (wave 2, as of October 2026)

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | A2 — Concurrency and Async / Track A (Senior Python Backend Mastery) |
| Audience | Experienced Python backend engineers (L3-L4) who use FastAPI/Django/Celery daily but cannot yet explain concurrency behaviour from first principles under interview pressure. L1-L2 learners use the Simpler path (blocking vs non-blocking, what `await` really does). |
| Prerequisites and placement skip | Working Python, functions/classes/generators (A1 is a required prerequisite per the boss dependency graph, A1.M3 GIL semantics and A1.M4 generators; a placement pass on A1 waives it; GIL semantics are owned by A1.M3). Placement bands per module follow G-5 (section 8, LF-2). G-5 applies to every module (score >= 85 skips lessons, the mastery check is always taken). **M1 and M2 are the standing skip candidates for any level**; **M7 (FastAPI async patterns) is the expected additional skip for L4 learners with daily production FastAPI use** (boss section 4.4 `[judgement]`); M3 may also be lesson-skipped by L4 learners on a measured score. M4, M5, M6, M8, M9 and M10 are never banded "skip" from inference (a skip needs at least 4 answered items in the module, placement blueprint section 5.2). No band waives the final assessment. |
| Outcomes (each tied to a competency tag owned by A2) | 1. Choose between threads, processes, asyncio and a task queue for a workload and justify with numbers and failure modes (`concurrency.model-selection`). 2. Explain the event loop: ready queue, selector, timers, task stepping, and why one blocking call stalls everything (`concurrency.event-loop`). 3. Write race-free, deadlock-free threaded code and diagnose either from symptoms (`concurrency.sync-primitives`). 4. Use structured concurrency (TaskGroup, timeouts, cancellation, ExceptionGroup) and shut down gracefully (`concurrency.structured`). 5. Design in-process backpressure, bounded queues and concurrency limits (`concurrency.backpressure`). 6. Build and tune async FastAPI endpoints, streaming, async DB access (`api.fastapi-async`, `db.async-drivers`). 7. Choose and operate a background-job system with correct worker-level semantics (`concurrency.background-jobs`). 8. Test and debug concurrent code, and defend a concurrency design (`concurrency.testing-debugging`). |
| Estimated effort (G-2, section 5 of the boss matrix) | **15.5 h Simpler / 10.5 h Deeper**, plus 2.5 h assessment allowance. Simpler = L1-L2 route with full scaffolding; Deeper = L3-L4 route, hours net of typical skip-ahead, so shorter in wall-clock but harder per hour. Module hours in section 3 sum to these figures. |
| Interview coverage tags (owned) | `concurrency.model-selection`, `concurrency.event-loop`, `concurrency.sync-primitives`, `concurrency.structured`, `concurrency.backpressure`, `concurrency.background-jobs`, `api.fastapi-async`, `db.async-drivers`, `concurrency.testing-debugging`. Secondary (no credit): `python.concurrency-basics` (owner A1.M3, GIL semantics). |
| IDE / sandbox (P-01, P-02, P-04, P-13, P-14) | Python **3.14 default**; 3.13 and 3.15 selectable; **free-threaded 3.14t selectable (A2.M1 benchmark only)**. Per-attempt disposable Redis-compatible and PostgreSQL 18. Celery in eager and in-memory-broker modes plus Redis-backed; RQ with Redis. **No RabbitMQ, Kafka or SQS live.** Fake clock and repeat-run stress runner (xN), task-dump view, <= 4 processes and <= 64 threads, 60 s limit on stress tasks. |
| Badge and certificate | Module badges M1-M10: mastery check >= 80 percent + all required IDE tasks at >= 70 percent hidden tests. Course badge: all module badges + course final mastery check >= 80 percent + timed task and capstone rubric overall >= 3.5 with no dimension below 3 + voice mock at "Meets" (average >= 3.0, none < 2) (G-7). Certificate evidence: code submissions, capstone write-up, scored voice transcript. Interview-ready status uses G-6, not the badge. |
| Authoring owner / reviewer / cadence | SME agent A2 / to be assigned / every 6 months; out-of-cycle at each CPython minor (October) and when free-threading status changes. Last reviewed: 2026-10-05. |

## 2. Module table

Goal tags `B/A/H` = Senior Python Backend / AI Engineer / hybrid (M MUST, S SHOULD, N NICE).

| # | Module (tags) | Module outcome | Key topics | Visuals / analogy | Practice | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Concurrency foundations and the decision guide (`B:M A:S H:M`) | Classify a workload as CPU-, IO- or latency-bound and pick a model with measured justification. | Concurrency vs parallelism; workload classification; Amdahl; context-switch and memory cost per thread/task/process; **owner of the decision guide** (threads, processes, asyncio, queue, offload); GIL = one-slide pointer to A1.M3 (preview recap only); free-threaded 3.14t as a measured experiment; subinterpreters overview | One-page decision flowchart; timeline of CPU- vs IO-bound threads. Analogy: one chef (loop), many chefs sharing one stove (GIL), many kitchens (processes) | Benchmark harness across sequential, threads, processes, asyncio, and 3.14t; write a justification | "GIL makes code thread-safe"; "threads are useless"; "asyncio is faster" with no workload context; concurrency vs parallelism confusion |
| M2 | Threads and synchronisation (`B:M A:S H:M`) | Write correct threaded code; diagnose races and deadlocks. | `threading`, `Lock/RLock/Semaphore/Event/Condition/Barrier`, `queue.Queue`, thread-locals, `ThreadPoolExecutor`; races (check-then-act, read-modify-write); deadlock conditions, lock ordering, executor self-deadlock; pool sizing; multi-process races need DB/Redis coordination | Lost-update interleaving; resource-allocation graph. Analogy: two people editing one whiteboard | Fix lost update; fix two-lock deadlock; bounded worker pool with graceful shutdown | Global lock everywhere; "dict ops are atomic so no lock"; swallowed executor exceptions; daemon thread data loss |
| M3 | Processes and multiprocessing (`B:S A:S H:S`) | Parallelise CPU-bound work and explain start methods, pickling and IPC cost. | `ProcessPoolExecutor`; start methods: **POSIX non-macOS default is `forkserver` since 3.14 (confirmed Oct 2026, docs 3.14.8)**, macOS and Windows `spawn`; fork hazards with threads and connections; pickling; `shared_memory`; chunking; `BrokenProcessPool`; `max_tasks_per_child`; `InterpreterPoolExecutor` overview; Gunicorn workers vs pools | Fork vs spawn state diagram; IPC copy-cost diagram. Analogy: photocopying a binder for each worker vs pointing at the shelf | Parallelise a transform, measure chunk size; fix pickling; survive worker crash; `shared_memory` gain | Processes for tiny tasks; sharing DB connections over fork; assuming globals are shared |
| M4 | asyncio fundamentals and event loop internals (`B:M A:M H:M`) | Trace how a coroutine becomes a Task and how the loop schedules it. | Coroutine vs Task vs Future; `await` as suspension; ready deque, timer heap, selector; `call_soon_threadsafe`; `to_thread`, `run_in_executor`; contextvars; eager tasks (3.12+); uvloop trade-offs; 3.14 loop API changes (policy system deprecated, child-watcher APIs removed, `get_event_loop` no longer auto-creates a loop; use `asyncio.run` or `Runner`) | Loop-iteration step-through; coroutine state machine. Analogy: a single waiter taking orders, never standing at one table | Toy event loop (timers + sockets); hand-trace then verify by logging; convert a threaded fetcher | "`async def` is concurrent"; "asyncio is multithreaded"; fire-and-forget task GC; never-awaited coroutine |
| M5 | Structured concurrency, cancellation, timeouts, graceful shutdown (`B:M A:M H:M`) | Write async code with predictable lifetimes, correct cancellation and shutdown. | `TaskGroup`, `ExceptionGroup`/`except*`; `asyncio.timeout`; `CancelledError` (BaseException, re-raise); `shield` limits; `uncancel/cancelling`; cancelling `to_thread` work does not stop the thread; AnyIO/Trio nursery comparison; `gather` pitfalls; SIGTERM drain; Kubernetes grace period (pod lifecycle -> A7.M3) | Task tree with cancel propagation; orphan vs structured. Analogy: a parent who cannot leave the building until all children are back | `gather` -> TaskGroup + timeouts; SIGTERM drain; fix swallowed cancel; cancel-safe wrapper | Swallowing cancel; `shield` as "safe cleanup"; unawaited cancelled tasks; lost fire-and-forget exceptions |
| M6 | In-process async pitfalls, backpressure and limits (`B:M A:M H:M`) | Design a bounded, observable async pipeline that degrades gracefully. | Blocking calls in coroutines and detection; bounded `asyncio.Queue`; `Semaphore` as concurrency limiter **vs** rate limiter; backpressure vs buffering vs load shedding (block, drop newest/oldest, spill); fan-out/fan-in; **30-line in-process token bucket**; async-lock deadlocks; head-of-line blocking; Little's Law. **Pointers:** Redis rate limiters, sliding window, distributed limiting -> A5.M4 (O-17); retry, backoff, jitter, circuit breaker, bulkhead -> X2 (A5.M4) | Queue fill-level timeline; loop-lag graph. Analogy: a motorway ramp meter | Detect a blocking call (debug mode); bounded crawler (semaphore + queue); in-process token bucket; shed-load pipeline | Unbounded `gather`; unbounded queue; semaphore as rate limiter; threading locks in async code |
| M7 | FastAPI and ASGI async patterns, streaming (`B:M A:M H:M`) | Design endpoints, lifespan resources and streaming correctly for sync/async mixes under load. | ASGI (Uvicorn, Starlette, AnyIO); `def` runs in a threadpool, **AnyIO default limiter 40 tokens (confirmed Oct 2026, AnyIO docs)**, a per-loop cap not a pre-spawned pool; yield-dependencies; lifespan; `BackgroundTasks` limits; **server-side SSE and WebSocket implementation, backpressure to slow clients, disconnect handling (A2 owns; protocol choice -> A5.M9)**; middleware cost; shared `httpx.AsyncClient`; CPU offload; **one lesson: LLM-style workloads mechanics**: provider-limit semaphore, `to_thread` for sync SDKs, cancel upstream on disconnect (provider specifics, 429, token budgets -> B2.M8/M9; Django async note -> M8) | Request path through ASGI; threadpool vs loop dispatch; lifespan ownership. Analogy: restaurant with 40 back-kitchen stoves | Fix sync call in `async def`; lifespan client + pool; SSE with disconnect cleanup; load-test two configs | `async def` with blocking calls; client per request; `BackgroundTasks` for critical work; ignoring disconnects |
| M8 | Async data access (`B:M A:S H:M`) | Use async drivers and pools correctly; reason about connection exhaustion and transactions. | asyncpg, psycopg 3 async, SQLAlchemy 2.x async (greenlet bridge, `MissingGreenlet`, `selectinload`, `expire_on_commit`); pool x workers vs `max_connections`; PgBouncer modes (DB-level -> A4.M7); one session per task; transaction vs cancellation; `redis.asyncio`; `httpx` limits; **one-page Django async note** (async ORM partial, `sync_to_async`, thread sensitivity; Channels out of scope; -> A5.M10) | Pool checkout timeline; connection-budget diagram. Analogy: a car park with fixed bays | Compute pool settings; fix lazy-load error; reproduce pool exhaustion; shared-session bug | One session across `gather`; pool x workers > DB limit; connection held across external call |
| M9 | Background jobs and worker semantics (`B:M A:M H:M`) | Choose a job system and reason about worker-level delivery, retries and operations. | In-process vs queue; Celery worker semantics (`acks_late`, visibility timeout, prefetch, pools, time limits, beat, chord risks); RQ; async-native options; poison messages and DLQ; **decision matrix: DB-backed queue vs broker** (primitive owner A4.M6) with a 30-min lab that is a self-contained preview (the `SKIP LOCKED` worker is provided in the lab; A4.M6 is recommended before it and owns the primitive, G-1); long-job checkpointing; autoscale on queue depth; graceful deploys. **Pointers:** idempotency keys -> A5.M4; outbox -> A5.M8; delivery theory, queue vs log, Kafka-style essentials -> A6.M4; AWS queues -> A7.M5 | Job lifecycle state diagram; duplicate-delivery timeline. Analogy: a post office parcel that may be delivered twice | Celery (eager/memory/Redis) task with retries and DLQ handling; crash-mid-job experiment; `SKIP LOCKED` lab | Claiming "exactly once"; ack before work; ORM objects in payloads; visibility timeout < task time |
| M10 | Testing, debugging, observability and capstone (`B:M A:S H:M`) | Test deterministically, debug stalls in production-like conditions, defend a design. | `pytest-asyncio`/AnyIO plugin, loop scope (`asyncio_default_fixture_loop_scope` must be set explicitly; unset gives a warning in 1.x), fake clocks, `AsyncMock`; testing races with barriers; debug mode, slow callbacks; **`python -m asyncio ps/pstree <pid>` (3.14, confirmed in docs Oct 2026)** plus `asyncio.print_call_graph`; `py-spy dump`, `faulthandler`; loop lag, queue depth, pool utilisation (generic telemetry -> A7.M9; service load testing -> A8.M10; Python profiling -> A1.M11) | "Service hangs" triage flowchart; task-tree dump read-through. Analogy: a hospital triage tree | Deterministic timeout and cancel tests; diagnose three seeded hangs; add loop-lag metric; capstone | `sleep`-fixed flaky tests; over-mocking; no concurrency observability |

**Ownership notes (G-1):**

| Module | OWNS | POINTS |
|---|---|---|
| M1 | decision guide (O-4), workload classification | GIL semantics, free-threading phases -> A1.M3 |
| M2-M5 | threads, processes, asyncio, structured concurrency, cancellation, graceful shutdown (process) (O-11, O-26) | pod lifecycle -> A7.M3 |
| M6 | in-process limits, bounded queues, load shedding (O-12) | rate-limit algorithms and Redis -> A5.M4; retry/jitter/breaker -> X2 (A5.M4) |
| M7 | ASGI async patterns, server-side streaming (O-13) | SSE vs WebSocket choice -> A5.M9; LLM provider streaming and 429 -> B2.M8/M9 |
| M8 | async pools, sessions, async drivers (O-14) | replication, PgBouncer at DB level -> A4.M7; ORM pitfalls -> A4.M10 |
| M9 | worker semantics, DB-queue-vs-broker decision (O-15) | `SKIP LOCKED` -> A4.M6; idempotency -> A5.M4; outbox -> A5.M8; delivery theory -> A6.M4; AWS queues -> A7.M5 |
| M10 | async test and stall diagnosis | telemetry -> A7.M9; load testing -> A8.M10; profiling -> A1.M11 |

## 3. Per-module detail

Question-bank targets per module (G-10): 12 MCQ, 6 code-output, 4 coding (hidden tests), 8 verbal (3 seeded below), 3 scenario. Mastery check: 8 items, 80 percent pass, revision loop on misses. Lesson IDs `A2.Mx.n`. Hours Simpler / Deeper: M1 1.0/0.75, M2 1.7/1.0, M3 1.4/0.9, M4 2.0/1.3, M5 2.0/1.3, M6 1.5/1.0, M7 1.9/1.3, M8 1.6/1.1, M9 1.5/1.0, M10 0.9/0.85 (sums 15.5 / 10.5). Module badge = mastery check + required practice at hidden-test threshold, plus extras below.

- **M1** — .1 Workload types and Amdahl (S: worked examples; D: speed-up ceilings). .2 GIL recap preview, one slide (pointer to A1.M3). .3 The decision guide and flowchart (S: table; D: memory, latency, isolation, debuggability, 6 scenario items). .4 Free-threading and subinterpreters (S: what to say; D: ecosystem readiness, single-thread overhead). .5 Benchmark lab. Extras: justification >= rubric 3.
- **M2** — .1 Executors (D: sizing, shutdown). .2 Races (D: TOCTOU, atomicity). .3 Primitives (D: Condition, Barrier patterns). .4 Deadlock, livelock, starvation (D: Coffman, thread dumps). .5 Thread-safe design (D: confinement, immutability). Extras: three seeded bugs fixed, 200 consecutive stress passes.
- **M3** — .1 Why processes. .2 Start methods and fork hazards (D: 3.14 default change, platform table, `--preload`). .3 Data movement (D: `shared_memory`, mmap). .4 `ProcessPoolExecutor` in practice. .5 Alternatives (D: native offload, 3.14t, subinterpreters). Extras: shared-memory gain measured.
- **M4** — .1 Coroutines, Tasks, Futures (D: `Task.__step`). .2 Anatomy of a loop iteration. .3 Scheduling APIs (D: eager tasks, task factories). .4 Bridging worlds. .5 Loop implementations and 3.14 API changes. Extras: toy loop passes hidden tests.
- **M5** — .1 Why structure. .2 TaskGroup/ExceptionGroup. .3 Timeouts and deadlines. .4 Cancellation semantics. .5 Graceful shutdown (D: drain budget vs grace period). Extras: shutdown completes in-flight work within budget.
- **M6** — .1 Blocking the loop. .2 Bounded concurrency (D: per-tenant fairness). .3 Backpressure and load shedding (D: 429/503 + `Retry-After`, admission control). .4 In-process token bucket (30 lines) and limiter-vs-semaphore. .5 Pointer lesson (<= 20 min): where distributed limiting, retries, breakers live (A5.M4/X2). Extras: crawler holds throughput with bounded memory.
- **M7** — .1 ASGI path. .2 sync vs async endpoints (D: limiter, mixed loads). .3 Resource lifecycle. .4 Streaming server side (D: WebSocket, disconnect, slow-client backpressure). .5 LLM-style workload mechanics. .6 Offloading (`BackgroundTasks` vs queue). Extras: load-test report interpreting two configs.
- **M8** — .1 Drivers and pools. .2 SQLAlchemy 2.x async. .3 Connection budgeting (D: Lambda storms, RDS Proxy pointer A7.M5). .4 Transactions under cancellation. .5 Redis/HTTP clients and Django async note. Extras: pool exhaustion before/after metrics.
- **M9** — .1 Choosing: in-process vs queue vs DB-backed vs broker (matrix). .2 Celery worker semantics. .3 Worker-level semantics: at-least-once, redelivery, poison messages (pointers to A5). .4 Alternatives (RQ, Taskiq, Dramatiq, Arq status). .5 Operating workers. Extras: crash-mid-job shows no duplicate side effects; 30-min `SKIP LOCKED` lab.
- **M10** — .1 Testing async. .2 Testing races. .3 Debugging toolbox. .4 Observability hooks. .5 Capstone (S: guided; D: open). Extras: three seeded hangs diagnosed; capstone rubric >= 3.5 (no dimension below 3).

## 4. Seed interview questions (36 style-of-question items; not actual questions from any company)

Seed IDs: `Qm.k` is the k-th seed of module m and maps to bank ID `Q-A2-Mm-0k` (so the decision-guide seed Q1.2 is `Q-A2-M1-02`, the `ref` target used by A1.M3.3). Format: Question — Difficulty / Competency / Cluster / goal tag. Then outline, weak answers, probes. **Senior-bar** line after each module group. Canonical Redis rate limiter coding and idempotency items are owned by A5.M4 (referenced by ID, not copied). Difficulty words map to the data-model integer scale 1-5 at import: easy 1, easy-medium 2, medium 3, medium-hard 4, hard 5 (same mapping as Track C); calibrated after the pilot.

### M1
**Q1.1 (ref: A1.M3-Q1) "Does the GIL make Python code thread-safe? How does it affect a web service?"** — Medium / `concurrency.model-selection` (secondary `python.concurrency-basics`) / IT-services, global-product / `B:M A:M H:M`
- Pointer item: GIL semantics, `x += 1` non-atomicity and the 3.14t status are owned by A1.M3-Q1 and A1.M3-Q2; this item is asked here only as a `ref` and the answer is graded against the A1.M3-Q1 outline plus one service consequence (threads help only blocking IO, so model choice follows Q1.2).
**Q1.2 "A service is slow. How do you decide between threads, processes and asyncio?"** — Medium / `concurrency.model-selection` / Big4, global-IT-consulting / `B:M A:M H:M`
- Ideal: measure first (CPU, IO wait, lock wait); many IO connections with async-capable stack: asyncio; blocking-only libs: threads; CPU: processes or native; mixed: async front + offload; consider horizontal scale and queues; memory, startup, IPC, isolation, debugging cost; validate under load.
- Weak: "asyncio always faster"; no measurement. Probes: one blocking SDK without async? limits of tasks vs threads? when skip all three for a queue?
**Q1.3 "Your CPU-bound work runs in a process pool. How would you decide whether to move it to free-threaded 3.14t, and how would you prove it?"** — Hard / `concurrency.model-selection` / global-product, AI-startup / `B:S A:S H:S`
- Ideal: decision guide, not a status recital (3.14t status and PEP facts are owned by A1.M3-Q2, referenced not repeated): list what must hold (all C extensions declare free-threaded support, otherwise the GIL is re-enabled); single-thread overhead measured; benchmark design: same workload, process pool vs threads on 3.14t, p50/p99 and CPU time, memory per worker, pool size sweep, warm vs cold, repeat runs; new race risks tested with stress tests; keep processes as default until the dependency audit and benchmark both pass; define reversal criteria.
- Weak: "GIL is gone, so threads everywhere"; benchmark on one run; ignores extensions. Probes: dependency audit steps? testing for new races? why per-object locks still do not make `x += 1` safe?
- **Senior-bar:** mid names the three tools; senior justifies with measurement and failure isolation; staff adds operational unit (instances, queues), cost and migration risk. **Staff probes:** "Design a benchmark that proves 3.14t helps your service"; "What would make you reverse the decision in six months?"

### M2
**Q2.1 "Explain a race condition and how you would find and fix one."** — Medium / `concurrency.sync-primitives` / IT-services, Indian-product / `B:M A:S H:M`
- Ideal: outcome depends on interleaving; read-modify-write, check-then-act; reproduce via barriers/stress; fixes: lock, queue confinement, immutability, atomic DB ops (unique constraint, `UPDATE x = x + 1`, `FOR UPDATE`), optimistic version column; multi-process races need DB/Redis, not `threading.Lock`; regression stress test.
- Weak: in-process lock only; "GIL prevents it"; "add sleep". Probes: 4 Gunicorn workers? distributed lock failure modes (expiry, fencing)? how prove the fix?
**Q2.2 "How can deadlock arise and how do you prevent or diagnose it?"** — Medium / `concurrency.sync-primitives` / Big4, global-IT-consulting / `B:M A:S H:M`
- Ideal: four conditions; opposite lock order; lock held across calls; full `queue.put`; executor waiting on its own pool; prevention: ordering, timeouts, small scope, queues; diagnosis: `faulthandler`, `py-spy dump`; DB deadlocks are retried separately.
- Weak: textbook only; "restart". Probes: executor self-deadlock? RLock vs Lock? detect in production without restart?
**Q2.3 "Design a thread-pool worker system with graceful shutdown."** — Medium / `concurrency.sync-primitives` / IT-services, Indian-product / `B:M A:S H:M`
- Ideal: bounded queue; N workers sized by measurement; sentinel or `Event`; `task_done/join`; per-task exception handling; timeouts on gets; shutdown order: stop intake, drain, join with deadline, log leftovers; signals in main thread; alt: `shutdown(wait=True, cancel_futures=True)`.
- Weak: global flag, unbounded queue, daemon threads dropping work. Probes: hung task? sizing? safe retries?
- **Senior-bar:** mid fixes with a lock; senior picks confinement or DB atomicity and proves with stress; staff addresses multi-instance coordination and fencing. **Staff probes:** "Your lock fix passes locally but fails across 8 pods; what now?"; "How would you coach a team off global locks?"

### M3
**Q3.1 "Compare fork, spawn and forkserver. Why do some multiprocessing programs deadlock or break?"** — Hard / `concurrency.model-selection` / global-product, AI-startup / `B:S A:S H:S`
- Ideal: fork = CoW, inherits locks/sockets, unsafe with threads; spawn = fresh interpreter, needs `__main__` guard and picklable args; forkserver = clean server forks workers; defaults: macOS/Windows spawn, **Linux and other POSIX moved from fork to forkserver in 3.14** (confirmed Oct 2026); mitigate: connections after fork, explicit context; CoW + refcount memory growth.
- Weak: "fork is faster so always". Probes: memory growth despite CoW? `--preload`? large read-only data?
**Q3.2 "A CPU-bound transform takes 2 s per request. Options?"** — Medium / `concurrency.model-selection` / Big4, IT-services / `B:S A:S H:S`
- Ideal: profile and vectorise first; never on the loop; process pool via `run_in_executor` for small scale (pickling, saturation); queue + 202 + polling for larger; limits and timeouts; separate autoscaling workers; measure p95 and queue wait.
- Weak: "make it async". Probes: 50 simultaneous requests? cancel work in a process pool? progress?
**Q3.3 "How would you share a large dataset among worker processes?"** — Hard / `concurrency.model-selection` / AI-startup, global-product / `B:N A:S H:S`
- Ideal: avoid per-task pickling; load before fork (read-only), `shared_memory` + NumPy, mmap/Arrow, external store, pass paths; Manager is slow; lifecycle (unlink, resource tracker); writer sync.
- Weak: "pass as argument". Probes: who unlinks on crash? read-mostly vs read-write? why Manager slow?
- **Senior-bar:** mid knows `ProcessPoolExecutor`; senior reasons overhead and start methods; staff weighs native offload vs separate service vs 3.14t on cost. **Staff probes:** "At what task size do processes pay off, and how did you measure it?"; "Move this into a separate worker fleet: what changes operationally?"

### M4
**Q4.1 "Explain how the asyncio event loop works. What happens on `await`?"** — Medium / `concurrency.event-loop` / global-product, Indian-product / `B:M A:M H:M`
- Ideal: loop computes timeout from timer heap, polls selector, converts events to callbacks, runs ready queue; Task `__step` calls `coro.send`; `await` on a pending future suspends and registers a done-callback; cooperative: no preemption; threads return via `call_soon_threadsafe`.
- Weak: "runs coroutines in parallel"; "uses threads". Probes: awaiting a completed future? `sleep(0)`? how `run_in_executor` returns results?
**Q4.2 "Coroutine vs Task vs Future? What does `gather` do?"** — Medium / `concurrency.event-loop` / IT-services, global-IT-consulting / `B:M A:M H:M`
- Ideal: coroutine inert until awaited/scheduled; Task = scheduled coroutine, Future subclass; `gather` returns ordered results, first exception propagates but siblings keep running, `return_exceptions=True`; TaskGroup preferred; hold task references.
- Weak: "all the same"; thinks gather cancels siblings. Probes: sibling fate? "never awaited" warning? why store references?
**Q4.3 "Latency spikes every few minutes though CPU is low. Investigate."** — Hard / `concurrency.event-loop` / global-product, AI-startup / `B:M A:M H:M`
- Ideal: suspect loop blocking (sync call, big JSON, regex, DNS, sync SDK) or GC; debug mode and `slow_callback_duration`; loop-lag metric; `py-spy dump`; correlate with cron/cache refresh; other causes: pool exhaustion, threadpool saturation, cgroup CPU throttling; fix by offload, chunking, bounds, timeouts.
- Weak: "scale up". Probes: measuring loop lag? container throttling signature? GC correlation?
- **Senior-bar:** mid recites the loop; senior traces interleavings and diagnoses with tools; staff builds loop-health SLIs and runbooks. **Staff probes:** "Design a loop-lag SLI and alert"; "When is uvloop not worth it?"

### M5
**Q5.1 "How does cancellation work in asyncio, and what mistakes do people make?"** — Hard / `concurrency.structured` / global-product, AI-startup / `B:M A:M H:M`
- Ideal: `cancel()` raises `CancelledError` at next suspension; BaseException; swallowing breaks timeouts and TaskGroup; `try/finally` then re-raise; cooperative; `shield` caveats; `uncancel/cancelling`; rollback on cancel; thread/process work is not stopped.
- Weak: "kills immediately". Probes: `to_thread` after cancel? how `timeout` uses cancel? cancel a task in a blocking call?
**Q5.2 "Why TaskGroup over `gather`? Walk through error handling."** — Medium / `concurrency.structured` / global-product, Big4 / `B:M A:M H:M`
- Ideal: child lifetimes bounded by block; failure cancels siblings, raises `ExceptionGroup`, `except*`; `gather` leaks work; 3.11+ (AnyIO backport); partial results via child try/except; combine with `timeout` and semaphore.
- Weak: "newer API". Probes: partial results? `except*` vs `except`? bound 10,000 items?
**Q5.3 "Design graceful shutdown for an async worker during a Kubernetes rolling deploy."** — Hard / `concurrency.structured` / Big4, global-product / `B:M A:M H:M`
- Ideal: SIGTERM handler; stop consuming, fail readiness; drain within deadline < grace period; unacked stragglers redeliver (idempotent handlers, pointer A5.M4); flush; close pools last; preStop for LB lag; test with a signal in integration; log abandoned work.
- Weak: "kill -9 and retry". Probes: 30 min job, 30 s grace? ack placement? long-lived WebSockets?
- **Senior-bar:** mid knows the APIs; senior never swallows cancel and budgets shutdown; staff ties it to deploy strategy, SLO and failure cost. **Staff probes:** "Rolling deploy drops 0.5 percent of jobs: how do you prove the cause?"; "Draining vs fast-fail: how do you decide?"

### M6
**Q6.1 "What is backpressure and how do you implement it in an async service?"** — Medium / `concurrency.backpressure` / global-product, global-IT-consulting / `B:M A:M H:M`
- Ideal: signal producers to slow; bounded queue, semaphore, stream `drain()`, admission control 429/503 + `Retry-After`, priority shedding; unbounded buffers hide the problem (Little's Law); policy: block, drop newest, drop oldest, spill durably; metrics: depth, wait, rejections.
- Weak: "more workers"; "big queue". Probes: Little's Law? where shed in gateway vs service? slow SSE client memory growth?
**Q6.2 "Semaphore vs rate limiter: write an in-process token bucket and say where distributed limiting lives."** — Medium / `concurrency.backpressure` / global-product, Indian-product / `B:M A:M H:M`
- Ideal: semaphore bounds concurrency, bucket bounds rate; lazy refill with monotonic time; `asyncio.Lock` or single-task ownership for atomicity; bursts via capacity; return `Retry-After`; per-key buckets need eviction; distributed/Redis atomic limiting, fail-open vs closed -> A5.M4 (`ref` Q-A5-M4 rate-limiter items; not re-authored here).
- Weak: sleep-based throttle; uses a semaphore as rate. Probes: burst semantics? clock choice? why in-process breaks with 4 workers?
**Q6.3 "A crawler launching 100k fetch coroutines with `gather` exhausts memory and sockets. Fix it."** — Medium / `concurrency.backpressure` / IT-services, AI-startup / `B:M A:M H:M`
- Ideal: bound with semaphore or fixed workers on a bounded queue; stream inputs via async generator; shared client with limits and timeouts; per-domain politeness; failures collected not fatal; checkpoint; find the throughput knee (retry policy -> X2).
- Weak: shrink the list; sleep. Probes: semaphore vs worker pool? choose N? resume after crash?
- **Senior-bar:** mid adds a semaphore; senior adds bounds end to end with metrics; staff states shedding policy per client class and cost. **Staff probes:** "Per-tenant fairness with a global cap in-process?"; "How would you prove no unbounded buffer remains?"

### M7
**Q7.1 "FastAPI `def` vs `async def`: when is each wrong?"** — Medium / `api.fastapi-async` / IT-services, global-IT-consulting, Indian-product / `B:M A:M H:M`
- Ideal: `async def` on the loop, must not block; `def` in AnyIO threadpool (default 40 tokens, per-loop cap, threads created lazily); wrong: blocking in `async def`; `def` with huge IO concurrency saturating limiter; CPU needs processes/queue; dependencies follow same rule; tune limiter or move to async libs; per-worker; measure.
- Weak: "async always faster"; "`def` blocks the server". Probes: sync SDK from `async def`? raising limiter downside? multiple workers?
**Q7.2 "Manage DB pools and HTTP clients in FastAPI; what are `BackgroundTasks` good for?"** — Medium / `api.fastapi-async` / global-product, AI-startup / `B:M A:M H:M`
- Ideal: build in lifespan, hold on `app.state`; yield-dependency sessions with rollback; no per-request clients; pools per worker; `BackgroundTasks` in-process, no durability or retry, lost on SIGTERM; durable queue for critical work; no request-scoped session after close.
- Weak: globals at import; payments in `BackgroundTasks`. Probes: SIGTERM fate? cleanup on error? 8 workers on 3 pods sizing?
**Q7.3 "Design SSE/WebSocket endpoints for LLM-style token streaming, handling slow clients and disconnects."** — Hard / `api.fastapi-async` / AI-startup, global-product / `B:S A:M H:M`
- Ideal: SSE one-way, WebSocket bidirectional (protocol choice -> A5.M9); async generator over upstream with timeouts; detect disconnect (`CancelledError`, `WebSocketDisconnect`, `is_disconnected`) and cancel upstream; bounded per-connection buffer; heartbeat; disable proxy buffering; per-worker connection caps; semaphore for provider limits; `to_thread` for sync SDKs; drain on deploy (provider 429/token budgets -> B2.M8/M9).
- Weak: whole response first; ignores disconnects. Probes: upstream on tab close? resume after reconnect? connections per worker limit?
- **Senior-bar:** mid picks sync vs async; senior owns lifecycle and disconnects; staff sizes workers, reasons cost per abandoned stream and rollout. **Staff probes:** "Your p99 doubled after moving to async: five hypotheses ranked by test cost"; "Cost of an abandoned stream?"

### M8
**Q8.1 "SQLAlchemy async: what changes and what errors do you hit?"** — Medium / `db.async-drivers` / global-product, global-IT-consulting / `B:M A:S H:M`
- Ideal: async driver + greenlet bridge; all IO awaited; `MissingGreenlet` from lazy loads -> `selectinload`/`awaitable_attrs`; `expire_on_commit=False` common; one session per task/request; `run_sync`; sync driver blocks the loop.
- Weak: "just add await"; shared session in `gather`. Probes: why no shared session? `expire_on_commit`? N concurrent queries?
**Q8.2 "Size pools for 4 pods x 4 workers vs `max_connections=200`; handle growth."** — Medium / `db.async-drivers` / Big4, global-product / `B:M A:S H:M`
- Ideal: pods x workers x (pool + overflow) plus headroom; Little's Law for real need; more connections do not mean throughput; PgBouncer transaction mode and prepared-statement caveats (DB-level -> A4.M7); pool timeout, statement timeout, pre-ping; no connection across external calls; autoscaling multiplies; Lambda needs a proxy.
- Weak: "pool 100". Probes: transaction-mode breakage? user-visible exhaustion? find leaks?
**Q8.3 "A request holds a transaction open while calling an external API. What goes wrong?"** — Medium / `db.async-drivers` / IT-services, Big4 / `B:M A:S H:M`
- Ideal: held connection -> exhaustion, locks, vacuum bloat, cascade; restructure: read, release, call with timeout, short write transaction; consistency via idempotency/outbox (pointers A5.M4, A5.M8); `idle_in_transaction_session_timeout`; rollback on cancel.
- Weak: "increase pool". Probes: DB and external consistency? external ok but commit failed?
- **Senior-bar:** mid fixes the exception; senior budgets connections arithmetically; staff plans migration, headroom policy and incident detection. **Staff probes:** "Autoscaler doubled pods and the DB fell over: prevention?"; "Who owns the connection budget across teams?"

### M9
**Q9.1 "Celery reliability: retries, `acks_late`, visibility timeout, idempotency."** — Hard / `concurrency.background-jobs` / IT-services, Indian-product, global-product / `B:M A:M H:M`
- Ideal: default early ack (at-most-once on crash); `acks_late` -> at-least-once, duplicates possible; visibility timeout > longest task (Redis/SQS); `reject_on_worker_lost`; prefetch 1 for long tasks; `autoretry_for` with backoff (pointer X2); soft/hard limits; idempotency pointer A5.M4; queues per workload class; IDs not objects; monitor depth and age.
- Weak: "exactly once"; retries without idempotency. Probes: 2 h task, 1 h timeout? why chords risky? dedupe a payment task (pointer)?
**Q9.2 "Celery, RQ, async-native queue, or Postgres queue: how do you choose?"** — Medium / `concurrency.background-jobs` / global-IT-consulting, AI-startup / `B:M A:M H:M`
- Ideal: Celery mature, heavy, sync-first; RQ simple Redis fork-per-job; Taskiq and Dramatiq active, Arq maintenance-only (as of Oct 2026; do not start new projects on it without a reason); Postgres `SKIP LOCKED` (A4.M6) gives transactional enqueue and avoids dual-write at moderate throughput; managed queues/streams for durability or replay (pointers A7.M5, A6.M4); workflow engines for long flows; criteria: durability, latency, throughput, scheduling, ops, team skill.
- Weak: name-dropping. Probes: transactional enqueue advantage? Kafka wrong for jobs when? incremental migration?
**Q9.3 "Design an async report export: click, 10-minute job, file delivered."** — Medium / `concurrency.background-jobs` / Big4, IT-services, Indian-product / `B:M A:M H:M`
- Ideal: POST creates job record, enqueues, returns 202 + ID (job-resource contract -> A5.M3); worker streams queries, writes to object store, updates progress; status by polling/SSE/email with signed URL; deterministic output key makes retries safe; limits, cancellation per tenant; read replica; expiry; authz; observability; autoscale on depth.
- Weak: synchronous request; file in DB. Probes: worker dies at minute 9? double-click duplicates? memory for 50M rows?
- **Senior-bar:** mid configures Celery; senior states delivery semantics and failure timelines; staff chooses system by ops cost and builds poison/DLQ runbooks. **Staff probes:** "Queue depth grows 3x after a deploy: first five checks"; "Retire Celery incrementally: plan?"

### M10
**Q10.1 "How do you test async code and avoid flaky tests?"** — Medium / `concurrency.testing-debugging` / global-product, Indian-product / `B:M A:S H:M`
- Ideal: `pytest-asyncio` or AnyIO plugin with explicit loop scope and async fixtures; events/fake clocks not sleeps; `AsyncMock`; test timeout and cancel (cleanup runs); order via events/barriers; stress repeats; real Redis/Postgres over over-mocking; per-test timeouts; no leaked tasks; `httpx.AsyncClient` with ASGI transport.
- Weak: `sleep(1)` everywhere; mocking the concurrency away. Probes: test backoff fast? detect leaked tasks? test a race?
**Q10.2 "A production async service hangs with no errors in logs. Diagnose."** — Hard / `concurrency.testing-debugging` / global-product, Big4 / `B:M A:S H:M`
- Ideal: scope (pod, endpoint); dashboards: loop lag, in-flight, pool use, queue depth, downstream latency; capture without restart: `py-spy dump`, `python -m asyncio ps/pstree <pid>` (3.14), `faulthandler`; culprits: blocking call, exhausted pool, unresolved future with no timeout, async-lock deadlock, full queue with dead consumer, file descriptors; mitigate then root-cause: timeouts everywhere, loop-health liveness, runbook.
- Weak: "restart and hope". Probes: loop-health liveness safely? who holds the connections? default timeouts?
**Q10.3 "Design: an API calling three downstream services and an LLM, p95 < 2 s, 500 rps."** — Hard / `concurrency.model-selection` / global-product, AI-startup, Big4 / `B:M A:M H:M`
- Ideal: SLOs; pooled async clients; TaskGroup fan-out with per-call timeouts plus deadline; bulkheads and breakers (pointers X2/A5.M4); degradation; single-flight; admission control; streamed LLM calls with provider-limit semaphore (specifics -> B2.M8/M9); Little's Law (500 rps x 1 s ~ 500 in flight); worker and pool sizing; load-test plan; graceful shutdown. Rule IDs on the canvas come only from the A6 registry (boss matrix section 7).
- Weak: "asyncio scales". Probes: one downstream at 10 s? retry storms? pool sizes from SLO?
- **Senior-bar:** mid lists patterns; senior does capacity math and failure thinking; staff adds ownership, rollout, cost per request and SLO negotiation. **Staff probes:** "Defend this design to a CFO and to an on-call engineer"; "What do you instrument before launch?"

### Incident-style scenario items (6, scenario bucket; `B:M A:M H:M`)
S1 "Service hangs, no errors" (all tasks awaiting pool); S2 "Queue depth growing after deploy" (visibility timeout vs long task, prefetch hoarding); S3 "p99 spike every 5 minutes, CPU 20 percent" (blocking call + cgroup throttling); S4 "Connection exhaustion after autoscaling" (pods x workers x pool); S5 "Duplicate emails after a worker restart" (ack timing, pointer A5.M4); S6 "Streaming endpoint leaks cost after tab close" (disconnect not cancelling upstream). Each: symptom evidence pack, expected triage order, fix, prevention.

## 5. Final assessment
- **Course final mastery check:** 30 items, 40 min, MCQ + predict-the-output (interleavings, cancellation, TaskGroup) + race-spotting; **pass 80 percent (G-7)**, revision loop on missed modules.
- **Timed coding task (60 min, hidden tests plus stress runs):** async pipeline: bounded concurrent fetch with in-process limiter, TaskGroup and timeouts, cancellation-safe cleanup, graceful shutdown (retry code may import the X2 helper), plus one seeded threaded race. >= 70 percent hidden tests and stated complexity/bounds.
- **Voice mock (20 min):** GIL/model selection (secondary), event loop, cancellation, one incident scenario; badge at "Meets"; readiness at Senior bar.
- **Capstone design / canvas (30 min):** Q10.3-style with capacity numbers, failure modes, rule IDs from the A6 registry only; rubric overall >= 3.5 with no dimension below 3 and no critical rule findings (G-7; same bar for badge and readiness). **Challenge variant (optional, not required):** voice/LLM streaming service (provider-limit semaphore, barge-in cancellation, per-session backpressure); real-time audio specifics -> B6.M3.

## 6. Course-level risks and freshness notes (verified 2026-10-05)
- **Free-threading:** 3.14 supported but optional (PEP 779); 3.15 (final due 2026-10-09; rc3 current) adds a stable ABI for free-threaded builds, and the default `python3.15` still has the GIL, free-threaded is the separate `3.15t` build. `abi3t` (PEP 803) and the separate `cp315t` free-threaded build are confirmed in the 3.15.0rc3 What's New (verified 2026-10); [verify: re-read the 3.15.0 final What's New after the 2026-10-09 release] (1 item).
- **Confirmed via official docs (Oct 2026):** Python 3.14 `forkserver` default on POSIX except macOS; asyncio call-graph introspection and command-line tools (`python -m asyncio ps/pstree`, added 3.14); 3.14 removed child-watcher APIs and `get_event_loop` no longer auto-creates a loop; AnyIO default worker-thread limiter 40 (re-verified against AnyIO 4.15.1 docs, 2026-10).
- **Confirmed (secondary sources, project READMEs and 2026 comparisons):** Arq maintenance-only (confirmed on the python-arq/arq README, verified 2026-10); Taskiq and Dramatiq active (secondary sources, not re-verified from primary pages). Re-check each quarter.
- **pytest-asyncio:** 1.x line (1.4.0 stable released 2026-05-26 and shown as latest on the GitHub releases page, verified 2026-10); set `asyncio_default_fixture_loop_scope` explicitly (unset emits a warning; the 1.4.0 release notes only say the warning text was improved, so do not teach a future default change); loop-scope semantics changed in 1.1 (tasks cancelled when the `loop_scope` ends); 1.4.0 deprecates overriding the `event_loop_policy` fixture in favour of the `pytest_asyncio_loop_factories` hook and requires pytest 8.4.0 or newer. The exact pin lives in the platform manifest [verify: engineering to record the pinned version at image build] (1 item).
- **Pinned versions** for Celery 5.x, FastAPI/Starlette, SQLAlchemy 2.x live in the platform manifest (G-8); Celery docs list 5.6.3 (2026-03-26) as the latest 5.6 release seen at review, supported on CPython 3.9-3.13 officially with 3.14 compatibility fixes (verified 2026-10); [verify: FastAPI/Starlette and SQLAlchemy pins are an engineering decision, not checkable as course facts; record at image build] (1 item). Free-threaded 3.14t availability in the sandbox image is an engineering item (P-01). **Compatibility risk to close before M9 labs ship:** the Celery release seen at review lists official support for CPython 3.9-3.13 while the sandbox default is 3.14 (G-8); `[verify: engineering to run the M9 Celery eager/memory-broker/Redis labs on the 3.14 image, or pin the M9 labs to the selectable 3.13 image and say so in the lab header]`.
- Fundamentals (race, deadlock, event loop, blocking pitfalls, Celery reliability) stay >= 70 percent of content to limit staleness. All code runs in CI on pinned versions; concurrency tests run repeatedly.

## 7. Open items for boss
1. Confirm M7.5 (LLM-style workload mechanics) fits the 1.3 h Deeper budget without trimming M7 streaming.
2. Resolved in the INT-A pass: the A6 rule registry is published (A6 section 1.3), so the A2 capstone canvas task cites registry IDs only, as permitted by boss matrix section 7 cross-use.
3. Resolved in the INT-A pass: the 30-minute `SKIP LOCKED` lab is a self-contained preview with a provided worker, and A4.M6 is recommended before M9 (A4 comes later in the default route; no hard dependency).

## 8. Learner-Fit Appendix (section 6 of the template; G-11)

### LF-1 Level paths

| Level | Route | Hours | Practice mix | Skipped | Challenge |
|---|---|---|---|---|---|
| L1 | Simpler, all lessons, link A1.M4 and A1.M3 first | ~15.5 | 70% guided labs, 30% predict-output | none | none |
| L2 | Simpler, skim known lessons | ~12-14 | 50% guided, 50% independent | .1 lessons of M2, M4 if band >= 70 | M6 crawler stress |
| L3 | Deeper: skim foundations, study "explain at depth" lessons | ~10.5 | 70% independent, incident scenarios | M1-M2 lessons if band >= 85 | 3.14t benchmark; seeded hang bonus |
| L4 | Deeper plus Challenge; mastery check first | ~7-9 (net of skips) | 80% incident drills and design | M1, M2, M3 and M7 lessons by measured score; never the final assessment | voice/LLM streaming capstone; staff probes |

### LF-2 Module matrix

Placement items: 6 per module drawn from MCQ/predict buckets (`placement` tag, IDs `Q-A2-Mx-P1..P6`; the six topics listed below are the minimum, M4 and M6 carry a seventh item). Band actions G-5: >= 85 skip lessons + mastery check; 70-84 skim (Deeper-only lessons + check); 50-69 study; < 50 deep-study (Simpler + remediation). Weights are percent of interview-weighted study time per target, rationale: concurrency questions in senior Python rounds concentrate on event loop, cancellation, FastAPI blocking and Celery reliability `[judgement]`.

| Module | Diagnostics (examples) | B/A/H | Weight B / A / H | Rehearsal modes | Module readiness (G-6 specialised) |
|---|---|---|---|---|---|
| M1 | GIL-compound-op trap; workload classification; Amdahl ceiling; flowchart choice; 3.14t status; fork vs spawn default | B:M A:S H:M | 8 / 6 / 7 | voice drill | classify 6 scenarios correctly in 10 min |
| M2 | race spotting; deadlock order; executor self-deadlock; Condition use; pool sizing; daemon loss | B:M A:S H:M | 8 / 5 / 6 | voice, coding | stress-clean fix (200 runs) + voice 3.5 |
| M3 | fork+threads; pickling error; chunking; shared memory; BrokenProcessPool; 3.14 default | B:S A:S H:S | 6 / 4 / 5 | voice | explain start methods incl. 3.14 default |
| M4 | trace interleaving; gather sibling fate; `sleep(0)`; never-awaited; eager task; `call_soon_threadsafe` | B:M A:M H:M | 14 / 14 / 14 | voice, coding | hand-trace 3 programs correctly, timed |
| M5 | swallowed cancel; TaskGroup ExceptionGroup; shield; timeout nesting; SIGTERM drain; `to_thread` cancel | B:M A:M H:M | 14 / 14 / 14 | voice, coding, design | shutdown task within budget + voice 3.5 |
| M6 | semaphore vs rate; Little's Law; bounded queue policy; blocking detection; token bucket refill; head-of-line | B:M A:M H:M | 10 / 12 / 11 | voice, coding | crawler bounded memory; policy argued |
| M7 | def vs async def; limiter 40; lifespan; `BackgroundTasks` loss; disconnect cleanup; middleware cost | B:M A:M H:M | 14 / 18 / 16 | voice, coding, design | load-test interpretation correct; streaming design 3.5 |
| M8 | pool arithmetic; `MissingGreenlet`; session sharing; PgBouncer mode; held connection; cancel rollback | B:M A:S H:M | 10 / 8 / 9 | voice, scenario | pool arithmetic exact on 2 seeded cases |
| M9 | visibility timeout; `acks_late`; chord risk; DB queue vs broker; poison message; prefetch | B:M A:M H:M | 10 / 8 / 9 | voice, design | crash-mid-job lab clean + matrix defended |
| M10 | flaky-test fix; leaked tasks; hang triage order; `asyncio ps`; loop-lag metric; fake clock | B:M A:S H:M | 6 / 11 / 9 | voice, design canvas | 3 seeded hangs diagnosed; capstone >= 3.5 |

### LF-3 Experience bridge ("you already know" -> gap to senior depth)

| Module | You already know | Gap between using and explaining |
|---|---|---|
| M1 | Gunicorn/Uvicorn workers, Celery prefork | why each choice by measurement; failure isolation; 3.14t trade-off |
| M2 | `ThreadPoolExecutor`, Django threads | races in your own code; executor self-deadlock; multi-process races need DB |
| M3 | Gunicorn `--preload`, Celery pools | CoW and refcount growth; start-method defaults; pickling cost |
| M4 | `async def` in FastAPI | loop mechanics; interleaving traces; why sync call stalls all |
| M5 | `asyncio.gather`, SIGTERM in Docker/ECS | cancel semantics; TaskGroup; shutdown budget vs grace period |
| M6 | rate-limit middleware, Redis | in-process vs distributed boundary; shedding policy; Little's Law |
| M7 | FastAPI endpoints, LLM streaming, voice agent sockets | threadpool limiter, lifespan ownership, disconnect and cost of abandoned streams |
| M8 | SQLAlchemy/psycopg, RDS, Lambda | pods x workers x pool vs `max_connections`; session scope; PgBouncer caveats |
| M9 | Celery, Redis broker, SQS with Lambda | visibility timeout vs long task; ack timing; decision matrix |
| M10 | pytest, logs, CloudWatch | deterministic async tests; no-restart hang diagnosis; loop-lag metric |

### LF-4 Senior-bar framing
Mid answer: names the API or pattern. Senior answer: states mechanism, failure mode, trade-off, and a measurement. Staff answer: adds scale and capacity arithmetic, operations and on-call runbooks, cost, migration path and how they would mentor the team. Per-module signals are the **Senior-bar** lines in section 4; unifying staff signals: Little's Law arithmetic (M6/M8), deadline budgets (M5), bulkheads per dependency (Q10.3 design, implementation owned by A5.M4), queue-age SLOs (M9), loop-lag SLI (M4/M10).

### LF-5 Adaptive hooks
- **Mastery thresholds:** module check 80; readiness retake >= 85 after >= 7 days (G-6).
- **Remediation links:** M2 -> A1.M3; M3 -> M2; M4 -> A1.M4 (generators); M5 -> M4; M6 -> M4, M5; M7 -> M4, M6; M8 -> M7, A4.M5 (isolation) if shaky; M9 -> M7, A4.M6; M10 -> M4, M5.
- **Challenge extensions:** 3.14t benchmark (M1), toy loop with timers (M4), voice/LLM streaming capstone (M7/M10), staff probes.
- **Reorder/defer:** engine may defer M3 (B:S A:S) and M2 for L4; may pull M7 and M9 earlier for AI route; M5 never before M4; capstone last.

### LF-6 Applied labs (production-framed)
M1 pick a model for a PDF-parse + LLM-call service; M2 fix a race in an MQTT message counter (IoT); M3 parallelise report rendering for a SaaS export; M4 convert a threaded webhook fetcher to asyncio; M5 SIGTERM-safe consumer for a notification worker; M6 bounded ingestion pipeline with shedding for sensor telemetry; M7 voice-assistant token stream with disconnect cancel; M8 fix pool exhaustion in a multi-tenant API; M9 crash-mid-job invoice emailer, plus 30-min `SKIP LOCKED` lab; M10 diagnose three seeded hangs in an enterprise service.

### LF-7 Currency and quality bar (dated 2026-10-05)

| Module | Visual / analogy | Top 3 misconceptions handled | Stamp |
|---|---|---|---|
| M1 | decision flowchart; chefs and stove | GIL = thread-safe; threads useless; asyncio always faster | 2026-10 |
| M2 | whiteboard interleaving | dict atomic; global lock fine; GIL prevents races | 2026-10 |
| M3 | photocopied binder | fork always safe; globals shared; processes for tiny tasks | 2026-10 (3.14 default) |
| M4 | single waiter | `async def` concurrent; asyncio threaded; gather cancels siblings | 2026-10 |
| M5 | parent and children | cancel kills instantly; shield safe; swallow cancel OK | 2026-10 |
| M6 | ramp meter | semaphore = rate limit; big queue fine; more workers fix | 2026-10 |
| M7 | 40 stoves | async faster; `def` blocks server; BackgroundTasks durable | 2026-10 |
| M8 | car park | shared session OK; pool 100; PgBouncer transparent | 2026-10 |
| M9 | double-delivered parcel | exactly once; ack early OK; Celery for everything | 2026-10 |
| M10 | triage tree | sleep fixes flakiness; mock everything; restart fixes | 2026-10 |

## 9. Boss compliance log
- G-1: ownership notes row added; pointers for O-17 to O-22, O-23 applied. G-2: hours 15.5/10.5 + 2.5 assessment, semantics sentence. G-3/G-11: LF-1..LF-7. G-4, G-5: level and band tables. G-6: readiness rows. G-7: final mastery 80, voice "Meets" badge, canvas 3.5. G-8: 3.14 default, 3.14t selectable, 3.13/3.15 selectable. G-9: tags hyphenated, `python.concurrency-basics` secondary. G-10: goal tag + senior-bar + 2 staff probes per module; Q-ID scheme referenced. G-12: dated facts.
- R-A2-1: M6 narrowed, Redis/sliding-window lab removed, X2 pointer; M9 worker semantics only. R-A2-2: decision matrix + 30-min lab. R-A2-3: sandbox services per P-13. R-A2-4: no Channels, Django note in M8. R-A2-5: LLM mechanics in M7.5, provider specifics pointers, generic capstone plus optional Challenge.
- Upgrade items 1-9 of section 8 applied. `[verify]` items closed: 3.14 forkserver, asyncio CLI introspection, AnyIO limiter 40, pytest-asyncio version and loop scope, Arq status, 3.15 `abi3t`. Remaining: 4 (3.15 final What's New re-read; pytest-asyncio pin; FastAPI/Starlette/SQLAlchemy pins; Celery on the 3.14 image before M9 labs ship; the last three are engineering image-build items).

- Track A lead review (2026-10-05, TRACK-LEAD-A): A4.M6 preview dependency and 3.14-vs-Celery compatibility check recorded; open verify count restated (4). Verdict: Approve with conditions. See _reviews/TrackLead_A_Report.md.
