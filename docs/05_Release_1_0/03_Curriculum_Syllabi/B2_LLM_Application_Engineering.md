---
name: B2 — LLM Application Engineering
course_id: B2
track: B (AI Engineer)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
facts_as_of: October 2026
review_cadence: every 3 months (fast-moving AI content)
volatility_note: Provider features, model names, limits and prices change quickly. Lessons teach durable concepts in tier language (no model names); dated "Provider Notes" sidebars (template X4) carry parameter names, limits and prices, and prices come from a dated data file.
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-13 (see _reviews/Boss_Final_Approval.md)
---

# B2 — LLM Application Engineering (Track B)

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | B2 — LLM Application Engineering / Track B (AI Engineer Mastery) |
| Audience | Senior Python backend engineers moving into AI Engineer roles (L3-L4) who already call Claude/OpenAI APIs and ship chatbots or voice agents; L1-L2 learners follow the Simpler path. The course closes the gap between *building* an LLM feature and *explaining and defending* it at senior depth (token economics, determinism, caching, failure modes, structured-output limits) |
| Prerequisites | A2 (Concurrency and Async) is required (solid edge A2 to B2 in the boss graph; a placement pass waives it), plus Python backend skills (FastAPI, Pydantic). B1 is recommended (dotted edge): tokens, embeddings and transformer intuition help (tokenisation mechanics are owned by B1.M8) |
| Placement skip | Per-module bands (section 8, LF-2): >= 85 skip lessons and take the mastery check; 70-84 skim; 50-69 study; < 50 deep-study. Reference learner is expected to skip/skim M1 and M3. M2, M5, M9, M10 (fast-changing, senior-probed) are never skipped without passing the module mastery check. No band waives the final assessment; whole-course test-out needs final mastery >= 85% plus voice mock at Senior bar (G-5) |
| Outcomes (each tied to a competency tag owned by B2) | 1. Explain token, context, sampling and determinism behaviour and estimate cost/latency for a scenario (`llm.fundamentals`). 2. Choose between standard and reasoning models, set effort per request and justify with latency, cost and quality (`llm.model-selection`). 3. Design, version and regression-test prompts, and produce validated structured output with schema-constrained decoding plus a validation/repair fallback (`llm.prompting`, `llm.structured`). 4. Design and harden a single-turn/short tool-calling loop (`llm.tools`). 5. Extract fields from images/PDFs with validation and cost control (`llm.multimodal`). 6. Manage context budgets, memory hooks and compaction (`llm.context`). 7. Build streaming endpoints with cancellation and cut cost/latency with caching, batching, routing and fallbacks (`llm.production`). 8. Apply security/PII/eval basics and design an LLM feature architecture with a client-side gateway concept (`llm.safety-basics`, `llm.architecture`) |
| Secondary tags (no credit) | `llm.security` (B5), `rag.architecture` (B3), `agents.protocols-mcp` (B4), `llm.model-routing` (B7), `llm.eval-design` (B5) |
| Estimated effort | **16 h Simpler / 11.5 h Deeper**, plus 2.5 h assessment allowance (boss effort table, R-B2-5). Simpler = L1-L2 full-scaffolding route; Deeper = L3-L4 route with foundations skimmed per placement, hours stated net of typical skip-ahead, so Deeper is shorter in wall-clock time but harder per hour. Module hours sum to the header (section 3) |
| Interview coverage | Interview Master tags above; feeds AI Engineer, Python+AI hybrid and (core M1, M4, M5, M9, M10) Senior Python Backend loops |
| Badge and certificate | Course badge "LLM Application Engineer": all 12 module badges, final mastery >= 80% (G-7), tool-calling service >= 70% hidden tests with stated complexity, canvas design rubric >= 3.5 with no dimension < 3, voice mock "Meets" (avg >= 3.0, none < 2). Certificate lists evidence (score, repo task result, design report). Interview readiness is a separate G-6 test (section 8) |
| Authoring owner / reviewer | SME agent B2; reviewer = to be assigned |
| Review cadence | Every 3 months; Provider Notes re-verified each cycle (section 6) |
| Code and test policy | All code uses the thin in-house `LLMClient` protocol (R-B2-1): one implementation in messages-style, one in responses-style, same interface (`generate`, `stream`, `count_tokens`, `usage`). Graded tasks run on recorded fixtures through the LLM Lab Harness (P-07); optional live mode via AI Gateway with per-learner caps never gates badges (R-B2-4, P-23). No bring-your-own keys. A LiteLLM-style library is named once, in an M12 comparison sidebar |

### Scope boundaries (ownership, G-1)

| Topic | B2 holds | Owner / pointer |
|---|---|---|
| Retrieval, chunking, vector DBs, RAG vs long-context decision | Context-budget management only; one pointer slide | B3 (decision tree at B3.M1) |
| Prompt vs RAG vs fine-tune framework | Pointer slide | B7.M1 |
| Agent loops, planning, MCP and tool-design depth | Function-calling mechanics (single turn, short loop) | B4.M2 (tool design, MCP), B4 loops |
| Eval methodology, judges, red-teaming, OWASP LLM Top 10, guardrails, PII depth | Basics <= 1 h each (M10, M11) | B5.M1-M2, M4-M6 |
| Voice pipelines, barge-in, latency budgets | Streaming fundamentals, one case study line | B6 |
| Model routing, open-weight vs hosted, fine-tuning | Per-request model/effort choice | B7.M6 |
| Rate-limit algorithms, retry/backoff/jitter implementation | Provider-specific 429 handling, token-metered limits | A5.M4 (X2) |
| FastAPI SSE/WebSocket server mechanics | Provider stream parsing, usage events, billing on cancel | A2.M7 |
| LLM gateway system design | Client-side gateway concept | A6 canvas task #16 |

## 2. Module table

| # | Module | Outcome | Key topics | Visuals | Practice | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | How LLMs behave | Estimate tokens, cost and latency and explain non-determinism | Token accounting in application cost (input, cached, output, reasoning tokens; tokenisation mechanics pointer B1.M8), context window = input + output, prefill vs decode (TTFT vs tokens/sec), sampling (temperature, top-p, top-k; some reasoning and newer models fix, ignore or reject sampling parameters, so check the Provider Notes before relying on them), why temperature 0 is not deterministic, hallucination causes | Token flow; prefill vs decode timeline. Analogy: a taxi meter with a different rate for each leg of the trip | Estimate tokens/cost/latency for 3 scenarios using the provider counter (fixtures) | "Temperature 0 = deterministic"; "context window is only input"; ignoring output price asymmetry |
| M2 | Model landscape and reasoning models | Choose model class and effort per request | Standard vs reasoning models, effort levels or thinking budgets, reasoning tokens billed as output, hidden vs summarised reasoning, reasoning state across tool turns, tiers (small/fast vs frontier), messages-style vs responses-style APIs, capability matrix, aliases vs pinned versions and deprecation playbook. Routing/open-weight vs hosted: pointer to B7.M6 | Model-selection decision tree; latency vs quality curve. Analogy: a hospital triage desk (GP, specialist, surgeon) | Run one task across tiers and effort levels on fixtures; cost/latency/quality table | "Always the biggest model"; long CoT prompts to reasoning models; hard-coded model names |
| M3 | Prompting patterns | Refactor brittle prompts and version them | Roles and instruction hierarchy, few-shot, decomposition and chaining, format control, CoT vs reasoning models, delimiters, XML/markdown sections, prompt templates and git versioning, prompt regression | Prompt anatomy; chain-of-prompts flow. Analogy: a prompt is an API contract, not a wish | Refactor a brittle prompt; add a versioned prompt registry | Pile of "NEVER" rules; no versioning; changes without a regression set |
| M4 | Structured outputs | Get reliably valid, typed output | Output-control ladder (prompt, JSON mode, schema-constrained decoding, strict tool schemas), schema subset limits, Pydantic validation, retry with error feedback, refusals and truncation breaking JSON, syntax vs semantic validity, streaming structured output | Validate-retry loop; decoding-constraint diagram. Analogy: guard rails on a road (keeps you on the road, not going to the right town) | Messy text to a Pydantic model with fallback path | "JSON mode guarantees schema"; no truncation/refusal handling; valid JSON trusted as correct |
| M5 | Tool use / function calling | Design a safe single-turn/short tool loop | Tool schema basics, tool-choice modes, parallel calls, result formatting, errors returned to model, idempotency and side-effect classes, timeouts, iteration cap, hosted vs own tools, reasoning state across tool turns. Tool design depth and MCP: pointer to B4.M2 | Tool-loop sequence diagram. Analogy: the model is a manager writing requests on slips; your code is the clerk who decides what gets executed | 3-tool assistant with one write tool behind confirmation | Vague descriptions; non-idempotent tools retried; no loop cap; secrets passed through model |
| M6 | Multimodal inputs | Handle images, documents and audio inputs correctly | Image/PDF inputs, token cost of media, resolution trade-offs, upload vs inline base64, native audio vs STT (pointer B6), vision vs OCR pipelines, privacy, failure modes (small text, rotated scans) | Multimodal request anatomy. Analogy: a photocopier that charges by area | Invoice extraction (image + PDF) to structured output | Vision assumed free; unvalidated numbers; unredacted PII images |
| M7 | Context and memory management | Manage context budgets, compaction and memory hooks | Conversation state storage, budget per section, truncation, rolling summarisation, sliding window, pinned facts, memory-store hooks, provider context-editing/compaction (verified 2026-10: Anthropic context editing and server-side compaction, both beta; OpenAI Responses compaction endpoint), context failure modes (lost-in-the-middle, poisoning). Pointers: RAG vs long-context to B3.M1; prompt vs RAG vs fine-tune to B7.M1 | Context budget bar. Analogy: a desk of fixed size, deciding what stays on it and what goes to the filing cabinet | Context management for a support chatbot with a summary-fidelity test | Summaries dropping constraints; unbounded history; "big window means no memory design" |
| M8 | Streaming and API plumbing | Ship a robust provider-streaming endpoint | Stream event types (text deltas, tool-call deltas, usage events), partial JSON, cancellation and billing on disconnect, backpressure, proxy buffering, streaming with tools, mid-stream errors and retries. Server SSE/WebSocket mechanics: A2.M7 | Streaming timeline; SSE event anatomy. Analogy: a tap you can turn off; the meter runs until you do | FastAPI streaming endpoint with cancel and usage logging | Proxy buffering; ignoring disconnects (tokens still billed); naive partial JSON parse |
| M9 | Cost, latency, caching and fallbacks | Cut cost/latency with measured changes | Cost model and template X3, prompt caching (prefix layout, TTL, minimums, read/write pricing), response/semantic cache risks, batch APIs, cascades, token optimisation, provider rate limits and token-metered limits, 429 handling (retry mechanics: X2 in A5.M4), provider fallback and degraded modes, Provider Notes template X4 | Cost/latency trade-off chart; cache-prefix layout. Analogy: a loyalty card that only counts if the stamps are in the same order | Reduce a pipeline's cost by 40% with before/after measurement | Changing prefix; semantic cache cross-tenant leakage; retry without idempotency |
| M10 | Reliability and safety basics | Harden an LLM endpoint at basic level | Timeouts and first-token budgets, output validation, injection awareness (direct/indirect), PII hygiene and retention, budget guardrails and quotas. Attack taxonomy, red-team, guardrails, OWASP LLM Top 10: B5.M4-M6 | Failure-mode map; trust-boundary diagram. Analogy: an airport, every bag is untrusted until scanned | Harden an endpoint; 5 injection test cases | "System prompt prevents injection"; raw PII in logs; broad tool credentials |
| M11 | Observability and eval basics | Instrument calls and build a mini eval | Trace fields per call (model, tokens, latency, cost, cache hits, prompt version), safe logging, golden set of 20-50 cases, before/after run. Eval methodology and judges: B5 | Trace diagram. Analogy: a flight recorder | Add tracing; mini eval and one regression run | Unredacted logging; no baseline before prompt change |
| M12 | Feature architecture patterns | Design an LLM feature end-to-end | Client-side gateway concept (provider abstraction and its limits, config-driven routing), async job and queue patterns, multi-tenant budgets/quotas, secrets, voice feature at architecture level. Gateway system design: A6 #16 | Reference architecture. Analogy: a switchboard with a billing meter | Design a multi-tenant voice-support assistant on the canvas | Lowest-common-denominator abstraction; no per-tenant cost control |
| | **Ownership notes** | | **OWNS:** token accounting in app cost (M1), reasoning-model behaviour and per-request model/effort (M2), structured outputs and single-turn tool mechanics (M4-M5), context-window management (M7), provider streaming, 429 and token-metered limits, caching, batching, fallbacks, cost template X3 and Provider Notes X4 (M8-M9). **POINTS:** tokenisation mechanics -> B1.M8; RAG vs long-context -> B3.M1; prompt vs RAG vs fine-tune -> B7.M1; MCP and tool design -> B4.M2; OWASP LLM Top 10, red-team, PII depth, guardrails -> B5.M4-M6; eval methodology -> B5.M1-M2; routing and open-weight -> B7.M6; retry/jitter -> A5.M4; SSE server -> A2.M7; voice -> B6; gateway design -> A6 #16 | | | | |

## 3. Per-module detail

Question-bank target per module (template section 3, IDs `Q-B2-Mn-k`): 12 MCQ, 6 code-output, 4 coding, 8 verbal (3 seeded below), 3 scenario; 12 sit in the owner bank (no re-authoring in pointer lessons). Mastery check: 6-10 items, 80% pass, revision loop on misses. Each lesson ships a dated Provider Notes sidebar and a "last reviewed" stamp. Hours are Simpler / Deeper. Every module also ships one annotated worked example (the section 2 practice task solved on fixtures through `LLMClient`) and alt text for each visual (template section 2).

| Module (S/D h) | Lessons (Simpler / Deeper notes) | Mastery focus | Module badge criteria |
|---|---|---|---|
| M1 (1.25/0.75) | .1 Tokens in application cost (S: counting by tool / D: language effects incl. Indic text; mechanics pointer B1.M8). .2 Context window and token accounting (S: input+output / D: cached and reasoning tokens). .3 Sampling and determinism (S: temperature / D: top-p, seeds, batch nondeterminism). .4 Latency anatomy (S: TTFT / D: prefill vs decode, KV cache intuition). .5 Why models hallucinate (S: causes / D: mitigation menu) | Cost estimation, determinism claims | Mastery >= 80% + cost estimate within 20% |
| M2 (1.5/1.0) | .1 Tiers and selection criteria (S: tiers / D: eval-driven choice). .2 Reasoning models (S: what/when / D: effort, budget, reasoning tokens, state across tool turns). .3 Capability matrix and API styles (S: reading docs / D: portability pitfalls). .4 Lifecycle (S: aliases vs pinned / D: deprecation playbook). Pointer: routing -> B7.M6 | When reasoning pays off; no hard-coded names | Mastery + comparison table with measured numbers |
| M3 (1.25/0.75) | .1 Anatomy and roles. .2 Few-shot and format control (D: example selection). .3 Decomposition and chaining. .4 CoT vs reasoning models. .5 Versioning and regression | Refactoring, role hierarchy | Mastery + refactored prompt beats baseline on fixed set |
| M4 (1.5/1.25) | .1 Output-control ladder. .2 Pydantic schemas and validation. .3 Retry and repair (D: error-feedback prompts). .4 Edge cases: refusals, truncation, schema limits. .5 Streaming structured output | Guarantees vs limits | Mastery + extraction >= 95% schema-valid on hidden set |
| M5 (1.5/1.0) | .1 Tool schema basics. .2 The tool loop. .3 Parallel calls, tool-choice, reasoning state across turns. .4 Errors, idempotency, confirmations. .5 Hosted tools (pointer B4.M2) | Loop correctness, safety | Mastery + 3-tool assistant passes hidden tests with failure injection |
| M6 (1.0/0.75) | .1 Image/PDF mechanics. .2 Token and cost behaviour of media. .3 Extraction pipeline and validation. .4 Audio input options (pointer B6) | Cost, validation | Mastery + invoice extraction passes |
| M7 (1.5/1.0) | .1 Conversation state patterns. .2 Truncation and summarisation. .3 Memory hooks and long-term store design. .4 Compaction/context editing and failure modes. .5 Pointer slides: B3.M1, B7.M1 (preview, 20 min max) | Budget design, summary fidelity | Mastery + context-manager task keeps pinned constraints in tests |
| M8 (1.5/1.0) | .1 Stream event anatomy and usage. .2 Cancellation and backpressure. .3 Streaming with tools and structured output. .4 Proxy buffering and mid-stream failure | Disconnect handling | Mastery + endpoint passes hidden tests (cancel, usage logged) |
| M9 (1.75/1.5) | .1 Cost model and template X3. .2 Prompt caching design. .3 Batch and async. .4 Cascades and token optimisation. .5 Rate limits, 429, token-metered limits, fallbacks; Provider Notes template X4 | Measured optimisation | Mastery + measured >= 40% cost cut with quality held on eval |
| M10 (1.25/1.0) | .1 Timeouts and failure modes. .2 Output validation and injection awareness. .3 PII hygiene and retention. .4 Budget and quota guardrails; preview of B5 | Threat reasoning at basic level | Mastery (MCQ on attack classes, R-B5-1) + endpoint passes 5 injection tests |
| M11 (1.0/0.75) | .1 What to trace per call. .2 Safe logging, redaction and retention. .3 Golden set and one regression run (hand-off B5) | Instrumentation | Mastery + eval set with regression run committed |
| M12 (1.0/0.75) | .1 Client-side gateway concept. .2 Abstraction trade-offs. .3 Async/queue and multi-tenant budgets. .4 Voice feature at architecture level (hand-off B6) | Trade-off articulation | Mastery + canvas design rubric >= 3.5 |

## 4. Seed interview questions (3 per module; 36 total)

Notation: Diff = Med/Hard/Expert. Cluster tags: IT = IT-services, GC = global-IT-consulting, B4 = Big4, GP = global-product, IP = Indian-product, AI = AI-startup. Goal tag `B:x A:x H:x` = Senior Python Backend / AI Engineer / hybrid (M/S/N). Style-of-question items only, not from any named company. Each module also has 2 staff-level "senior-bar upgrade" probes.

### M1 How LLMs behave

**M1-Q1. Why can temperature 0 still give different outputs, and how does that affect your design?** Med; `llm.fundamentals`; IT, GP, AI; B:S A:M H:M
- Ideal: greedy decoding not guaranteed bitwise stable; floating-point non-associativity with batching/parallel kernels; provider-side infra/model changes; seed best-effort; design for tolerance (validation, threshold tests); cache where exactness needed; pin versions.
- Weak: "temperature 0 is deterministic"; "provider bug".
- Probes: How do you test non-deterministic features? When is determinism required (audit)? What does top-p add?
- Senior-bar: mid states cause; senior designs tests and caches; staff defines a determinism policy per feature class.

**M1-Q2. A request has 6k input and 800 output tokens at 1M requests a month. Estimate cost and latency.** Med; `llm.fundamentals`; B4, GC, IP; B:S A:M H:M
- Ideal: separate input/output (and cached/reasoning) rates; monthly totals; retries and tool loops multiply calls; latency = TTFT (grows with input) + output / decode speed; use provider counter; peak headroom and rate limits; label numbers as assumptions.
- Weak: word count only; ignores output price and retries.
- Probes: What changes with caching? A reasoning model? Hindi input?
- Senior-bar: mid gives a total; senior gives a range with sensitivity; staff adds budget alarms and unit economics per tenant.

**M1-Q3. What causes hallucinations and what do you do about them in production?** Hard; `llm.fundamentals`; AI, GP; B:S A:M H:M
- Ideal: plausibility objective; knowledge gaps/stale data; ambiguous prompts; context pollution; mitigations: grounding (B3), "I don't know" path, citations, constrained outputs, verification steps, evals (B5); design for detection and blast radius.
- Weak: "better prompt"; "temperature 0".
- Probes: Measure hallucination rate? When is citation checking insufficient? Which uses should not be LLM-only?
- Senior-bar: mid lists fixes; senior ties to risk tiers; staff sets acceptance thresholds and human-in-loop economics.

Staff probes (M1): (a) How would you define a determinism SLO for an audit feature? (b) Show how you would attribute a 30% bill jump to tokens by type.

### M2 Model landscape and reasoning models

**M2-Q1. When would you use a reasoning model over a standard one, and what does it cost?** Hard; `llm.model-selection`; AI, GP, GC; B:S A:M H:M
- Ideal: multi-step planning/maths/code/agentic decisions benefit; costs: reasoning tokens billed as output, higher TTFT, less verbosity control; effort/budget knob; simple extraction better on small tier; decide by eval on your task; route/cascade.
- Weak: "always better"; unaware reasoning tokens are billed.
- Probes: How set the budget? Streaming behaviour? Reasoning state across tool calls (provider-specific: preserved blocks/items must be passed back, see Provider Notes)?
- Senior-bar: mid knows trade-off; senior sets per-request effort with measured evidence; staff builds an effort policy tied to cost SLOs.

**M2-Q2. How do you avoid being locked to a model name that gets deprecated?** Med; `llm.model-selection`; IT, IP, B4; B:S A:M H:M
- Ideal: ids in config; pinned in prod, aliases in dev; capability registry; eval suite to qualify replacements; deprecation monitoring and runbook; canary via gateway.
- Weak: "update the string".
- Probes: What breaks when switching provider? How compare candidates fairly?
- Senior-bar: mid configures; senior has qualification suite; staff owns a lifecycle calendar and budget for migrations.

**M2-Q3. Design a model-selection policy for FAQ, document analysis and code generation.** Hard; `llm.model-selection`; AI, IP, GP; B:S A:M H:M
- Ideal: classify by difficulty and risk; cheap tier for FAQ with escalation; stronger tier for analysis; per-route evals and SLOs; fallbacks; telemetry; router must cost less than it saves. (Routing platforms and open-weight vs hosted: B7.M6.)
- Weak: single model; "ask the LLM which model" with no cost analysis.
- Probes: Evaluate the router? Silent regression of cheap tier? Tenant overrides?
- Senior-bar: mid picks tiers; senior measures per route; staff includes governance and rollback.

Staff probes (M2): (a) What is your evidence that effort level X is worth its cost, and who re-checks it each model release? (b) How do you keep reasoning state correct when you fail over providers mid-conversation?

### M3 Prompting patterns

**M3-Q1. How do system, developer and user messages differ, and what are the security implications?** Med; `llm.prompting`; GP, AI, IT; B:S A:M H:M
- Ideal: hierarchy sets precedence but is soft, not a security boundary; system holds policy/persona/format; user content untrusted; no secrets in prompts; delimit untrusted data; enforce critical rules in code.
- Weak: "system prompt cannot be overridden".
- Probes: Test hierarchy robustness? Where do retrieved documents sit?
- Senior-bar: mid describes roles; senior moves enforcement to code; staff defines prompt governance and review.

**M3-Q2. Your prompt works on demos but fails on 15% of production inputs. Fix it.** Hard; `llm.prompting`; AI, IP, GC; B:S A:M H:M
- Ideal: collect failures into labelled set; categorise; change one thing at a time; add examples; decompose; structured output/validation; measure before/after; version; avoid overfitting.
- Weak: adds instructions ad hoc; tests three examples.
- Probes: Test-set size? When stop prompting and change architecture?
- Senior-bar: mid iterates; senior has measured loop; staff builds failure-to-eval flywheel.

**M3-Q3. Is "think step by step" still good advice?** Hard; `llm.prompting`; AI, GP; B:S A:M H:M
- Ideal: helps standard models at token cost; reasoning models reason internally so heavy CoT may be redundant; prefer effort settings; ask for brief justification fields; shown reasoning is not guaranteed faithful.
- Weak: always yes/no.
- Probes: How test it? Exposing reasoning to users?
- Senior-bar: mid answers yes/no with caveat; senior tests it; staff discusses faithfulness and audit policy.

Staff probes (M3): (a) How do you review and roll back a prompt change like code, including who approves? (b) How do you stop a prompt registry becoming a dumping ground at 200 prompts?

### M4 Structured outputs

**M4-Q1. How do you make LLM output reliably structured?** Med; `llm.structured`; all; B:S A:M H:M
- Ideal: ladder: clear prompt, provider schema-constrained output or strict tool schema for syntax, Pydantic for semantics, bounded retry with error feedback, fallback/escalation; handle truncation and refusals; keep schemas simple; log failures into eval set.
- Weak: "ask for JSON and json.loads"; regex repair.
- Probes: What can constrained decoding not guarantee? Which schema features are unsupported per provider (see Provider Notes: subsets differ, e.g. numeric/string-length constraints and recursion, required-fields rules)? First-use schema compile latency?
- Senior-bar: mid uses a library; senior knows limits and failure paths; staff defines schema evolution and cross-provider portability tests.

**M4-Q2. The model returned valid JSON that is wrong. What now?** Hard; `llm.structured`; AI, B4, IP; B:S A:M H:M
- Ideal: syntax is not semantics; cross-field/business validation; source quotes; "unknown" enum; second-pass or deterministic checks; human review for low confidence; per-field metrics.
- Weak: retry until it passes.
- Probes: Detect silent errors at scale? Cost of verification?
- Senior-bar: mid adds validators; senior adds sampling audits; staff sets per-field risk and review budget.

**M4-Q3. Stream a structured response to a UI. How?** Hard; `llm.structured`; GP, IP; B:S A:M H:M
- Ideal: incremental partial-JSON parser; UI-critical fields first; or separate text and structured streams; validate on completion; mid-stream failure fallback.
- Weak: waits for full output; naive json.loads on chunks.
- Probes: Arrays of objects? Cancel?
- Senior-bar: mid streams text; senior parses incrementally; staff designs UX contract for partial and invalid states.

Staff probes (M4): (a) How would you version a schema used by 10 consumers and several prompt versions? (b) When does strict constrained decoding hurt quality, and how would you detect it?

### M5 Tool use

**M5-Q1. Walk through a tool-calling loop and what you guard against.** Med; `llm.tools`; all; B:S A:M H:M
- Ideal: send tools and messages; model emits call(s); app validates arguments, authorises, executes with timeout; returns result or error; loop until final text; guards: max iterations, validation, permissions, idempotency keys, output size limits, logging; model never executes code itself.
- Weak: "the model calls the function"; no loop cap.
- Probes: Parallel calls? 1 MB result? Tool errors?
- Senior-bar: mid describes loop; senior lists guards and failure injection; staff discusses audit trail and replay.

**M5-Q2. How do you handle a write tool that the model might call twice?** Hard; `llm.tools`; AI, GP, IT; B:S A:M H:M
- Ideal: classify read/write side effects; idempotency keys derived from conversation and call id; confirmation for irreversible actions; dedupe store; retry semantics distinct from model re-asks; return prior result on duplicate; log. (Tool-design principles and MCP: B4.M2.)
- Weak: "tell the model not to repeat".
- Probes: Partial failure? Timeout after side effect? Two parallel duplicate calls?
- Senior-bar: mid adds a flag; senior builds idempotency with transactional store; staff reasons about exactly-once illusions and reconciliation.

**M5-Q3. What happens to reasoning state when the model calls tools, and what breaks if you drop it?** Hard; `llm.tools`; AI, GP; B:S A:M H:M
- Ideal: reasoning models may emit reasoning blocks/items that the next request must carry back (replay blocks, or continuation by response id, per provider); dropping it degrades quality or errors; storage and privacy of encrypted/summarised reasoning; cost in context tokens; adapter must hide this behind `LLMClient`. Details dated in Provider Notes.
- Weak: unaware reasoning exists between calls.
- Probes: Fallback to another provider mid-loop? Logging reasoning?
- Senior-bar: mid unaware; senior preserves state and tests it; staff designs the adapter contract and fixtures.

Staff probes (M5): (a) How do you measure whether your tool loop's cost grows linearly or quadratically with steps? (b) How do you prove to a security reviewer that the model cannot reach an unauthorised tool?

### M6 Multimodal inputs

**M6-Q1. Build invoice field extraction from photos and PDFs. Design it.** Hard; `llm.multimodal`; IT, GC, B4, IP; B:N A:S H:S
- Ideal: native PDF/image vs OCR+LLM by cost/accuracy; preprocess; schema-constrained output with per-field source/confidence; validate arithmetic and formats; human review for low confidence; PII handling; eval set from real docs; cost per page.
- Weak: send image, trust output.
- Probes: Multi-page? Handwriting? Measuring accuracy?
- Senior-bar: mid wires it; senior validates and prices; staff sets review SLAs and drift monitoring.

**M6-Q2. How are images and pages billed, and how do you control it?** Med; `llm.multimodal`; AI, IP; B:N A:S H:S
- Ideal: media converted to tokens by size/tiling or per-page rules (provider-specific; verified 2026-10: Claude images cost ceil(w/28) x ceil(h/28) tokens within a cap of 1,568 tokens (standard tier) or 4,784 (high-resolution tier, newest model generations only), PDF pages are rasterised and billed as images; OpenAI tile models charge base 85 + 170 per 512 px tile at high detail and base only at low detail, patch models use 32 px patches; always measure with the provider counter); downscale; crop; cache repeated media; batch; count before sending.
- Weak: "images are free".
- Probes: Quality loss from downscaling? Upload vs inline?
- Senior-bar: mid knows it costs; senior measures per resolution; staff adds per-tenant media budgets.

**M6-Q3. Native audio input versus STT then LLM: trade-offs?** Hard; `llm.multimodal`; AI, GP; B:N A:S H:S
- Ideal: native keeps prosody and removes a hop but costlier, less controllable transcript and tooling; STT gives transcript for logging/redaction, swappable providers, hotwords; latency and streaming differ; compliance; hand-off to B6.
- Weak: "always native".
- Probes: Where redact PII? Indian-English accuracy?
- Senior-bar: mid picks one; senior compares with measurements; staff includes compliance and vendor risk.

Staff probes (M6): (a) How would you build a per-field accuracy dashboard for extraction in production? (b) What is your policy when the model's extracted total disagrees with line items?

### M7 Context and memory management

**M7-Q1. How do you manage context in a long conversation?** Med; `llm.context`; all; B:S A:M H:M
- Ideal: token budget per section (system, memory, history, retrieved, output reserve); sliding window plus rolling summary; pinned facts; summarise tool outputs; full history stored outside prompt; test that summaries keep constraints; provider compaction/context-editing features (verified 2026-10: Anthropic `clear_tool_uses_20250919` context edit via beta header `context-management-2025-06-27` and server-side compaction `compact_20260112` via beta header `compact-2026-01-12`; OpenAI Responses compaction endpoint; beta status changes, so keep in Provider Notes).
- Weak: "send full history"; "truncate oldest".
- Probes: Summary drift? Multi-user thread? Long-term memory contents?
- Senior-bar: mid truncates; senior budgets and tests; staff designs cost and privacy of memory.

**M7-Q2. Design long-term memory hooks for an assistant.** Hard; `llm.context`; AI, GP; B:S A:M H:M
- Ideal: what to store (facts, preferences, episodic summaries); extraction with confidence; write policy and dedupe; retrieval by relevance and recency (retrieval depth: B3); user visibility and deletion; contradiction handling; expiry; poisoning risks; evaluate recall and harm.
- Weak: "store everything in a vector DB".
- Probes: GDPR/DPDP deletion? Poisoning via injected content?
- Senior-bar: mid stores; senior governs writes; staff handles consent, audit and cross-tenant isolation.

**M7-Q3. The summary dropped a key constraint and the assistant misbehaved. Root cause and fix?** Hard; `llm.context`; AI, IP, GC; B:S A:M H:M
- Ideal: summaries are lossy; pin constraints outside summary; structured summary with required slots; fidelity tests that replay conversations and check constraint retention; summarise by section; keep recent verbatim turns; monitor drift across repeated summarisation.
- Weak: "use a longer summary".
- Probes: Compare summarisation vs compaction features? Cost of summarising each turn?
- Senior-bar: mid patches prompt; senior builds fidelity tests; staff creates metrics for constraint loss.
- Pointer note: the long-context vs RAG decision is owned by B3.M1 and prompt vs RAG vs fine-tune by B7.M1 (one `ref` item allowed from each).

Staff probes (M7): (a) How do you cap the cost of context for a 200-turn session without hurting quality? (b) Describe a memory deletion flow that also covers derived summaries and caches.

### M8 Streaming and API plumbing

**M8-Q1. Your streaming works locally but arrives in one burst in production. Why?** Med; `llm.production`; IP, GP, IT; B:S A:M H:M
- Ideal: proxy/CDN/load balancer buffering; disable buffering headers; compression; idle timeouts; heartbeat events; HTTP/2 limits; test through real ingress. (Server SSE mechanics: A2.M7.)
- Weak: "WebSocket is always better".
- Probes: Resume after disconnect? Mobile networks?
- Senior-bar: mid guesses; senior diagnoses by layer; staff adds synthetic streaming probes to monitoring.

**M8-Q2. The user closes the tab mid-generation. What should happen?** Hard; `llm.production`; GP, AI; B:S A:M H:M
- Ideal: detect disconnect; cancel upstream request to stop spend; record partial usage; clean up; persist partial message if product requires; background mode only deliberately; async cancellation handling.
- Weak: "nothing, it continues".
- Probes: Usage on cancelled streams (provider usage events may not arrive; estimate from counter)? Backpressure on slow clients?
- Senior-bar: mid cancels; senior accounts for billing; staff designs reconciliation against provider invoices.

**M8-Q3. How does streaming interact with tool calls and validation?** Hard; `llm.production`; AI; B:S A:M H:M
- Ideal: tool-call arguments arrive as deltas, accumulate until complete before executing; text and tool blocks interleave; usage in final event; guardrails on partial text have limits; mid-stream error handling; UX during tool pauses.
- Weak: executes tools on partial args.
- Probes: Moderation on streamed text? Parallel tool deltas?
- Senior-bar: mid streams text; senior assembles tool deltas; staff designs failure UX and retry boundaries.

Staff probes (M8): (a) What do you do when a stream fails at token 900 of 1,000: retry, resume or fail, and who pays? (b) How would you load-test 5,000 concurrent streams and what breaks first?

### M9 Cost, latency, caching and fallbacks

**M9-Q1. How would you cut cost and latency of an LLM pipeline?** Med; `llm.production`; all; B:S A:M H:M
- Ideal: measure first (per-step tokens, latency, cost); shrink prompts; prompt caching with stable prefix; smaller tiers; batch for non-interactive; cap output; parallelise independent steps; exact response cache; stream for perceived latency; evals after each change.
- Weak: "cheaper model" only.
- Probes: Cache hit rate tracking? Regression detection? Reasoning effort trade-off?
- Senior-bar: mid lists levers; senior orders by measured ROI; staff owns unit economics per tenant.

**M9-Q2. Explain prompt caching and how to structure prompts for it.** Hard; `llm.production`; AI, GP; B:S A:M H:M
- Ideal: provider reuses computation for identical prefixes; static content (tools, system, examples, documents) first, variable last; automatic vs explicit breakpoints; TTL and minimum prefix size vary by provider and tier; reads cheap, writes may cost extra (see Provider Notes, dated Oct 2026); any prefix change invalidates; tenant isolation; monitor cache-read tokens.
- Weak: confuses with response caching; timestamp at top.
- Probes: Why low hit rate? Does changing tools or effort invalidate? Semantic cache risk?
- Senior-bar: mid places static first; senior computes break-even of write premium; staff designs prefix governance across teams.

**M9-Q3. The provider returns 429s and then goes down for 20 minutes. What happens in your system?** Hard; `llm.production`; GC, IP, B4, GP; B:S A:M H:M
- Ideal: honour retry-after; backoff with jitter (X2: A5.M4), bounded retries and retry budget; client-side limiter/queue with token-metered limits (tokens per minute, not only requests); circuit breaker; fallback via adapter with capability mapping and schema compatibility tests; degrade gracefully; idempotency; alerting.
- Weak: infinite retry; single provider.
- Probes: Fallback quality differences? Mid-stream failure? Batch vs interactive priorities?
- Senior-bar: mid retries; senior adds budgets and fallback tests; staff defines degradation tiers and error-budget policy.

Staff probes (M9): (a) Your cache hit rate dropped from 70% to 20% after a release: walk through diagnosis. (b) How do you decide between batch discount and interactive latency for a nightly pipeline that sometimes needs same-day results?

### M10 Reliability and safety basics

**M10-Q1. Which basic controls do you put in front of an LLM feature before launch, given untrusted user content?** Med; `llm.safety-basics`; all; B:S A:M H:M
- Ideal: pre-launch basics only: treat external content as untrusted data; least-privilege tools and no secrets in prompts; validate and constrain outputs (schema, allow-listed actions); human confirmation for sensitive actions; a small set of injection test cases in CI; no complete prevention exists. Attack classes, taxonomy and red-teaming are owned by B5.M4 (this item is MCQ-assessed on attack classes only, R-B5-1).
- Weak: "tell the model to ignore malicious instructions".
- Probes: Which control would you add first? How do you test it? (Depth: B5.M4.)
- Senior-bar: mid lists controls; senior places them at trust boundaries; staff sets residual-risk acceptance.

**M10-Q2. What PII and data controls do you put around LLM calls?** Hard; `llm.safety-basics`; B4, IT, GC; B:S A:M H:M
- Ideal: classify data; redact/tokenise before sending; provider contract and data-use/zero-retention options (verified 2026-10: Anthropic commercial API data is deleted within 30 days by default and not used for training by default, zero data retention by per-organisation approval; OpenAI abuse-monitoring logs kept up to 30 days by default, Zero Data Retention or Modified Abuse Monitoring by prior approval and per-endpoint eligibility, data residency regions beyond the US need approval plus a Modified Retention amendment; contract wording needs human review `[verify: legal/privacy reviewer compares the provider DPA and ZDR addendum text against this summary before publish]`); region and residency; log redaction and retention limits; trace access control; consent and deletion; vendor review (compliance awareness: X1 in A8.M9.4).
- Weak: "providers do not store anything".
- Probes: DPDP/GDPR implications? Redaction vs utility?
- Senior-bar: mid redacts; senior reviews contracts and logs; staff builds a data-flow register.

**M10-Q3. Design timeouts and circuit breaking for an LLM endpoint with p50 4 s and p99 60 s.** Hard; `llm.production`; GP, IP; B:S A:M H:M
- Ideal: separate connect/first-token/total timeouts; per-route budgets; cap output tokens; shed load with queues and 429/503; circuit breaker on error rate; async job with polling/webhook for long tasks; SLOs and alerts.
- Weak: one global 30 s timeout.
- Probes: Retrying a slow request? UX while waiting?
- Senior-bar: mid sets timeouts; senior budgets by stage; staff ties to SLO and cost.

Staff probes (M10): (a) How do you set per-tenant spend guardrails so an injected loop cannot burn the monthly budget? (b) What residual risk do you document and who signs it off?

### M11 Observability and eval basics

**M11-Q1. What minimal trace record do you write for every LLM call?** Med; `llm.production`; all; B:N A:S H:S
- Ideal: the minimal record only: request id, tenant, prompt template and version, model and params, input/output/cached/reasoning tokens, latency (TTFT and total), cost, finish reason, validation result; redact PII. Production monitoring, sampling, dashboards and alerting are owned by B5.M7 (`ref`).
- Weak: "log prompt and response".
- Probes: Retention? Who can read traces?
- Senior-bar: mid logs; senior designs fields and redaction; staff sets retention and cost of telemetry.

**M11-Q2. You changed a prompt. How do you know it is better?** Hard; `llm.prompting`; AI, GP, B4; B:N A:S H:S
- Ideal: golden set with diverse and adversarial cases; per-task metrics; before/after; per-slice regressions; small-set statistical caution; shadow/canary; hand-off to B5 for judge pitfalls (methodology owned by B5.M1-M2).
- Weak: "tried a few examples".
- Probes: Set size? Judge bias? Online vs offline?
- Senior-bar: mid compares scores; senior slices and gates; staff defines release policy.

**M11-Q3 (ref: B5.Q1.1). How do you build a first eval set from nothing?** Med; `llm.production`; AI, IP; B:N A:S H:S
- Pointer item (the single `ref` item of the module): the golden-set method (sampling, stratification, labelling, versioning, held-out split) is owned by B5.Q1.1 and B5.M2; here the learner states only the minimal first set (20-50 labelled cases from real or synthetic inputs, including edge cases) and routes the method to B5.
- Weak: "generate with an LLM and trust it".
- Probes: Privacy of production data? Contamination?
- Senior-bar: mid writes a few examples; senior covers edge and adversarial cases with a held-out split; staff owns the eval-set lifecycle (method: B5).

Staff probes (M11): (a) How do you keep trace volume affordable at 100M calls a month? (b) How do you stop the golden set going stale?

### M12 Feature architecture patterns

**M12-Q1. Describe a client-side gateway for several providers inside one service and where the abstraction leaks.** Hard; `llm.architecture`; GP, GC, AI, IP; B:N A:S H:S
- Ideal: thin interface (`LLMClient`) with provider adapters; config-driven routing and fallback; budgets and rate limits; logging and cost attribution; guardrail hooks; streaming support; escape hatches for provider-specific features; leaks: structured-output subsets, reasoning state, caching semantics, tool-call shapes; build vs buy (LiteLLM-style). Org-wide gateway system design: A6 #16.
- Weak: thin proxy without budgets/observability; lowest common denominator.
- Probes: Latency overhead? Provider-specific features? Versioned prompts?
- Senior-bar: mid wraps SDKs; senior lists leaks and contract tests; staff decides platform vs library.

**M12-Q2. Your LLM feature gets a voice channel. What changes in the feature architecture, and what do you hand to the voice stack?** Hard; `llm.architecture`; AI, GP, IP; B:N A:S H:S
- Ideal: LLM-side changes only: streamed output in short sentence-sized chunks, prompts written for speech, interruptible generation (cancellation propagates to the model call), tool calls and structured outputs that tolerate barge-in, per-turn cost and token budgets, fallbacks; a clear contract at the boundary. Latency budgets by stage, endpointing, STT/TTS and turn-taking are owned by B6 (B6.M6-Q1: `ref`).
- Weak: sequential batch STT, LLM, TTS; designs the whole voice pipeline here.
- Probes: Where does cancellation stop the spend? Guardrail placement? Cost per minute?
- Senior-bar: mid lists stages; senior defines the LLM-side contract; staff owns trade-offs for quality vs latency vs cost.

**M12-Q3. Multi-tenant LLM SaaS: isolate cost and data.** Hard; `llm.architecture`; GP, IP, B4; B:N A:S H:S
- Ideal: tenant-scoped keys/quotas/budgets; per-tenant prompt/config versions; cache namespacing; data isolation in memory/retrieval; metering for billing; noisy-neighbour control via queues; audit logs; model allow-lists and residency.
- Weak: shared pool, global key, no limits.
- Probes: Semantic cache leakage? Tenant-specific fine-tunes?
- Senior-bar: mid adds quotas; senior isolates data and cache; staff handles billing disputes and contractual limits.

Staff probes (M12): (a) What would you cut first if the platform team had two engineers and a quarter? (b) How do you migrate 30 features from provider A to provider B with zero downtime?

## 5. Final assessment

- Mastery check: 20 items across modules, 80% pass (G-7), weighted toward M2, M4, M5, M9, M10 (attack-class items MCQ only, R-B5-1).
- Timed coding task (60 min), on recorded fixtures: tool-calling service in FastAPI via `LLMClient` with structured output, streaming, retry/fallback, token/cost logging; hidden tests with failure injection (429, malformed JSON, tool error, client disconnect); pass >= 70% hidden tests plus correct stated complexity.
- Design task on canvas: LLM feature architecture (client-side gateway, cache, queue, guardrail hooks, observability); rubric >= 3.5, no dimension < 3, no critical findings; the rule engine cites registry IDs only (R-AI-1 fallback, R-AI-2 or R-COST-2 budget and metering, R-RATE-1, R-QUEUE-1, R-OBS-1), with stated non-goals suppressed.
- Voice mock (20 min): "Design and productionise an LLM feature"; badge at "Meets"; readiness at Senior bar.
- Capstone evidence: mini eval set and trace of the capstone service.

## 6. Course-level risks and freshness notes (facts as of October 2026)

Verification method: web search of provider documentation and secondary summaries, 5 Oct 2026. Items below marked "confirmed" were found consistent across sources; they stay in dated Provider Notes, not in lesson text.

| Item | Status (Oct 2026) | Source type |
|---|---|---|
| Anthropic prompt caching: 5-minute default TTL (refreshed on read), optional 1-hour TTL; reads about 0.1x base input price, 5-min writes about 1.25x, 1-hour writes about 2x; up to 4 breakpoints; minimum cacheable prefix varies by model tier (about 512 to 4,096 tokens) | Confirmed against official docs, verified 2026-10 (512 on the newest tiers, 1,024 or 2,048 on mid tiers, 4,096 on some larger and smaller tiers; exact model-to-minimum map kept in the dated Provider Notes file); exact per-model minimums stay in Provider Notes | Vendor docs summaries |
| OpenAI prompt caching: automatic for prompts of 1,024 tokens or more, hits in 128-token increments, exact-prefix match, extended retention up to 24 h available (newer models default to 24h); cached-input discount varies by model | Confirmed (verified 2026-10); discount varies by model generation (older generations about 50%, mid generations about 75%, the newest about 90% per the docs excerpt; the very newest generations add a cache-write surcharge with discounted reads), so percentages and the model-to-discount map stay in the dated price file and are not hard-coded | Docs and vendor cookbook |
| Batch APIs: about 50% discount with up-to-24 h completion window (Anthropic Message Batches, OpenAI Batch; flex tier also 50%) | Confirmed | Vendor docs summaries |
| Structured outputs: Anthropic supports schema-constrained JSON and strict tool use with a JSON Schema subset (no recursion, no external refs, numeric/string-length constraints stripped or rejected, additionalProperties must be false, optional fields allowed); OpenAI strict mode requires all fields in required and additionalProperties false | Confirmed at concept level; per-keyword support lists change, so portability tests run in CI | Vendor docs |
| Reasoning controls: Anthropic thinking budget minimum 1,024 tokens, interleaved thinking with tools (beta header at last check `[verify: interleaved-thinking header or GA status in the extended-thinking page of the provider docs]`), newer models move to an effort parameter (verified 2026-10: manual `budget_tokens` deprecated on the previous mid-tier generation and removed on the newest, which uses adaptive thinking plus effort); OpenAI reasoning effort levels and reasoning items preserved via response-id continuation or explicit replay (encrypted content option; verified 2026-10: in stateless mode, store=false or Zero Data Retention, reasoning items carry `encrypted_content` to pass back) | Confirmed (verified 2026-10); effort values are model-dependent among none, minimal, low, medium, high, xhigh and max, and a model may reject a value it does not support, so test per model and keep the supported set in the dated Provider Notes | Vendor docs |
| Media tokenisation rules per provider (image tiling/resolution, PDF page cost) | Verified 2026-10 against vendor docs: Claude images ceil(w/28) x ceil(h/28) tokens, capped at 1,568 (standard) or 4,784 (high-resolution tier, 2,576 px max edge), PDF pages billed as images; OpenAI tile models 85 base + 170 per 512 px tile (low detail = base only), patch models 32 px patches. Still measure with count_tokens | Vendor docs |
| Context-management features (context editing, compaction) and their availability | Verified 2026-10: Anthropic context editing (tool-result clearing, beta header `context-management-2025-06-27`) and server-side compaction (beta header `compact-2026-01-12`); OpenAI Responses compaction endpoint exists; beta status moves, so keep in dated Provider Notes | Vendor docs |
| Provider data-use and zero-retention options (eligibility, regions) | Verified 2026-10: Anthropic deletes commercial API inputs/outputs within 30 days by default (not trained on by default), ZDR per-organisation by approval; OpenAI abuse-monitoring logs up to 30 days, ZDR and Modified Abuse Monitoring by prior approval with per-endpoint eligibility, residency regions (US, EEA+CH, AU, CA, JP, IN, SG, KR, UK, UAE) need approval beyond US; `[verify: legal/privacy reviewer checks provider DPA and ZDR addendum wording and the current residency list]` | Vendor docs and contracts |
| Prices, context sizes, rate limits | Never hard-coded; dated price file updated each cycle | Data file |
| API style shifts (messages-style vs responses-style) | Hidden behind `LLMClient`; fixtures recorded per style | Harness |
| MCP and hosted tools evolve fast | Pointer only; B4.M2 owns | B4 |
| Drift with B3-B7, A2, A5, A6 | Ownership matrix; one-way pointers | Boss matrix |

Review cadence: every 3 months; Provider Notes re-verified each cycle; last-reviewed stamp per lesson. Remaining `[verify]` markers in this file after the 2026-10 verification and the 2026-10-06 track-lead review: (1) contractual wording and residency list for data-use and retention terms (human legal/privacy review); (2) interleaved-thinking header or GA status (one provider-doc check, owner of the Provider Notes file); all other former markers resolved with dated facts.

## 7. Open items for boss

1. Confirm the `LLMClient` fixture contract must include reasoning-state round-tripping (needed for M2/M5 labs and B4 loops) so the harness scripted mock can emit and verify reasoning blocks.
2. Confirm the Provider Notes data file (prices, cache TTLs, schema subsets) has a named owner and a quarterly update slot.
3. Confirm media-token calculators for M6 are provided by the harness (count_tokens fixtures) rather than live calls.

## 8. Learner-Fit Appendix

Tag key: B = Senior Python Backend, A = AI Engineer, H = hybrid; M/S/N = MUST/SHOULD/NICE (G-3). Cluster emphasis (aligned to the Company Pack map): IT-services probe M10 (data governance) and M6 extraction; global-IT consulting M1, M12; Big4 M12 multi-tenant and M10; Indian-product M9 cost; global-product B2 core for AI roles; AI-startup M4, M5, M7, M9 (and M2).

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge |
|---|---|---|---|---|
| L1 | Simpler 16 h; all lessons; B1 and A2 remediation links open | 60% guided labs, 40% mastery items; fixtures with hints | Nothing | None until mastery >= 80% |
| L2 | Simpler 16 h, skim lessons already known (API-call basics) | 50% labs, 30% items, 20% verbal | M1.1, M3.1 if band >= 70 | Optional M4/M5 extension |
| L3 | Deeper 11.5 h; skim foundations, study "explain at depth" lessons | 50% applied labs, 20% items, 30% voice drills | Per bands | One Challenge per MUST module |
| L4 | Deeper about 9-10 h after skips; go straight to mastery check where scored | 30% labs, 70% senior-bar probes and voice | M1, M3 lessons; M2.1, M4.1-.2 likely | All staff probes; adversarial fixtures |

### LF-2 Module matrix

| Mod | Placement diagnostics (6 each, `placement`) | Bands (>=85 / 70-84 / 50-69 / <50) | B/A/H | Weight % B / A / H | Rehearsal modes | Module readiness (G-6 specialised) |
|---|---|---|---|---|---|---|
| M1 | P1 cost of 6k/800 request; P2 temp-0 determinism; P3 prefill vs decode; P4 cached/reasoning tokens; P5 context=in+out; P6 top-p vs temp | skip / skim D-only / study / S path + B1.M8 | B:S A:M H:M | 10 / 8 / 9 | voice drill | cost estimate within 20% on 2 scenarios; voice >= 3.5 on `llm.fundamentals` |
| M2 | P1 effort vs cost; P2 reasoning tokens billed; P3 alias vs pinned; P4 state across tool turns; P5 tier choice for task; P6 deprecation steps | same | B:S A:M H:M | 5 / 10 / 8 | voice drill, design canvas (routing mini) | comparison table with measured numbers; voice >= 3.5 |
| M3 | P1 role hierarchy; P2 few-shot pitfalls; P3 CoT vs reasoning; P4 prompt regression; P5 delimiters; P6 negative instructions | same | B:S A:M H:M | 5 / 8 / 6 | voice drill | refactor beats baseline; voice >= 3.5 |
| M4 | P1 JSON mode vs schema; P2 truncation breaking JSON; P3 schema subset limit; P4 valid-but-wrong; P5 retry feedback; P6 refusal handling | same | B:S A:M H:M | 15 / 12 / 13 | coding, voice drill | extraction >= 95% valid on hidden set; timed task passed |
| M5 | P1 loop steps; P2 tool_choice modes; P3 idempotency; P4 parallel calls; P5 iteration cap; P6 reasoning state | same | B:S A:M H:M | 15 / 12 / 13 | coding, voice drill | hidden tests incl. failure injection; voice >= 3.5 |
| M6 | P1 image token cost; P2 OCR vs native; P3 validation of totals; P4 PII images; P5 upload vs inline; P6 audio native vs STT | same | B:N A:S H:S | 2 / 5 / 3 | voice drill, project deep-dive | invoice task passed; voice >= 3.5 |
| M7 | P1 context budget sections; P2 rolling summary risk; P3 pinned facts; P4 lost-in-middle; P5 memory write policy; P6 compaction | same | B:S A:M H:M | 8 / 10 / 9 | design canvas, voice drill | context task keeps constraints; voice >= 3.5 |
| M8 | P1 SSE buffering; P2 disconnect billing; P3 tool deltas; P4 usage event; P5 backpressure; P6 partial JSON | same | B:S A:M H:M | 12 / 8 / 10 | coding, voice drill | endpoint hidden tests (cancel, usage); voice >= 3.5 |
| M9 | P1 cache prefix order; P2 TTL and write premium; P3 batch trade-off; P4 429 vs outage; P5 token-metered limit; P6 semantic cache risk | same | B:S A:M H:M | 15 / 10 / 12 | coding, voice drill, design canvas | measured >= 40% cut with quality held; voice >= 3.5 |
| M10 | P1 injection myth; P2 PII flow; P3 timeout types; P4 excessive agency; P5 budget guard; P6 output validation | same | B:S A:M H:M | 8 / 7 / 8 | voice drill | 5 injection tests pass; voice >= 3.5 |
| M11 | P1 trace fields; P2 redaction; P3 golden-set size; P4 before/after run; P5 slice regression; P6 canary | same | B:N A:S H:S | 2 / 5 / 4 | voice drill | regression run committed; voice >= 3.5 |
| M12 | P1 gateway leaks; P2 tenant budgets; P3 queue vs sync; P4 voice latency hiding; P5 abstraction limits; P6 build vs buy | same | B:N A:S H:S | 3 / 5 / 5 | design canvas, voice drill | canvas rubric >= 3.5; voice >= 3.5 |

Module tags follow the boss ruling (M1, M4, M5, M9, M10 are the Backend-route SHOULD core). Weights sum to 100 per target. Rationale `[judgement]`: backend loops probe structured output, tool safety, streaming and cost most; AI Engineer loops spread weight to model selection, context and evals.

### LF-3 Experience bridge ("you already know" -> senior-depth gap)

| Mod | You already know | Gap between using it and explaining it |
|---|---|---|
| M1 | Calling chat APIs, seeing token usage fields | Pricing asymmetry, sensitivity analysis, why determinism fails, prefill/decode physics |
| M2 | Picking a model by name for a chatbot | Eval-driven effort policy; reasoning tokens in cost; lifecycle and qualification |
| M3 | System prompts, few-shot in your bots | Regression-tested, versioned prompts; roles as soft boundary |
| M4 | JSON mode or Pydantic parsing | Limits of constrained decoding, valid-but-wrong, schema subsets, truncation |
| M5 | Function calling in a bot | Idempotency, failure injection, reasoning state, loop cost growth |
| M6 | Sending an image to an API | Media token economics, validation, PII, native vs pipeline |
| M7 | Passing chat history | Budgeting, summary fidelity tests, memory governance |
| M8 | FastAPI SSE/StreamingResponse, voice agent streams | Billing on cancel, tool deltas, buffering diagnosis (async fundamentals: A2.M7) |
| M9 | Redis caching, retries on AWS calls, Lambda cost sense | LLM prefix caching economics, token-metered limits, fallback contracts |
| M10 | API auth, input validation, secrets handling | LLM-specific trust boundaries, PII flows, spend guardrails |
| M11 | Logging/metrics with CloudWatch-style tools | LLM trace schema, golden sets, safe logging |
| M12 | Service architecture, queues, multi-tenant SaaS | Abstraction leaks, per-tenant cost, voice latency budget |

### LF-4 Senior-bar framing

Per-question "Senior-bar" lines and per-module staff probes are in section 4. Module-level signals:

| Mod | Mid answer | Senior adds | Staff adds |
|---|---|---|---|
| M1 | Defines tokens, says temp 0 is "mostly" stable | Cost range with sensitivity by token type; determinism tests | Determinism policy per feature class; unit economics per tenant |
| M2 | Knows reasoning models cost more | Per-request effort backed by eval evidence; pinned ids and qualification suite | Effort policy tied to cost SLOs; lifecycle calendar |
| M3 | Describes roles and few-shot | Measured prompt loop with regression set; enforcement moved to code | Prompt governance, review and rollback across teams |
| M4 | Uses a schema library | Knows constrained-decoding limits; valid-but-wrong audits | Schema evolution and cross-provider portability tests |
| M5 | Describes the loop | Guards, idempotency, failure injection, reasoning-state round-trip | Audit and replay; proof of tool reachability to security |
| M6 | Wires image input | Validates totals, prices per page, handles PII | Review SLAs, per-field drift monitoring |
| M7 | Truncates history | Budgets sections, fidelity-tests summaries | Memory consent, deletion, cross-tenant isolation |
| M8 | Streams text | Cancels upstream, accounts billing on disconnect, assembles tool deltas | Reconciles against invoices; load-test and failure UX |
| M9 | Lists cost levers | Orders by measured ROI; cache break-even; retry budgets | Degradation tiers, error-budget policy, prefix governance |
| M10 | Lists generic controls | Places controls at trust boundaries; stage-wise timeouts | Residual-risk sign-off; spend guardrails per tenant |
| M11 | Logs prompt and response | Designs trace fields and redaction; slices regressions | Retention and telemetry cost; release gating policy |
| M12 | Wraps SDKs | Names abstraction leaks; contract tests; tenant data and cache isolation | Platform vs library decision; migration plan |

### LF-5 Adaptive hooks

| Module | Remediation links | Challenge extension | Reorder/defer |
|---|---|---|---|
| M1 | B1.M8 (tokenisation) | Sensitivity analysis on cost model | May move first or skip |
| M2 | M1 | Effort-policy design from eval data | After M1 |
| M3 | M2 | Prompt-regression flywheel | May defer after M4 |
| M4 | M3, A2 basics | Schema evolution across providers | With M5 |
| M5 | M4, A5.M4 (idempotency) | Quadratic loop-cost proof | With M4 |
| M6 | M4 | Accuracy dashboard | Deferrable (A:S, B:N) |
| M7 | M1 | Memory deletion flow | After M5 |
| M8 | A2.M7 | 5,000-stream load plan | After M5; before M9 |
| M9 | M1, A5.M4 | Break-even model of caching | Core; not deferrable |
| M10 | M5, A8 (security basics) | Spend-guardrail design | Not deferrable |
| M11 | M3, M9 | Golden-set staleness plan | Compressible |
| M12 | M8, M9, M10 | Provider migration plan | Last |

Thresholds: module mastery 80%; readiness G-6 (retake >= 7 days later at >= 85%; timed task passed; strict-mode voice drill at Senior bar, average >= 3.5 with no competency < 3, in two sessions >= 48 h apart; no open prerequisite remediation flag). Badge "Meets" voice threshold: average >= 3.0, none < 2. Adaptive engine may reorder M3/M6/M11 and defer M6 for Backend route.

### LF-6 Applied labs (production-framed)

M1: cost-and-latency forecast for a SaaS support bot. M2: effort and tier table for a document-analysis feature. M3: refactor a failing enterprise HR-policy bot prompt. M4: extract purchase-order fields into typed records. M5: IoT-operations assistant with a guarded "restart device" write tool. M6: invoice/receipt extraction for an accounts-payable queue. M7: support-chatbot context manager with pinned account constraints. M8: FastAPI streaming endpoint behind a buffering proxy with cancel and usage. M9: cut an enterprise ticket-triage pipeline cost by 40%. M10: harden a customer-facing assistant against 5 injection cases. M11: tracing and a 30-case golden set for the support bot. M12: multi-tenant voice-support assistant design.

### LF-7 Currency and quality bar

All modules: stamped "Oct 2026"; every module has a named analogy (section 2); fixtures keep lessons deterministic. Top misconceptions per module: M1 temp-0 determinism, window is input only, ignore output price; M2 biggest is best, long CoT for reasoning models, hard-coded names; M3 NEVER-lists, no versioning, no regression set; M4 JSON mode guarantees schema, valid equals correct, truncation/refusal; M5 model executes tools, retry non-idempotent, no loop cap; M6 vision is free, unvalidated numbers, PII images; M7 big window ends memory design, lossy summaries, unbounded history; M8 proxy buffering, disconnect still bills, partial JSON; M9 changing prefix, semantic cache leakage, retry without idempotency; M10 system prompt as defence, raw PII logs, broad credentials; M11 unredacted logs, no baseline, tiny eval set; M12 lowest-common-denominator abstraction, no tenant budgets, ignoring latency stages.

## 9. Boss compliance log

- G-1: Ownership-notes row added; pointers (B3.M1, B7.M1, B4.M2, B5, A5.M4, A2.M7, A6 #16) <= 20 min, one `ref` item each. G-2/section 5: header 16 S / 11.5 D + 2.5 assess; module hours sum; semantics sentence present. G-3/G-11: Learner-Fit Appendix LF-1..LF-7; goal tags per module and question. G-4, G-5, G-6, G-7: level paths, bands, readiness and thresholds applied. G-8: tier language, no model names, dated Provider Notes. G-9: tags normalised, secondary tags listed. G-10: seeds carry goal tag and senior-bar note; 2 staff probes per module. G-13: owner of X3, X4.
- R-B2-1: `LLMClient` adapter in all code, both API styles. R-B2-2: M7 trimmed to context management with pointers. R-B2-3 and R-B5-1: M10 basics (<= 1 h D), attack items MCQ. R-B2-4: recorded fixtures only for graded tasks; optional capped live mode (P-07, P-23). R-B2-5: M11 <= 1 h, M12 lite, effort 16/11.5. O-69..O-73, O-86, O-90: followed.
- Section 8 B2 items 1-10 applied; item 8 `[verify]` results in section 6.
- Track lead B review (2026-10-06): see `_reviews/TrackLead_B_Report.md`. Applied: model names and unsupported model-specific claims removed from section 6 (tier language); [verify] markers made specific (contract/residency legal check, interleaved-thinking header); sampling-parameter caveat added to M1; worked-example/alt-text note added; M11-Q3 senior-bar line and M12-Q2 goal tag fixed; LF-4 expanded to per-module mid/senior/staff table; LF-5 readiness thresholds aligned to G-6; cluster emphasis aligned to Company Pack map; last_reviewed bumped.
