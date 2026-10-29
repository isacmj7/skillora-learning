---
name: B4 — Agents and Workflows
course_id: B4
track: B (AI Engineer)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05; track-lead review 2026-10-06
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-15 (see _reviews/Boss_Final_Approval.md)
---

# B4 — Agents and Workflows (Track B)

Version 2.0 (wave-2 upgrade). Last reviewed: 2026-10-06 (as of October 2026). Volatility note: agent frameworks, protocol specs and model tool-use features change quickly; facts below carry an as-of date and source type (official spec/docs, vendor engineering blog, or secondary web report). Concepts are stable and carry the lessons; named products are a dated sidebar only (G-8, G-12). No model names appear in lessons.

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | B4 — Agents and Workflows / Track B (AI Engineer Mastery) |
| Audience | Senior backend engineers (L3–L4) moving into AI Engineer roles who have used LLM APIs and built chatbots and voice agents; also serves L1–L2 learners through the Simpler path (full scaffolding, mock-model loops) and Python Backend learners who take the course as a NICE-tag primer (M1.2-M1.3, M2.3, M7.1-M7.2, per section 8) |
| Prerequisites | B2 (tool use/function calling, structured outputs, context basics) is required. B3 (RAG) is helpful for the memory module. A2 (async, via B2) and A5 (idempotency, retries, X2 in A5.M4) are assumed. Placement: per-module bands (G-5; section 8 LF-2) decide skip, skim, study or deep-study; M7–M9 are never skipped without passing the mastery check because they are the senior differentiators; no band waives the final assessment |
| Outcomes (each tied to an interview competency) | 1. Decide and defend workflow vs agent vs single call for a given problem (`agents.architecture-choice`). 2. Implement an agent loop from scratch with stop conditions, budgets and tool-error recovery in Python (`agents.loop`). 3. Design tools that models use reliably: schemas, descriptions, errors, idempotency, pagination, least privilege (`agents.tool-design`). 4. Select and compose orchestration patterns (routing, parallelisation, orchestrator-workers, evaluator-optimiser) and justify multi-agent vs single-agent (`agents.orchestration`). 5. Design memory and state: short-term context, long-term stores, checkpoints and resumability (`agents.memory-state`). 6. Engineer reliability: timeouts, retries, idempotency keys, loop limits, token and cost budgets, durable execution, human-in-the-loop gates (`agents.reliability`). 7. Trace, evaluate and cost-control an agent: trajectory evals, task success, tool-call accuracy, regression sets (`agents.eval-observability`). 8. Threat-model an agent: prompt injection through tools and retrieved data, excessive agency, confused deputy, sandboxing (`agents.security`) |
| Estimated effort | **15.5 h Simpler / 10.5 h Deeper** plus 2.5 h assessment allowance (final assessment, voice mock, capstone revision) per the boss effort table. Simpler = L1–L2 route with full scaffolding; Deeper = L3–L4 route, hours net of typical skip-ahead, so it is shorter in wall-clock time but harder per hour. Module hours (S/D): M1 1.5/1.0, M2 1.75/1.25, M3 1.5/1.0, M4 1.5/1.0, M5 1.75/1.25, M6 1.5/1.0, M7 2.5/1.5, M8 1.75/1.25, M9 1.75/1.25 (sum 15.5/10.5). Judge and red-team depth moved to B5 (R-B4-3) |
| Interview coverage tags | `agents.architecture-choice`, `agents.loop`, `agents.tool-design`, `agents.protocols-mcp`, `agents.memory-state`, `agents.orchestration`, `agents.multi-agent`, `agents.hitl`, `agents.reliability`, `agents.eval-observability`, `agents.security`, `agents.cost`, `agents.frameworks` (owned tags). Secondary tags, no credit: `design.ai-systems` (feeds the A6 orchestration-platform design seed), `llm.tools` (B2 owns), `llm.judge` and `llm.security` (B5 owns) |
| Badge and certificate | Module badges M1–M9 (each: mastery check at or above 80% plus the module practice artefact passing hidden tests or the design rubric). Course badge "Agents and Workflows": all module badges plus final mastery check at or above 80% (G-7), practical and rubric components at their own thresholds, timed coding task at 70% or more of hidden tests with stated complexity, design-canvas rubric overall 3.5 or higher with no dimension below 3 and no critical rule finding, and a voice mock at "Meets" (average 3.0 or higher, none below 2) (G-7). Certificate evidence: the capstone repo (agent with tests, trace and eval report), the design-canvas submission and the voice mock score report |
| Authoring owner / reviewer | Owner: SME agent B4. Reviewer: to be assigned |
| Review cadence | Every 3 months (G-12). Framework, protocol and model-feature facts re-verified at each review; dated notes in each lesson. Next review due January 2027 |
| Lab environment | All graded labs use the deterministic scripted mock model of the LLM Lab Harness (P-07, R-B4-1) behind the thin provider-agnostic `LLMClient` loop; live mode is optional, gateway-capped (P-23) and never gates a badge. Durability labs use a SQLite or Postgres state machine with crash injection (P-12, R-B4-4). No framework installs anywhere (R-B4-2) |

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice tasks | Pitfalls and misconceptions interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Workflows vs agents and the agent loop | Choose between single call, workflow and agent for a scenario and implement a minimal agent loop | Definitions (workflow = predefined code paths; agent = model directs its own process and tool use); autonomy spectrum; cost, latency, determinism trade-offs; the loop (observe, think, act, observe) as messages plus tool results; stop conditions (final answer, max steps, budget, no-progress detection); ReAct-style loop vs native tool-calling loop; parallel tool calls; when not to use an agent | Autonomy spectrum ladder; agent loop sequence diagram; decision tree "single call, workflow, agent" | In the IDE: write a 60-line agent loop with a mock model and 3 tools; add max-steps and token budget; hidden tests check termination, tool errors fed back, and budget stop. Design exercise: classify 6 business scenarios | "Agents are always better", "an agent is just a while loop" without limits, no termination guarantee, confusing "agentic" marketing with an architectural choice, ignoring that error compounds across steps |
| M2 | Tool design, error handling and protocols (MCP) | Design and implement tools a model can use reliably and safely, and explain MCP vs native function calling | Tool naming and descriptions as prompts; JSON Schema for arguments, enums, constrained args; return shape (concise, model-friendly, stable ids, pagination, truncation); errors as actionable messages not stack traces; idempotent writes with idempotency keys; read vs write tools; tool count and tool-selection accuracy (provider-side tool search with deferred schema loading now exists at two major providers, as of October 2026); poka-yoke design; MCP overview (host, client, server; tools, resources, prompts; transports stdio and Streamable HTTP; OAuth-aligned authorisation for remote servers; the 2026-07-28 revision made the protocol stateless with no initialize handshake or session id, so servers scale behind ordinary load balancers; official spec, checked October 2026); MCP vs direct function calling vs agent-to-agent protocols (overview only: A2A reached v1.0 in March 2026 and is hosted by the Linux Foundation, positioned as complementary to MCP; vendor blog and secondary reports, verified 2026-10) | Tool contract anatomy; MCP architecture (host, client, server); error-feedback loop | Refactor 3 poorly designed tools (vague names, free-text args, opaque errors) and show tool-selection accuracy improve on a small eval set; build a small MCP server (read-only tool plus one write tool with idempotency key) and call it from a client | Tools that return 50 KB of raw JSON; throwing exceptions that kill the loop; non-idempotent "create" tools under retry; too many overlapping tools; treating MCP as a security boundary; MCP described as "a framework" instead of a protocol |
| M3 | Planning, reflection and reasoning control | Apply planning and self-critique patterns with measured benefit and bounded cost | Plan-then-execute vs interleaved ReAct; explicit plan objects and re-planning on failure; reflection and self-critique (and where it fails: sycophantic self-approval); verifier tools and external ground truth; code-as-action and computer-use or browser agents as loop variants (overview only, 20 min or less; sandbox and approval implications are taught in M6 and M9); extended thinking and reasoning-model controls as they affect the agent loop (reasoning-model behaviour and per-request effort choice are owned by B2.M2; here only loop-level implications: when scaffolding becomes redundant, reasoning-state round-tripping across tool turns; parameter names live in the dated B2 provider notes); task decomposition granularity; stuck detection and escalation | Plan-execute-replan state diagram; reflection loop with external verifier | Implement plan-and-execute with re-plan on tool failure; add a verifier (unit test runner or schema check) to a code-fixing agent; measure success rate vs steps with and without reflection | Believing self-reflection fixes errors without new information; unbounded replanning; long plans that go stale; treating chain-of-thought text as a faithful explanation; ignoring that stronger reasoning models may need less scaffolding |
| M4 | Memory and state | Design memory tiers and persistent state for a long-running agent | Context window as working memory; trimming, summarisation, compaction; scratchpad/notes files; tool-result clearing; context engineering as a discipline (what enters the window each step) including on-demand loading of instructions and skills, the same idea as deferred tool loading in M2; long-term memory (episodic, semantic, procedural) with vector/KV/relational stores; write policy (what to remember, when, who approves); retrieval of memories; memory poisoning and staleness; per-user isolation and PII/consent/deletion; session state vs checkpoint state; relation to RAG (B3) | Memory tiers diagram; context budget bar across a 40-step run; memory write/read lifecycle | Implement context compaction for a 50-step run and show quality preserved within a token cap; build a long-term memory store with a write policy, per-user namespace, TTL and delete-on-request | "Just stuff everything into the context", unbounded history growth, storing raw conversation as memory, no deletion path, cross-user leakage, memory treated as trusted input |
| M5 | Orchestration patterns | Pick and implement the five core workflow patterns and justify the choice | Prompt chaining with gates; routing (classifier or cheap model then specialist); parallelisation (sectioning and voting); orchestrator-workers (dynamic subtask decomposition); evaluator-optimiser loop with stop criteria; combining patterns; async fan-out/fan-in with `asyncio` and partial-failure handling; cost and latency of each; graph/state-machine modelling | One diagram per pattern; fan-out/fan-in timeline with a failing branch; pattern selection matrix | Implement routing and parallel fan-out with timeouts and partial results in `asyncio`; implement evaluator-optimiser with max iterations and a score-improvement stop; design-canvas task: support-ticket triage pipeline | Choosing orchestrator-workers when a fixed chain suffices; fan-out with no per-branch timeout; evaluator-optimiser with no external criterion so it loops; router misroutes with no fallback path |
| M6 | Multi-agent systems and human-in-the-loop | Argue when multi-agent is worth its cost and design approval and escalation gates | Why multi-agent (context isolation, parallel breadth, specialisation) and why not (token multiplication, coordination bugs, shared-state conflicts, debugging difficulty); supervisor, handoff and swarm topologies; subagents with clean contexts returning condensed results; contracts between agents (typed messages); shared state vs message passing; HITL patterns: approval gates for irreversible actions, confidence-based escalation, review queues, interrupt and resume, edit-before-approve; UX of asynchronous approval; approval fatigue; audit trail | Supervisor vs handoff vs peer topology; interrupt-and-resume sequence; risk-tier gate matrix (read, reversible write, irreversible or financial) | Convert a single-agent research assistant into orchestrator plus 2 subagents and compare cost, latency and quality; implement an approval gate that pauses a run, persists state, and resumes after a simulated human decision | "More agents means smarter", agents debating with no external signal, sharing full context between all agents, approval gates only in the UI (not enforced server-side), approving blindly in bulk, no timeout for an unanswered approval |
| M7 | Reliability and durable execution | Make an agent run survive failures and bound its blast radius | Timeouts at model, tool and run level; retries with exponential backoff and jitter; retry classification (transient vs semantic); idempotency keys and effect-once semantics; loop limits, step caps, wall-clock and token and cost budgets; circuit breakers and fallbacks (model fallback, degraded mode); deterministic replay; state machines and checkpointing; durable execution engines (Temporal-style workflow code, step functions, queue plus DB outbox; conceptual only in 1.0, the graded lab is a hand-built SQLite/Postgres state machine, R-B4-4); compensation and sagas for multi-step side effects; poison-task handling and dead-letter queues; rate-limit handling; long-running agents via queues and workers in FastAPI/Celery-style backends | Failure-mode map of one agent run; checkpoint and resume timeline; saga compensation diagram | Add retries, idempotency keys, budgets and checkpoint/resume to the M1 agent; kill the process mid-run and prove it resumes without duplicate side effects (hidden test checks side-effect count); chaos test with a flaky tool | Retrying non-idempotent writes; retrying on semantic errors (same bad call 10 times); no global budget; in-memory-only state lost on deploy; replay that calls the live LLM again and diverges; treating an LLM call as deterministic for replay |
| M8 | Observability, evaluation and cost control | Trace, evaluate and cost-optimise an agent with evidence | Trace model (run, step, LLM call, tool call, tokens, latency, cost); OpenTelemetry GenAI semantic conventions (every gen_ai attribute still at Development status in mid-July 2026 and the group moved to its own repository in June 2026, so use them as a naming guide, not a contract; secondary web reports, October 2026) and vendor tracing tools overview (vendor-neutral, no install); logging prompts and tool IO safely (PII redaction, retention); eval levels: end-to-end task success, trajectory (tool-call correctness, step efficiency), component (router accuracy, tool selection); outcome-based graders (judge design and calibration: pointer to B5.M3); regression sets from production traces; non-determinism handling (repeat runs, pass@k versus pass^k style consistency metrics (any success in k tries versus all k succeed)); cost levers (model routing, prompt caching, tool-result trimming, parallel vs sequential, early exit, caching tool outputs, batch for offline work); per-run and per-tenant budgets | Annotated trace waterfall; eval pyramid for agents; cost per task breakdown chart | Instrument the agent with spans and cost accounting; build a 20-case trajectory eval with deterministic graders and run it 3 times to report variance; cut cost per task by a measured amount without dropping success rate | "It looks good in demos"; only final-answer eval; one-run evals on a non-deterministic system; LLM judge with no calibration; logging raw PII; optimising cost before measuring where tokens go; ignoring tool-result tokens |
| M9 | Security, framework landscape and capstone | Threat-model an agent, choose build vs framework, and ship a capstone | Prompt injection (direct and indirect via tool results, web pages, emails, documents, MCP tool descriptions); excessive agency; confused deputy; the "lethal trifecta" (private data, untrusted content, external communication; term coined by Simon Willison, blog post dated 16 June 2025, verified 2026-10; cut any one leg to break the exfiltration path); least-privilege scoped credentials per tool and per user; sandboxed code execution; egress controls; allow-lists; output filtering; human approval for dangerous sinks; data exfiltration paths (markdown images, URLs); tool poisoning and supply chain for MCP servers; multi-tenant isolation. Framework landscape as a **code-reading and design exercise, no install** (R-B4-2): one graph-style framework chosen by engineering (candidates: LangGraph, which reached 1.0 in October 2025: PyPI 1.0.0 on 2025-10-17, general availability announced 22 October 2025; verified 2026-10) is read against the learner's own thin loop; dated sidebar covers the others (see section 6 status table); build-vs-buy criteria (control, debuggability, lock-in, durability, observability, team skill); how to read a framework's abstractions critically | Attack-path diagram for indirect injection to exfiltration; trust-boundary diagram; framework comparison matrix with "verify date" | Threat-model a mail-reading agent on the canvas and add three mitigations (the canvas must be clean of R-AGENT-1 and R-AGENT-2); run the harness-supplied injected-document fixture against your M2 tools and show the guard blocks the exfiltration (red-team method and guardrail depth: pointer to B5.M4); canvas capstone using registry rules R-AGENT-1 and R-AGENT-2 (see section 5); capstone: build, trace, evaluate and harden an agent | "Prompt guardrails alone prevent injection"; trusting tool output as instructions; one shared admin credential for all users; classifier filters presented as complete defence; picking a framework by GitHub stars; being unable to explain what a framework does under the hood |
| Own. | Ownership notes (G-1) | OWNS: B4.M1 loop and workflow-vs-agent choice, B4.M2 tool design and MCP, B4.M3 planning and reflection, B4.M4 agent memory and state, B4.M5 orchestration, B4.M6 multi-agent and HITL, B4.M7 durable execution, B4.M8 agent evaluation and trace specifics, B4.M9 agent security design. POINTS: single-call tool-calling mechanics and structured outputs -> B2.M4-M5; reasoning-model behaviour and effort choice -> B2.M2; context-window management -> B2.M7; prompt caching, token cost, X3 -> B2.M9; RAG and retrieval -> B3.M1, B3.M5; eval methodology and regression gating -> B5.M1-M2; LLM-as-judge -> B5.M3; injection, red-team, guardrails, PII, OWASP LLM -> B5.M4-M6; LLM observability and drift -> B5.M7; retry, backoff, jitter (X2) -> A5.M4; asyncio task groups -> A2.M5; idempotency keys, outbox, sagas generic -> A5.M4, A2.M9; generic OTel and SLOs -> A7.M9; voice pipelines -> B6 | | | | |

## 3. Per-module detail blocks

Question-bank targets per module (template minimums): 12 MCQ/concept checks, 6 code-output/predict-the-result, 4 coding tasks with hidden tests, 8 verbal questions with follow-up trees (3 seeded here), 3 scenario/design questions. Mastery check per module: 6–10 items, pass at 80% or higher, revision loop on misses. Module badge: mastery check passed plus the named practice artefact accepted.

Every module also ships one annotated worked example (the section 2 practice task solved on the mock-model harness, code run in CI) and alt text for each visual (template section 2).

### M1 — Workflows vs agents and the agent loop
- Lessons: (1) The autonomy spectrum and the decision tree; (2) Anatomy of an agent loop (messages, tool calls, results); (3) Stop conditions, budgets and no-progress detection; (4) When an agent is the wrong answer (cost and error compounding).
- Simpler: build the loop with one tool and a scripted mock model, step through each message. Deeper: parallel tool calls, interleaved thinking, comparing loops across providers, compounding-error arithmetic (e.g. per-step success p over n steps is roughly p to the power n if independent).
- Mastery focus: definitions, choosing the right shape, loop termination, what goes into the message history per step.
- Badge: loop passes termination, budget and error-feedback hidden tests; 6 scenario classifications at or above 5 correct with reasoning.

### M2 — Tool design, error handling and protocols
- Lessons: (1) Tool contract design (names, descriptions, schemas); (2) Return values and error messages the model can act on; (3) Idempotent and safe write tools; (4) MCP and protocol landscape overview.
- Simpler: improve one tool description and see behaviour change. Deeper: tool-count scaling, provider tool search and deferred loading (supported by Anthropic and OpenAI APIs, October 2026, per vendor docs; cached prefixes stay intact because loaded definitions are appended), MCP auth flows including the iss-parameter validation hardening in the 2026-07-28 revision, versioning of tool schemas, pagination and truncation strategy.
- Mastery focus: schema design, error design, idempotency, MCP roles and transports, when to use MCP vs plain function calling.
- Badge: refactor task shows measurable tool-selection accuracy gain; MCP server passes harness-supplied protocol smoke tests (no external install; stdio in-process transport).

### M3 — Planning, reflection and reasoning control
- Lessons: (1) Plan-and-execute vs ReAct; (2) Re-planning and recovery; (3) Reflection with and without external verifiers; (4) Reasoning-model controls and when scaffolding hurts.
- Simpler: static plan list executed step by step. Deeper: dynamic plan graphs, verifier design, budgeted thinking, measuring the benefit of reflection.
- Mastery focus: why external signal matters, bounded replanning, honest cost/benefit claims.
- Badge: code-fixing agent with verifier beats the no-verifier baseline on the supplied eval set within a step budget.

### M4 — Memory and state
- Lessons: (1) Context as working memory: trim, summarise, compact; (2) Notes and scratchpad patterns; (3) Long-term memory stores and write policies; (4) Privacy, isolation, staleness and poisoning.
- Simpler: sliding window plus summary. Deeper: hierarchical memory, retrieval scoring (recency, importance, relevance), memory conflict resolution, temporal facts.
- Mastery focus: tier selection, write policy, deletion and isolation, relation to RAG.
- Badge: compaction task meets quality threshold under the token cap; memory store passes isolation and delete tests.

### M5 — Orchestration patterns
- Lessons: (1) Chaining and routing; (2) Parallelisation: sectioning and voting; (3) Orchestrator-workers; (4) Evaluator-optimiser; (5) Choosing and combining patterns.
- Simpler: one pattern per lab with fixed mock model. Deeper: partial failure semantics, dynamic fan-out limits, graph state machines, cost modelling per pattern.
- Mastery focus: matching pattern to problem, termination of loops, partial failure, cost.
- Badge: async fan-out passes timeout and partial-result tests; evaluator-optimiser terminates on criteria.

### M6 — Multi-agent systems and human-in-the-loop
- Lessons: (1) Single vs multi-agent decision; (2) Topologies and handoffs; (3) Subagent context isolation and result contracts; (4) Approval gates, interrupt and resume; (5) Escalation and audit trails.
- Simpler: supervisor with two workers. Deeper: shared-state conflicts, token-multiplication cost model, risk-tiered approvals, approval timeouts and delegation.
- Mastery focus: when multi-agent pays off, gate enforcement location, state persistence across the human wait.
- Badge: comparison report with measured cost/latency/quality; approval gate survives process restart.

### M7 — Reliability and durable execution
- Lessons: (1) Timeouts, retries, backoff and error classes; (2) Idempotency and effect-once; (3) Budgets, loop limits and circuit breakers; (4) Checkpointing, state machines and durable execution; (5) Sagas, compensation and dead-letter handling.
- Lab (P-12): a SQLite or Postgres-backed state machine with the crash-injection API (kill-and-resume); the hidden test counts side effects across N random kill points. Retry/backoff/jitter is the shared lesson X2 (owner A5.M4), used here by reference.
- Simpler: wrap tool calls in retry plus timeout, add step cap. Deeper: outbox pattern, deterministic replay with recorded LLM responses, workflow-engine determinism constraints, back-pressure on provider rate limits.
- Mastery focus: failure taxonomy, duplicate side-effect prevention, resumability, bounding cost and time.
- Badge: kill-and-resume test passes with zero duplicate side effects; chaos tool test passes.

### M8 — Observability, evaluation and cost control
- Lessons: (1) Tracing an agent run; (2) Safe logging and retention; (3) Trajectory and outcome evals; (4) Handling non-determinism and regression sets; (5) Cost accounting and optimisation.
- Pointers: eval methodology, golden sets and CI gating -> B5.M1-M2; judge design and calibration -> B5.M3; LLM observability stack -> B5.M7 (this module keeps agent-trace specifics, trajectory metrics and cost per successful task).
- Simpler: log per-step tokens and latency, hand-grade 10 runs. Deeper: automated graders, repeat-run variance, production-trace-to-eval pipeline, per-tenant budgets.
- Mastery focus: what to measure, how to evaluate non-deterministic systems, where cost hides.
- Badge: instrumented agent plus eval report with variance and measured cost reduction.

### M9 — Security, framework landscape and capstone
- Lessons: (1) Threat model of agents: injection, agency, exfiltration; (2) Least privilege, sandboxing and egress control; (3) MCP and tool supply-chain risks; (4) Framework landscape and build-vs-buy; (5) Capstone integration review.
- Simpler: one injection demo and one mitigation. Deeper: multi-layer defence, dual-model or privileged/quarantined designs, capability-based tokens, red-team harness.
- Mastery focus: indirect injection paths, architectural (not prompt-only) mitigations, honest framework selection.
- Pointers: injection taxonomy, red-team method, guardrail and PII depth -> B5.M4-M6 (<= 20 min recap here, one `ref` item).
- Framework lesson (M9.4) is a code-reading exercise: map a short graph-framework agent to the learner's own loop (state, edges, checkpointer, interrupt), then write a one-page build-vs-adopt memo.
- Canvas capstone: the agent design must pass registry rules R-AGENT-1 (write-capable tools without approval or scope limit) and R-AGENT-2 (loop without step, token or time budget); cite registry IDs only.
- Badge: red-team fixture test shows the mitigation blocks the exfiltration case; canvas capstone free of R-AGENT-1/2 findings; capstone submitted.

## 4. Seed interview questions (27 module seeds, 18 staff probes, 3 incident scenarios)

Tag format: competency / difficulty / company-style cluster. Style-of-question items only; not real questions from any company.

### M1 seeds

**M1-Q1. "When would you use an agent instead of a workflow, and how do you decide?"** Competency `agents.architecture-choice` / Medium / global-product, AI-startup
- Ideal outline:
  - Define both: workflow has code-defined paths; agent lets the model choose steps and tools dynamically.
  - Start from the simplest thing: a single LLM call with good context, then a workflow, then an agent only if the path cannot be enumerated in advance.
  - Agent fits open-ended problems with unpredictable step counts and where some autonomy is acceptable; workflow fits well-understood, repeatable, auditable processes.
  - Trade-offs: agents cost more, add latency, are less predictable, and errors compound across steps.
  - Risk of action: higher blast radius pushes toward workflows or gated agents.
  - Needs a measurable success criterion and eval set before choosing autonomy; trial both on that set.
  - Example from own experience (e.g. support triage as workflow, research task as agent).
- Weak answers: "agents are the modern way"; no mention of cost, latency or evals; cannot define the difference; names a framework instead of a reason.
- Follow-ups: How would you migrate a workflow to an agent incrementally? What evidence would convince you to move back? How do you explain the choice to a non-technical stakeholder?
- Goal tag: `B:N A:M H:M`
- Senior-bar: mid lists the definitions; senior decides from an eval set and blast radius; staff sets an org rubric for when autonomy is allowed and who signs off.

**M1-Q2. "Walk me through an agent loop. What stops it?"** `agents.loop` / Medium / IT-services, global-IT-consulting, AI-startup
- Ideal outline:
  - Messages list: system prompt, user task, assistant turn possibly with tool calls, tool results appended, repeat.
  - Model returns either final text or tool call(s); code executes tools (possibly in parallel) and returns results including errors.
  - Stop conditions: final answer, max steps, wall-clock timeout, token or cost budget, no-progress detection (repeated identical calls), explicit "give up/escalate" tool.
  - Tool errors go back to the model as data so it can recover; fatal errors abort.
  - Context growth management each iteration.
  - Logging each step for tracing; deterministic code owns the loop, not the model.
- Weak: "while True until the LLM says done"; no budget; crashes on tool exception; ignores context growth.
- Follow-ups: How do you detect a loop that is making no progress? What do you return to the user if the budget is hit? How do parallel tool calls change the message structure?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid says 'loop until done'; senior names five stop conditions and what the user sees at a budget stop; staff defines platform-wide default budgets.

**M1-Q3. "A teammate says each step of our agent is 95% reliable, so the 10-step agent is fine. Respond."** `agents.reliability` / Hard / global-product, AI-startup
- Ideal outline:
  - Compounding: 0.95 to the 10th is about 0.60 if independent; real failures are correlated and recoveries are possible, so measure end to end.
  - Step reliability is not uniform; one bad early step poisons later ones.
  - Reduce steps, make steps verifiable, add checkpoints and verification tools, allow retries on classified errors.
  - Use deterministic code for steps that do not need judgement.
  - Measure end-to-end task success on an eval set with repeated runs.
  - Gate irreversible actions.
- Weak: just says "test more"; does the arithmetic but offers no mitigations; claims retries always fix it.
- Follow-ups: How would you measure per-step vs end-to-end reliability? Where would you add verification? When does recovery behaviour make the naive arithmetic too pessimistic?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid does the 0.95^10 arithmetic; senior challenges independence and designs verification points; staff turns it into an SLO on end-to-end task success.

### M2 seeds

**M2-Q1. "How do you design tools so a model uses them correctly?"** `agents.tool-design` / Medium / AI-startup, global-product
- Ideal outline:
  - Tool name and description are prompt text: say what it does, when to use it, when not to, with examples.
  - Narrow typed schemas, enums and required fields; avoid free-text where an id or enum works.
  - Return concise, relevant, model-friendly output with stable ids; paginate and truncate with hints on how to get more.
  - Errors are actionable messages ("date must be ISO, got 12/05") rather than stack traces.
  - Fewer, well-separated tools beat many overlapping ones; consolidate multi-call workflows into one tool where sensible.
  - Evaluate with a small tool-selection eval and iterate on descriptions from failed traces.
  - Least privilege per tool.
- Weak: "just wrap every REST endpoint as a tool"; only discusses JSON schema syntax; ignores output size.
- Follow-ups: You have 80 endpoints; how do you expose them? How do you version a tool schema without breaking agents? How do you test tool descriptions?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid covers schema syntax; senior treats descriptions as prompts, trims outputs and runs a selection eval; staff owns a tool-contract standard and versioning policy.

**M2-Q2. "A create-order tool is called twice because of a retry. How do you prevent a duplicate order?"** `agents.reliability` / Medium / Indian-product, IT-services
- Ideal outline:
  - Accept a client-generated idempotency key (derived deterministically from run id plus step id plus args hash) and store key with result; return the stored result on repeat.
  - Unique constraint at the database, not only an application check.
  - Retry only on transient errors; the model re-issuing a call after a timeout must be safe.
  - Distinguish "unknown outcome" (timeout) from "failed"; provide a status-check tool or reconcile.
  - TTL and scope of keys; key collision with different payload returns an error.
  - Log and trace the key.
- Weak: "tell the model not to call twice in the prompt"; only retries without keys; in-memory dedupe.
- Follow-ups: Where does the key come from if the model calls the tool freshly? What if the downstream API has no idempotency support? How does this change under parallel tool calls?
- Goal tag: `B:M A:M H:M`
- Senior-bar: mid adds an idempotency key; senior covers unknown-outcome, DB uniqueness and key derivation; staff handles downstreams with no idempotency via reconciliation and ledger design.

**M2-Q3. "What is MCP, and when would you use it rather than plain function calling?"** `agents.protocols-mcp` / Medium / AI-startup, global-product
- Ideal outline:
  - Open protocol standardising how AI applications (hosts with clients) connect to servers exposing tools, resources and prompts.
  - Transports: local stdio and remote HTTP-based; remote servers need authorisation (OAuth-based; the current spec revision is 2026-07-28, which is stateless: no initialize handshake and no session id; verified 2026-10).
  - Value: write an integration once, reuse across hosts; ecosystem of servers; separation of tool ownership from agent code.
  - Plain function calling is simpler when tools are in-process, few, and owned by the same team.
  - Concerns: trust in third-party servers, tool description poisoning, credential scoping, version drift, extra latency and ops.
  - MCP is a connectivity standard, not an orchestration framework or a security boundary.
  - Mention agent-to-agent protocols only as a distinct layer: A2A v1.0 was released in March 2026 under the Linux Foundation and is positioned as complementary to MCP (verified 2026-10).
- Weak: "MCP is Anthropic's agent framework"; cannot name roles; no security view.
- Follow-ups: How do you vet and pin a third-party MCP server? How does user-level authorisation propagate through an MCP server? What are the context cost issues of exposing many tools?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid defines MCP roles; senior weighs trust, auth, pinning and context cost; staff sets an internal server registry and vetting process.

### M3 seeds

**M3-Q1. "Compare plan-and-execute with ReAct-style interleaving. When does each win?"** `agents.loop` / Medium / AI-startup, global-product
- Ideal outline:
  - Plan-and-execute: produce a plan up front, execute steps, optionally replan; good for predictable decomposition, cheaper executor calls, easier to show and approve the plan.
  - ReAct interleaving: decide the next action after each observation; adaptive to surprises; more calls, risk of drift.
  - Hybrid: plan as a living artefact, replan on failure or new information.
  - Plans go stale in dynamic environments; interleaving costs more tokens.
  - A plan is a natural human-approval point.
  - Evaluate on the task distribution rather than by preference.
- Weak: treats them as brand names; no trade-offs; claims one is strictly better.
- Follow-ups: How do you represent the plan (text vs structured object)? How do you bound replanning? How would a stronger reasoning model change your design?
- Goal tag: `B:N A:M H:M`
- Senior-bar: mid names both; senior chooses by task distribution and bounds replanning; staff treats the plan as an approval artefact and re-tunes as models improve.

**M3-Q2. "Does self-reflection make agents more accurate?"** `agents.loop` / Hard / AI-startup, global-product
- Ideal outline:
  - Reflection helps when there is new information or an independent check (tests, schema validation, retrieved ground truth, a separate rubric); pure self-critique often confirms the first answer.
  - Judge and generator share biases; a separate model or prompt helps only somewhat.
  - Cost: extra calls and latency; need a stop criterion (score threshold, max iterations).
  - Measure delta on an eval set; do not assume it.
  - Best in tasks with verifiable outcomes (code with tests, structured extraction, SQL with execution).
  - Mention failure modes: sycophantic approval, oscillation, over-editing.
- Weak: "yes, always"; no verifier; no cost view.
- Follow-ups: Design a verifier for a SQL-generating agent. How do you detect oscillation between revisions? When would you use a cheaper model as critic?
- Goal tag: `B:N A:M H:M`
- Senior-bar: mid says 'it helps'; senior demands an external verifier and measures the delta; staff budgets reflection cost per task class.

**M3-Q3. "Your agent keeps repeating the same failing tool call. Diagnose and fix."** `agents.loop` / Medium / IT-services, AI-startup
- Ideal outline:
  - Inspect the trace: is the error message informative? is the tool description ambiguous? is context truncated so the failure is lost?
  - Fix the tool error text to say what to change; include the failure in context (not summarised away).
  - Add no-progress detection (same tool plus same args N times) triggering replan, alternative tool, or escalation.
  - Cap retries per tool and classify errors (transient vs semantic).
  - Add a regression case from the trace to the eval set.
  - Consider whether the task is feasible with available tools and offer a "cannot complete" exit.
- Weak: "increase max steps"; "tell the model to try something different" only.
- Follow-ups: How do you detect near-duplicate calls (args differ slightly)? What should the user see when the agent gives up? How do you prevent the fix from over-triggering?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid raises max steps; senior reads the trace, fixes the error text and adds no-progress detection; staff adds the case to the regression set and a platform-level repeat-call guard.

### M4 seeds

**M4-Q1. "How do you manage memory for an agent running 100+ steps?"** `agents.memory-state` / Medium / AI-startup, global-product
- Ideal outline:
  - Treat the context window as scarce working memory with a budget; track tokens per step.
  - Clear or truncate stale tool results (keep references), summarise or compact older turns keeping decisions, open issues and identifiers.
  - Externalise notes to a scratchpad or file/DB the agent can re-read; store structured state separately from chat history.
  - Retrieval from long-term store for relevant facts instead of carrying everything.
  - Use prompt caching by keeping a stable prefix; compaction breaks the cache, so batch it.
  - Evaluate quality after compaction (information loss tests).
  - Mention subagents with clean contexts for subtasks.
- Weak: "use a bigger context window"; plain truncation of oldest messages; "vector DB" as the only answer.
- Follow-ups: What must never be summarised away? How do you test that compaction preserved key facts? How does compaction interact with caching cost?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid truncates or summarises; senior keeps pinned facts, externalises notes and tests information loss; staff weighs compaction against cache cost across the fleet.

**M4-Q2. "Design long-term memory for a personal assistant used by many users. What are the risks?"** `agents.memory-state` / Hard / global-product, Indian-product, AI-startup
- Ideal outline:
  - Memory types: episodic events, semantic facts/preferences, procedural rules; choose store (relational for facts, vectors for fuzzy recall, KV for profile).
  - Write policy: what is worth storing, extraction step, dedupe and conflict resolution, user visibility and edit/delete.
  - Retrieval: relevance, recency, importance scoring; inject limited tokens.
  - Isolation: per-user namespaces enforced in the data layer; tenant filters not left to the model.
  - Privacy: consent, PII minimisation, retention, deletion on request, encryption.
  - Poisoning: memories derived from untrusted content (web, emails) are untrusted; do not auto-store instructions.
  - Staleness: timestamps, confirmation, decay.
- Weak: "store all chats in a vector DB"; no isolation or deletion; trusts model-written memories.
- Follow-ups: How do you handle a fact that changes (moved city)? How do you comply with a deletion request including derived summaries and embeddings? How do you evaluate memory quality?
- Goal tag: `B:M A:M H:M`
- Senior-bar: mid designs a vector store; senior adds write policy, isolation and deletion incl. derived data; staff handles consent, audit, poisoning and DPDP/GDPR erasure end to end.

**M4-Q3. "How is agent memory different from RAG?"** `agents.memory-state` / Easy-Medium / IT-services, global-IT-consulting
- Ideal outline:
  - RAG: retrieves from an externally curated, mostly static corpus to ground answers.
  - Memory: agent- or user-specific information created during interaction, written by the system, mutable.
  - Overlap in mechanics (embedding retrieval) but different lifecycles: write policy, update, deletion, ownership.
  - Different trust: curated corpus vs self-written or user-supplied memories.
  - Different evaluation: retrieval relevance vs recall of correct, current personal facts.
  - Can combine: memory entries retrieved with the same infrastructure.
- Weak: says they are the same; only describes vector search.
- Follow-ups: Where would you put a user's uploaded documents? How do you version memory? What goes wrong if you treat memory as a corpus?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid says memory is RAG with history; senior separates lifecycle, trust and eval; staff decides shared infrastructure with separate governance.

### M5 seeds

**M5-Q1. "Describe the common workflow patterns and choose one for a customer-support triage system."** `agents.orchestration` / Medium / IT-services, global-IT-consulting, Big4
- Ideal outline:
  - Prompt chaining (fixed steps with gates), routing, parallelisation (sectioning/voting), orchestrator-workers, evaluator-optimiser.
  - Support triage: routing by intent and urgency to specialised handlers, cheap classifier first, escalation path on low confidence.
  - Parallel guardrail checks alongside the main response (sectioning).
  - Evaluator-optimiser only for high-stakes drafted replies with clear criteria.
  - Fallback route and human handoff.
  - Evaluate router accuracy separately from downstream quality; track misroute cost.
  - Keep it a workflow because categories are known; avoid agent autonomy for refunds without gates.
- Weak: lists names without mapping to the problem; picks multi-agent by default; no fallback.
- Follow-ups: What if the classifier is 92% accurate? How do you add a new category safely? How do you cost the pipeline per ticket?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid lists patterns; senior maps one to the problem with fallback and per-stage eval; staff costs per ticket and plans category evolution.

**M5-Q2. "Design a parallel fan-out over 20 documents with an LLM call per document. What can go wrong?"** `agents.orchestration` / Medium-Hard / Indian-product, global-product
- Ideal outline:
  - Use `asyncio` with a semaphore to cap concurrency to provider rate limits; per-call timeouts; retry with backoff and jitter on 429/5xx.
  - Partial failure policy: collect results with status per item; decide fail-fast vs best-effort; report incomplete items.
  - Idempotent, cacheable per-document calls; checkpoint finished items for resume.
  - Aggregation (map-reduce) step with context size limits; hierarchical reduce if needed.
  - Cost and token budget guard before launch; ordering and determinism of results.
  - Observability: per-item spans, tail latency.
  - Cancellation propagation (task groups).
- Weak: `gather` with no limits or timeouts; lets one failure kill all; ignores rate limits.
- Follow-ups: How do you pick the concurrency cap? What if the reducer input exceeds the context window? How do you make this resumable?
- Goal tag: `B:M A:M H:M`
- Senior-bar: mid uses gather; senior adds semaphore, timeouts, partial-failure policy and resume; staff sizes concurrency against shared provider quota across services.

**M5-Q3. "Explain orchestrator-workers vs prompt chaining. How do you prevent the orchestrator from over-decomposing?"** `agents.orchestration` / Hard / AI-startup, global-product
- Ideal outline:
  - Chaining: subtasks known in advance; orchestrator-workers: subtasks decided at runtime from the input (e.g. code changes across unknown files).
  - Orchestrator plans, delegates to workers with focused context, synthesises.
  - Control over-decomposition: cap number of subtasks and depth, give guidance on granularity, require justification, budget per worker.
  - Typed worker inputs and outputs; workers return condensed results.
  - Failure handling: worker failure triggers retry or replan; synthesis step validates completeness.
  - Cost: orchestrator plus N workers multiply tokens; compare against simpler chain.
  - Observability across the tree.
- Weak: describes both as "agents calling agents"; no limits; no cost awareness.
- Follow-ups: How do workers share necessary context without sharing everything? How do you evaluate decomposition quality? When does this beat a single agent with tools?
- Goal tag: `B:N A:M H:M`
- Senior-bar: mid contrasts static vs dynamic subtasks; senior caps depth and fan-out and types worker contracts; staff compares total cost against a single agent baseline.

### M6 seeds

**M6-Q1. "Single agent or multi-agent? Make the case for each."** `agents.multi-agent` / Hard / global-product, AI-startup
- Ideal outline:
  - Multi-agent benefits: context isolation (fresh window per subtask), parallel breadth on independent subtasks, specialised prompts/tools/models, permission separation.
  - Costs: token multiplication (Anthropic's June 2025 research-system write-up reports agents using about 4x and multi-agent systems about 15x the tokens of chat; one system's result, quote only with source and never as a law; verified 2026-10), coordination bugs, inconsistent assumptions, hard debugging, shared-state conflicts.
  - Works best for breadth-first, parallelisable, read-heavy tasks (research); weaker for tightly coupled work like many coding tasks.
  - Default to a single agent with good tools; split when context or permissions force it.
  - Typed contracts between agents, condensed returns, one owner of final output.
  - Evaluate with cost, latency and quality vs the single-agent baseline.
- Weak: "specialist agents are more intelligent"; role-play teams with no measurement; ignores cost.
- Follow-ups: How do you stop two agents editing the same resource? How do you debug a wrong final answer across 5 agents? What would make you collapse the design back to one agent?
- Goal tag: `B:N A:M H:M`
- Senior-bar: mid lists pros and cons; senior defaults to single agent and quotes the cost multiple with a source; staff owns the collapse-back criteria and measurement.

**M6-Q2. "Design human approval for an agent that can send emails and issue refunds."** `agents.hitl` / Medium-Hard / Big4, IT-services, Indian-product
- Ideal outline:
  - Risk-tier actions: read-only auto, reversible low-risk auto with logging, irreversible or financial gated by approval; thresholds by amount and recipient.
  - Enforce gates in server-side tool execution, not in the prompt or UI only.
  - Pause: persist run state and pending action, notify approver, resume on decision (approve, edit, reject with reason) via durable state.
  - Show the approver the exact action, evidence and agent reasoning summary; allow edit before approve.
  - Timeouts and escalation for unanswered approvals; expiry of stale approvals; idempotent execution after approval.
  - Audit log of who approved what; avoid approval fatigue via sensible thresholds and batching with care.
  - Feed rejections into evals.
- Weak: "add a confirm button"; approval is a prompt instruction; no resumability.
- Follow-ups: What if the world changed between request and approval? How do you stop the agent from splitting a large refund into small ones? How would you measure whether approvals are meaningful?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid adds a confirm button; senior enforces risk tiers server-side with durable pause/resume; staff measures approval quality and fatigue and handles split-refund evasion.

**M6-Q3. "How do you hand off context between agents without losing information or blowing the budget?"** `agents.multi-agent` / Medium / AI-startup, global-IT-consulting
- Ideal outline:
  - Define typed handoff payloads: goal, constraints, relevant facts, artefact references, open questions.
  - Pass references (ids, file paths, store keys) to large artefacts rather than content.
  - Subagent returns a condensed result plus evidence pointers, not its whole transcript.
  - Keep the full trace in observability, not in the next agent's context.
  - Validate handoff payloads against a schema; reject incomplete ones.
  - Test for information loss with end-to-end evals.
  - Preserve user identity and permissions across handoffs.
- Weak: "pass the whole conversation"; free-text summaries only; loses identity or permissions.
- Follow-ups: What belongs in a handoff vs a shared store? How do you detect telephone-game degradation? How are permissions narrowed for a subagent?
- Goal tag: `B:N A:M H:M`
- Senior-bar: mid passes summaries; senior uses typed payloads and references; staff defines permission narrowing and handoff schema governance.

### M7 seeds

**M7-Q1. "Your agent run takes 20 minutes and the pod gets restarted mid-run. What happens, and what should happen?"** `agents.reliability` / Hard / global-product, Indian-product, global-IT-consulting
- Ideal outline:
  - Naive: run state lives in memory and is lost; side effects may be repeated on retry.
  - Persist after each step: message history, step index, tool results, pending actions (checkpoint in DB or workflow engine).
  - Run as a queued job with a worker, lease or heartbeat, and visibility timeout; another worker resumes from the checkpoint.
  - Idempotency keys on side-effecting tools; record intent before execution (outbox) and outcome after.
  - Replay: reuse recorded tool and LLM results rather than re-calling live; resume rather than restart.
  - Durable-execution engines (Temporal-style) give this by deterministic workflow code plus activities (Temporal replays workflow code against a durable event history, so workflow code must be deterministic while side effects live in activities whose results are recorded; verified 2026-10 against Temporal docs); or a hand-rolled state machine in Postgres.
  - Handle in-flight "unknown outcome" steps via reconciliation.
  - Test with kill-and-resume chaos tests.
- Weak: "just retry the whole run"; "use Redis" without semantics; ignores duplicate side effects.
- Follow-ups: What if a tool call finished but the result was not saved? How do you version agent code while runs are in flight? What does the user see during recovery?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid says 'persist state'; senior covers leases, outbox, unknown outcomes and replay; staff covers code versioning of in-flight runs and recovery UX.

**M7-Q2. "How do you bound the cost and time of an autonomous agent?"** `agents.cost` / Medium / global-product, AI-startup, Indian-product
- Ideal outline:
  - Layered limits: max steps, max tool calls per tool, wall-clock deadline, max tokens and cost per run, per-user and per-tenant daily budgets.
  - Enforced in the loop code with pre-checks before each model call; remaining budget optionally shown to the model.
  - Graceful degradation at budget: summarise progress, return partial result, or ask the user to extend.
  - Model routing: cheaper model for easy steps; prompt caching; tool-result trimming.
  - Alerts and kill switch; anomaly detection on cost per task.
  - Track cost in traces per step.
  - Rate-limit and concurrency controls to avoid runaway fan-out.
- Weak: "set max_tokens"; only per-call limits; no tenant-level control.
- Follow-ups: What do you return when budget is exhausted halfway? How do you set the budget values? How do you detect a recursion in multi-agent delegation?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid sets max_tokens; senior layers step, time, cost and tenant budgets with graceful exit; staff adds anomaly alerts, kill switch and unit economics.

**M7-Q3. "Explain retries for LLM agents: what to retry, what not to, and how."** (generic backoff and jitter: `ref` X2 / A5.M4; this item covers agent-specific error classes only) `agents.reliability` / Medium / IT-services, global-IT-consulting, Indian-product
- Ideal outline:
  - Classify errors: transient (timeouts, 429, 5xx, overloaded) retry with exponential backoff, jitter, cap and honour Retry-After; permanent (4xx validation, auth) do not retry blindly.
  - Semantic failures (invalid args, wrong tool) are fed back to the model with a clear message, capped attempts; not blind retry with the same input.
  - Idempotency for side effects before enabling retries.
  - Retry budget so retries do not amplify load (retry storms); circuit breaker and fallback model/provider.
  - Timeouts at each layer; propagate deadlines.
  - Structured output failures: validate and repair with bounded attempts.
  - Observe retry rates.
- Weak: "wrap everything in a retry decorator"; ignores idempotency and jitter.
- Follow-ups: How do you avoid retry storms across many concurrent runs? When would you fail over to another provider and what changes? How do you retry a streaming response that failed halfway?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid says backoff; senior classifies errors and refuses blind retry of writes; staff designs retry budgets and breakers to prevent fleet-wide storms.

### M8 seeds

**M8-Q1. "How would you evaluate an agent, given it is non-deterministic?"** `agents.eval-observability` / Hard / global-product, AI-startup
- Ideal outline:
  - Define task success criteria and build a dataset of realistic tasks with expected outcomes or programmatic checks; seed from production traces and failures.
  - Evaluate outcome (state of the world after the run) in preference to exact transcript match; deterministic graders where possible.
  - Evaluate trajectory: correct tools, argument validity, step count, redundant calls, safety violations.
  - Component evals: router accuracy, tool selection, retrieval quality.
  - Repeat runs per task to measure variance; report success rate with spread and consistency (all-k-succeed) for reliability-critical tasks.
  - LLM-as-judge only with calibration against human labels (details in B5).
  - Run in CI as regression gate, in a sandbox with mocked or resettable tools; track cost and latency with quality.
- Weak: "try some prompts and see"; single-run pass/fail; compares transcripts verbatim.
- Follow-ups: How many repeats do you need? How do you build an environment that resets between runs? How do you stop overfitting prompts to the eval set?
- Goal tag: `B:N A:M H:M`
- Senior-bar: mid says 'run it a few times'; senior grades outcomes, trajectories and variance; staff builds the production-trace-to-regression pipeline with CI gating (B5 method).

**M8-Q2. "A customer says the agent gave a wrong answer yesterday. How do you investigate?"** `agents.eval-observability` / Medium / IT-services, global-IT-consulting, Indian-product
- Ideal outline:
  - Find the run by id; open the full trace: prompts (versions), model and parameters, each tool call with args and results, retrieved context, latency and tokens.
  - Locate the first wrong step (bad retrieval, tool error, misread result, context truncated, injection).
  - Reproduce with recorded inputs and tool outputs (replay); check versioning of prompts and tools.
  - Fix at the right layer (tool, prompt, retrieval, guard) and add the case to the regression set.
  - Privacy: trace access controlled; PII redacted; retention policy.
  - Close the loop with release and monitoring for recurrence.
- Weak: "re-run it and see"; no trace data retained; prompt tweak without regression test.
- Follow-ups: What do you log by default and what is too sensitive? How do you reproduce when tools hit live systems? How would you detect this class of failure proactively?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid re-runs the prompt; senior finds the first wrong step in a versioned trace and replays; staff adds proactive detection and access/retention policy.

**M8-Q3. "Where do the tokens and money go in an agent, and how do you cut cost without hurting quality?"** `agents.cost` / Medium-Hard / global-product, Indian-product, AI-startup
- Ideal outline:
  - Measure first: cost per task broken down by step, by input vs output tokens, by tool-result tokens, by model.
  - Context re-sent on each loop iteration means cost without caching grows roughly quadratically with steps; trimming tool outputs and compaction help.
  - Prompt caching on stable prefixes (system prompt, tool definitions) cuts input cost and latency (cache semantics verified 2026-10 against one provider's public docs, dated provider-note material: the cached prefix is ordered tools, then system, then messages; reads are priced below base input and writes above it, with 5-minute and 1-hour TTLs; exact prices and minimum lengths differ per model and are not stated here, see B2 provider notes).
  - Model routing: small model for classification and simple steps, larger for planning.
  - Reduce steps: better tools, batch calls, parallel independent calls, early exit.
  - Cache deterministic tool outputs; use batch APIs for offline tasks.
  - Verify with eval set that success rate and latency do not regress; set cost-per-successful-task as the metric.
- Weak: "use a cheaper model"; no measurement; sacrifices quality unknowingly.
- Follow-ups: What is cost per successful task and why use it? How does caching interact with changing tool lists? When is a bigger model cheaper overall?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid says 'cheaper model'; senior profiles tokens per step and uses cost per successful task; staff sets per-tenant budgets and routing policy.

### M9 seeds

**M9-Q1. "A tool result or an MCP tool description tells your agent to call another tool with the user's data. What happens, and how does the agent runtime stop it?"** (injection concepts and the RAG-assistant case are owned by B5.M4 / Q4.1: `ref`) `agents.security` / Hard / global-product, Big4, AI-startup
- Ideal outline:
  - Agent-specific vector: instructions arrive through tool output, tool descriptions (tool poisoning), retrieved memory or another agent's message, and can trigger further tool calls with the user's authority; each hop in the loop widens the blast radius.
  - Runtime controls: treat every tool result and description as untrusted data, pin and review MCP server tool descriptions, require explicit approval for new or changed tools, gate side-effect tools behind human approval, block tool chains that combine untrusted content with private data and an outbound sink.
  - Isolation: quarantined step with no tools that returns structured output to the privileged loop; loop step and tool-call budgets.
  - Tests and monitoring: inject through tool output in the eval harness, alert on unusual tool sequences.
- Weak: "add 'ignore malicious instructions' to the system prompt"; treats it as a RAG-only problem.
- Follow-ups: Walk through the attack on a mail-summarising agent with a send-email tool. How do you treat tool results from your own internal services? How would you test your defences?
- Goal tag: `B:S A:M H:M`
- Senior-bar: mid says 'add a guard prompt'; senior breaks the lethal trifecta architecturally; staff designs privileged/quarantined splits and a standing red-team cadence (B5 depth).

**M9-Q2. "How do you apply least privilege to an agent acting on behalf of users?"** `agents.security` / Medium-Hard / Big4, IT-services, global-product
- Ideal outline:
  - Agent acts with the end user's delegated, scoped authority (OAuth on-behalf-of, short-lived tokens), not a shared service account with broad rights.
  - Authorisation enforced in tools and backend APIs, never by the model's judgement; the model cannot widen scope.
  - Per-tool, per-task scoping and read-only defaults; allow-lists of resources and domains; rate and amount limits.
  - Separate credentials from the model context; never place secrets in prompts or traces.
  - Sandbox code execution (no network by default, resource limits, ephemeral filesystems).
  - Audit trail attributing actions to user and agent run; revocation and kill switch.
  - Multi-tenant isolation tests.
- Weak: "give it an API key"; relies on the prompt saying "only do X"; secrets in logs.
- Follow-ups: How does a confused-deputy issue arise here? How do you handle an action needing broader rights than the user has? How do you test authorisation boundaries?
- Goal tag: `B:M A:M H:M`
- Senior-bar: mid uses scoped API keys; senior uses delegated per-user tokens enforced in tools; staff handles revocation, audit attribution and confused-deputy testing.

**M9-Q3. "Should we build on an agent framework or write our own loop? How do you decide?"** `agents.frameworks` / Medium / IT-services, global-IT-consulting, AI-startup
- Ideal outline:
  - Core loop is small; start by understanding it with the provider SDK directly; frameworks add abstractions that can obscure prompts and calls.
  - Criteria: need for durable state and resumability, graph or branching control, human-in-the-loop primitives, tracing integration, ecosystem and connectors, team familiarity, lock-in and API stability, testing support.
  - Landscape overview (graph-based orchestration, provider agent SDKs, role-based crews, typed-Python agent libraries, vendor platforms) (status verified 2026-10, see the section 6 table).
  - Check maintenance activity, release stability, license, how easy it is to inspect raw model requests, and exit cost.
  - Possible hybrid: framework for orchestration and persistence, own code for tools and prompts.
  - Prototype both on a real task and compare against the eval set.
  - Revisit as models improve and make scaffolding unnecessary.
- Weak: picks by popularity; "frameworks are bad" or "always use X"; cannot explain what the framework does underneath.
- Follow-ups: What do you do if the framework hides the prompt it sent? How do you keep tools portable across frameworks? How would you migrate away?
- Goal tag: `B:N A:S H:M`
- Senior-bar: mid picks by popularity; senior reads the framework against own loop with criteria and exit cost; staff writes the build-vs-adopt memo and migration plan.

### Senior-bar probe sets (2 staff-level probes per module)

| Module | Staff probe 1 | Staff probe 2 |
|---|---|---|
| M1 | Your org has 14 teams building agents with no shared rubric. Write the decision policy for when a team may use autonomy and the evidence they must bring. | Per-step success is 0.97 but measured end-to-end success over 8 steps is 0.80. Explain both numbers and decide where to invest. |
| M2 | Design the versioning and deprecation policy for tool schemas that 30 agents depend on. | Your tool catalogue grows to 200 tools. Choose between consolidation, tool search and per-task tool sets, and justify with an eval. |
| M3 | A newer model makes your elaborate planner unnecessary. How do you decide, safely, to delete scaffolding? | Reflection added 2 points of success at 2.4x cost. Who decides whether it ships, and on what metric? |
| M4 | A user invokes erasure rights. Trace every place their data lives (summaries, embeddings, traces, caches) and how you prove deletion. | Design conflict resolution when two memories disagree and one came from an untrusted web page. |
| M5 | Provider quota is shared by five services. Allocate concurrency to your fan-out pipeline without starving the others. | Show with arithmetic when a routing layer pays for itself and when misroute cost exceeds the saving. |
| M6 | Argue to a VP why you are collapsing a 6-agent design back to one agent. What data do you show? | Design approvals for 5,000 requests a day without approval fatigue, and detect rubber-stamping. |
| M7 | Roll out a new agent code version while 2,000 runs are in flight. What breaks and how do you migrate? | Write the runbook for 'duplicate refunds were issued': detection, containment, compensation, prevention. |
| M8 | Build the pipeline that turns production failures into regression cases weekly without leaking PII. | Pick 3 dashboards and 3 alerts for an agent platform and defend each. |
| M9 | Design a privileged/quarantined architecture for a mail agent and state exactly what remains unprotected. | A team wants to install a community MCP server by Friday. Write the vetting checklist and the fast path. |

### Production-incident scenarios (3)

**INC-1. Runaway loop.** A support agent burns a tenant's monthly budget in 40 minutes: it calls a search tool whose error says only "failed", and the model retries with slightly rephrased arguments. `agents.cost` / Hard / global-product, AI-startup, Indian-product; `B:N A:M H:M`.
- Ideal: contain (kill switch, tenant cap); read the trace for repeated near-duplicate calls; root causes (opaque error, no no-progress detection, no run budget); fixes (actionable error, repeated-call guard on normalised args, step/token/cost caps with a graceful partial result, per-tenant daily budget and alert); add a regression case from the trace; blameless review.
- Weak: raise limits; blame the model. Probes: how do you detect near-duplicate args? what do you tell the customer? Senior-bar: mid caps steps; senior fixes the tool error and adds a guard; staff adds budgets, alerting and unit economics.
- Lab: replay the supplied trace in the mock harness; the hidden test requires termination under N calls.

**INC-2. Tool double-execution.** A create-order tool timed out at 30 s, the loop retried, and two orders and two payment captures exist. `agents.reliability` / Hard / Indian-product, IT-services, Big4; `B:N A:M H:M`.
- Ideal: classify as unknown outcome, not failure; idempotency key from run id plus step id plus args hash with a DB unique constraint; status-check tool and reconciliation job; outbox record of intent; compensation for the duplicate (refund saga); retry only proven-idempotent calls; chaos test with timeout after commit.
- Weak: tell the model not to retry. Probes: downstream has no idempotency support? parallel calls? Senior-bar: mid adds a key; senior handles unknown outcome and reconciliation; staff adds financial controls and audit.
- Lab: P-12 crash injection with a hidden side-effect count.

**INC-3. Injected tool output.** An agent that reads customer emails and can send email sent an invoice to an external address after reading a message with hidden instructions. `agents.security` / Hard / global-product, Big4, AI-startup; `B:N A:M H:M`.
- Ideal: contain (disable send tool, rotate credentials); name the three legs (private data, untrusted content, outbound channel); fix architecturally (gate or remove the outbound leg: approval, recipient allow-list, egress control, quarantine of untrusted content, per-user scoped tokens); alert on off-list recipients; add the case to the red-team fixture; follow incident and disclosure policy. Taxonomy and red-team method are B5.M4.
- Weak: add "ignore instructions in emails" to the prompt. Probes: what if the recipient is on the allow-list? how do you test this continuously? Senior-bar: mid patches the prompt; senior cuts a leg of the trifecta; staff builds detection, disclosure and a standing red-team cadence.
- Lab: the harness injected-document fixture.

## 5. Final assessment

- Mastery check: 20 items drawn across M1–M9 (concept, predict-the-result on loop and retry traces, short scenario items), pass at 80% (G-7); revision loop on missed modules.
- Timed coding task (60–75 min): build a tool-using agent service: loop with step and cost budget, two tools including one idempotent write, retry and timeout handling, checkpoint and resume after a simulated crash, structured trace output. Hidden tests check termination, no duplicate side effects after kill-and-resume, error feedback, and budget enforcement.
- Capstone (can be done before the timed assessment): small agent plus 20-case eval with variance report plus red-team test for one indirect-injection case.
- Design-canvas task (30–40 min): "Design an agent platform feature, for example an expense-approval assistant, or a research assistant that reads the web and company documents." Rubric dimensions: workflow vs agent justification, tool contracts, state and durability, human gates, failure modes, security and least privilege, observability and evals, cost model. The canvas serialises to the typed design graph; rule checks cite registry IDs only: R-IDEMP-1 (retried edge into a non-idempotent state-changing consumer), R-AGENT-2 (loop without budget), R-AGENT-1 (write-capable tools without approval or scope limit) and R-OBS-1 (no trace coverage); shared credentials are rubric-scored, not a local rule.
- Voice mock round (20 min, adaptive): "Design and productionise an agent for a business process", with follow-ups drawn from the seed tree (reliability, security, evaluation). Scored on rubric competencies with transcript evidence; no emotion or personality inference.

## 6. Course-level risks and freshness notes (facts as of October 2026)

Verification: web search of vendor docs and secondary summaries, 5 October 2026. Facts stay in dated provider notes, not lesson text (G-8, G-12).

| Item | Status (Oct 2026) | Source type |
|---|---|---|
| MCP specification | Revision 2026-07-28 is stateless (no initialize handshake or session id); transports stdio and Streamable HTTP; OAuth-aligned authorisation with issuer-validation hardening; Roots, Sampling and Logging features, the HTTP+SSE transport and Dynamic Client Registration are marked Deprecated; verified 2026-10 | Official spec changelog; re-check each review |
| A2A | v1.0 released March 2026 (Google Open Source Blog, 16 April 2026); hosted by the Linux Foundation since June 2025; the earlier claim of Agentic AI Foundation membership was not confirmed and is removed; verified 2026-10 | Vendor blog and secondary reports |
| LangGraph | 1.0 GA October 2025 (PyPI 1.0.0 on 2025-10-17; latest 1.2.12 on 2026-09-21; verified 2026-10); no 2026 feature additions are claimed or taught (lessons depend only on the 1.0 concepts of state graph, checkpointer and interrupt) | Vendor and secondary reports; engineering picks the code-reading framework |
| OpenAI Agents SDK | Lightweight Python SDK (first PyPI release March 2025), provider-agnostic through a model interface, tracing and guardrails; still pre-1.0 (latest 0.23.1 on 2026-10-02), so expect API churn; verified 2026-10 | PyPI and GitHub releases |
| Microsoft Agent Framework | Successor to AutoGen and Semantic Kernel (announced October 2025), Python and .NET 1.0 GA on 2 April 2026 (latest 1.20.0 on 2026-10-02); AutoGen maintenance-only since 2 October 2025; verified 2026-10 | PyPI and secondary reports |
| Google ADK | Python 2.0 GA on 19 May 2026 (PyPI; latest 2.11.0 on 2026-10-02), moves to a graph-based workflow runtime with breaking changes from 1.x; Go and TypeScript 2.0 releases reported mid-2026 by secondary sources; verified 2026-10 | PyPI, vendor docs, secondary reports |
| CrewAI, Pydantic AI, LlamaIndex, Claude Agent SDK | Active on PyPI as of 2026-10-05 (crewai 1.15.23, pydantic-ai 2.54.0, llama-index-core 0.14.25, claude-agent-sdk 0.2.163); versions not pinned in lessons; verified 2026-10 | PyPI; sidebar only |
| Multi-agent token cost | Anthropic's 2025 research-system write-up reports agents about 4x and multi-agent about 15x chat tokens, with token usage explaining about 80% of variance on its browsing benchmark. Quote only with source and as one system's result, never as a law | Vendor engineering blog via secondary summaries |
| "Lethal trifecta" | Term coined by Simon Willison, post dated 16 June 2025; verified 2026-10 | Author blog |
| OpenTelemetry GenAI conventions | GenAI conventions moved to a dedicated repository in June 2026 and were still Development status in August 2026; naming guide only; verified 2026-10 | Secondary reports |
| Provider tool search and deferred tool loading | Anthropic tool search tool and OpenAI Responses API tool_search both use defer_loading to load tool definitions on demand; verified 2026-10 (feature names only; limits not stated) | Vendor docs via search |
| Prompt caching, thinking/effort parameter names, prices | Pointer to B2 provider notes; not hard-coded here | B2 data file |
| Temporal-style engines | Conceptual only in 1.0 | n/a |

- Stable (12-month shelf life): loop anatomy, workflow patterns, idempotency, budgets, state machines, least privilege, eval design.
- Teaching risk: over-indexing on framework syntax. Mitigation: thin provider-agnostic loop in all labs; the framework lesson is a code-reading and design exercise with no install (R-B4-2).
- Graded labs use the deterministic mock model (P-07) and crash-injection API (P-12); live mode never gates badges.
- Overlap: B2 owns single-call tool use and caching; B3 retrieval; B5 eval methodology, judge, injection, red-team, guardrails, PII and LLM observability; B6 voice; A2 async; A5 generic retry/idempotency; A6 generic distributed design. B4 supplies the orchestration-platform design seed to A6.
- Remaining **[verify]**: none open; the earlier LangGraph feature-addition marker was removed because no lesson depends on those features. Cadence every 3 months; next review January 2027.

## 7. Open questions for the boss agent

All earlier questions were ruled (R-B4-1..4). Remaining: (1) confirm engineering's choice of the one graph-style framework for the code-reading lesson (R-B4-2 `[verify currency]`: at selection, check the framework's current major version and that its checkpointer and interrupt APIs still match the M9.4 reading guide) and that the harness supplies its sample agent fixture; (2) confirm the injected-document fixture and crash-injection API ship with the P-07/P-12 release that B4 M7 and M9 depend on.

## 8. Learner-Fit Appendix

Tag key: B = Senior Python Backend, A = AI Engineer, H = hybrid; M/S/N = MUST/SHOULD/NICE (G-3). Per boss ruling, M1-M7 are `A:M H:M B:N` and M8-M9 are `A:M H:M` (B:N). Backend learners take the NICE primer slice: M1.2-.3, M2.3, M7.1-.2. Seed-question tags may be higher than the module tag where a question is a backend-relevant reliability item. Cluster emphasis: IT-services and Big4 probe M6 approvals, M7 reliability, M9 least privilege; product and AI-startup probe M3, M5, M8, M9.

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge |
|---|---|---|---|---|
| L1 | Simpler 15.5 h; all lessons; B2 and A2 remediation open; scripted mock-model step-throughs | 60% guided labs, 40% items | Nothing | None until mastery >= 80% |
| L2 | Simpler 15.5 h; skim tool-call basics | 50% labs, 30% items, 20% verbal | M1.1 if band >= 85 (G-5) | Optional M5 extension |
| L3 | Deeper 10.5 h; skim M1 and M2 basics, study M2 (design and trust), M5, M7, M8, M9 in full (matches Learner Journey gap list) | 50% applied labs, 20% items, 30% voice | Per bands | One Challenge per MUST module |
| L4 (reference learner) | Deeper about 8-9 h after skips; straight to mastery check where scored; saved time goes to M7, M8, M9 and incident drills | 30% labs, 70% senior-bar probes and voice | M1.1-.2, M4.1 likely (M2 trust and idempotency stay in scope) | Staff probes, INC-1..3, adversarial fixtures |

### LF-2 Module matrix

Six diagnostics per module (tag `placement`, drawn from MCQ and predict buckets). Bands: >=85 skip lessons and take mastery check; 70-84 skim (Deeper-only lessons); 50-69 study; <50 deep-study with prerequisite remediation (G-5). No band waives the final assessment; M7-M9 mastery checks are never skipped.

| Mod | Diagnostics P1-P6 | B/A/H | Weight % B / A / H | Rehearsal modes | Module readiness (G-6) |
|---|---|---|---|---|---|
| M1 | P1 workflow vs agent for a fixed 5-step process; P2 stop conditions; P3 0.9^8; P4 tool error as message; P5 parallel result ordering; P6 when a single call wins | B:N A:M H:M | 9 / 10 / 10 | voice drill | 6 scenarios classified >= 5 with reasons; voice >= 3.5 on `agents.architecture-choice` |
| M2 | P1 description as prompt; P2 enum vs free text; P3 idempotency key source; P4 oversized output; P5 MCP roles; P6 MCP vs function calling | B:N A:M H:M | 13 / 12 / 12 | coding, voice drill | selection accuracy gain on eval; MCP smoke tests; voice >= 3.5 |
| M3 | P1 plan vs ReAct; P2 reflection without verifier; P3 replan cap; P4 stale plan; P5 thinking budget; P6 stuck detection | B:N A:M H:M | 7 / 10 / 9 | voice drill | verifier agent beats baseline; voice >= 3.5 |
| M4 | P1 what compaction keeps; P2 memory vs RAG; P3 write policy; P4 isolation; P5 deletion; P6 poisoned memory | B:N A:M H:M | 8 / 11 / 10 | canvas, voice drill | isolation and delete tests; compaction quality; voice >= 3.5 |
| M5 | P1 pattern selection; P2 semaphore and timeouts; P3 partial failure; P4 evaluator stop; P5 router fallback; P6 over-decomposition | B:N A:M H:M | 12 / 12 / 12 | coding, canvas, voice drill | fan-out hidden tests; triage canvas >= 3.5 |
| M6 | P1 multi-agent cost; P2 handoff payload; P3 server-side gate; P4 resume after restart; P5 approval timeout; P6 approval fatigue | B:N A:M H:M | 10 / 10 / 10 | canvas, voice drill | gate survives restart; comparison report; voice >= 3.5 |
| M7 | P1 retryable errors; P2 unknown outcome; P3 checkpoint contents; P4 replay determinism; P5 saga; P6 budget layering | B:N A:M H:M | 17 / 13 / 15 | coding, voice drill, canvas | zero duplicate side effects over N kill points; voice >= 3.5 at Senior bar |
| M8 | P1 trace fields; P2 outcome vs transcript eval; P3 variance and repeats; P4 cost per successful task; P5 redaction; P6 trace to regression | B:N A:M H:M | 9 / 12 / 11 | voice drill, coding | eval report with variance; measured cost cut; voice >= 3.5 |
| M9 | P1 trifecta legs; P2 indirect injection path; P3 delegated least privilege; P4 egress; P5 MCP supply chain; P6 build vs framework | B:N A:M H:M | 15 / 10 / 11 | canvas, voice drill | red-team fixture blocked; canvas free of R-AGENT-1/2; voice >= 3.5 |

Weights sum to 100 per target (B, A and H columns each checked). Rationale `[judgement]`: backend interviews probe reliability, idempotency and security; AI Engineer interviews probe patterns, evals, memory and cost.

### LF-3 Experience bridge

| Mod | You already know | Gap between using it and explaining it |
|---|---|---|
| M1 | Chatbot tool-use loop, voice-agent turn loop | Naming stop conditions, compounding error, when a workflow beats an agent |
| M2 | Function schemas, REST API design | Descriptions as prompts, output trimming, unknown-outcome idempotency, MCP trust model |
| M3 | Prompting for step-by-step answers | Verifier-backed reflection, bounded replanning, honest measurement |
| M4 | Redis sessions, Postgres, chat history | Compaction fidelity tests, write policy, per-user erasure incl. embeddings |
| M5 | Celery and asyncio fan-out, queues | Partial-failure semantics and per-branch timeouts for LLM calls, routing economics |
| M6 | Approval workflows, RBAC | Server-side gates, durable pause and resume, approval fatigue, multi-agent cost |
| M7 | Retries, idempotency, outbox, Lambda timeouts | Applying them to nondeterministic loops: replay from recorded outputs, kill-and-resume proof, in-flight versioning |
| M8 | CloudWatch or OTel, structured logging | Trajectory evals, variance, cost per successful task, safe prompt logging |
| M9 | OAuth, secrets, least privilege | Instructions and data share one channel; trifecta; confused deputy for agents |

### LF-4 Senior-bar framing

Per-question lines in section 4 and the staff probe table state mid vs senior vs staff. Summary: mid describes the pattern; senior states trade-offs, failure modes, measured evidence and cost; staff sets policy (budgets, approval tiers, tool contracts, migration, incident response) across teams.

### LF-5 Adaptive hooks

| Module | Remediation links | Challenge extension | Reorder or defer |
|---|---|---|---|
| M1 | B2.M5 | Compounding-error model with correlated failures | May move first or skim |
| M2 | B2.M4-M5, A5.M4 | Tool-schema versioning policy | After M1 |
| M3 | M1, M2 | Reflection cost-benefit study | Deferrable |
| M4 | B2.M7, B3.M1 | Erasure proof across derived data | With M3 |
| M5 | M1, A2.M5 | Cost model per pattern | After M2 |
| M6 | M5 | Approval-quality metrics | Deferrable |
| M7 | M1, A5.M4, A2.M9 | Rolling upgrade of in-flight runs | Core; not deferrable |
| M8 | M1, B5.M1-M2 | Trace-to-regression pipeline | Before M9 |
| M9 | M2, B5.M4 | Privileged/quarantined design | Core; not deferrable |

Thresholds: module mastery 80%; readiness per G-6 (retake >= 7 days later at >= 85%, two voice sessions >= 48 h apart at Senior bar). The adaptive engine may reorder M3, M4, M6.

### LF-6 Applied labs

M1: classify and build the loop for an IoT-fleet operations assistant. M2: refactor a SaaS billing tool set and build a small MCP server. M3: code-fixing agent with a test-runner verifier. M4: personal-assistant memory with erasure. M5: support-ticket triage and a 20-document fan-out. M6: refund agent with enforced approvals surviving restart. M7: kill-and-resume order-processing agent (P-12). M8: instrument and evaluate the M1 agent with variance and a measured cost cut. M9: mail-agent threat model, red-team fixture and capstone hardening.

### LF-7 Currency and quality bar

All modules stamped October 2026. Analogies: M1 a junior engineer handed a ticket vs a runbook; M2 an API for a colleague who only reads the docs; M3 a student who cannot mark their own exam; M4 a desk with a notebook and an archive; M5 a kitchen pass; M6 a surgeon needing sign-off; M7 a game save point; M8 a flight recorder; M9 a mail clerk who obeys any note in the envelope. Top misconceptions: M1 agents always better, no limits, independent errors; M2 wrap every endpoint, retry creates, MCP as security boundary; M3 reflection fixes errors alone, unbounded replans, CoT is faithful; M4 stuff the context, store raw chats, no deletion; M5 orchestrator when a chain suffices, no per-branch timeout, evaluator with no criterion; M6 more agents is smarter, UI-only gates, bulk approval; M7 retry writes, in-memory state, replay hits the live model; M8 one-run evals, uncalibrated judge, optimise before measuring; M9 prompt guardrails suffice, shared admin credential, pick framework by stars.

## 9. Boss compliance log

- Item 1: tags in LF-2. Item 2: labs on P-07 thin loop, no installs. Item 3: M8 agent-specific scope with B5.M1-M3 pointers. Item 4: M9 security design, B5.M4 pointer, R-AGENT-1/2 canvas capstone, A6 seed noted. Item 5: M7 lab on P-12, Temporal conceptual. Item 6: M2 owns MCP; spec revision and A2A verified (section 6). Item 7: frameworks, token figure and trifecta attribution verified 2026-10 (section 6). Item 8: LF-3 bridge and INC-1..3. Item 9: goal tag and senior-bar note on all 27 seeds plus two staff probes per module.
- G-1..G-13 and R-B4-1..4 applied (ownership row, effort 15.5/10.5 plus 2.5, bands, readiness, 80% final mastery, no model names, tags, question rules, LF-1..LF-7, freshness).
- Track lead B review (2026-10-06): see `_reviews/TrackLead_B_Report.md`. Applied: header primer list aligned to LF; explicit G-7 badge thresholds; code-as-action/computer-use and context-engineering currency lines; tool-search source label corrected; stray R-B2-2 reference removed; pass^k and no-cache cost wording; M7-Q3 X2 ref; LF-1/LF-2/LF-5 fixes (band 85, L3/L4 skips, A5.M4, M5 voice drill); framework-currency check named in section 7.
