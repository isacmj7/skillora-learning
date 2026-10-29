---
name: A8 — Testing, Quality and Security
course_id: A8
track: A (Backend)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-06, HAR-09 (see _reviews/Boss_Final_Approval.md)
---

# A8 — Testing, Quality and Security (Track A)

> Version 2.0 (wave-2 upgrade: boss rulings R-A8-1 to R-A8-5, Learner-Fit Layer, currency checks). Last reviewed: October 2026. Review cadence: testing core 12 months; security 6 months; LLM-security pointer 3 months.

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | A8 — Testing, Quality and Security / Track A (Senior Python Backend Mastery) |
| Audience | Senior Python backend engineers (L3–L4) who ship Django/FastAPI services and are expected to own test strategy, review quality and secure-by-default design. Not a beginner pytest course: assumes the learner has written tests and used a framework. |
| Prerequisites | Working Python (A1 recommended, dotted edge A1 to A8: typing and decorators help), HTTP/REST basics, basic SQL, Git. Recommended (dotted edges): A2 (async), A5 (auth/API design); helpful: A4 (transactions). **Placement skip (see section 8 for bands):** a learner who scores 85%+ (G-5) on the placement "testing and security" block may skip Lessons marked Simpler in M1, M2 and M6 and go straight to Deeper variants and mastery checks; M7–M9 (security) are skippable only with 85%+ on a security-specific diagnostic, since most resumes under-cover them. |
| Outcomes (each tied to an interview competency) | 1. Design a risk-based test strategy (pyramid or trophy, with justification) for a service and defend what is *not* tested — `eng.test-strategy`. 2. Write idiomatic, fast, deterministic pytest suites (fixtures, scopes, parametrisation, markers, plugins) and diagnose flaky tests — `eng.pytest`. 3. Choose correctly between mocks, stubs, fakes and real dependencies, apply contract testing between services, and test async code, FastAPI/Django endpoints and databases with isolation and realistic infrastructure — `eng.test-doubles`, `eng.contract-testing`, `eng.integration-testing`. 4. Use coverage, mutation testing and property-based testing with an honest view of what each proves — `eng.test-quality`. 5. Review code and refactor safely using characterisation tests, static analysis and type checking — `eng.code-review`, `eng.code-quality`. 6. Identify and fix the OWASP API Top 10 class of flaws (BOLA, broken authN/authZ, injection, SSRF, mass assignment) in Python code and prove each fix with a regression test — `sec.api`, `sec.authn-authz`, `sec.injection`, `sec.ssrf`. 7. Manage secrets and dependency/supply-chain risk, run a lightweight threat model, and state engineering obligations from DPDP/GDPR at awareness level (X1) — `sec.secrets`, `sec.supply-chain`, `sec.threat-modelling`, `sec.compliance`; LLM-app threats at pointer level only (`llm.security`, secondary, owner B5). 8. Profile and load-test a service, interpret latency percentiles and find the bottleneck — `perf.load-testing`. |
| Estimated effort | ~27 h Simpler / ~17.5 h Deeper (10 modules), plus 3.0 h assessment allowance (final assessment, voice mock, revision loops). Includes the X1 Compliance primer (+1.5 h S / +1 h D) and LLM-app security trimmed to a pointer (-0.5 h / -0.5 h) per the effort table. Per-module hours are in section 8. |
| Interview coverage tags | `eng.test-strategy`, `eng.pytest`, `eng.test-doubles`, `eng.contract-testing`, `eng.integration-testing`, `eng.test-quality`, `eng.code-quality`, `eng.code-review`, `sec.api`, `sec.authn-authz`, `sec.injection`, `sec.ssrf`, `sec.secrets`, `sec.supply-chain`, `sec.threat-modelling`, `sec.compliance`, `perf.load-testing`. Secondary (no credit): `llm.security` (owner B5) |
| IDE | Python 3.14 platform image with the platform pin manifest (pytest 9.x, pytest-asyncio 1.x, pytest-cov, pytest-xdist, Hypothesis, mutmut, respx, time-machine, Locust, FastAPI, Django, SQLAlchemy 2), a seeded FastAPI app and a seeded Django project, the platform **Lab Service** (P-10: intentionally vulnerable and slow API plus isolated SSRF target, network-isolated, ephemeral, gated on S-6), a disposable PostgreSQL 18 per attempt (P-04, replaces testcontainers in the sandbox), repeat-run runner xN for flakiness (P-02), fake-clock utilities (P-14), coverage and mutation runners (P-17), fault-injection harness (P-09), multi-file workspace (P-16). Limits: 10 s per run, 60 s perf tasks, 120 s for A8 practicals, CPU-only. Version pins and facts dated October 2026. |
| Badge and certificate | Course badge "Quality and Security Engineer (Senior)": all 10 module badges, final assessment >= 80%, voice mock at "Meets" (average >= 3.0, none < 2; G-7), plus evidence: (a) a green pytest suite written by the learner against the lab app with >= 85% branch coverage and a mutation score target, (b) a written test strategy for a sample service, (c) a fixed vulnerable API (all seeded flaws remediated, verified by hidden exploit tests), (d) a one-page threat model. Certificate lists module scores; no hiring claims. |
| Ownership (binding rulings) | A8 **owns**: contract testing and property-based API testing (M3), OWASP API Top 10 depth, injection and SSRF with exploit-and-fix labs (M7), Python dependency supply chain and secrets practice (M8), SOLID, clean code and code review (M6), threat modelling and the X1 Compliance primer (M9.4), load-testing method (M10). A8 **points to**: A5.M6 (auth protocols, token design, object-level authorisation design), A5.M2 (contract-first OpenAPI, breaking-change classification), A5.M7 (design-time controls), A7.M8 (pipeline design, SAST and scanning gates), A7.M2 (container supply chain), A1.M11 (Python profiling), A2.M10 (async stall diagnosis), A7.M9 (production telemetry), B5.M4-M6 (prompt injection, PII, guardrails, red-team), B4.M9 (agent tool permissions), B5.M9 (AI governance). |
| Authoring owner | SME agent A8 |
| Reviewer | to be assigned |
| Review cadence | Testing core (M1–M6): every 12 months. Security (M7–M9): every 6 months (OWASP editions, CVE-driven advice, supply-chain tooling). LLM-app security pointers (part of M9): every 3 months. Performance (M10): every 12 months. |

Cross-course overlap notes (binding): async testing depth defers to A2 for event-loop mechanics; auth protocol design (OAuth2/OIDC flows) and object-level authorisation design live in A5.M6, this course focuses on *flaws* and testing them; CI pipeline design and security gates live in A7.M8 (A8.M8 teaches tool concepts and local usage only); LLM prompt-injection, PII, guardrail and red-team depth lives in B5 (A8.M9 keeps a pointer lesson of at most 45 min); Python profiling tutorials live in A1.M11. Lab release is gated on spike S-6 (Lab Service legal and security review); fallback is code-review and static-analysis tasks on vulnerable snippets.

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice (IDE or design) | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Test strategy and the pyramid | Produce a risk-based test strategy for a service and justify the mix of unit, integration, contract and end-to-end tests. | Pyramid vs trophy vs honeycomb; risk-based testing; what a "unit" is (sociable vs solitary); test doubles taxonomy (dummy, stub, spy, mock, fake); TDD vs test-after; characterisation tests; test smells (over-mocking, logic in tests, shared state); flaky-test causes; cost of test suites in CI; testing in production awareness (canaries, feature flags, synthetic checks). | Pyramid vs trophy comparison with cost/speed/confidence axes; flaky-test root-cause tree. | Design exercise: strategy for an order-service with payment gateway and queue. Critique 3 bad test files and list smells. | "100% coverage means well tested"; "unit tests mock everything"; "E2E tests for everything"; no answer for flakiness beyond "rerun". |
| M2 | Pytest in depth | Write fast, readable, deterministic suites using fixtures, parametrisation, markers and plugins. | Fixture scopes (function/class/module/package/session), teardown with yield, fixture factories, `conftest.py` layering, autouse pitfalls, fixture finalisation order, `request` object, `params` on fixtures; `@pytest.mark.parametrize` (stacking, ids, indirect), markers and `-m` selection, `xfail`/`skip`, `tmp_path`, `monkeypatch`, `capsys/caplog`, `pytest.raises`/`approx`; plugins (xdist, cov, randomly, timeout, mock, django, asyncio, Hypothesis); rewriting asserts; config in `pyproject.toml`; running in parallel and test-order independence. | Fixture dependency graph and scope lifetime timeline; parametrisation expansion table. | 8 tasks: refactor copy-paste tests into parametrised ones; build a factory fixture; fix an order-dependent suite; speed up a suite with scope changes and xdist. | Session-scoped mutable fixtures leaking state; autouse everywhere; parametrising with side effects; testing implementation not behaviour; not knowing fixture teardown order. |
| M3 | Mocking, fakes and contract testing | Isolate units correctly with the right double, and verify service boundaries with contract tests. | `unittest.mock`: `Mock`, `MagicMock`, `patch` target rule ("patch where it is looked up"), `autospec`/`create_autospec`, `side_effect`, `assert_called_*`, `AsyncMock`, `patch.object`, `mock_open`; fakes vs mocks (in-memory repository, fake clock); HTTP mocking (respx for httpx, responses for requests, VCR-style recording and its risks); time control (freezegun / time-machine); dependency injection as a testability tool; consumer-driven contracts (Pact concept), provider verification, OpenAPI-based contract and schema-based fuzzing (Schemathesis) and property-based API testing (Hypothesis strategies generated from the spec; **A8.M3 is the owner**, A5.M2 points here for consumer-driven contracts and schemathesis), backward-compatibility testing, event/message schema contracts. | Patch-target diagram (module namespace lookup); contract test flow between consumer, broker and provider. | Fix 6 broken patches; replace over-mocked tests with fakes; write a consumer contract and verify a provider; run schema-based tests against an OpenAPI spec. | Patching the definition site instead of the usage site; mocks that always pass (no autospec); asserting call counts as the only check; recorded cassettes that rot; "contract test = integration test". |
| M4 | Testing async code, APIs and databases | Build integration tests that are isolated, realistic and fast for async services, HTTP APIs and databases. | pytest-asyncio 1.x modes (strict vs auto), `loop_scope` and `asyncio_default_fixture_loop_scope` (the `event_loop` fixture was removed in 1.0.0, released 26 May 2025; scoped loops are created once rather than per scope; verified October 2026), anyio plugin, async fixtures, testing timeouts/cancellation and task groups; FastAPI `TestClient` vs `httpx.AsyncClient` + `ASGITransport`, `dependency_overrides`, lifespan events in tests; Django `TestCase` vs `TransactionTestCase`, pytest-django (`db`, `transactional_db`), `django_assert_num_queries`; DB isolation patterns (transaction rollback per test, truncate, template DB, savepoints with SQLAlchemy sessions); real Postgres via testcontainers vs SQLite substitute and why SQLite lies; factories (factory_boy / model_bakery), migration tests (apply from zero, forward/backward), testing Celery tasks (eager vs real worker), testing Redis (fakeredis limits); testing webhooks and idempotency. | Per-test transaction-rollback timeline; test DB topology (template DB, parallel workers). | 6 tasks: async service with deliberate race; override dependency in FastAPI; assert query count; migration test; write idempotency test for webhook handler; make suite parallel-safe. | SQLite for tests of Postgres code; sharing DB state between tests; `asyncio.run` inside tests; sleeps instead of awaiting conditions; not testing the failure path (timeouts, retries). |
| M5 | Coverage, mutation and property-based testing | Judge test-suite quality with evidence: coverage, mutation score and generated inputs. | Line vs branch coverage, `coverage.py` config (omit, `fail_under`, parallel/combine), diff coverage in CI, coverage-gaming; mutation testing (mutmut or Cosmic Ray; equivalent mutants; cost control by targeting changed files); property-based testing with Hypothesis (strategies, shrinking, `@given`, stateful testing basics, settings/profiles, reproducing failures with seed/`@reproduce_failure`); typical properties (round-trip, idempotence, invariants, oracle comparison); snapshot/golden tests and their risks; flaky quarantine process; test-suite KPIs. | Mutation testing loop; Hypothesis shrinking walk-through. | Find surviving mutants and add tests that kill them; write 4 properties (serialiser round-trip, sort invariants, money-split sum, rate-limiter invariant); find a seeded bug with Hypothesis. | "High coverage = high quality"; no distinction between executed and asserted; Hypothesis as random fuzzing without invariants; ignoring non-determinism; mutation testing in every PR (cost). |
| M6 | Code quality, review and refactoring | Review a pull request like a senior, refactor safely under tests, and wire static analysis and type checking into CI. | Code review practice (what to look for in order: correctness, design, security, tests, readability; PR size, review turnaround, nit vs blocker, tone, checklists, author's responsibilities); clean code in Python (naming, function size, cohesion, SOLID without dogma, composition over inheritance, dependency inversion, exceptions design, no mutable default args, `dataclass`/`pydantic` for data); code smells and refactoring catalogue (extract function/class, replace conditional with polymorphism, parameter object, introduce seam), refactoring under characterisation tests, strangler pattern, branch by abstraction, feature flags for risky changes, technical debt triage; static analysis: ruff (lint+format), mypy / pyright (strict flags, gradual typing, `Protocol`, `TypedDict`, generics, `py.typed`) and newer Rust-based checkers (October 2026: Pyrefly reached stable 1.0 in May 2026; Astral's ty is in beta with a stable release targeted for 2026; teach as fast-feedback and editor tooling, with mypy/pyright as the CI-of-record unless the team decides otherwise), bandit/ruff-S security rules and semgrep (local usage only, pipeline design is A7.M8), complexity metrics (McCabe), dead-code detection, pre-commit and CI gates, handling legacy baselines. | Review checklist funnel; refactoring-under-tests loop (green, refactor, green); typing gradual adoption ladder. | Review 3 seeded PRs (find 10+ issues including 2 security bugs); refactor a 300-line view function into services with characterisation tests; add mypy strict to a module and fix errors; configure ruff rules. | Review comments about style only; rewriting instead of incremental refactor; "type hints are just documentation"; `# type: ignore` sprawl; refactor without tests; abstracting too early. |
| M7 | API security: OWASP, authN/authZ, injection, SSRF | Find and fix the dominant API vulnerability classes in Python code and write tests proving the fix. | OWASP API Security Top 10 (2023 edition: API1 BOLA, API2 broken authentication, API3 broken object property level authorisation, API4 unrestricted resource consumption, API5 BFLA, API6 unrestricted access to sensitive business flows, API7 SSRF, API8 security misconfiguration, API9 improper inventory management, API10 unsafe consumption of APIs) (October 2026: still the current edition, no newer API edition published); OWASP Top 10:2025 for web applications (released November 2025, finalised January 2026: A01 Broken Access Control, A02 Security Misconfiguration, A03 Software Supply Chain Failures, A04 Cryptographic Failures, A05 Injection, A06 Insecure Design, A07 Authentication Failures, A08 Software or Data Integrity Failures, A09 Logging and Alerting Failures, A10 Mishandling of Exceptional Conditions) mapped against the API list; **boundary:** token and OAuth2/OIDC design, JWT issuance and object-level authorisation design are taught in A5.M6 and referenced, not re-taught; design-time controls (CORS, CSRF, headers, TLS) are A5.M7; authN flaws: JWT pitfalls (`alg: none`, algorithm confusion, no `exp/aud/iss` checks, long-lived tokens, storing in localStorage trade-offs, revocation), session fixation, password reset flaws, brute force and credential stuffing, MFA bypass; password hashing (argon2id / bcrypt / scrypt, never fast hashes); authZ flaws: IDOR/BOLA (object-level checks per request, scoped querysets), BFLA, mass assignment (Pydantic `extra=forbid`, DRF serializer fields, Django `ModelForm` fields), tenant isolation, privilege escalation, deny-by-default and policy centralisation; injection: SQL (parameterised queries, ORM raw pitfalls, `.extra()`/`text()`), command injection (`subprocess` with `shell=True`), template injection (SSTI), NoSQL, header/log injection, unsafe deserialisation (`pickle`, `yaml.load`), path traversal and zip-slip, XXE; SSRF (cloud metadata endpoint 169.254.169.254, IMDSv2, allow-lists, DNS rebinding, redirects, URL parser differentials, egress controls); rate limiting and resource consumption (pagination caps, payload size, GraphQL depth, ReDoS). | Request path with trust boundaries and where each OWASP item bites; BOLA before/after queryset; SSRF to metadata service step-through; JWT validation checklist flow. | Exploit-and-fix labs on the shared Lab Service (P-10) with 10 seeded flaws (static-analysis and code-review fallback on vulnerable snippets if spike S-6 fails) (BOLA, mass assignment, SQLi, SSRF, weak JWT, ReDoS, path traversal, pickle, missing rate limit, verbose errors); fix each and add a regression test; hidden exploit tests grade. | "We use JWT so it's secure"; authN confused with authZ; blacklist validation; checking role but not ownership; trusting client-supplied IDs/fields; "internal service so no SSRF risk"; string-formatting SQL "just once". |
| M8 | Secure coding in Django/FastAPI, secrets and supply chain | Configure frameworks securely, manage secrets without leaks and reduce dependency risk. | Django: `check --deploy`, `SECRET_KEY`, `DEBUG`, `ALLOWED_HOSTS`, CSRF, session cookie flags, `SECURE_*` settings, clickjacking, auto-escaping and `mark_safe` risk, ORM safety, file-upload validation, admin hardening, `django-axes`-style lockout; DRF throttling and permission classes; FastAPI: dependency-based authN/authZ (`Security`, scopes), response models to avoid data leaks, CORS configuration (no wildcard with credentials), trusted hosts, docs endpoint exposure, request size limits, exception handlers not leaking internals; cross-cutting: security headers (CSP, HSTS, X-Content-Type-Options), CORS vs CSRF distinction, cookie vs bearer trade-offs, logging without PII/secrets, error handling. Secrets: never in code/images/logs, env vars vs secret managers (AWS Secrets Manager / SSM Parameter Store / Vault), IAM roles over static keys, rotation, secret scanning (gitleaks, trufflehog, GitHub push protection), what to do when a secret leaks (revoke first, then rewrite history), `.env` hygiene, Docker build secrets, least privilege. Supply chain: lockfiles and hash pinning (`pip --require-hashes`, uv/pip-tools/poetry lock), `pip-audit`, Dependabot/Renovate, SBOM (CycloneDX/SPDX), typosquatting and dependency confusion, malicious package campaigns, PyPI trusted publishing and PEP 740 digital attestations (live since November 2024; generated by default for GitHub Actions Trusted Publishing with the pypa publish action; shown as Provenance on PyPI; verified October 2026), vulnerability triage (reachability, severity, CVSS vs EPSS awareness), container base-image scanning, SLSA concepts, pinning GitHub Actions by SHA (as a concept; workflow and pipeline design is A7.M8; container image scanning, SBOM and signing is A7.M2). Tools covered at concept and local-usage level only: pip-audit, bandit, semgrep, ruff security rules, gitleaks/trufflehog. | Secrets lifecycle diagram; dependency attack surface map (direct, transitive, build, runtime, base image); "secret leaked" incident runbook. | Harden a seeded Django settings file and a FastAPI app (CORS, headers, docs); find and rotate a leaked secret in git history; run pip-audit and triage 5 findings; write a lockfile install with hash checking. | Rotating nothing after deleting a commit; "private repo so secrets are fine"; `CORS_ALLOW_ALL`; DEBUG on in staging; blindly bumping all dependencies; ignoring transitive risk; confusing CSRF and CORS. |
| M9 | Threat modelling and LLM-app security pointers | Run a 30-minute threat model on a design and identify the top LLM-application threats with mitigations. | Threat modelling basics: data-flow diagrams, trust boundaries, STRIDE, attack trees, abuse cases, risk rating (likelihood x impact, DREAD as legacy), mitigation vs acceptance, security requirements to tests; secure design principles (least privilege, defence in depth, fail safe, minimise attack surface, zero trust at a conceptual level); logging, detection and incident response basics; **Lesson 4 = X1 Compliance primer** (shareable standalone lesson, see below). LLM-app security is a **pointer lesson of at most 45 min** that maps OWASP Top 10 for LLM Applications (2025 edition, current in October 2026; list: prompt injection, sensitive information disclosure, supply chain, data and model poisoning, improper output handling, excessive agency, system prompt leakage, vector and embedding weaknesses, misinformation, unbounded consumption) to the B5 lessons (B5.M4 injection and red-team, B5.M5 PII, B5.M6 guardrails; agent tool permissions B4.M9): only the generic principles stay here (model is an untrusted component, output is untrusted input, least-privilege tools, tenant isolation). Depth is not taught in A8. **X1 Compliance primer (tag `sec.compliance`; owner A8.M9.4; shared lesson, pulled into the AI route even when A8 is NICE; awareness only, not legal advice, carries a disclaimer):** India DPDP Act 2023 and DPDP Rules 2025 (notified 13 November 2025; phased: Consent Manager rule effective 13 November 2026, most operational obligations on notice, safeguards, breach notification, erasure and children's data effective 13 May 2027; dated October 2026), GDPR basics (lawful basis, data-subject rights, DPIA, 72-hour breach notification to the regulator, cross-border transfer), data retention and minimisation, consent and purpose limitation, data residency, breach notification runbook, ASVS levels and SOC 2 / ISO 27001 awareness, what an engineer must build (consent records, deletion pipeline, audit logs, access controls). | Annotated DFD with trust boundaries; STRIDE table; LLM tool-call blast-radius diagram. | Threat-model a "chat assistant with tools and RAG" (the canvas task stays) and a "file upload + webhook" service (written exercise, scored by rubric); write 3 security tests for an LLM tool executor (over-privilege, output handling), with red-team test depth in B5.M4; X1 scenario: draft a retention and deletion design note for a customer-data service. | Listing generic threats without trust boundaries; "the model will refuse malicious instructions"; one-time threat model never revisited; security as a final gate not design input; treating system prompt as a secret control. |
| M10 | Performance and load testing basics | Profile a service, design a load test and read the results to find the real bottleneck. | Latency vs throughput, percentiles (p50/p95/p99) vs averages, coordinated omission, Little's Law, saturation and the knee of the curve, open vs closed workload models; test types (smoke, load, stress, spike, soak, capacity); tools: Locust (Python), k6, wrk/hey (as of 2026-10 Locust is 2.46.x on PyPI and k6 is 2.3.0 (21 September 2026); pins come from the platform manifest; verified 2026-10); test design (realistic data, think time, warm-up, test environment parity, avoiding load-generator bottleneck); finding bottlenecks at service level (Python profiler usage is taught in A1.M11 and async stall diagnosis in A2.M10; this module only *reads* profiler output to attribute the bottleneck; production telemetry is A7.M9): SQL (`EXPLAIN`, slow query log, N+1), connection pools, GIL and worker model (gunicorn/uvicorn workers), blocking calls in async code, cache hit ratio, downstream latency; SLO-linked pass/fail criteria; micro-benchmarks (`timeit`, `pytest-benchmark`) and their traps; performance regression gates in CI (budgeted, noisy-aware); load testing safely (never against prod without controls, rate caps, test data hygiene). | Latency-vs-load hockey-stick curve; flame graph reading guide; open vs closed model diagram. | Write a Locust scenario for the Lab Service; find a seeded N+1 and a blocking call in an async handler using supplied profiler output (profiling skills from A1.M11); produce a one-page capacity note from run results. | Reporting average latency; load-testing from a laptop over Wi-Fi; no warm-up; optimising before measuring; "add more workers" for a DB-bound service; benchmarks that measure the cache. |

## 3. Per-module detail

Lesson IDs are `A8.Mn.k`, where k is the number of the lesson in the module list below (so the X1 primer is `A8.M9.4`). Question-bank targets for every module (template minimum): 12 MCQ/concept, 6 code-output/predict-the-result, 4 coding tasks with hidden tests, 8 verbal with follow-up trees, 3 scenario/design. Mastery check for every module: 6–10 items, pass 80%, revision loop on misses. Where a module has no meaningful coding tasks (M1, M9), the 4 "coding" items are written/analysis tasks graded by rubric (for example, review this test file, fix this threat model) or small code-review exercises.

### M1 — Test strategy and the pyramid
- **Lessons:** (1) Why we test: risk, feedback speed, confidence. (2) Pyramid, trophy, honeycomb: choosing the shape. (3) Test doubles taxonomy and the "unit" debate. (4) Test smells and flaky tests. (5) Testing beyond pre-merge: canaries, synthetic checks, feature flags.
- **Simpler:** pyramid definitions, doubles vocabulary, flaky-test checklist. **Deeper:** economics (cost per test, feedback time), strategy for microservices vs monolith, testing in production safely, TDD trade-offs.
- **Mastery-check focus:** identify the right test level for a scenario; classify doubles; diagnose flaky causes (time, ordering, shared state, network, concurrency, randomness).
- **Badge criteria:** pass mastery check; submit a strategy doc for the sample service with explicit "not tested and why" and a flaky-test policy.

### M2 — Pytest in depth
- **Lessons:** (1) Fixtures: scope, yield teardown, factories. (2) Parametrisation and markers. (3) Built-in fixtures and monkeypatching. (4) Plugins and config (xdist, cov, randomly, timeout). (5) Speed and determinism.
- **Simpler:** fixture and parametrize basics, assertion idioms. **Deeper:** fixture resolution and teardown order, `indirect` parametrisation, `request.param`, hooks (`conftest` hooks, `pytest_collection_modifyitems`), xdist worker-safe resources, writing a tiny plugin.
- **Mastery-check focus:** predict fixture execution order and scope behaviour; select the right marker/parametrisation form; fix order-dependent tests.
- **Badge criteria:** pass mastery check; refactor task suite to be order-independent under `pytest-randomly` and parallel under xdist, passing hidden tests.

### M3 — Mocking, fakes and contract testing
- **Lessons:** (1) `unittest.mock` correctly: patch target, autospec, side effects. (2) Fakes vs mocks, DI for testability. (3) HTTP and time boundaries (respx/responses, freezegun/time-machine). (4) Consumer-driven contracts and provider verification. (5) OpenAPI and schema-based testing; event contracts.
- **Simpler:** patch where used, simple fakes, one contract example. **Deeper:** async mocks, mock leakage and `patch` lifecycle, contract versioning and can-i-deploy-style gating, backward-compatible schema evolution, Schemathesis stateful links.
- **Mastery-check focus:** choose double type; spot broken patch target; contract vs integration vs E2E distinctions; breaking vs non-breaking API change.
- **Badge criteria:** pass mastery check; replace an over-mocked suite with fakes while keeping behavioural coverage; contract pair passing hidden provider verification.

### M4 — Testing async code, APIs and databases
- **Lessons:** (1) Async tests and fixtures. (2) Testing FastAPI and Django endpoints. (3) Database isolation strategies. (4) Real infrastructure with containers vs fakes. (5) Testing background jobs, webhooks and idempotency.
- **Simpler:** run async tests, use test clients, rollback isolation. **Deeper:** testing cancellation and timeouts, race-condition tests with barriers/events, parallel-safe DBs with xdist, migration testing, query-count budgets, testing retries with fault injection.
- **Mastery-check focus:** why SQLite is not a substitute; rollback vs truncate trade-offs; async fixture scope pitfalls; override mechanics.
- **Badge criteria:** pass mastery check; integration suite for lab API with real Postgres, parallel-safe, includes failure-path and idempotency tests.

### M5 — Coverage, mutation and property-based testing
- **Lessons:** (1) Coverage: what it measures and doesn't. (2) Mutation testing. (3) Hypothesis fundamentals and shrinking. (4) Stateful properties and model-based testing intro. (5) Quality gates in CI and flaky quarantine.
- **Simpler:** read coverage reports, basic `@given`. **Deeper:** custom strategies, `assume` pitfalls, stateful rule machines, mutation scope strategy, diff-coverage gates.
- **Mastery-check focus:** interpret coverage vs mutation results; pick properties; reproduce a Hypothesis failure.
- **Badge criteria:** pass mastery check; kill >= N seeded surviving mutants; Hypothesis finds and the learner fixes a seeded bug.

### M6 — Code quality, review and refactoring
- **Lessons:** (1) Code review as a skill. (2) Clean code and design heuristics in Python. (3) Refactoring safely (characterisation tests, seams, strangler). (4) Static analysis: ruff, bandit/semgrep rules. (5) Type checking in practice.
- **Simpler:** review checklist, basic smells, ruff setup, basic hints. **Deeper:** Protocols and generics, strictness ladder, large-scale refactor strategy, debt prioritisation, reviewing for security and concurrency, review at scale (CODEOWNERS, bots, SLAs).
- **Mastery-check focus:** prioritise review findings; pick refactoring moves; fix typing errors; justify tool choices.
- **Badge criteria:** pass mastery check; seeded-PR review scored against rubric (recall of planted issues, tone, prioritisation); refactor passes hidden tests with unchanged behaviour.

### M7 — API security: OWASP, authN/authZ, injection, SSRF
- **Lessons:** (1) OWASP API Top 10 map and threat thinking. (2) Authentication flaws and JWT pitfalls. (3) Authorisation flaws: BOLA, BFLA, mass assignment, tenant isolation. (4) Injection family and unsafe deserialisation. (5) SSRF and resource consumption.
- **Simpler:** recognise each vulnerability and canonical fix. **Deeper:** exploit chaining, DNS rebinding and redirect SSRF bypasses, algorithm-confusion walk-through, authorisation architecture (policy layer, ABAC/ReBAC awareness), ReDoS analysis.
- **Mastery-check focus:** identify flaw from snippet; choose correct fix; distinguish authN/authZ; spot unsafe patterns in Django/FastAPI code.
- **Badge criteria:** pass mastery check; remediate all 10 lab flaws with regression tests; hidden exploit suite returns no successful exploit.

### M8 — Secure coding in Django/FastAPI, secrets and supply chain
- **Lessons:** (1) Django secure configuration and `check --deploy`. (2) FastAPI secure patterns: CORS, deps, response models. (3) Secrets management and leak response. (4) Dependency and supply-chain security. (5) Headers, cookies and browser-facing controls.
- **Simpler:** checklist-style hardening, scanner usage. **Deeper:** CSP design, secret-manager integration and rotation design, hash-pinned reproducible installs, SBOM usage, SLSA levels conceptually, CI hardening as concepts only (pinned actions, least-privilege tokens, OIDC to cloud; pipeline design is A7.M8).
- **Mastery-check focus:** misconfiguration spotting, leak response ordering, triage of vulnerability reports, CSRF vs CORS.
- **Badge criteria:** pass mastery check; hardened projects pass hidden config checks; leaked-secret exercise completed with correct response order; pip-audit triage memo.

### M9 — Threat modelling and LLM-app security pointers
- **Lessons:** (1) Threat modelling in 30 minutes: DFD, trust boundaries, STRIDE. (2) From threats to requirements and tests. (3) Detection, logging and incident response basics. (4) **X1 Compliance primer** (DPDP, GDPR basics, retention, consent, residency, breach notification, ASVS and SOC 2 awareness; standalone and shareable by stable ID). (5) LLM-app security pointer (<= 45 min): OWASP LLM Top 10 map to B5.M4-M6 and B4.M9, plus the tool-use blast-radius sketch.
- **Simpler:** STRIDE walk-through on a simple app; LLM threat list with one mitigation each. **Deeper:** attack trees, abuse cases tied to tests, compliance-to-engineering mapping (DPIA inputs, deletion pipelines), threat-modelling the LLM assistant on the canvas; indirect injection, agent permission design and red-team harnesses are taught in B5/B4 (pointers).
- **Mastery-check focus:** place trust boundaries; map STRIDE categories; spot excessive agency at pointer level; X1 scenario questions (which obligation applies, what to log, breach timeline).
- **Badge criteria:** pass mastery check; threat models for two designs scored against rubric (coverage of boundaries, quality of mitigations, prioritisation).

### M10 — Performance and load testing basics
- **Lessons:** (1) Performance vocabulary: percentiles, throughput, saturation, Little's Law. (2) Designing a load test (Locust/k6). (3) Attributing the bottleneck (using A1.M11 profiling outputs; no profiler tutorial here). (4) Reading results and finding the bottleneck. (5) Performance in CI and safe load testing.
- **Simpler:** run a provided Locust test, read percentiles. **Deeper:** coordinated omission, open vs closed models, capacity modelling, async blocking diagnosis, DB and pool tuning interplay, benchmark noise management.
- **Mastery-check focus:** interpret graphs; identify test design flaws; choose the right profiler; reason about worker models.
- **Badge criteria:** pass mastery check; Locust scenario plus capacity note scored against rubric; seeded bottlenecks located.

## 4. Seed interview questions (verbal, 3 per module)

Each: ideal answer outline, common weak answers, follow-up probes, difficulty, competency tag, company-style cluster. Style-of-question items only; none are real questions from any company.

### M1 — Test strategy and the pyramid

**Q1.1 "How would you design the test strategy for a new payments-adjacent order service with a queue and third-party gateway?"** — Difficulty: Senior. Tag `eng.test-strategy`. Cluster: global-product, Indian-product.
- Ideal answer:
  - Start from risks: money correctness, idempotency, duplicate events, gateway failure, data loss, security.
  - Many fast unit tests for pure domain logic (pricing, state machine) with fakes for ports.
  - Integration tests against real Postgres and queue (containers) for repositories, transactions and consumers.
  - Contract tests with the gateway (consumer-driven or recorded sandbox verification) plus provider-side schema checks.
  - A few E2E/smoke tests on critical journeys, run post-deploy with synthetic checks.
  - Non-functional: load test on checkout path, security tests for authZ, fault-injection for retries and timeouts.
  - CI design: fast feedback (< 10 min), parallelism, flaky quarantine, diff coverage and mutation on critical modules.
  - Explicitly state what is not tested and compensating controls (monitoring, canary, feature flags).
- Weak answers: "unit tests for everything then some integration"; quoting percentages (70/20/10) without reasoning; no mention of failure modes or contracts.
- Follow-ups: What would you cut if CI time doubled? How do you test the gateway without hitting it? How do you test exactly-once-ish processing?

**Q1.2 "Mocks, stubs, fakes, spies: explain the difference and when overusing mocks hurts."** — Difficulty: Mid-Senior. Tag `eng.test-doubles`. Cluster: IT-services, global-IT-consulting.
- Ideal answer:
  - Define: dummy (placeholder), stub (canned answers), spy (records calls), mock (pre-programmed expectations, verifies interactions), fake (working lightweight implementation).
  - State-based vs interaction-based testing; prefer state/behaviour verification.
  - Over-mocking couples tests to implementation; refactors break tests; green tests with broken integration.
  - Fakes preserve behaviour and are reusable, but need their own tests against the real thing (contract tests for the fake).
  - Mock only at architectural boundaries you own (ports), not internals or third-party types you do not own (wrap them).
  - Use autospec to keep mocks honest.
  - Example from experience: replaced N mocks with an in-memory repository.
- Weak: "mock is a fake object" with no distinction; mock everything for speed; cannot give a downside.
- Follow-ups: How do you keep a fake faithful to the real DB? When is a mock the right call? What is "don't mock what you don't own"?

**Q1.3 "A test fails once in fifty CI runs. What do you do?"** — Difficulty: Senior. Tag `eng.test-strategy`. Cluster: global-product, AI-startup.
- Ideal answer:
  - Treat flakiness as a bug in the test or the code; do not blanket-retry.
  - Reproduce: rerun with the same seed/order, loop the test, run under load/xdist, capture logs and artefacts.
  - Common causes: time and timezone, test order and shared state, unawaited async work, real network, randomness, resource races, port clashes, DB leakage.
  - Distinguish a flaky test from a real intermittent product race (it may be a real bug).
  - Fix at the cause: freeze time, deterministic seeds, await conditions not sleeps, isolate state.
  - Process: quarantine with owner and deadline, track flake rate, fail the build on new flakes, retry only as a measured mitigation.
  - Tooling: pytest-randomly, `--lf`, pytest-timeout, flake dashboards.
- Weak: "add retries / increase sleep"; "CI is just flaky".
- Follow-ups: How do you tell test flake from product race? What do you do when the owner has left? What metric do you track?

### M2 — Pytest in depth

**Q2.1 "Explain fixture scopes, teardown order and a bug you can introduce with a wrong scope."** — Difficulty: Mid-Senior. Tag `eng.pytest`. Cluster: IT-services, Indian-product.
- Ideal answer:
  - Scopes: function (default), class, module, package, session; fixtures are created lazily, cached per scope.
  - A higher-scope fixture cannot depend on a lower-scope one (ScopeMismatch).
  - `yield` fixtures: code after yield runs at teardown in reverse order of setup.
  - Bug: session-scoped mutable object (DB session, dict) shared across tests leading to order dependence; or module-scoped DB data making tests interfere.
  - Prefer function scope for state, wider scope for expensive immutable resources (engine, container, app) and reset state per test.
  - Parallel runs (xdist): session scope is per worker, so shared resources need per-worker naming.
  - `autouse` used sparingly; `conftest.py` layering and overriding closest-wins.
- Weak: only lists scope names; cannot explain teardown order; "use session scope for speed" with no state caveats.
- Follow-ups: How do you make a session-scoped DB container safe under xdist? How do you override a fixture for a subset? What is `request.addfinalizer` vs yield?

**Q2.2 "How do you parametrise tests well, and when is parametrisation the wrong tool?"** — Difficulty: Mid. Tag `eng.pytest`. Cluster: IT-services, global-IT-consulting.
- Ideal answer:
  - `@pytest.mark.parametrize` for data-driven cases with readable ids (`ids=` or `pytest.param(..., id=)`); stacking decorators gives the cartesian product.
  - `indirect=True` or fixture `params` when setup depends on the parameter.
  - Per-case marks (`xfail`, `skip`) via `pytest.param`.
  - Use tables for equivalence classes and boundaries; include edge cases explicitly.
  - Wrong tool when each case needs different assertions or setup logic (conditionals in tests), when parameters hide intent, or when a failing case obscures others in a giant matrix; also when properties would be better (Hypothesis).
  - Avoid computing the expected value with the same logic as production code.
- Weak: "just loops over a list inside a test" (single failure hides others); no ids.
- Follow-ups: How would you test 5 x 5 x 5 combinations without exploding runtime? Loop vs parametrise: what differs on failure? When move to Hypothesis?

**Q2.3 "Your test suite takes 25 minutes. How do you get it under 5 without losing confidence?"** — Difficulty: Senior. Tag `eng.pytest`. Cluster: global-product, Indian-product.
- Ideal answer:
  - Measure first: `--durations`, profile collection and fixture setup time.
  - Parallelise with xdist (needs isolation; per-worker DBs).
  - Reduce expensive setup: reuse DB schema (template DB), session-scope immutable resources, avoid per-test migrations, `--reuse-db`.
  - Push tests down the pyramid; replace slow E2E with contract/integration tests.
  - Remove sleeps and real network; fake time.
  - Split CI: fast pre-merge subset (changed-code selection, markers) and slower nightly full suite.
  - Cache dependencies/containers in CI; shard across runners.
  - Guard against loss of confidence: coverage/mutation checks on critical paths, nightly full run, track escaped defects.
- Weak: "buy bigger CI machines" only; delete slow tests.
- Follow-ups: How do you shard tests fairly? What if tests share a database? How do you decide which tests to run on a PR?

### M3 — Mocking, fakes and contract testing

**Q3.1 "`patch('app.utils.send_email')` does not take effect in your test. Why, and how do you fix it?"** — Difficulty: Mid-Senior. Tag `eng.test-doubles`. Cluster: IT-services, Indian-product.
- Ideal answer:
  - Python names are bound in the module namespace; `patch` replaces the attribute on the object you name, so it must be where the code under test looks it up.
  - If `service.py` did `from app.utils import send_email`, the name lives in `app.service`; patch `app.service.send_email`.
  - If code does `import app.utils` and calls `app.utils.send_email`, patching `app.utils.send_email` works.
  - Use `autospec=True` to catch signature drift; `patch.object` for clarity.
  - Prefer dependency injection (pass the mailer) to avoid patching altogether.
  - Mind decorator order of `@patch` arguments and context-manager scope; clean-up by default.
  - Async: `AsyncMock` for coroutines (`patch` auto-detects async functions in modern Python).
- Weak: "mock is broken"; patches at definition site always; no mention of import binding.
- Follow-ups: How would you design the code so it needs no patching? What goes wrong with a `MagicMock` that silently accepts any call? How do you assert call arguments safely?

**Q3.2 "Explain consumer-driven contract testing. How is it different from integration or E2E tests?"** — Difficulty: Senior. Tag `eng.contract-testing`. Cluster: global-IT-consulting, global-product.
- Ideal answer:
  - Problem: services evolve independently; end-to-end environments are slow and fragile.
  - Consumer writes expectations (requests it makes and responses it relies on) producing a contract; provider verifies it in its own CI against the real code with stubbed dependencies.
  - Contracts capture only what consumers use, allowing providers to change unused fields safely.
  - A broker/registry and a "can I deploy" check gate releases; versioned by consumer and provider versions and environments.
  - Differs from integration tests (which need real dependencies running together) and E2E (whole system); contract tests are fast, isolated, run per service.
  - Also covers async messages (event schemas) and can complement OpenAPI schema validation and Schemathesis.
  - Limits: does not verify business behaviour or non-functional properties; needs organisational buy-in; state setup on provider side.
- Weak: "it is just API tests with mocks"; no mention of provider verification.
- Follow-ups: How do you handle a breaking change with many consumers? Where does OpenAPI-based testing fit? Who owns the contract?

**Q3.3 "How do you test code that calls a flaky third-party HTTP API and depends on the current time?"** — Difficulty: Mid-Senior. Tag `eng.test-doubles`. Cluster: IT-services, AI-startup.
- Ideal answer:
  - Wrap the third party behind a port/client class; unit-test business logic with a fake client.
  - Test the client with HTTP-level mocking (respx/responses) for status codes, timeouts, malformed bodies, retries and backoff, rate-limit headers.
  - Use recorded cassettes sparingly; know they rot; re-record periodically against a sandbox in a scheduled job.
  - Add a contract/smoke test against the vendor sandbox, outside the pre-merge path.
  - Time: inject a clock or freeze time (freezegun/time-machine); test boundaries (expiry, DST, timezone, month end).
  - Verify idempotency keys and retry safety; test circuit-breaker behaviour.
  - Avoid `sleep`; test backoff with a fake sleep.
- Weak: "just mock requests.get"; real calls in CI; sleeps for time.
- Follow-ups: How do you test retry with jitter deterministically? How do you detect vendor schema changes? What if the vendor has no sandbox?

### M4 — Testing async code, APIs and databases

**Q4.1 "How do you test async code and what mistakes break async test suites?"** — Difficulty: Senior. Tag `eng.integration-testing`. Cluster: AI-startup, global-product.
- Ideal answer:
  - Use pytest-asyncio (strict mode with explicit marks) or anyio plugin; async fixtures with correct loop scope.
  - Test with `httpx.AsyncClient` and `ASGITransport` for FastAPI; run lifespan explicitly if needed.
  - Await conditions with timeouts, never fixed sleeps; use `asyncio.Event`/barriers to force interleavings for race tests.
  - Test cancellation, timeout and exception propagation (TaskGroup behaviour) deliberately.
  - Mistakes: calling `asyncio.run` inside tests, mixing sync DB drivers (blocking the loop), loop-scope mismatch across fixtures, un-awaited coroutines (enable warnings as errors), leaked tasks.
  - Use `AsyncMock` for async collaborators; do not mock the event loop.
  - Detect blocking calls (asyncio debug mode, slow-callback logging).
- Weak: "just add `async` to the test"; sleeps; no cancellation testing.
- Follow-ups: How do you test a race condition deterministically? What changes with session-scoped async fixtures? How do you catch a blocking call in a handler?

**Q4.2 "Why not use SQLite for tests of a Postgres-backed service, and how do you isolate DB state per test?"** — Difficulty: Senior. Tag `eng.integration-testing`. Cluster: IT-services, Indian-product.
- Ideal answer:
  - SQLite differs: types, JSONB, arrays, constraints and locking, isolation/MVCC behaviour, `SELECT FOR UPDATE`, partial/GIN indexes, migrations and DDL transactionality; tests pass but prod fails.
  - Use real Postgres via containers (testcontainers or CI service), schema created once (migrations or template DB).
  - Isolation options: per-test transaction rolled back (fast, but cannot test commit/concurrency behaviour), truncate tables, per-test schema, template DB clone per worker.
  - Django `TestCase` wraps in transaction; `TransactionTestCase` for commit semantics and on-commit hooks.
  - Parallelism: DB per xdist worker.
  - Test migrations separately, and test constraint/locking behaviour with real concurrent connections.
  - Factories for readable data; avoid giant shared fixtures.
- Weak: "SQLite is faster so fine"; global shared test DB; no mention of transaction semantics.
- Follow-ups: How do you test `on_commit` hooks or `SELECT FOR UPDATE SKIP LOCKED`? How do you keep it fast? How do you test a migration on production-sized data?

**Q4.3 "How do you test a webhook handler and a Celery task for idempotency and failure?"** — Difficulty: Senior. Tag `eng.integration-testing`. Cluster: global-IT-consulting, Indian-product.
- Ideal answer:
  - Idempotency: deliver the same event twice (and concurrently) and assert a single side effect; unique constraints/idempotency keys enforce it.
  - Signature verification tests (valid, invalid, replayed, expired timestamp).
  - Out-of-order events and duplicate deliveries; partial failure then retry.
  - Task testing: call the function directly for logic; eager mode for wiring but know it hides serialisation/broker behaviour; at least one test with a real broker for routing, serialisation and retry policy.
  - Fault injection: raise at each step, assert state is consistent (transactional outbox / on-commit).
  - Assert observability: logs/metrics emitted, dead-letter handling.
- Weak: only the happy path; "eager mode covers it".
- Follow-ups: How do you test concurrent duplicate delivery? What does eager mode miss? How do you test poison messages?

### M5 — Coverage, mutation and property-based testing

**Q5.1 "Your team mandates 90% coverage. Argue for and against, and propose something better."** — Difficulty: Senior. Tag `eng.test-quality`. Cluster: IT-services, global-IT-consulting, Big4.
- Ideal answer:
  - Coverage shows what is executed, not what is verified; assertion-free tests inflate it.
  - Line vs branch coverage; conditions and data-dependent bugs are missed.
  - Useful as a negative signal (uncovered code is untested) and for trends; poor as a target (Goodhart's law, gaming).
  - Better: diff coverage on new code, branch coverage with a floor, mutation testing on critical modules, review of test quality, escaped-defect tracking.
  - Exclusions must be explicit and reviewed; avoid blanket `# pragma: no cover`.
  - Risk-weighted targets: higher for money/auth paths, lower for glue.
  - Combine with property-based tests for input space.
- Weak: "100% is the goal" or "coverage is useless" with no nuance.
- Follow-ups: How would you roll out mutation testing without slowing CI? How do you handle legacy code at 30%? Give an example of covered-but-untested code.

**Q5.2 "What is mutation testing and what does a surviving mutant tell you?"** — Difficulty: Senior. Tag `eng.test-quality`. Cluster: global-product, AI-startup.
- Ideal answer:
  - Tool makes small changes (flip operator, change constant, remove statement), reruns tests; a mutant is killed if a test fails, survives if all pass.
  - Mutation score = killed / (total - equivalent).
  - Survivors point to missing assertions, untested branches, or equivalent mutants (no observable behaviour change).
  - Cost: runs the suite many times; mitigate via targeting changed files, test selection by coverage, parallelism, nightly runs.
  - Python tooling: mutmut, Cosmic Ray (mutmut 3.x is maintained; Cosmic Ray 8.7.0 released August 2026, so also maintained; verified 2026-10 on PyPI).
  - Use on critical, pure logic; not on glue or I/O-heavy code.
  - Act on it: write a test that kills the mutant, or decide it is equivalent.
- Weak: describes concept only; thinks 100% score is required.
- Follow-ups: What are equivalent mutants? How would you integrate this into CI? Mutation vs coverage: example where they disagree?

**Q5.3 "Explain property-based testing with Hypothesis and give properties for a function that splits a bill among N people."** — Difficulty: Senior. Tag `eng.test-quality`. Cluster: global-product, AI-startup.
- Ideal answer:
  - Specify properties over generated inputs rather than examples; Hypothesis generates, shrinks failures to minimal cases, and stores failing examples in a database.
  - Properties for bill split: shares sum exactly to total (in integer minor units), no share is negative, shares differ by at most one minor unit, number of shares equals N, order-independence where relevant, N=1 returns total.
  - Strategies: `integers`, `lists`, composite strategies for domain objects; constrain with strategies not `assume` where possible.
  - Other property patterns: round-trip (serialise/parse), idempotence, commutativity, model/oracle comparison, invariants in stateful testing.
  - Reproducibility: seed/`@reproduce_failure`, deterministic profiles in CI, example database.
  - Complement to example tests; examples document intent, properties explore space.
  - Pitfalls: floating-point money, slow properties, flaky because of non-determinism or deadlines.
- Weak: "random testing"; cannot state any property beyond "does not crash".
- Follow-ups: How does shrinking work conceptually? How do you test a stateful system (e.g., cart)? How do you handle deadlines for slow functions?

### M6 — Code quality, review and refactoring

**Q6.1 "Walk me through how you review a 600-line pull request."** — Difficulty: Senior. Tag `eng.code-review`. Cluster: global-product, Indian-product, Big4.
- Ideal answer:
  - First: push back on size; ask for split or a description with context, risk and test evidence.
  - Read description and tests first to understand intent; check CI and static analysis already green so human time goes to what tools miss.
  - Review order: correctness and edge cases, design fit and boundaries, security (authZ, input validation, secrets), failure modes and concurrency, data and migration safety, tests quality, readability and naming.
  - Distinguish blocking issues from nits (label them), explain why, suggest alternatives, ask questions rather than assert.
  - Run the code or key tests locally for risky changes.
  - Timebox, respond within agreed SLA; avoid style bike-shedding (automate with formatter/linter).
  - Approve with follow-up tickets for non-blocking debt; praise good work.
  - As author: self-review, small PRs, draft PRs for early feedback.
- Weak: "I read line by line for style"; only finds typos; rubber-stamps due to seniority of author.
- Follow-ups: Author pushes back on a blocker, what do you do? How do you review a database migration? How do you review AI-generated code?

**Q6.2 "You inherit a 400-line Django view with no tests. How do you refactor it safely?"** — Difficulty: Senior. Tag `eng.code-quality`. Cluster: IT-services, global-IT-consulting.
- Ideal answer:
  - Do not rewrite; first pin current behaviour with characterisation tests at the HTTP level (golden/snapshot of responses and side effects), including edge and error paths discovered from logs/production data.
  - Make tests deterministic (freeze time, fake external calls).
  - Small, reversible steps: extract functions, then service/domain layer, move queries to selectors/repositories, replace conditionals with strategies where it clarifies.
  - Keep each step green and committed; use coverage and mutation to see unpinned logic.
  - Introduce seams for dependencies; add type hints incrementally.
  - For risky paths, branch by abstraction or feature flag and shadow-run new vs old (compare outputs) before cutting over (strangler).
  - Communicate and timebox; do not mix refactor and behaviour change in one PR.
- Weak: "rewrite from scratch"; refactor first then write tests.
- Follow-ups: How do you test behaviour you do not understand? When is rewrite justified? How do you sell the refactor to the product manager?

**Q6.3 "What do static analysis and type checking give you, and how do you adopt mypy in a large untyped codebase?"** — Difficulty: Mid-Senior. Tag `eng.code-quality`. Cluster: global-product, AI-startup.
- Ideal answer:
  - Tools: ruff for lint/format (fast, replaces flake8/isort/black in many setups), mypy/pyright for types, bandit or ruff security rules and semgrep for security patterns.
  - Types catch None-handling, wrong arguments, interface drift; they also document and enable refactoring and IDE support; they do not check runtime data (use Pydantic for validation at boundaries).
  - Gradual adoption: start with new code and leaf modules; per-module strictness in config; baseline existing errors; ratchet (error count must not increase); then enable stricter flags (`disallow_untyped_defs`, `no_implicit_optional`, `warn_return_any`).
  - Use `Protocol`, `TypedDict`, generics, `Self`, `Literal`; avoid blanket `Any` and `# type: ignore` without codes.
  - Library stubs and plugins (Django, Pydantic); run in CI and pre-commit.
  - Trade-offs: false positives, tool differences (mypy vs pyright), speed.
- Weak: "type hints enforce types at runtime"; flips on strict everywhere day one.
- Follow-ups: mypy vs pyright differences? How do you type Django models/querysets? How do you handle third-party untyped libs?

### M7 — API security: OWASP, authN/authZ, injection, SSRF

**Q7.1 "What is BOLA, why is it the top API risk, and how do you prevent it in Django or FastAPI?"** — Difficulty: Senior. Tag `sec.authn-authz`. Cluster: global-product, Big4, Indian-product.
- Ideal answer:
  - Broken Object Level Authorisation: an authenticated user accesses other users' objects by changing an identifier (`/orders/123`); IDs are everywhere in APIs, and developers check login but not ownership.
  - Distinguish from BFLA (function-level, e.g., regular user calling admin endpoint).
  - Prevention: enforce object-level checks on every access; scope queries by tenant/owner (`Order.objects.filter(owner=request.user)`, selectors that require the actor), 404 vs 403 policy choice.
  - Centralise authorisation (policy layer, DRF object permissions, FastAPI dependency per resource) instead of ad-hoc checks; deny by default.
  - Do not rely on unguessable UUIDs as authorisation.
  - Test: automated two-user tests (user A requests B's resources) across all endpoints and methods; include in CI; check list endpoints, nested resources and bulk operations.
  - Related: mass assignment, property-level authorisation (API3).
- Weak: "use UUIDs"; "check the JWT is valid"; role checks only.
- Follow-ups: How do you test for BOLA at scale? How do you handle multi-tenant data (row-level security)? What about GraphQL or batch endpoints?

**Q7.2 "Here is a FastAPI endpoint that verifies a JWT. Find the exploitable flaws, show the exploit and fix them."** (exploit-and-fix on a seeded snippet; JWT design, token lifecycle and session trade-offs are owned by A5.M6 and Q-A5-M6-02: `ref`) — Difficulty: Senior. Tag `sec.authn-authz`. Cluster: IT-services, global-IT-consulting, Indian-product.
- Ideal answer:
  - Reads the snippet and names concrete flaws: decoding without verifying the signature, accepting `alg: none` or letting the token header choose the algorithm (algorithm confusion), missing `exp`/`aud`/`iss` checks, hard-coded HS256 secret in source.
  - Shows the exploit: a forged token with changed role claim accepted, with the request that proves it; writes the failing test first.
  - Fixes in code: pinned algorithm allow-list, required claims, key from secret store, failing test turned green, regression test kept in the suite.
  - Notes follow-on hardening (rotation, revocation) as a pointer to A5.M6 only.
- Weak: describes JWT theory without touching the code; fixes by adding a longer secret only.
- Follow-ups: Write the hidden exploit test you would ship. How do you prove the fix did not break valid tokens? What does the scanner miss?

**Q7.3 "Describe SSRF, a real-world impact path and layered defences."** — Difficulty: Senior. Tag `sec.ssrf`. Cluster: global-product, AI-startup, Big4.
- Ideal answer:
  - Server fetches a user-controlled URL (webhooks, image fetch, URL preview, PDF generation, LLM browsing tools) and can be pointed at internal resources.
  - Impact: reach internal services, cloud metadata endpoint (credentials via instance role), internal admin panels, port scanning.
  - Defences in layers: avoid fetching user URLs; allow-list hosts/schemes; resolve DNS and validate the IP against deny-list of private/link-local/loopback ranges, then connect to that validated IP (prevents DNS rebinding); disable or re-validate redirects; restrict ports and methods; timeouts and size limits.
  - Infrastructure: IMDSv2 required with hop limit, egress firewall/proxy, network segmentation, least-privilege IAM roles so stolen creds are low-value.
  - Parser differentials and encodings (decimal/octal IPs, IPv6, `@` tricks), blind SSRF.
  - Testing: seeded targets, automated SSRF test payload list in CI.
- Weak: "block 127.0.0.1"; regex on URL string; ignores redirects and DNS.
- Follow-ups: Why is validating then fetching by hostname vulnerable? What is IMDSv2 changing? How does an LLM browsing tool change SSRF risk?

### M8 — Secure coding in Django/FastAPI, secrets and supply chain

**Q8.1 "An API key was committed to a public repo 20 minutes ago. What do you do?"** — Difficulty: Mid-Senior. Tag `sec.secrets`. Cluster: IT-services, global-product, Indian-product.
- Ideal answer:
  - Treat as compromised immediately; revoke/rotate first, before cleaning history.
  - Assess blast radius: what the key can do, check provider access logs for use since the commit, check for lateral keys.
  - Remove from code and replace with secret-manager or environment reference; deploy the new secret.
  - History rewrite (filter-repo/BFG) is secondary, since forks, caches and scrapers may already have it; coordinate with the team.
  - Notify security/stakeholders per policy; if data exposure, follow incident and regulatory process.
  - Prevent recurrence: pre-commit secret scanning (gitleaks), CI scanning, GitHub push protection, short-lived credentials/IAM roles/OIDC instead of static keys, least privilege and scoped keys.
  - Blameless post-mortem.
- Weak: "delete the commit and force-push"; no rotation; no log review.
- Follow-ups: How do you design secrets for Lambda or ECS? How do you rotate without downtime? What about secrets in Docker images and CI logs?

**Q8.2 "How do you assess and reduce supply-chain risk in a Python service?"** — Difficulty: Senior. Tag `sec.supply-chain`. Cluster: global-product, Big4, global-IT-consulting.
- Ideal answer:
  - Threats: known-vulnerable dependencies, malicious packages, typosquatting, dependency confusion (internal names on public index), compromised maintainer accounts, poisoned build/CI, vulnerable base images.
  - Controls: lockfiles with hashes (`--require-hashes`, uv/pip-tools/poetry lock), reproducible builds, private index precedence rules, minimal dependency surface.
  - Continuous scanning: `pip-audit`, Dependabot/Renovate, container scanning, SBOM generation (CycloneDX/SPDX).
  - Triage by exploitability and reachability, not just CVSS; EPSS/KEV awareness; define SLAs by severity.
  - Review new dependencies (maintenance, popularity, licences, install scripts), pin GitHub Actions by SHA, least-privilege CI tokens, OIDC to cloud, protected branches, signed artefacts and provenance (SLSA concepts, PyPI trusted publishing and PEP 740 attestations, generated by default by pypa/gh-action-pypi-publish v1.11.0 or later with a trusted publisher; verified 2026-10).
  - Balance: auto-update with tests vs stale pins.
- Weak: "run pip-audit"; "pin versions" only; no mention of malicious packages.
- Follow-ups: How would dependency confusion happen here? How do you triage 200 findings? How do you verify a new package before adding it?

**Q8.3 "What are the security settings you check on a Django or FastAPI service before go-live?"** — Difficulty: Mid-Senior. Tag `sec.api`. Cluster: IT-services, Big4, Indian-product.
- Ideal answer:
  - Django: `DEBUG=False`, strong secret key from secret store, `ALLOWED_HOSTS`, `python manage.py check --deploy`, HTTPS redirect and HSTS, secure/HttpOnly/SameSite cookies, CSRF middleware, `X_FRAME_OPTIONS`, password validators and hashing (argon2), admin path/MFA/IP restriction, DRF throttling and default-deny permissions, no `mark_safe` on user input, file upload validation and storage outside web root.
  - FastAPI: response models to control output fields, dependency-based authN/authZ, CORS with explicit origins (no `*` with credentials), TrustedHost, docs/openapi exposure policy, body size limits, rate limiting at gateway, exception handlers hiding internals, input validation via Pydantic with `extra=forbid` for write models.
  - Common: security headers (CSP, nosniff, referrer policy), structured logs without PII/secrets, dependency scanning, least-privilege DB user, migrations user separate, TLS to DB, health endpoints without data.
  - Distinguish CORS (browser read policy) from CSRF (cross-site state change); CORS is not an authZ control.
  - Test: security regression tests, DAST scan, header checks.
- Weak: checklist recited without reasons; "CORS protects the API".
- Follow-ups: Explain CSRF vs CORS. Why use response models for security? What would CSP stop and not stop?

### M9 — Threat modelling and LLM-app security pointers

**Q9.1 "Run a quick threat model for a file-upload plus webhook service."** — Difficulty: Senior. Tag `sec.threat-modelling`. Cluster: Big4, global-IT-consulting, global-product.
- Ideal answer:
  - Four questions: what are we building, what can go wrong, what are we doing about it, did we do a good job.
  - Draw a data-flow diagram: client, API, object store, queue, worker, DB, third-party webhook sender; mark trust boundaries (internet to API, API to storage, worker to external fetch).
  - STRIDE per element: spoofing (webhook sender authenticity, signature), tampering (upload content, replayed webhooks), repudiation (audit logs), information disclosure (pre-signed URL scope, bucket policies, error messages), denial of service (large files, zip bombs, queue flooding), elevation of privilege (worker role too broad, malicious file executing in processor).
  - File-specific: type and size validation, content sniffing, antivirus/sandboxed processing, random names, no execution, private bucket and short-lived pre-signed URLs, path traversal.
  - Prioritise by likelihood and impact; decide mitigate, transfer, accept; record assumptions.
  - Convert mitigations into requirements and tests; revisit on design change.
- Weak: lists generic OWASP items without a diagram or boundaries; no prioritisation.
- Follow-ups: Where is the highest-risk trust boundary? How do you keep threat models alive in an agile team? How would you rate these risks?

**Q9.2 (ref: B5.M4) "What are the main security risks of an LLM application with tool use and RAG, and how do you mitigate them?"** — Difficulty: Senior. Tag `llm.security` (secondary, owner B5). Cluster: AI-startup, global-product. This is the single pointer item of the course (R-A8-4); graders accept an answer that states the principles below and routes depth to B5.M4.
- Ideal answer (principles only): treat the model as an untrusted component and its output as untrusted input; prompt injection (direct and indirect) has no complete prompt-level fix; limit tools and scope credentials with server-side authorisation independent of the model; never pass output to `eval`, SQL, shell or unescaped HTML; tenant isolation and ACL filtering before retrieval; token and rate budgets; pin models, providers and libraries; log and monitor. Depth (OWASP LLM Top 10, red-teaming, privilege design) is B5.M4 and B4.M9.
- Weak: "add a system prompt saying ignore malicious instructions"; ignores tools and RAG; no authorisation boundary.
- Follow-ups: How would an indirect injection exfiltrate data here? How do you enforce per-user access in RAG?

**Q9.3 "How do you embed security into the delivery process without becoming the bottleneck?"** — Difficulty: Senior-Staff. Tag `sec.threat-modelling`. Cluster: Big4, IT-services, global-IT-consulting.
- Ideal answer:
  - Shift left with paved roads: secure templates, vetted libraries, default-deny patterns so the easy path is secure.
  - Automation in CI: secret scanning, SAST (semgrep/bandit/ruff-S), dependency and container scanning, IaC scanning, DAST on staging; thresholds and exceptions with expiry.
  - Risk-based threat modelling for new or high-risk designs only (lightweight template), security champions per team.
  - Security acceptance criteria in stories (derived from ASVS level), abuse cases and security tests next to functional ones.
  - Review gates proportional to risk (auth, payments, data export need security review).
  - Operations: logging and alerting, incident runbooks, patch SLAs, periodic pen tests and bug bounty where relevant.
  - Metrics: time to remediate, escaped vulnerabilities, scan false-positive rate; keep noise low to maintain trust.
  - Culture: blameless, education, make reporting easy.
- Weak: "a security team reviews everything at the end"; "buy a scanner".
- Follow-ups: How do you handle scanner noise? A team misses a deadline due to a blocking finding, what next? How do you measure success?

### M10 — Performance and load testing basics

**Q10.1 "Your load test shows average latency 200 ms but users complain. What is going on and how do you investigate?"** — Difficulty: Senior. Tag `perf.load-testing`. Cluster: global-product, Indian-product.
- Ideal answer:
  - Averages hide tail latency; look at p95/p99 and distributions; mean is dominated by fast requests.
  - Per-endpoint and per-user-journey breakdown; error rates and timeouts excluded from latency can hide failures.
  - Check coordinated omission in the load tool (closed model slows sending when server slows); use open arrival-rate model to see real queuing.
  - Tail causes: GC pauses, lock contention, cold caches, connection pool exhaustion, slow queries on some data shapes, noisy neighbours, retries, downstream timeouts.
  - Correlate with resource metrics (CPU, memory, DB connections, queue depth) and traces to find where time goes.
  - Use percentiles aggregated correctly (do not average percentiles); histograms.
  - Tie to SLO (for example p99 < X at Y rps); verify the workload resembles production.
  - Fix, then retest and compare against a baseline.
- Weak: "add more servers"; "average is fine".
- Follow-ups: What is coordinated omission? Why can't you average p99 across instances? How do you pick a realistic workload?

**Q10.2 "Design a load test for a checkout API before a sale event."** — Difficulty: Senior. Tag `perf.load-testing`. Cluster: Indian-product, global-product, IT-services.
- Ideal answer:
  - Goals: capacity target (expected peak rps with headroom, for example 2–3x), SLO pass/fail criteria, find the bottleneck and breaking point.
  - Workload model from production analytics: mix of endpoints, think time, data distribution (hot items, user sessions); arrival-rate (open) model for spike behaviour.
  - Environment: production-like sizing and data volume (or scale-adjusted), isolated; mocks or sandboxes for payment gateways with realistic latency; protect real third parties.
  - Test types: baseline smoke, ramp load to target, stress beyond target to find the knee, spike (sale start), soak for leaks.
  - Test data hygiene: unique users/carts, inventory resets, idempotency keys, cleanup.
  - Load generators: distributed, monitored so they are not the bottleneck; warm-up phase.
  - Observability during the test: APM, DB metrics, queue depth, error budget; abort conditions.
  - Output: report with percentile charts, saturation point, bottleneck evidence, recommendations, and rerun after fixes; plan autoscaling and rate limiting/queueing for overload.
- Weak: "run Locust with 1000 users"; no pass criteria; hits real payment gateway.
- Follow-ups: How do you test DB-heavy inventory contention? How do you validate autoscaling? How do you handle third-party limits?

**Q10.3 "Your load test shows throughput plateauing as concurrency rises while CPU stays low. Using the data, attribute the bottleneck."** (async-specific diagnosis such as blocking calls and event-loop lag is owned by A2.M10 and A2.Q4.3; `ref`) — Difficulty: Senior. Tag `perf.load-testing`. Cluster: AI-startup, global-product.
- Ideal answer:
  - Reads the load-test output: latency percentiles versus concurrency, the knee in the curve, error rate and queueing; applies Little's Law (concurrency = throughput x latency) to see which resource is saturated.
  - Attribution method: per-stage timing from traces, pool and queue wait metrics, DB and downstream saturation, worker and connection counts multiplied across processes; separates queueing delay from service time.
  - Distinguishes coordinated omission and warm-up effects from a real plateau; reruns with one variable changed.
  - Reports the finding with evidence and a before/after test; event-loop blocking is named and handed to the A2.M10 debugging toolbox.
- Weak: "async is always faster"; "increase workers" without data; no control run.
- Follow-ups: How do pool sizes multiply across workers? How do you know the load generator is not the bottleneck? What is the capacity number you would publish?

(Total seed questions: 30.)

## 5. Final assessment

| Component | Detail |
|---|---|
| Mastery check | 30 items drawn across M1–M10 (3 per module, adaptive weighting towards weak modules), pass 80%, revision loop on missed items. |
| Timed practical task (90 min, IDE) | Given the lab service: (1) add a test suite for 3 seeded behaviours incl. one async and one DB-backed path, (2) fix 3 seeded vulnerabilities (BOLA, SSRF, SQL injection or mass assignment) with regression tests, (3) locate and fix one performance defect using a profiler. Hidden tests, hidden exploit checks and a mutation-score threshold on the supplied module grade it. If the Lab Service is not released (S-6), part (2) becomes code-review and static-analysis fixes on vulnerable snippets graded by hidden tests on the fixed code. |
| Review exercise (20 min) | Review a seeded 200-line PR containing logic bugs, a security flaw, a flaky test and a missing migration safeguard; scored on recall, prioritisation and tone. |
| Voice mock round (20 min) | Interviewer persona runs 4 verbal questions with follow-ups (one each from: test strategy, mocking or contract, API security, performance or threat model); rubric scoring with transcript evidence; strict mode. |
| Design task (canvas, 30 min) | "Secure and testable design of an LLM assistant with tool access and RAG": canvas with trust boundaries, test strategy per component and threat mitigations; evaluated on boundaries, authZ placement, test coverage plan, failure modes, observability. |
| Certificate | Awarded when mastery >= 80%, practical passes, and all module badges are earned. |

## 6. Course-level risks and freshness notes

- **OWASP editions (resolved October 2026):** API Security Top 10 2023 is the current API edition (no newer one found); OWASP Top 10:2025 (web) is final (A03 Software Supply Chain Failures and A10 Mishandling of Exceptional Conditions are new); OWASP Top 10 for LLM Applications 2025 is current. Re-check every 6 months.
- **Verified October 2026:** pytest 9.0 (8 November 2025; subtests built in, deprecation clean-up) with 9.x point releases; pytest-asyncio 1.0 (26 May 2025, `event_loop` fixture removed); Pyrefly 1.0 stable (May 2026); ty in beta; mutmut 3.x line active (3.0.0 to 3.0.4 are yanked on PyPI; 3.8.0 was current on 2026-10-05); pytest 9.1.1 and pytest-asyncio 1.4.0 current on PyPI; PyPI attestations live. **Remaining [verify] (engineering, at image build; not course facts):** record the pinned versions of pytest, pytest-asyncio (loop-scope semantics), Hypothesis, coverage.py, mutmut, Django 5.2 LTS, FastAPI and Pydantic v2 from the platform manifest and re-run the M2, M4, M5 and M10 lab suites on them. Free-threading and the GIL story are owned by A1.M3 and A2.M1 and are not restated here.
- **Tooling churn:** type checkers (mypy, pyright, and newer Rust-based checkers such as ty and pyrefly: Pyrefly 1.0 stable, ty beta, October 2026), ruff rule sets, uv adoption for lockfiles and hash pinning, Locust vs k6 positioning. Teach concepts first, tools as current examples.
- **Supply-chain landscape moves fast:** PyPI attestations/trusted publishing, new malicious-package campaigns, SBOM/regulatory expectations. Keep examples generic and dated.
- **LLM security is volatile:** attack techniques and defences change quarterly; mark each LLM-security lesson with a "last reviewed" date and a model/version volatility note; avoid claiming any defence is complete.
- **Lab safety:** the vulnerable lab API and SSRF target must be network-isolated, ephemeral and non-exploitable beyond the sandbox; exploit payloads must stay educational (no weaponised tooling); require legal/security review of the lab before publishing.
- **Regulatory references (DPDP Act 2023, GDPR):** awareness-level only; DPDP Rules 2025 were notified 13 November 2025 with phased commencement (Consent Managers 13 November 2026; most obligations 13 May 2027), verified October 2026; amendments or extensions are possible, so re-check every 6 months; not legal advice.
- **Overlap risk:** A5 (auth design), A7 (CI/CD, observability), B5 (LLM evals and red-teaming), A2 (async mechanics), A4 (isolation and locking). Boss agent to reconcile boundaries.
- **Interview-calibration risk:** seed questions are style-of-question; difficulty labels need pilot calibration with at least 3 reviewers.
- **Review cadence:** testing core 12 months; security 6 months; LLM-security 3 months; performance 12 months.

## 7. Open questions for the boss agent

All five wave-1 questions were ruled on (R-A8-1 to R-A8-5) and are implemented above. Remaining real item: spike S-6 (Lab Service legal and security review) gates M7 labs and the timed practical; fallback is documented in the header.

Boss final (2026-10-06): (1) a qualified privacy/legal reader must review the X1 compliance primer (A8.M9.4, including the DPDP Rules phase dates) before it is published, then every 6 months (HAR-09); (2) the inherited seed goal tags and senior-bar notes (section 8.1 and 8.5) are accepted under FR-05; (3) replace Q9.2 with a bare bank-ID `ref` to B5.M4 once B5 publishes item IDs.


## 8. Learner-Fit Layer (template section 6; binding for authors, placement and the adaptive engine)

Reference learner (user zero): senior backend engineer, 11+ years, about 4 in Python, daily FastAPI/Django/Flask, SQL, Redis, Docker, AWS Lambda/S3, LLM APIs and voice agents, targeting Senior Python Backend and AI Engineer interviews. The layer is written as rules so other learners (L1-L4) are served too. Judgement-based weights are marked `[judgement]`.

G-11 mapping: LF-1 = 8.3; LF-2 = 8.1, 8.1a and 8.2 (tags, weights, diagnostics, bands) plus 8.9 (rehearsal and readiness); LF-3 = 8.4; LF-4 = 8.5; LF-5 = 8.7; LF-6 = 8.8; LF-7 = 8.6 (the stamp for every module is 2026-10; security modules M7 to M9 re-reviewed every 6 months, the LLM pointer every 3).

Seed-question conventions (G-10): seed labels `Qn.k` (module n, item k) are legacy labels; the importer assigns the canonical `Q-A8-Mn-nn` number and keeps the label. Difficulty words map to the 1-5 integer scale as Mid 2, Mid-Senior 3, Senior 4, Senior-Staff 5 (A8 labels are seniority-based, so they sit one step higher than the easy/medium/hard words used in A1 to A7; align after the pilot). Seed goal tags and senior-bar notes are inherited from the module rows in 8.1 and 8.5.

### 8.1 Goal tags and interview-weighted time

Goal tags use the platform format `B:x A:x H:x` (G-3): B = Senior Python Backend, A = AI Engineer, H = Python+AI hybrid; M = MUST, S = SHOULD, N = NICE. Course-level: `B:S A:N H:S`. Seed questions inherit their module's tag and the module's senior-bar line in 8.5 serves as the senior-bar note (G-10). X1 Compliance primer is `B:S A:S H:S` and is pulled into the AI route even when A8 is NICE.

| Module | Goal tags | Simpler h | Deeper h | Time weight rationale `[judgement]` |
|---|---|---|---|---|
| M1 Test strategy and pyramid | B:S A:N H:S | 2.0 | 1.5 | Appears as a senior "how do you test X" opener in services and product rounds; high leverage, low hours |
| M2 Pytest in depth | B:S A:N H:S | 2.5 | 1.0 | Daily skill, rarely a deep-dive topic; screening-level, so skip-able by seniors |
| M3 Mocking, fakes, contract testing | B:S A:N H:S | 2.5 | 2.0 | Contract testing is a frequent microservice-round follow-up; patch-target questions are common in coding screens |
| M4 Async, API and DB testing | B:S A:N H:S | 3.0 | 2.0 | Links A2 and A4; strong signal for backend seniors |
| M5 Coverage, mutation, property-based | B:S A:N H:S | 2.5 | 1.5 | Differentiator ("what does coverage prove?"); rarely asked at depth |
| M6 Code quality, review, refactoring | B:S A:N H:S | 2.5 | 1.5 | Senior-bar behavioural-technical crossover; feeds C3.M8 and leadership rounds |
| M7 API security (OWASP) | B:M A:S H:M | 4.5 | 3.0 | Most-probed A8 topic in Big 4 and product security-aware rounds; resumes under-cover it; owner module |
| M8 Secure coding, secrets, supply chain | B:S A:N H:S | 2.5 | 2.0 | Frequent in services and consulting rounds ("secret leaked, what now?") |
| M9 Threat model, X1 compliance, LLM pointer | B:S A:S H:S | 2.5 | 1.5 | Includes X1 (+1.5 S / +1 D) and the LLM pointer (<= 45 min); Big 4 and consulting weight compliance |
| M10 Performance and load testing | B:S A:N H:S | 2.5 | 1.5 | Appears inside system design; method matters more than tools |
| **Total (ties to the header and the boss effort table)** | | **27.0** | **17.5** | Module hours sum exactly to the header; assessment allowance (3.0 h) is separate. Skip and skim bands bring a typical L4 learner below the Deeper total |

### 8.1a Interview weight and diagnostics (G-11 LF-2)

Weight = percent of A8-topic interview content per target (each column sums to 100) `[judgement]`, to be calibrated after the 3-learner pilot. Rationale: backend rounds probe API security, test strategy and performance most; AI-engineer rounds weight threat modelling, X1 and LLM-tool risk; hybrid sits between. Diagnostic items are `Q-A8-Mn-D1..Dk` (placement-tagged MCQ or predict items; the written item for M1, M6, M9 is counted).

| Module | M1 | M2 | M3 | M4 | M5 | M6 | M7 | M8 | M9 | M10 | Sum |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Weight B | 10 | 6 | 9 | 10 | 6 | 10 | 18 | 10 | 10 | 11 | 100 |
| Weight A | 6 | 3 | 8 | 8 | 6 | 6 | 16 | 9 | 24 | 14 | 100 |
| Weight H | 8 | 5 | 9 | 9 | 6 | 8 | 17 | 10 | 17 | 11 | 100 |
| Diagnostics (count) | 6 | 6 | 6 | 6 | 6 | 6 | 8 | 6 | 7 | 6 | 63 |

Company-style clusters: IT-services and global-IT-consulting weight M1, M3, M6, M8, M9 (compliance, review process); Big 4 weights M7, M8, M9, X1; global-product weights M1, M4, M5, M7, M10; Indian-product weights M4, M7, M10; AI-startup weights M9 pointer, M7 and B5 follow-ups.

### 8.2 Entry diagnostics (placement items per module and score bands)

Each module has 5-8 placement items (G-5; counts in 8.1a) (mix of MCQ, predict-the-output and one short written item for M1, M6 and M9). Bands use the percent score on that module's items:

| Band | Score | Decision | What the learner does |
|---|---|---|---|
| Skip | >= 85 and the written item passes (M1, M6, M9) | Test out | Go straight to the mastery check; module badge on pass; revisit only missed topics |
| Skim | 70-84 | Skim | Watch visuals, do Deeper lessons and challenge tasks, skip Simpler lessons and basic practice |
| Study | 50-69 | Study | Full Simpler path plus selected Deeper lessons |
| Deep-study | < 50 | Deep-study | Full Simpler and Deeper, all practice, prerequisite remediation links below |

Sample diagnostic themes (items to be authored from these seeds):
- M1: pick the test level for 5 scenarios; classify five doubles; list three causes of a once-in-fifty failure; write what you would not test.
- M2: predict fixture order and teardown for a nested conftest; spot an autouse leak; choose `indirect` vs plain parametrisation; xdist-safe resource.
- M3: which line to patch; why autospec; consumer vs provider verification; breaking vs non-breaking change; "is this a contract test".
- M4: why SQLite lies for Postgres code; rollback vs truncate; `dependency_overrides`; pytest-asyncio 1.x `loop_scope`; idempotency test shape.
- M5: line vs branch coverage output; surviving mutant meaning; choose a property; shrinking; why mutation only on changed files.
- M6: rank 6 review findings; spot SOLID violation; characterisation test purpose; mypy strictness ladder.
- M7: BOLA vs BFLA; mass assignment fix; JWT `alg` pitfall; SSRF allow-list bypass; parameterised query vs ORM raw; ReDoS.
- M8: `check --deploy` items; CORS vs CSRF; leaked-secret response order; hash pinning; triage a pip-audit finding.
- M9: draw trust boundaries; STRIDE mapping; DPDP vs GDPR breach notification; consent vs legitimate use; LLM output as untrusted input.
- M10: p99 vs mean; coordinated omission; Little's Law arithmetic; open vs closed model; "DB-bound, add workers?".

Reference-learner expectation `[judgement]`: M2 skip or skim, M3 skim (Pact-style contracts likely new), M4 skim, M5 study (mutation and Hypothesis likely new), M6 skim, M7 study (resumes under-cover it), M8 skim, M9 study (threat modelling, X1 new), M10 skim.

### 8.3 Level paths (L1-L4) per module

L1 = new to testing and security, L2 = junior-mid, L3 = experienced, L4 = senior or expert. Simpler maps to L1-L2, Deeper to L3-L4. Practice mix shown as share of tasks.

| Module | L1 | L2 | L3 | L4 |
|---|---|---|---|---|
| M1 | Vocabulary and pyramid; 80% guided, flaky checklist | Choose levels for a scenario; draft a strategy with help | Write a risk-based strategy with an explicit "not tested" list | Strategy across teams, cost per test, test governance, testing in production; defend trade-offs aloud |
| M2 | Basic fixtures and parametrize; 90% guided | Scopes, markers, tmp_path, monkeypatch | Teardown order, indirect params, xdist-safe resources | Write a small plugin and hooks; suite speed budgets; mentor others on fixture design |
| M3 | Patch where used; one fake | Autospec, side effects, respx; first contract example | Provider verification, schemathesis, async mocks, fakes tested against the real thing | Contract governance (can-i-deploy gating), schema evolution policy, fakes as shared team assets |
| M4 | Run async tests, TestClient, rollback | Overrides, factories, query counts | Parallel-safe DBs, migration tests, fault-injected retries, idempotency | Test-environment architecture, cost and time trade-offs, failure-path strategy for event-driven systems |
| M5 | Read coverage report | Branch coverage, basic `@given` | Mutation on changed files, custom strategies, reproduce failures | Stateful and model-based tests, honest quality KPIs, diff-coverage policy design |
| M6 | Review checklist, ruff setup | Smells and refactoring moves, basic typing | Review seeded PRs, Protocols and generics, refactor under characterisation tests | Review culture, CODEOWNERS and SLAs, debt triage, mentoring through review, large-scale refactor strategy |
| M7 | Recognise each vulnerability class and canonical fix (45% of tasks) | Fix labs with guidance | Fix all 10 flaws unaided, regression tests, exploit chaining | Authorisation architecture (policy layer, ABAC/ReBAC awareness), bypass analysis (DNS rebinding, parser differentials), advise a team on remediation order |
| M8 | Checklist hardening, run scanners | Secrets lifecycle, pip-audit triage | Hash-pinned installs, CSP design, rotation design, SBOM use | Org-level secrets and dependency policy, incident leadership, trade-offs of pinning vs patch velocity |
| M9 | STRIDE on a simple app; X1 vocabulary | Threat model with a template; X1 obligations list | Two threat models to rubric; compliance-to-engineering mapping | Facilitate threat modelling, set security requirements from ASVS, advise on compliance trade-offs |
| M10 | Run provided Locust test, read percentiles | Write a scenario, interpret curves | Coordinated omission, open vs closed, capacity note | Capacity planning, SLO-linked gates, negotiate performance budgets |

### 8.4 Experience bridge ("From what you already know")

| Module | You already know | Gap between using it and explaining it at senior depth |
|---|---|---|
| M1 | You have written unit tests on FastAPI/Django services | Justifying a mix by risk and cost, naming what you do not test, and a flaky-test policy |
| M2 | pytest fixtures and parametrize in daily work | Teardown ordering, scope lifetimes, xdist worker safety, plugin hooks |
| M3 | `unittest.mock`, patching HTTP calls to LLM APIs | Patch-target rule, autospec, fakes vs mocks, consumer-driven contracts across services |
| M4 | TestClient, pytest-django, Postgres, Lambda handlers | Isolation strategies, why SQLite lies, async loop scopes, idempotency and fault-injection tests |
| M5 | Coverage badge in CI | Coverage vs mutation score vs properties; what each proves |
| M6 | You review PRs and lead refactors | A reasoned review hierarchy, mentoring through review, characterisation tests, typing ladder |
| M7 | JWT auth in FastAPI, DRF permissions, parameterised ORM queries | BOLA at scale (scoped querysets), mass assignment, SSRF to IMDS from Lambda or EC2 fetchers, algorithm confusion; naming OWASP IDs under pressure |
| M8 | `.env` files, Secrets Manager, Docker builds, Dependabot | Leak response order, hash pinning, transitive risk, Django `check --deploy` rationale |
| M9 | Voice agents and chatbots that call tools | Trust boundaries and STRIDE on those systems; X1 obligations for customer voice and chat data |
| M10 | Latency dashboards, Locust or ab once | Percentiles vs averages, coordinated omission, Little's Law, attributing bottlenecks in an async plus DB service |

### 8.5 Senior-bar framing (mid vs senior vs staff)

| Module | Mid-level answer | Senior answer | Staff answer |
|---|---|---|---|
| M1 | Pyramid recited with percentages | Risk-driven mix, failure modes, CI budget, flaky policy, what is not tested | Org-wide strategy, ownership of test infrastructure, cost per signal, platform paved roads and metrics |
| M2 | Uses fixtures and parametrize | Scope and teardown reasoning, deterministic and parallel suites | Standards, shared plugins, suite-health SLOs across teams |
| M3 | Uses `patch` | Right double per boundary, autospec, fakes, contracts for service edges | Contract governance across many teams, deploy gating, schema-evolution policy |
| M4 | Uses TestClient with SQLite | Real Postgres, isolation trade-offs, failure-path and idempotency tests | Test-environment architecture and cost, ephemeral environments, reliability of the test platform |
| M5 | Quotes coverage % | Distinguishes executed from asserted, uses mutation and properties selectively | Defines quality KPIs and gates that teams trust, avoids metric gaming |
| M6 | Style comments in review | Prioritises correctness, design, security; refactors under tests; mentors | Review culture and tooling at scale, debt economics, coaching reviewers |
| M7 | "Use JWT and HTTPS" | Names classes (BOLA, BFLA, SSRF), shows fix plus regression test, authZ per object | Policy architecture, org-wide remediation order, secure-by-default libraries |
| M8 | Lists a checklist | Ordered leak response (revoke, rotate, clean), triage by reachability, hash pinning | Supply-chain policy, build provenance expectations, risk acceptance with expiry |
| M9 | Lists generic threats | DFD, boundaries, STRIDE, prioritised mitigations tied to tests, X1 obligations | Embeds threat modelling in delivery, security champions, risk register, regulator-facing evidence |
| M10 | Quotes average latency | Percentiles, correct test design, knee of the curve, SLO-linked pass/fail | Capacity planning with business growth, performance budgets in CI, cost-performance trade-offs |

### 8.6 Misconceptions, memorable visuals and analogies (one per module)

| Module | Visual or analogy | Misconception to correct (explicit remediation card) |
|---|---|---|
| M1 | "Smoke detector vs fire drill": cheap detectors everywhere, a few expensive drills | 100% coverage means well tested |
| M2 | Fixture scope as lifetimes on a timeline (function, module, session) | Session-scoped mutable fixtures are fine |
| M3 | "Patch the address book entry the caller reads, not the person's home" (namespace lookup diagram) | Patch where the object is defined |
| M4 | Each test wrapped in a transaction "undo button" | SQLite is close enough to Postgres |
| M5 | Mutation testing as "planting bugs to see if the guards notice" | High coverage means high quality |
| M6 | Review funnel: correctness first, nits last | Review is for style |
| M7 | Building access cards: BOLA is "the card opens every door on your floor plan because nobody checks the room number" | JWT means secure; internal service means no SSRF |
| M8 | Secret lifecycle as a water pipe: revoke the tap before cleaning the pipe | Deleting the commit fixes a leaked secret |
| M9 | Trust boundaries as border crossings on a DFD | The model will refuse malicious instructions; threat model once and done |
| M10 | Hockey-stick latency curve and the restaurant queue (Little's Law) | Averages tell the story; add workers to fix everything |

### 8.7 Adaptive hooks

| Hook | Rule |
|---|---|
| Mastery threshold | 80% per module; a miss triggers a revision loop on the weak competency tags only |
| Remediation links | M4 weak: A4 (isolation, transactions) and A2 (event loop). M3 weak: A5.M2 (contract-first OpenAPI). M7 weak: A5.M6 (auth protocols) and A5.M7 (design-time controls). M8 weak: A1.M9 (packaging, lockfiles). M10 weak: A1.M11 (profiling) and A2.M10 (async stalls). M9 X1 weak: B5.M9 for AI-governance context |
| Challenge extensions | Strong learner (>= 90 on diagnostics): mutation score target on a supplied module, exploit-chain lab, contract gating exercise, threat model of own past project, capacity note with Little's Law |
| Reorderable modules | Engine may reorder M1-M6 freely; M7 after M2 and before M8; M9 after M7; M10 any time after M4. M7 should be unlocked early for hybrid and backend routes (MUST). M9.4 (X1) can be taken stand-alone via stable ID by the AI route |
| Time-pressure mode | If interview date is under 3 weeks: M7, M3, M1, M9 first; M2, M5 test-out only |
| Route position | Backend route: weeks 9-13 after A5 (before A7). Hybrid route: weeks 11-16 after B4 |

### 8.8 Applied labs from real-world work

| Module | Applied lab (production-framed) |
|---|---|
| M1 | Strategy for an IoT telemetry ingestion service (MQTT to queue to Postgres) including what is not tested and the monitoring compensation |
| M2 | Make a 6-minute SaaS billing test suite order-independent and 3x faster with scopes and xdist |
| M3 | Replace over-mocked tests of an LLM-API client with a fake plus recorded-contract check; consumer contract for a voice-agent backend calling a CRM service |
| M4 | Race and idempotency tests for a payment webhook handler on real Postgres; test a Lambda-style handler with fault injection |
| M5 | Kill surviving mutants in a rate limiter and money-split module; Hypothesis finds a seeded rounding bug |
| M6 | Review a seeded PR for an enterprise export endpoint (logic bug, BOLA, flaky test, missing migration safeguard) |
| M7 | Exploit-and-fix on the Lab Service: tenant-scoped SaaS API with BOLA, SSRF via a webhook-URL feature, SQL injection, mass assignment (static-analysis fallback if S-6 fails) |
| M8 | Harden a Django plus FastAPI settings pair; find and rotate a leaked cloud key in git history; triage a pip-audit report |
| M9 | Threat model an AI voice assistant with tools and RAG (canvas) and a customer-data service with a retention and deletion design note (X1) |
| M10 | Load test an AI chatbot backend with streaming; capacity note with p95, saturation point and the bottleneck found |

### 8.9 Interview rehearsal link and readiness criteria

| Module | Interview Master modes | Readiness criterion (observable) |
|---|---|---|
| M1 | Voice drill (strategy), project deep-dive | Voice answer to a strategy question scores >= 3.5/5 on the rubric with failure modes and a "not tested" statement; mastery >= 80% |
| M2 | Coding screen (pytest tasks) | Timed 25-minute pytest refactor passes hidden tests; mastery >= 80% |
| M3 | Voice drill, coding, design canvas | Patch-target and contract questions answered without hints; contract pair passes provider verification |
| M4 | Coding, voice | Async plus DB integration task done in 40 min with failure path; mastery >= 80% |
| M5 | Voice drill | Explains coverage vs mutation vs properties in under 2 minutes with an example; kills seeded mutants |
| M6 | Behavioural, voice, coding (C3.M8 link) | Review exercise recall >= 70% of planted issues with correct priority; tone rubric pass |
| M7 | Voice drill, coding, project deep-dive | 10 lab flaws fixed with hidden exploit suite clean; names OWASP API classes with fix and test in a mock round (>= 4/5 on strict mode) |
| M8 | Voice drill, coding | Leaked-secret exercise in correct order; hardening checks pass; explains CSRF vs CORS cleanly |
| M9 | Design canvas, voice drill | Canvas threat model at rubric >= 3.5 (no dimension below 3) on boundaries, mitigations and prioritisation; X1 scenario questions >= 85% on retake |
| M10 | Design canvas, voice drill | Interprets a latency graph and names the bottleneck; capacity note passes rubric |

Course readiness ("ready for interviews on A8 topics") follows G-6: for each A8 tag, module mastery check >= 85% on a retake at least 7 days after first pass (the 80% figures in the table are the badge threshold), the module's timed task passed, strict-mode voice drills at the Senior bar (average >= 3.5, no competency < 3) in two sessions at least 48 hours apart, and no open remediation flag on prerequisites. Course-level additions: all MUST/SHOULD modules for the chosen route cleared, the final timed practical passed, and the review exercise >= 70%. For the AI route, readiness is X1 passed (>= 85% on the X1 scenario items) plus the M9 canvas task at rubric 3.5 with no dimension below 3.

### 8.10 Senior-bar items supplied to other courses

- C3.M8 receives `Q` items from M6: three review-prioritisation scenarios, a characterisation-test scenario and a SOLID-without-dogma discussion prompt (tags `eng.code-review`, `eng.code-quality`).
- B5 receives pointers from M9 and B2.M10 pointers remain at basics level; no prompt-injection depth is authored here.
- Seed Q9.2 stays as a pointer-level question: graders accept an answer that states the principles above and routes depth to B5; deeper probes are tagged to B5.
- Tag normalisation applied (G-9): all A8 tags are `<domain>.<topic>` with hyphens inside the topic (for example `eng.test-strategy`, `sec.supply-chain`, `perf.load-testing`); underscores and dotted-to-hyphen forms such as `eng-test-strategy` are invalid.

## 9. Boss compliance log

| Decision | Applied |
|---|---|
| G-1, O-46, O-53, O-57 to O-64 | Ownership stated in the header and per module; pointers one-way (A5.M2, A5.M6, A5.M7, A7.M2, A7.M8, A1.M11, A2.M10, A7.M9, B5.M4-M6, B4.M9); single `ref` item Q9.2 |
| G-2 | 27 S / 17.5 D plus 3.0 h assessment; module hours sum exactly (section 8.1); semantics sentence in the header |
| G-3, G-11 | Goal tags in 8.1, weights and diagnostics in 8.1a, LF mapping stated at the start of section 8 |
| G-4 to G-7 | Level paths 8.3, bands 8.2 (85/70/50), readiness 8.9 (G-6), thresholds 80 module and 85 readiness, Senior-bar voice, canvas 3.5 |
| G-8, G-9, G-10 | Python 3.14 image; hyphenated owned tags (including `sec.compliance`; `llm.security` secondary); goal tags and senior-bar notes; seed ID and difficulty mapping |
| G-12, G-13 | Dated October 2026, volatile content flagged; X1 owned at A8.M9.4 with disclaimer |
| R-A8-1 to R-A8-5 | M7 owns OWASP depth, A5.M6-M7 pointers; M8 tool concepts only; Lab Service gated on S-6 with fallback; LLM security pointer <= 45 min; M10 stays here |
| Section 8 items 1-10 | Tags, M3 contract ownership, M7 labs, X1 lesson, M8 scope, M6 Q items for C3.M8, M10 profiler scope, hyphen normalisation, senior-bar angle in M1 and M6, S-6 gate |

- Track A lead review (2026-10-05, TRACK-LEAD-A): full review; outcomes reduced to 8 and sec.compliance credited, LF-2 weights and diagnostics added (G-11), readiness aligned to G-6, hyphen-tag error in 8.10 fixed, Boss compliance log added, seed-label and difficulty mapping stated. Verdict: Approve with conditions (S-6 Lab Service review, legal read of X1). See _reviews/TrackLead_A_Report.md.
