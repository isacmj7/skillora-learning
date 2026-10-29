---
name: C2 Project Deep-Dive and Resume Defence
course_id: C2
track: C (Interview Craft)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
precedence: Where this file contradicts the Boss Decisions document, that document wins.
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-20 (see _reviews/Boss_Final_Approval.md)
---

# C2 — Project Deep-Dive and Resume Defence (Track C)

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | C2 — Project Deep-Dive and Resume Defence / Track C (Interview Craft, Senior) |
| Audience | Senior engineers (about 8+ years total) whose interview loops will include one or more rounds that are essentially "walk me through your project and defend your resume". Written for a Senior Python Backend or AI Engineer profile, but the method is stack-neutral. |
| Prerequisites | A resume (or draft) with at least 3 real projects the learner personally worked on. Working knowledge of the learner's own stack. C1 is a required prerequisite (solid edge C1 to C2): C1.M1-M3 (frameworks, competency map, story bank) before C2.M1, with the rest of C1 continuing as a parallel thread. Recommended, not required (dotted edges): A5, A6 and B6 for architecture and voice vocabulary; B2 for LLM vocabulary. |
| Placement skip | Per G-5 bands on each module's diagnostics. In addition, L4 learners are skip-ahead on M1 and M2 when the Profile Facts Ledger shows confirmed status and at least one project already passes the "5-level why" drill at 80%+. Skipping never skips the final assessment (G-5). M5 is personalised: only technologies the learner lists are drilled; mastered ones are skipped. |
| Claim ledger (R-C2-1, P-20) | The single claim ledger is the **Profile Facts Ledger**, a profile-level entity (roles, dates, team sizes, metrics, projects, confirmed claims) shared with C1 consistency checks, C4 role-plays and Interview Master. Confirmation is one onboarding step used by all interview types. **C2 is a workshop that teaches the method and reads and writes the ledger.** Consent and deletion note: the ledger is optional, learner-owned, stored only with explicit consent, exportable and deletable at any time (deleting removes all derived probes and stops claim-risk reports); it is never used for model training and never shown to other learners; the platform never asks for proprietary code or data. |
| Outcomes (measurable, each tied to a competency) | 1. Build a claim ledger from the resume and classify every claim as defensible, needs-evidence or remove/reword (`resume.claim-audit`). 2. Deliver any project at 30 s, 3 min and 10 min depth with a consistent spine, within time limits and with the 10-minute version containing context, constraints, role, architecture, decisions, results, failure and hindsight (`project.narrative`). 3. Defend a design decision through 5 levels of "why" without contradiction, naming at least one rejected alternative and its cost (`project.tradeoffs`). 4. Answer probes on every technology listed on the resume at the depth the stated years of experience implies, and say "I have not used that in production" cleanly when true (`resume.tech-probe`). 5. State impact with honest, attributable numbers, or an honest qualitative statement when no numbers exist, and survive "how did you measure that?" (`project.impact`). 6. Separate team contribution from personal contribution using "we" and "I" precisely, without under-claiming or over-claiming (`project.narrative`; `behav.ownership` is attached as secondary, credit owned by C1). 7. Handle confidentiality limits by abstracting a project without losing technical substance (`project.confidentiality`). 8. Reproduce your own architecture on a whiteboard or the canvas in 5 to 8 minutes and answer "what if this component fails or load grows 10x" using it (`design.own-architecture`). |
| Estimated effort | **12 h Simpler / 8 h Deeper** module time (matches the boss effort table) plus **2.5 h** for final assessment, voice mocks and revision loops. Simpler is the L1-L2 route with full scaffolding; Deeper is the L3-L4 route with foundations skimmed per placement and stated net of typical skip-ahead, so it is shorter in wall-clock hours but harder per hour (G-2). Per-module hours are in LF-1/LF-2 and sum to the header. Excludes the learner's own writing time; effort scales with the number of projects (default plan: 3). |
| Interview coverage tags (owned, issue credit) | `resume.claim-audit`, `resume.tech-probe`, `project.narrative`, `project.tradeoffs`, `project.impact`, `project.failure`, `project.confidentiality`, `design.own-architecture`, `comm.multi-depth`. Tags use hyphens per G-9. |
| Secondary tags (no credit) | `behav.ownership` (credit owned by C1; M7 attaches it as secondary), A6 canvas rule IDs (R-*, cited only from the A6 registry; no tag credit) |
| Company-style clusters served | IT-services (project explanation, delivery, onsite-offshore roles), global-IT-consulting (solution and client-facing framing), Big4 (structured, stakeholder-aware narrative), global-product (depth and ownership), Indian-product (speed, scale, impact), AI-startup (what you personally built and shipped) |
| Badge and certificate criteria | Course badge "Resume-Defence Ready" requires: all 9 module badges; a confirmed claim ledger with no unresolved high-risk claims; at least 2 projects each passing the final voice deep-dive at the "Meets" bar (average >= 3.0, none < 2; strict mode, no hints; the Senior bar of 3.5 is the separate readiness criterion); one project rendered on the canvas and defended; final mastery check at 80%+. Evidence is stored as the learner's own project artefacts and scored transcripts. The badge states what was assessed (practice rubric) and carries no hiring claim. |
| Goal tags (G-3 route multipliers) | Course `B:M A:M H:M` (MUST for Senior Python Backend, AI Engineer and hybrid). Module-level tags in LF-2. |
| Authoring owner / reviewer | Authoring owner: SME agent C2. Reviewer: to be assigned. |
| Review cadence | Stable method content every 12 months. AI/voice archetype examples and technology-version notes every 3 months. Interview Master integration notes whenever the claim-risk feature changes. Last reviewed October 2026. |

### Authoring rules specific to this course

- **No invented specifics.** The four archetypes below are generic worked examples. Every number, name and incident in a lesson is a labelled placeholder (for example `[your metric]`) or an explicitly fictional example marked as such. Never present sample numbers as real outcomes or as the learner's.
- **The learner's real projects are the content.** The course is a workshop: lessons teach a technique, the learner applies it to their own ledger, the platform scores the result. Worked examples must be short and clearly generic.
- **Honesty is a scored criterion, not a tone.** The rubric penalises inflated claims, "we" used to hide non-involvement, and fabricated metrics at least as heavily as it penalises vague answers.
- **Confidentiality.** The platform never asks learners to paste proprietary code or data. Lessons teach abstraction (see M7). Learners are reminded that they remain bound by their own NDAs.
- **Generic archetypes only (R-C2-5).** Shared content uses only the fictional archetypes below. Any real project of any learner (including the founder's) lives solely as private, learner-owned data in that learner's account and is never seeded into the product or examples. The experience bridge in section 8 teaches the method with archetypes, not with named real systems.
- **Pointer rule (G-1).** C2 owns project-level narrative, the claim-ledger method, the 5-level why, multi-depth delivery (including non-technical audiences) and own-architecture defence. It does not own the spoken career narrative or generic STAR failure and conflict stories (C1), the positioning thesis (C4), the seven-step coding loop (C3), or system design from scratch (A6).

### The four archetypes used as worked examples

| Code | Archetype (generic) | Typical technical surface | Typical probe magnets |
|---|---|---|---|
| ARC-1 | Python API backend with an AI assistant feature | FastAPI, PostgreSQL, Redis, background jobs, LLM provider API, prompt design, auth | Where does the LLM sit in the request path; sync vs async; cost and latency; hallucination handling; what happens when the provider is down; how prompts are versioned and tested |
| ARC-2 | Voice agent platform | Streaming STT, LLM, TTS, WebSocket or telephony transport, turn-taking, call state | Latency budget, barge-in, interruption handling, STT errors, concurrency per worker, cost per minute, fallback to human |
| ARC-3 | Django IoT access-control system using MQTT | Django, MQTT broker, device auth, event ingestion, door or lock commands, audit logs | Delivery guarantees (QoS levels), offline devices, retained messages, command acknowledgement, idempotency, security of device identity, time sync, audit trail |
| ARC-4 | Enterprise field-service platform | Scheduling, routing, mobile clients, work orders, multi-tenant data, notifications, reporting | Multi-tenancy, offline mobile sync, conflict resolution, scheduling and optimisation, role permissions, data model for jobs and visits, reporting performance |

## 2. Module table

| # | Module | Module outcome (testable) | Key topics | Visuals | Practice tasks | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Claim inventory and project selection | Produce a claim ledger where each resume line is tagged with evidence, risk and a one-sentence defence, and choose 3 flagship projects. | Claim types (technology, scale, role, impact, leadership, duration); evidence types (code you wrote, decision you made, incident you handled, metric you saw); risk scoring factors (specificity, verifiability, years of use, how recently, how much was yours, buzzword density); selecting flagship projects (recency, depth, relevance to target role, ability to talk 10 minutes); re-wording over-claims; removing claims you cannot defend; mapping interviewer reading behaviour (they pick the line you know least) | Claim ledger table; risk heat-map; "interviewer reading path" step-through of how a resume line becomes a 5-level probe | Build the ledger for the learner's resume in the platform form; self-score risk on a 1–5 rubric; rewrite 5 weak bullets; pick and justify 3 flagships | Listing a technology after one tutorial; "expert" next to a tool used for a week; verbs like "architected" or "led" without a decision you can narrate; leaving a claim in because it matches the job description |
| M2 | Narrative spine: the project story | Tell a project using the 8-part spine in order, 3 minutes, without notes. | The spine: context and business problem; constraints (time, team, budget, legacy, compliance, scale); your role and scope; architecture; key decisions and trade-offs; results; failures and incidents; what you would change; flow rules (outcome first, then back-fill); naming the problem before the solution; "so what" test per sentence; avoiding the tour-of-the-tech-stack anti-pattern; present tense for architecture, past tense for events | Spine diagram (8 boxes with time budget per box); before and after of a rambling answer vs a spine answer; timeline step-through | Write a spine for each flagship in the editor; record a 3-minute voice take; compare against the spine checklist; trim to time | Starting with the tech stack; no stated problem; no constraints so every decision looks arbitrary; results with no baseline; never mentioning a failure; a role that is unclear within the first minute |
| M3 | Multi-depth delivery: 30 s, 3 min, 10 min | Deliver the same project at three depths with consistent facts, ending each with an invitation to probe. | The 30 s "elevator" (problem, my role, outcome, one hard bit); the 3 min "walkthrough" (spine, light); the 10 min "deep dive" (spine plus 2 decisions in depth and one failure); consistency rule (facts must nest, never contradict across depths); signposting ("there are three parts, I will start with..."); handling interruption and re-entry; answering "tell me about yourself" through a project; adapting to audience (hiring manager vs staff engineer vs HR vs client); time-checking aloud | Nested-depth onion diagram; timeline per depth; audience-adaptation matrix | Three voice takes per flagship (30 s, 3 min, 10 min); speech-delivery report (pace, filler words, answer length; no emotion or confidence inference); one interrupted take with mid-answer probe | Memorised monologue that collapses at first interruption; facts changing between versions (for example team size or timeline); 10-minute answer that never reaches a decision; failing to ask what the interviewer wants to go deeper on |
| M4 | Decisions, trade-offs and the 5 levels of "why" | Defend three real decisions per flagship through 5 levels without contradiction or invented rationale. | Anatomy of a decision record (context, options, criteria, choice, cost accepted, review trigger); the five levels (what, why that, why not the alternative, what evidence, what would change your mind); reconstructing honest rationale when the decision was made by someone else or under time pressure; "we did not have a reason then, here is what I know now"; distinguishing a constraint-driven decision from a preference; trade-off vocabulary (latency vs cost, consistency vs availability, simplicity vs flexibility, build vs buy); handling "that seems wrong" challenges; retrofitting vs lying | 5-level "why ladder" interactive step-through; decision-record template; trade-off radar for one decision | Write 3 decision records per flagship; run the why-ladder drill (the AI interviewer pushes 5 levels; scoring checks consistency across levels and with the ledger) | Post-hoc rationalisation that collapses at level 3; "best practice" as the only reason; no rejected alternative; claiming a decision that was clearly the team lead's; defensiveness when challenged |
| M5 | Technology probes on listed skills | For every technology on the resume, answer a fundamentals probe, a production-experience probe and a failure probe at the depth implied. | Probe ladder per technology: definition, how you used it, what broke, internals one level below the API, alternatives; calibrating depth to years claimed; "used vs operated vs designed with" distinction; honest boundary statements; building a per-technology cheat card (3 facts, 1 failure, 1 trade-off); pairing each listed technology with a project where it was used; archetype-specific probe sets (see below); handling a probe on a technology you used long ago; bridging from adjacent knowledge without bluffing | Technology probe ladder diagram; "depth meter" per skill; archetype probe maps | For each of the top 10 listed technologies write a cheat card; run quick drills in voice; the platform generates probes from the confirmed skill list | Bluffing past the edge of knowledge; answering a different question; knowing the API but not what is underneath; skills section longer than the project evidence; "I only did that part" with no detail |
| M6 | Quantifying impact honestly | Express impact using real, attributable measures with baseline, method and your share, or an honest qualitative equivalent. | Metric types (latency, throughput, cost, reliability, adoption, time saved, defects, delivery speed); baseline and method ("measured how, over what period"); attribution (what you changed vs what the team or the business changed); ranges and rounding honestly; when you have no numbers (use before-and-after behaviour, scale facts you do know, stakeholder outcome, and say you did not instrument it); proxy metrics and their weaknesses; never inventing a figure under pressure; reconstructing numbers from logs, dashboards, invoices or tickets you legitimately still have; "how would you have measured it" answer when you did not | Metric card (value, baseline, method, window, attribution); honest-impact decision tree; before and after chart reading | Fill a metric card for each flagship; rewrite 5 impact bullets; role-play "how did you get that number" probes; practise the no-numbers answer | Round numbers with no method; percentages with no baseline; team results claimed as personal; metrics that change between retellings; "significant improvement" with nothing behind it |
| M7 | Contribution, failure, NDA and ownership | Describe personal vs team contribution precisely, narrate a failure with learning, and abstract a confidential project without losing substance. | "I" and "we" discipline (I decided, I built, we agreed, the team shipped); scoping your contribution by layer (design, code, review, operations, mentoring); talking about colleagues fairly; failure narratives (what happened, your part, detection, response, prevention, what changed in your practice); blameless framing; NDA and confidentiality limits (anonymise client and numbers, describe patterns not secrets, say what you cannot share and offer the abstract version); handling "show me the code" and "who was the client"; escalating when asked for proprietary detail; what to do if a past employer is mentioned negatively | Contribution map by layer; failure-story flow (timeline with decision points); abstraction ladder for confidential projects | Write the contribution map for each flagship; write one failure story and one near-miss; produce an NDA-safe version of the most sensitive project; role-play a probe that pushes for confidential detail | "We" hiding whether you did anything; "I" taking team credit; blaming a colleague or client; a failure story that is secretly a success; leaking client names or numbers to impress; refusing to discuss the project at all rather than abstracting |
| M8 | Own-architecture whiteboard and canvas | Draw your own architecture in 5 to 8 minutes, narrate it, and use it to answer failure and scale probes. | Choosing the abstraction level; standard drawing order (actors, entry points, core services, data stores, async paths, external dependencies); labelling protocols and data flows; marking trust boundaries and failure points; capacity and numbers you actually know; re-drawing the same system at a different depth; changing the diagram live in response to a probe ("what if the broker drops", "what if volume grows 10x"); converting a project to the platform canvas (components, arrows, notes) so it serialises to a structured graph; honest "not in my version" annotations; difference between the system as built and the system as you would build it now | Whiteboard drawing order step-through; sample canvas of each archetype (generic, labelled as examples); before and after annotated diagram | Draw each flagship on the canvas; narrate by voice; the evaluator checks internal consistency of diagram vs transcript and vs the ledger; answer 3 probes by editing the diagram | Diagram that does not match what you say; boxes with no protocol or data direction; no mention of failure paths; drawing the ideal system and presenting it as the built system; being unable to explain a box you drew |
| M9 | Archetype workshops and Interview Master integration | Run full mock deep-dives on two archetype-style projects and use the claim-risk report to close gaps. | Archetype workshops (ARC-1 to ARC-4) as structured probe maps; how Interview Master generates resume-claim questions from the confirmed profile only (never from unconfirmed claims); how claim-risk scoring works (see section 5); reading the report: high-risk claims, weak evidence, inconsistency between sessions, depth shortfalls; the repair loop (fix the resume, fix the story, learn the gap, or remove the claim); company-style adjustments (IT-services emphasis on role and delivery; Big 4 on stakeholder framing; product on depth and scale); the last-minute checklist for a project round | Probe-map diagrams per archetype; claim-risk dashboard walkthrough; repair loop flow | Two full mock deep-dives (voice), one strict and one coaching; act on the risk report: rewrite or remove at least 3 claims; re-run to show risk reduced | Treating mock results as a guarantee; "fixing" a risk by deleting evidence instead of learning; rehearsing a script so tightly that probes derail it; ignoring inconsistencies flagged across sessions |

| | **Ownership notes** | | **OWNS:** claim-ledger method and its use (O-105), project narrative spine, multi-depth delivery including non-technical audiences (R-C3-3), 5-level why, honest impact quantification, project-specific failure/ownership/NDA abstraction, own-architecture canvas defence (O-106), diagram-vs-story consistency rubric (R-C2-3), claim-risk rubric content. **POINTS:** spoken career narrative, "only 4 years of Python" probe, generic failure/conflict STAR stories -> C1.M1-M5, M9 (by reference, R-C2-2); positioning thesis -> C4.M2; system design from scratch, canvas vocabulary and rule IDs -> A6.M10 and canvas spec; seven-step loop -> C3.M1; technology depth -> owner courses (links in the M5 depth-probe packs below). Profile Facts Ledger entity -> platform (P-20), content owner C2. | | | |

### Archetype probe maps (for M5 and M9; generic, no invented specifics)

| Archetype | Probe themes an interviewer is likely to explore | What a strong senior answer shows |
|---|---|---|
| ARC-1 API backend with AI assistant | Request path and where the LLM call sits; sync vs streaming vs background; timeouts, retries and provider failure; prompt versioning and testing; structured output validation; cost and latency control; data privacy of user content; how answer quality was judged; async correctness in FastAPI; database access patterns and connection pooling | Clear request-path ownership; fallbacks designed not hoped for; honest statement of how quality was evaluated (even if informally); awareness of what you would add (evals, tracing) |
| ARC-2 Voice agent platform | End-to-end latency budget across STT, LLM and TTS; streaming vs batch; barge-in and cancellation; turn detection; transcription errors and domain vocabulary; concurrency and resource limits per worker; telephony or WebSocket transport; cost per minute; human handoff; recording consent | A budget you can reason about even if you did not measure every stage; specific mechanisms for interruption; honest limits of the system; consent and privacy awareness |
| ARC-3 Django IoT access control with MQTT | Topic design; QoS choice and what it does and does not guarantee; retained messages and last will; device identity and credential rotation; command acknowledgement and idempotency; offline and reconnect behaviour; ordering; message volume and ingestion path into Django; audit logging; failure of the broker; safety implications of door commands | Precise protocol vocabulary; reasoning about "exactly once" honestly; a security story for devices; awareness of fail-open vs fail-closed for access control |
| ARC-4 Enterprise field-service platform | Core data model (customers, sites, jobs, visits, technicians, parts or materials where relevant); multi-tenancy approach; scheduling and routing approach and its limits; offline-capable mobile sync and conflict handling; role-based permissions; notifications; reporting and large queries; integrations with billing or CRM; compliance and audit needs | Data model reasoning, tenant isolation reasoning, honest scheduling heuristics, clear separation of what was off-the-shelf vs built |

### M5 depth-probe packs (10 depth probes per listed technology, tied to owner courses)

For every technology the learner lists, the engine draws 10 probes in escalating order: (1) what it is for in your project; (2) one concrete task you did; (3) a failure or surprise; (4) one level below the API; (5) failure behaviour of the component; (6) scaling or limit; (7) security or data risk; (8) alternative and why not; (9) how you observed it in production; (10) what you would change today. Probes 4-8 are the "explain at senior depth" probes; each pack links the owner lesson for remediation (G-1: C2 holds the probe list, not the teaching).

| Technology family | Probe focus (probes 4-8) | Remediation owner |
|---|---|---|
| asyncio, threads, workers, Celery | event loop and blocking, cancellation, backpressure, acks and idempotent tasks, pool sizing | A2.M2-M9 |
| PostgreSQL, MySQL, SQL | indexes and plans, isolation and locking, pooling, migrations, replication lag | A4.M3-M7 |
| REST, FastAPI, Django, auth | idempotency, pagination, caching, rate limiting, token design, outbox | A5.M3-M8 |
| Redis-compatible store | structure choice, eviction, persistence, stampede, failure mode | A5.M5 (mechanics), A6.M2 (topology) |
| LLM APIs and prompts | token cost, structured output, retries and 429, context management, injection basics | B2.M1-M10 |
| RAG and vector search | chunking, hybrid retrieval, evaluation, index choice | B3.M2-M8 |
| Agents and tools | loop control, tool design, durability, evaluation | B4.M1-M8 |
| Voice and realtime | latency budget, VAD and barge-in, transport, cost per minute | B6.M1-M10 |
| Docker, AWS Lambda/S3, IAM, CI/CD | cold starts, limits, least privilege, deploy and rollback | A7.M1-M8 |
| MQTT and WebSocket | QoS semantics, sessions, retained and last will, ordering, device identity | A6.M9 (MQTT sessions, QoS, device ingestion; the owner lesson) plus A5.M9 (protocol choice); see ARC-3 note in section 7 |

### M8 diagram-versus-story consistency rubric (R-C2-3; owner C2)

LLM-scored, **qualitative** (not a deterministic rule engine; no rule IDs). Input: the canvas graph export (v0.1 vocabulary), the voice transcript and the ledger. Output per dimension: Consistent / Minor gap / Major gap with a quoted evidence pair (one graph element, one transcript span). Human sampling during calibration.

| Dimension | Consistent looks like | Major gap example (generic) |
|---|---|---|
| Component coverage | every component named in speech exists on the canvas and vice versa | narrates a queue that is not drawn |
| Flow fidelity | spoken request path matches arrow direction and protocol labels | says "async via broker", arrow drawn as direct call |
| State and data ownership | stores and who writes them match | two services claim the same table with no mention |
| Failure paths | failure answers edit the graph or point to existing elements | "we retry" with no retry element and no detail when asked |
| Built versus ideal | "not in my version" annotations present when true | ideal design presented as built |
| Ledger agreement | scale, team, dates and numbers match the ledger | team size or load differs from ledger |

Worked examples (3 per dimension, one Consistent, one Minor, one Major) are authored by C2 SMEs in wave 3 and double as calibration seeds. A6 owns canvas vocabulary and rule semantics; C2 only references the vocabulary.

## 3. Per-module detail blocks

All modules follow the platform lesson template (visual first, Simpler/Deeper switch, mastery check, practice, badge). Question-bank targets per module use the standard numbers: **12 MCQ/concept checks, 6 predict-the-result items, 4 coding/SQL tasks, 8 verbal questions with follow-up trees, 3 scenario/design questions**. Because this course is not a code course, "predict-the-result" items become "spot the weakness in this answer transcript", and (per G-10) the "coding/SQL" bucket becomes **applied artefact tasks (rubric-scored) x4 per module** (for example rewrite a claim, complete a decision record, fill a metric card, draw a canvas). Authoring records this substitution in the question-bank metadata so counts remain comparable. Item IDs are `Q-C2-M<n>-<nn>`; placement items carry the `placement` tag and are drawn from the MCQ and spot-the-weakness buckets, not extra authoring.

**Time stamps.** Module lessons carry "last reviewed: October 2026". Method lessons are stable; archetype and technology examples are reviewed every 3 months. Mastery check: 6 to 10 items, pass 80% (configurable) with revision loop.

### M1 — Claim inventory and project selection
- **Lessons:** (1) What an interviewer actually does with your resume. (2) Building the claim ledger. (3) Scoring and repairing risky claims. (4) Choosing your flagship projects.
- **Simpler:** guided ledger form with examples for each claim type; Deeper: adversarial pass where the learner predicts the probe for each claim and scores their own readiness.
- **Mastery-check focus:** classifying claims; identifying over-claim wording; picking flagships by criteria; recognising unverifiable claims.
- **Module badge:** complete ledger covering every resume project and technology; each claim tagged with evidence and a defence line; 3 flagships selected with written rationale.

### M2 — Narrative spine: the project story
- **Lessons:** (1) The 8-part spine. (2) Opening with problem and outcome. (3) Constraints that make decisions make sense. (4) Failure and hindsight as credibility.
- **Simpler:** fill-in-the-blanks spine template; Deeper: restructure a transcript of a rambling answer, then cut a 5-minute story to 3 minutes without losing the decision logic.
- **Mastery-check focus:** ordering the spine; spotting missing constraints; critiquing an answer transcript; converting stack-tour to story.
- **Module badge:** 3-minute voice take for each flagship scoring 3.5/5 or higher on structure, with all 8 parts present.

### M3 — Multi-depth delivery
- **Lessons:** (1) The 30 second version. (2) The 3 minute version. (3) The 10 minute version and where depth goes. (4) Consistency, interruption and audience adaptation.
- **Non-technical audience (owned here, R-C3-3):** lesson 4 includes explaining the project to a non-technical stakeholder (outcome, risk and cost in plain words; analogy without distortion; one number only if real). C3.M2 keeps a 20-minute pointer to this lesson for consulting-style rounds; C1.M4-M5 own stakeholder stories.
- **Simpler:** word-count and time guides per depth; Deeper: handling interruptions and audience switch mid-answer.
- **Mastery-check focus:** choosing what to include per depth; detecting contradictions between versions; adapting to audience.
- **Module badge:** three takes per flagship, factually consistent (checked by the platform against the ledger), within time limits.

### M4 — Decisions, trade-offs and 5 levels of "why"
- **Lessons:** (1) Decision records. (2) The why-ladder. (3) Defending a decision you did not make alone. (4) Admitting a decision was wrong or constrained.
- **Simpler:** decision template with prompts; Deeper: hostile-interviewer drill with challenge such as "I would have done it differently".
- **Mastery-check focus:** identifying the level at which an answer breaks; writing a rejected-alternative paragraph; distinguishing constraint from preference.
- **Module badge:** 3 decision records per flagship plus a why-ladder drill passed on at least 2 projects with no contradictions flagged.

### M5 — Technology probes on listed skills
- **Lessons:** (1) The probe ladder. (2) Calibrating depth to claimed years. (3) Cheat cards. (4) The honest boundary.
- **Simpler:** probe lists for common technologies in the learner's stack; Deeper: "one level below the API" internals drills and cross-technology comparison probes.
- **Mastery-check focus:** answering at the right depth; honest boundary phrasing; matching technology to project evidence.
- **Module badge:** cheat cards for the top 10 listed technologies; voice drill passed on at least 8; remaining 2 either reworded in the ledger or flagged as learning goals.

### M6 — Quantifying impact honestly
- **Lessons:** (1) Metric cards. (2) Baselines, windows and methods. (3) Attribution. (4) When you have no numbers.
- **Simpler:** metric card with examples of acceptable vs unacceptable statements; Deeper: reconstruct a measurement from partial evidence and defend its limits.
- **Mastery-check focus:** spotting unsupported metrics; rewriting a claim with method and attribution; the no-numbers answer.
- **Module badge:** a complete metric card or an honest qualitative statement for every impact claim on the resume; survival of a 3-probe "how do you know" drill.

### M7 — Contribution, failure, NDA and ownership
- **Lessons:** (1) "I" and "we". (2) Contribution map by layer. (3) Failure stories. (4) Confidentiality and abstraction.
- **Reuse rule (R-C2-2):** generic failure and conflict STAR stories are owned by C1 and are **reused by reference** (pointer to the C1 lessons, no re-teaching, no duplicate items). M7 adds only: project-specific failure and near-miss narration tied to a flagship's architecture and ledger, contribution-by-layer, and NDA abstraction. Question tags are separate: M7 items use `project.failure`, `project.confidentiality`; `behav.ownership` is secondary.
- **Simpler:** sentence-level rewrites from "we" to precise attribution; Deeper: handle a pushy interviewer asking for client names, internal numbers or code.
- **Mastery-check focus:** attribution precision; blameless wording; what is safe to disclose.
- **Module badge:** contribution map, one failure story, one NDA-safe abstraction, all scored at 3.5/5 or higher.

### M8 — Own-architecture whiteboard and canvas
- **Lessons:** (1) Drawing order and abstraction level. (2) Annotating flows, boundaries and failure points. (3) Narrating while drawing. (4) Editing live under probing.
- **Simpler:** template canvases per archetype to adapt; Deeper: draw from a blank canvas under a 6-minute timer, then answer three live "what if" probes.
- **Mastery-check focus:** correct component labelling; protocol and data-flow clarity; failure-path identification; consistency between diagram and narration.
- **Module badge:** each flagship on the canvas (serialised to graph) with an evaluator report showing no major diagram-versus-story inconsistencies; one voice defence passed.

### M9 — Archetype workshops and Interview Master integration
- **Lessons:** (1) Archetype probe maps. (2) Running a full deep-dive mock. (3) Reading the claim-risk report. (4) The repair loop and pre-interview checklist.
- **Simpler:** coaching mode with hints between probes; Deeper: strict mode with company-style adjustments and cross-session consistency checks.
- **Mastery-check focus:** interpreting the risk report; choosing repair actions; recognising when to remove a claim.
- **Module badge:** two full mock deep-dives completed, risk report reviewed, at least 3 repairs logged, and a re-run showing the targeted claims no longer high risk.

## 4. Seed interview questions

Format per question: ideal answer outline; common weak answers; follow-up probes; difficulty (1 easy to 5 hard, the data-model scale); competency tag; company-style cluster; goal tag and senior-bar note in the table that follows. These are style-of-question items for practice, not questions asked by any named company.

### M1 — Claim inventory and project selection

**Q1.1 "I see Kubernetes (or any tool) on your resume. Tell me where you used it and what you personally did with it."** — Difficulty 3 — `resume.tech-probe` — IT-services / global-product
- Ideal answer outline:
  - Names the specific project and environment (what the tool was for, in one sentence)
  - States the level honestly: used, operated, or designed with
  - Describes one concrete task personally done (for example a deployment change, a debugging session)
  - Describes one thing that went wrong or surprised them and how it was resolved
  - Shows understanding one level below the surface (what the tool does underneath for that task)
  - Names what they did not do or relied on a teammate for
  - Offers to go deeper where they are strongest
- Common weak answers: lists features from documentation; "we used it for deployment" with no personal part; claims expertise then retreats at the first internals question; defensive tone.
- Follow-ups: What would you check first if it stopped working? What would you change about how you used it? How would you compare it to the alternative you did not choose?

**Q1.2 "Which three projects on your resume would you most like to discuss, and why those?"** — Difficulty 2 — `resume.claim-audit` — Indian-product / AI-startup
- Ideal answer outline:
  - Picks projects with the learner's real depth, not only the newest
  - Gives selection criteria (ownership, difficulty, relevance to the role, learning)
  - Names the hardest problem in each in one line
  - Shows range (for example one design-heavy, one operations-heavy, one AI or data-heavy)
  - Acknowledges what is weaker on the resume and offers context
  - Keeps each description to 20 to 30 seconds
- Common weak answers: picks the most impressive-sounding project they barely touched; cannot say why; no range; speaks for minutes without structure.
- Follow-ups: Which line on your resume are you least comfortable defending? What would you remove if you had to cut the resume in half? Which claim would a sceptic probe first?

**Q1.3 "Your resume says you 'led' or 'architected' something. What did that concretely involve?"** — Difficulty 4 — `resume.claim-audit` — Big4 / global-product
- Ideal answer outline:
  - Defines the scope honestly (team size, duration, area owned) with real numbers they can stand behind
  - Names specific decisions they owned and a specific decision they escalated or deferred
  - Describes how work was divided and reviewed, and how disagreement was resolved
  - Distinguishes technical lead, project lead and people manager if relevant
  - Names a mistake or course correction during the work
  - Admits where the title on the resume is broader than the actual responsibility, if true, and states the real scope
- Common weak answers: repeats the resume verb with adjectives; cannot name a decision; claims every success personally; vague team structure.
- Follow-ups: Who disagreed with you and how did it end? What did you delegate and what happened? Who else could describe your contribution accurately if I asked them?

### M2 — Narrative spine: the project story

**Q2.1 "Walk me through a project you are proud of, in about three minutes."** — Difficulty 2 — `project.narrative` — all clusters
- Ideal answer outline:
  - Opens with the problem and why it mattered, before any technology
  - States constraints (time, team, legacy, scale, compliance)
  - Makes the personal role clear in the first minute
  - Describes the architecture in a few sentences at a consistent abstraction level
  - Highlights two or three decisions with the reason
  - States the outcome with honest evidence
  - Includes one difficulty or failure and one thing they would change
  - Ends by inviting the interviewer to choose where to dive in
- Common weak answers: technology tour; no problem statement; no mention of own role; ends abruptly with "and it went well"; runs over time.
- Follow-ups: What was the hardest part and why? What would you have done with another month? What would you drop if you had half the time?

**Q2.2 "What was the business problem, and who would have been unhappy if you had not solved it?"** — Difficulty 3 — `project.narrative` — IT-services / Big4
- Ideal answer outline:
  - Names the stakeholders and what they needed
  - Describes the cost of not solving it in business terms (time, money, risk, customer experience) without inventing figures
  - Connects the technical goal to the stakeholder need
  - Describes how requirements were gathered or clarified, and what was ambiguous
  - Describes a requirement that changed and how it was handled
  - Names how success was judged by the stakeholder, not only by engineering
- Common weak answers: restates the feature list; cannot name the stakeholder; technical goals disconnected from need; fabricated business numbers.
- Follow-ups: How did you find out what they really needed? What did you push back on? How did the stakeholder know it worked?

**Q2.3 "What were the constraints on that project, and how did they shape your design?"** — Difficulty 3 — `project.tradeoffs` — global-IT-consulting / Indian-product
- Ideal answer outline:
  - Lists real constraints (deadline, team skills, existing systems, budget, regulation, hardware, data availability)
  - Maps at least two constraints to two decisions explicitly
  - Describes a decision that would have been different without the constraint
  - Describes a constraint they challenged or negotiated
  - Notes technical debt knowingly taken and why
  - Says what would trigger revisiting it
- Common weak answers: "no constraints"; generic constraints not linked to decisions; blames constraints for avoidable mistakes.
- Follow-ups: Which constraint hurt most? Which did you manage to remove? What debt from that period still exists or caused trouble?

### M3 — Multi-depth delivery

**Q3.1 "Give me the 30-second version of your most recent project."** — Difficulty 2 — `comm.multi-depth` — all clusters
- Ideal answer outline:
  - One sentence on what the system does and for whom
  - One sentence on the learner's role
  - One sentence on the hardest technical problem
  - One sentence on the outcome (qualitative if no number)
  - Stops on time and signals willingness to go deeper
  - Language free of jargon that the interviewer has not yet shown they share
- Common weak answers: runs to two minutes; starts with the stack; no role; no outcome.
- Follow-ups: Now give it to a non-technical stakeholder. Now give the version for a staff engineer. Which part of that did you consider cutting?

**Q3.2 "I have 10 minutes. Take me deep into one project."** — Difficulty 4 — `comm.multi-depth` — global-product / AI-startup
- Ideal answer outline:
  - Brief orientation (60 to 90 seconds) of problem, role and architecture
  - Chooses two decisions and goes deep with options, criteria and cost
  - Describes one failure or incident with timeline and learning
  - Gives results with method and honest attribution
  - Pauses to check what the interviewer wants to explore
  - Handles interruption and returns to the thread cleanly
  - Keeps facts consistent with what a shorter version would have said
- Common weak answers: spends 8 minutes on orientation; no depth anywhere; refuses interruption or loses the thread; contradicts earlier statements.
- Follow-ups: Which decision would you reopen today? What did you not have time to say that matters? What would a teammate add or correct about your account?

**Q3.3 "You said earlier the team was five people, and now you describe it as eight. Which is it?"** — Difficulty 3 — `comm.multi-depth` — Big4 / IT-services
- Ideal answer outline:
  - Acknowledges the discrepancy calmly without defensiveness
  - Explains honestly (for example core team vs including part-time or other-team contributors) if true
  - Corrects the earlier statement if it was wrong
  - Shows a habit of stating precise facts and a method for keeping stories consistent
  - Does not invent a post-hoc explanation
  - Moves on without losing composure
- Common weak answers: bluffs a justification; becomes defensive; says "approximately" to everything; changes the answer to guess what the interviewer wants.
- Follow-ups: How do you keep facts consistent across retellings? Which of your numbers are you most certain of? Which are you least certain of?

### M4 — Decisions, trade-offs and the 5 levels of "why"

**Q4.1 "Why did you choose that framework (or database, or broker) for the project?"** — Difficulty 3 — `project.tradeoffs` — all clusters
- Ideal answer outline:
  - States the options actually considered, not a retrofitted list
  - States the decisive criteria (team skills, ecosystem, performance need, operational burden, timeline)
  - Names what was given up
  - Distinguishes what they decided from what was inherited
  - Gives evidence used (prototype, benchmark, prior experience) or admits it was judgment
  - Says how the choice held up and what they would reconsider
- Common weak answers: "it is popular"; "the team already knew it" as the only reason with no thought for cost; no alternative named; inventing a benchmark.
- Follow-ups: What would have made you choose the alternative? Did the choice survive the first production incident? If you started today with the current ecosystem, would you choose differently?

**Q4.2 "I think that design is wrong for your scale. Convince me otherwise, or tell me where I am right."** — Difficulty 5 — `project.tradeoffs` — global-product / Indian-product
- Ideal answer outline:
  - Listens, restates the interviewer's concern fairly
  - Clarifies the actual scale and load the system had, with honest numbers
  - Defends with constraints and evidence, not with authority
  - Concedes the valid parts specifically
  - Describes how the design would change at larger scale
  - Distinguishes "right for then" from "right forever"
  - Remains collaborative, not combative
- Common weak answers: stubborn defence; immediate capitulation without thought; invented scale; "it worked fine".
- Follow-ups: At what load does your design break first? What is the cheapest change that buys the most headroom? What did you monitor to know?

**Q4.3 "Level by level: why did you use a queue or background job there? Why that one? Why not the simpler option? How do you know it was needed? What would make you remove it?"** — Difficulty 4 — `project.tradeoffs` — AI-startup / global-product
- Ideal answer outline:
  - Level 1: states the requirement the queue serves (latency, decoupling, retry)
  - Level 2: explains why that queue technology given constraints
  - Level 3: names the simpler alternative (synchronous call, in-process task) and why it was insufficient
  - Level 4: provides the evidence (observed latency, failures, volume) or admits judgment
  - Level 5: names the trigger that would let them simplify, and the operational cost accepted
  - Answers stay mutually consistent across all five levels
- Common weak answers: levels 3 to 5 repeat level 1; "best practice"; contradictions with earlier narrative; invented evidence.
- Follow-ups: What happens to a message if the worker crashes mid-task? How do you make the task idempotent? How would you observe a growing backlog?

### M5 — Technology probes on listed skills

**Q5.1 "You list Redis. What did you use it for, and what happens in your design if Redis is unavailable?"** — Difficulty 3 — `resume.tech-probe` — Indian-product / global-product
- Ideal answer outline:
  - States the concrete use (cache, rate limit, queue, session, lock) in the project
  - Explains the key choices (data structure, TTL, eviction, persistence settings if relevant)
  - Describes failure behaviour: fail open or closed, fallback to database, stampede protection if relevant
  - Names a failure or surprise seen
  - Shows a limit of the approach (consistency, memory, single-node)
  - Is honest about operations done by others (managed service, platform team)
- Common weak answers: "it is an in-memory cache" only; no failure design; claims distributed lock correctness without discussing caveats.
- Follow-ups: How did you invalidate cached entries? What would you do about a cache stampede? Would you use Redis as a primary datastore for this? Why or why not?

**Q5.2 "You list AsyncIO. Explain what happens when you await a coroutine, and a place in your project where async caused a problem."** — Difficulty 4 — `resume.tech-probe` — global-product / AI-startup
- Ideal answer outline:
  - Explains the event loop yielding control at await points
  - Distinguishes concurrency from parallelism and I/O-bound from CPU-bound work
  - Gives a real example of blocking the loop (a sync library call, CPU work) and its symptom
  - Describes how it was diagnosed and fixed (thread executor, async library, separate worker)
  - Mentions task lifecycle issues (unawaited tasks, cancellation, timeouts) where relevant
  - Connects to the framework use (for example async endpoints calling sync code)
  - States what they would check in code review
- Common weak answers: "async makes things faster"; cannot describe a real incident; confuses threads with coroutines.
- Follow-ups: How do you detect a blocked event loop? How do you limit concurrent outbound calls? What happens to the task if the client disconnects?

**Q5.3 "You list MQTT (or WebSockets, or another protocol). What delivery guarantee did you rely on and what did you do about the cases it does not cover?"** — Difficulty 4 — `resume.tech-probe` — AI-startup / IT-services
- Ideal answer outline:
  - States the guarantee level used and why (for MQTT: the QoS level, with what it means)
  - Explains what is not guaranteed (duplicates at some levels, ordering across topics, delivery to offline clients without persistent sessions)
  - Describes application-level safeguards (idempotency keys, acknowledgements, retries, timeouts, reconciliation)
  - Describes behaviour on disconnect and reconnect
  - Mentions security of connections and device or client identity
  - Notes what was tested and what was assumed
  - Honest about broker operations owned by others
- Common weak answers: "QoS 2 means exactly once so there is no problem" without nuance; no application-level handling; no idea what happens when a device is offline.
- Follow-ups: How did you detect a device that silently went away? What would happen if the same command arrived twice? How would you scale the broker or what did the managed service do for you?

### M6 — Quantifying impact honestly

**Q6.1 "You say you improved performance. By how much, and how do you know?"** — Difficulty 3 — `project.impact` — all clusters
- Ideal answer outline:
  - States the metric chosen and why it mattered to users or the business
  - Gives baseline and result with the measurement method and window (or honestly says approximate)
  - Explains what part of the improvement was their change vs other changes
  - Notes limits (load conditions, caching effects, sample size)
  - If the numbers are not available, says so and offers the qualitative evidence
  - Explains how they would measure it now
- Common weak answers: round number with no method; "it felt faster"; takes credit for the team's whole effort; changes the number on follow-up.
- Follow-ups: What was the baseline measured on? Could any other change explain it? What did you not improve?

**Q6.2 "You did not instrument that project. How would you describe its impact without made-up numbers?"** — Difficulty 3 — `project.impact` — IT-services / AI-startup
- Ideal answer outline:
  - Says plainly that precise numbers are unavailable
  - Offers verifiable facts they do know (users or sites served in broad terms, workflows replaced, incidents avoided, manual steps removed)
  - Uses before and after behaviour
  - Cites stakeholder feedback honestly, labelled as such
  - Explains what they would instrument given another chance
  - Avoids presenting estimates as measurements
- Common weak answers: invents a figure when pushed; deflects; says impact was "huge".
- Follow-ups: Which of those facts could a colleague verify? What dashboard would you add on day one? How would you convince a sceptical manager without numbers?

**Q6.3 "Your resume says 'reduced cost by X percent'. Walk me through how that figure was derived."** — Difficulty 4 — `project.impact` — Big4 / global-product
- Ideal answer outline:
  - Defines the cost being measured (infrastructure, vendor, labour, API spend)
  - States baseline and comparison period
  - Explains the change responsible and the share from it
  - Accounts for confounders (volume change, pricing change, seasonality)
  - Mentions new costs introduced by the change (operations, complexity)
  - Admits uncertainty and rounding honestly
  - Corrects the resume if the claim was loosely worded
- Common weak answers: cannot reconstruct the calculation; counts only the savings; claims the whole team's saving.
- Follow-ups: What cost went up as a result? How sure are you of the baseline? Would the saving survive a pricing change from the vendor?

### M7 — Contribution, failure, NDA and ownership

**Q7.1 "What exactly did you build, as opposed to the team?"** — Difficulty 3 — `project.narrative` (secondary, no credit: `behav.ownership`) — IT-services / Big4 / global-product
- Ideal answer outline:
  - Describes the team structure and roles briefly
  - Lists specific components, modules or decisions they owned
  - Uses "I" for their work and "we" for shared decisions, precisely
  - Describes review and collaboration points (who reviewed their work, whose they reviewed)
  - Names contributions that were not code (design, mentoring, incident response)
  - Credits colleagues accurately without diminishing own contribution
  - Offers a way to verify (for example repository history on projects they can discuss)
- Common weak answers: "I did everything"; "we" throughout; cannot name a component; credits others to dodge the question.
- Follow-ups: Which part was written by someone else that you needed to understand deeply? What did you review for others? What would break if you left the project?

**Q7.2 "Tell me about a failure or production incident on one of these projects. What was your part?"** — Difficulty 4 — `project.failure` — all clusters
- Ideal answer outline:
  - Describes the incident clearly: impact, timeline, how it was detected
  - Describes their own part honestly, including any contributing mistake
  - Explains the diagnosis steps and the actual root cause
  - Describes the immediate mitigation and the lasting fix
  - Describes what changed in process, monitoring or code
  - Applies the learning to later work with an example
  - Blameless tone toward colleagues
- Common weak answers: a "failure" that is really a success story; blames others; no root cause; no learning; invents drama.
- Follow-ups: How could it have been detected earlier? What would you do differently in the first 15 minutes? What is the system's equivalent risk today?

**Q7.3 "I would like to know the client's name and see the code, but I understand you may be under NDA. How would you describe the project to me?"** — Difficulty 3 — `project.confidentiality` — Big4 / IT-services / global-IT-consulting
- Ideal answer outline:
  - States calmly that they respect the confidentiality terms, without drama
  - Offers an anonymised description (industry type, scale in ranges, problem pattern)
  - Preserves technical substance: architecture pattern, decisions, trade-offs, failure modes
  - Offers to whiteboard a generalised version
  - Does not leak names, numbers or code to please the interviewer
  - Offers other evidence that is shareable (public work, a personal project, a written write-up cleared for sharing)
  - Treats the interviewer's question as legitimate, not hostile
- Common weak answers: refuses to say anything; reveals confidential details; deflects with vague talk; implies fabricated stories are acceptable.
- Follow-ups: What was the hardest technical problem, described abstractly? How would you demonstrate your skill without the code? What would you say if pushed again for the client name?

### M8 — Own-architecture whiteboard and canvas

**Q8.1 "Draw the architecture of your main project and walk me through a request from start to finish."** — Difficulty 3 — `design.own-architecture` — all clusters
- Ideal answer outline:
  - Starts with actors and entry points, then core services, data stores, async paths and external dependencies
  - Labels protocols and data direction on the main arrows
  - Traces a single request or event end to end, naming where state changes
  - Marks trust boundaries and where authentication or authorisation happens
  - Notes the known weak points honestly
  - Keeps within about 6 to 8 minutes and a consistent abstraction level
  - Distinguishes what exists from what is planned
- Common weak answers: draws components with no flows; unable to explain a drawn box; mixes abstraction levels; ideal-state diagram presented as built.
- Follow-ups: Where is state stored at each step? Which box is the single point of failure? Where would you add logging first?

**Q8.2 "What happens in your system if this component fails?" (interviewer points to a box)** — Difficulty 4 — `design.own-architecture` — global-product / AI-startup
- Ideal answer outline:
  - Identifies upstream and downstream effects precisely
  - Distinguishes failure modes (down, slow, wrong, partially available)
  - States actual behaviour (timeouts, retries, fallbacks, queue buildup, data loss risk)
  - Distinguishes what was designed from what was observed in production
  - Describes detection (alerts, health checks) and recovery
  - Proposes an improvement with its cost
  - Updates the diagram live to show the failure path
- Common weak answers: "it would not fail"; hand-waves "we have retries"; no detection story; cannot connect to the diagram.
- Follow-ups: How long until someone is paged? What data could be lost or duplicated? What would you add if this failure were unacceptable?

**Q8.3 "Traffic grows ten times. What breaks first in your project, and what would you change?"** — Difficulty 4 — `design.own-architecture` — Indian-product / global-product
- Ideal answer outline:
  - States current load honestly (known numbers or ranges) and the headroom
  - Identifies the first bottleneck with reasoning (database, external API limits, single worker, broker, memory)
  - Proposes proportionate changes in order of cost and benefit
  - Notes what to measure before changing anything
  - Addresses data consistency and operational complexity introduced
  - Distinguishes tens-of-percent growth fixes from architectural changes
  - Updates the canvas to show the new design
- Common weak answers: "add more servers"; jumps to microservices; no measurement; ignores the data layer.
- Follow-ups: Which change would you do first and why? What would you refuse to do yet? What would you test before the launch?

### M9 — Archetype workshops and Interview Master integration

**Q9.1 "For your AI assistant (or voice agent) project: where does latency hide, and what did you do about it?"** — Difficulty 4 — `project.tradeoffs` — AI-startup / global-product
- Ideal answer outline:
  - Breaks down the request path into stages (network, STT if voice, retrieval or tool calls, model, TTS, post-processing)
  - States which stages they measured and which they estimated
  - Describes specific techniques used (streaming, caching, smaller model for simple steps, parallel calls, connection reuse)
  - Describes barge-in or cancellation behaviour if voice
  - Describes how tail latency and provider slowdowns were handled (timeouts, fallbacks)
  - Is honest about what remained slow and why
  - Connects to cost trade-offs
- Common weak answers: "the LLM is slow" with no breakdown; claims precise numbers not measured; no handling of provider failure.
- Follow-ups: How would you set a latency budget per stage? What would you do if the provider's p95 doubled? How would you test changes safely?

**Q9.2 "Your resume-claim risk report flags three claims as high risk. How do you decide whether to fix the story, learn the gap, or remove the claim?"** — Difficulty 3 — `resume.claim-audit` — all clusters
- Ideal answer outline:
  - Reviews each flagged claim against actual evidence (what was done, when, how deeply)
  - Fixes the wording when the claim is true but overstated
  - Learns the gap when the claim is true and relevant to the target role, with a time-boxed plan
  - Removes the claim when it cannot be defended honestly or is not relevant
  - Re-runs a mock to verify improvement
  - Treats the report as practice feedback, not a verdict on ability or a prediction of hiring outcomes
  - Never deletes evidence of weakness without addressing it
- Common weak answers: deletes everything flagged to be safe; ignores the report; memorises a script instead of learning; treats the score as a guarantee.
- Follow-ups: Which claim would you keep despite the risk and why? How would you know the repair worked? How much preparation time would you allocate to each?

**Q9.3 "Explain how you would prepare for a project round the night before, using what you have built so far."** — Difficulty 2 — `project.narrative` — IT-services / Indian-product
- Ideal answer outline:
  - Re-reads the claim ledger and cheat cards for the target role
  - Rehearses the 30 s, 3 min and 10 min versions for the top two projects aloud
  - Re-draws the main architecture from memory once
  - Rehearses three decisions with rejected alternatives and one failure story
  - Checks that numbers and team facts match across all versions
  - Prepares thoughtful questions for the interviewer about their systems
  - Sleeps; avoids learning new technologies at the last minute and avoids rehearsing a script verbatim
- Common weak answers: cramming new topics; memorising scripts; skipping the architecture re-draw; no plan for unknown probes.
- Follow-ups: What do you do when a probe goes beyond your knowledge? How do you recover from a blank moment? What would you say if you realise mid-answer that a number you gave was wrong?

(Seed question count: 27, 3 per module across 9 modules. Each module's remaining verbal-question target of 8 is to be authored from these patterns plus the archetype probe maps.)

### Goal tags and senior-bar notes for every seed (G-10)

All seeds are `B:M A:M H:M` unless stated. IDs follow `Q-C2-M<module>-<nn>` (Q1.1 = Q-C2-M1-01, and so on). Each seed also gets a diagnostic role: `placement` items are marked P.

| Seed | Tags | Senior-bar note (mid / senior / staff) |
|---|---|---|
| Q1.1 | B:M A:M H:M, P | mid lists features; senior states used/operated/designed and one level below; staff also names what the team learned and who else owned it |
| Q1.2 | B:M A:M H:M | mid picks newest; senior picks for depth and range with criteria; staff shapes the choice to the role's problem space |
| Q1.3 | B:M A:M H:M, P | mid repeats the verb; senior gives scope, decisions owned and escalated; staff adds how they grew others and corrected a title gap |
| Q2.1 | B:M A:M H:M, P | mid tours the stack; senior leads with problem, constraint, role, decisions; staff adds business framing and invites the dive |
| Q2.2 | B:M A:M H:M | mid restates features; senior ties technical goal to stakeholder; staff covers shifting requirements and success as the stakeholder judged it |
| Q2.3 | B:M A:M H:M | mid lists constraints; senior maps constraint to decision and debt; staff negotiated or removed a constraint |
| Q3.1 | B:M A:M H:M, P | mid runs long; senior nails four sentences; staff adapts jargon live |
| Q3.2 | B:M A:M H:M | mid spends it on orientation; senior goes deep on two decisions and a failure; staff checks interviewer intent and reopens a decision |
| Q3.3 | B:M A:M H:M | mid bluffs; senior corrects calmly; staff shows a fact-keeping habit (ledger) |
| Q4.1 | B:M A:M H:M, P | mid says popular; senior options, criteria, cost; staff adds reversibility and review trigger |
| Q4.2 | B:M A:M H:M | mid defends or folds; senior concedes precisely with load numbers; staff states the break point and cheapest headroom |
| Q4.3 | B:M A:M H:M, P | mid repeats level 1; senior holds five consistent levels; staff names the removal trigger and operating cost |
| Q5.1 | B:M A:M H:M | mid "in-memory cache"; senior failure mode and stampede; staff topology, persistence and when not to use it |
| Q5.2 | B:M A:M H:M | mid "faster"; senior loop blocking incident and fix; staff review checklist and cancellation semantics |
| Q5.3 | B:M A:M H:M | mid "QoS 2 is exactly once"; senior hop-level guarantee plus idempotency; staff session and reconnect design and security |
| Q6.1 | B:M A:M H:M, P | mid round number; senior baseline, method, attribution; staff confounders and what did not improve |
| Q6.2 | B:M A:M H:M | mid invents; senior states what is unknown and offers verifiable facts; staff proposes the instrumentation |
| Q6.3 | B:M A:M H:M | mid cannot rebuild; senior derives with confounders; staff includes new costs and corrects the resume |
| Q7.1 | B:M A:M H:M | mid "we" or "I did all"; senior precise by layer; staff includes mentoring, review and bus-factor |
| Q7.2 | B:M A:M H:M | mid success disguised; senior root cause and own part; staff systemic prevention and a current equivalent risk |
| Q7.3 | B:M A:M H:M | mid refuses or leaks; senior abstracts keeping substance; staff offers cleared alternative evidence |
| Q8.1 | B:M A:M H:M, P | mid boxes without flows; senior traces a request and marks trust boundaries; staff states built vs ideal and weak points |
| Q8.2 | B:M A:M H:M | mid "retries"; senior failure modes, detection, data risk; staff cost-justified improvement, edits diagram live |
| Q8.3 | B:M A:M H:M | mid "add servers"; senior first bottleneck with measurement; staff sequences by cost and refuses premature splits |
| Q9.1 | B:S A:M H:M | mid "LLM is slow"; senior stage breakdown, measured vs estimated; staff budgets per stage and cost trade-off |
| Q9.2 | B:M A:M H:M | mid deletes flagged claims; senior fix/learn/remove by evidence; staff time-boxes learning and verifies by re-run |
| Q9.3 | B:M A:M H:M | mid crams; senior rehearses depths and redraws; staff prepares interviewer questions and a blank-moment plan |

Q9.1 is `B:S` because the latency angle matters most for AI and voice projects; for a backend-only learner the same question runs on their own request path.

## 5. Interview Master integration: resume-claim risk scoring

This section specifies how this course feeds Interview Master (see scope document, project deep-dive type and final report). It is a design input for the engineering team, not a hiring-prediction model.

**Principles**
- Questions are generated only from the **learner-confirmed Profile Facts Ledger** (R-C2-1): the learner reviews and confirms each project, technology and claim before it can be probed. The ledger is optional, consent-based and deletable (see header).
- Scoring is rubric and evidence based, per claim, from the transcript. It never infers emotion, personality or confidence, and never predicts hiring outcomes.
- The learner can mark a claim "do not probe" and can edit or remove any claim; removed claims stop being generated.

**Risk dimensions per claim (each scored with evidence quotes)**

| Dimension | What is measured |
|---|---|
| Specificity | Does the answer include concrete, checkable detail (components, steps, failures) |
| Depth fit | Does depth match the claimed years or level (one level below the API for technologies) |
| Consistency | Do facts match the ledger and earlier sessions (team size, dates, numbers, role) |
| Attribution clarity | Is personal vs team contribution clear and plausible |
| Quantification honesty | Are impact numbers accompanied by baseline and method, or honestly labelled as unavailable |
| Boundary honesty | Does the learner state limits clearly when probes reach the edge |
| Recoverability | After a hard probe, does the learner reason sensibly or bluff |

**Claim-risk score**
- Each dimension is rated on a 1 to 5 rubric anchored by SME-labelled examples. Internally combine into a per-claim risk band (low, medium, high). Initial combination rule: weighted mean with a minimum-dimension override (for example a claim scoring 1 on consistency is at least medium regardless of the mean). Weights and thresholds are provisional internal design parameters (not externally verifiable facts) until calibrated.
- **Release policy (R-C2-4):** until calibration is met, learners see **qualitative feedback only** (dimension findings with transcript quotes, repair suggestion, no numeric score and no band). The band is enabled by an admin flag once calibration passes.
- **Calibration protocol:** (1) assemble at least 30 transcripts covering all four archetypes and both modes; (2) two SMEs label each claim per dimension and overall band independently, disagreements adjudicated; (3) run the scorer; (4) criterion: the scorer's band is within one band of the SME consensus on at least 80 percent of claims `[judgement]`; (5) report per-dimension agreement and fix weak dimensions first; (6) human sampling of live sessions after enabling; (7) re-calibrate whenever the rubric, model or prompt changes. Owner: Interview Master team, with SME-labelled sets supplied by C2 content.
- Report shows: high-risk claims first, the probe that exposed the weakness with a transcript quote, recommended repair (reword, learn, remove), and links to modules (M1, M4, M5, M6 and so on).
- Cross-session view: claims whose risk is not decreasing, and inconsistencies between sessions.
- Calibration: SME-labelled answer sets and periodic human review of sampled sessions, as for the rest of the adaptive engine.

**Question generation rules**
- Each confirmed claim maps to a probe ladder (definition, personal use, failure, one level below, alternative) and a why-ladder for decisions.
- The engine escalates depth until the learner's limit is found, then records the boundary rather than pushing indefinitely. Strict mode gives no hints; coaching mode offers a nudge between levels.
- Technology probes are limited to technologies on the confirmed list; the engine does not probe technologies the learner never claimed, except as comparison questions.

## 6. Final assessment

| Component | Description |
|---|---|
| Mastery check | 20-item mixed check (concept, spot-the-weakness in transcripts, rewrite items). Pass 80% (G-7). Revision loop on misses. |
| Claim ledger audit | The learner submits the confirmed ledger; the platform checks completeness, that each high-risk claim has a repair or a reasoned keep, and that numbers are consistent across all documents. |
| Timed task (structured writing) | 30 minutes: given a short generic scenario resembling one archetype (provided by the platform), produce the 8-part spine, one decision record, and one metric card, with honest placeholders where data is not available. Rubric scored. |
| Canvas task | 15 minutes: draw one flagship project on the canvas from blank, annotate failure paths, and answer 3 live "what if" probes by editing the diagram. Evaluator checks consistency between graph, transcript and ledger. |
| Voice mock round | 25 minutes, strict mode, no hints: a project deep-dive on two flagships generated from the confirmed profile, including 30 s and 3 min openers, a 5-level why-ladder, a technology probe set, an impact probe, a contribution probe and a failure probe. Scoring per the rubric above with evidence quotes, speech-delivery coaching (pace, fillers, structure only) and a claim-risk report. Badge pass ("Meets", G-7): average >= 3.0, none < 2, and no unresolved high-risk claims. **Readiness** uses the Senior bar (G-6): average >= 3.5, no competency < 3, in two sessions at least 48 hours apart. |
| Outputs | Competency radar, claim-risk report, model-answer comparisons, and a study plan linked to modules. No hiring-outcome claims. |

## 7. Course-level risks and freshness notes

| Risk or freshness item | Notes |
|---|---|
| Fabrication risk | The largest risk to learners is rehearsing invented specifics. Mitigation: the platform never supplies numbers, uses labelled placeholders, and the rubric penalises inconsistency and unsupported metrics. |
| Learner honesty and gaming | A learner can confirm claims they cannot defend. Mitigation: the report states limits openly; the platform frames risk as practice feedback. |
| Confidentiality | Learners may paste proprietary information. Mitigation: warnings in M7 and at the input fields; guidance on abstraction; retention and deletion controls per the privacy policy. |
| Calibration of the claim-risk score | Weights and thresholds are provisional internal design parameters (not externally verifiable); require SME-labelled calibration and human sampling before the score is shown as a single band to learners. |
| Archetype drift | AI and voice examples (ARC-1, ARC-2) change quickly: model families, streaming APIs, pricing and typical architectures. Review every 3 months. Protocol facts for ARC-3 **verified October 2026** against the OASIS MQTT Version 5.0 standard (MQTT 3.1.1 and 5.0 are both OASIS standards): QoS 0 at most once, QoS 1 at least once (duplicates possible), QoS 2 exactly once per hop via the PUBLISH/PUBREC/PUBREL/PUBCOMP handshake (not end-to-end: application-level idempotency still needed); sessions persist across connections only per Clean Start and Session Expiry Interval (MQTT 5); shared subscriptions deliver each message to one subscriber session. Re-check at each annual review. |
| Claim-risk calibration | Single remaining open item (internal calibration, not a web-verifiable fact): weights and thresholds of the claim-risk score until the 30-transcript calibration protocol (section 5) is met; qualitative feedback only until then. |
| Canvas dependency | Free-form "my architecture" mode and graph export confirmed for 1.0 (R-C2-3, P-19); if slipped, M8 falls back to structured-text diagram entry. |
| Cultural and company variation | Expectations for project rounds differ between IT-services, Big 4, product and startup interviews. Company packs should supply emphasis (role and delivery vs depth and scale); this course teaches the stable method. Treat any claim about what a specific company asks as unverified unless sourced from public material. |
| Overlap with C1 and A6 | Behavioural stories (C1), system design from scratch (A6) and this course's own-project defence are adjacent. Boundaries are stated in the ownership notes row (G-1) and the header authoring rules. |
| Review cadence | Method content (M1 to M8): every 12 months. Archetype probe maps and Interview Master integration: every 3 months or on feature change. |

## 8. Learner-Fit Appendix (template section 6, G-11)

Module hours (Simpler / Deeper) sum to 12 / 8 h. Tags are `B:` Senior Python Backend, `A:` AI Engineer, `H:` hybrid (M/S/N). Diagnostic IDs are `Q-C2-M<n>-P01...` (placement-tagged items from the MCQ and spot-the-weakness buckets).

### LF-1 Level paths (L1-L4)

| Level | Route and hours | Practice mix | Skipped | Challenge |
|---|---|---|---|---|
| L1 (new to interviews or to defending projects; 0-1 yr relevant) | Simpler, all lessons, 12 h; do C1 first; link A5/A6 vocabulary primers | 70% guided templates and fill-in spines, 30% voice takes in coaching mode | nothing | none until M9 passes |
| L2 (junior-mid; 1-3 yr) | Simpler with known lessons skimmed, about 10 h | 50% templates, 50% voice takes; coaching mode with hints | M1 L1 intro | optional M6 reconstruction |
| L3 (experienced, 4-8 yr; strong work, weak at explaining it) | Deeper, about 8 h | 30% writing, 70% voice and canvas; strict mode from M4 | foundations in M1-M2 per bands | hostile-interviewer drills (M4), blank-canvas timer (M8) |
| L4 (senior, 9+ yr; reference learner) | Deeper net of skip, about 5.5-6.5 h `[judgement]` (M1-M2 may be skipped via ledger status) | 15% writing, 85% voice, canvas and strict mock | M1, M2 by ledger; M3 and M6 by band | Challenge items per module: cross-session consistency audit, adversarial "I would have done it differently" probe, 10x variants |

### LF-2 Module matrix

| Module | Hours S / D | Diagnostics (count) | Bands: skip / skim / study / deep-study | Tags | Interview weight % B / A / H | Rehearsal modes | Module readiness criterion |
|---|---|---|---|---|---|---|---|
| M1 Claim inventory | 1.0 / 0.75 | P01-P06 (6) | >=85 / 70-84 / 50-69 / <50 (G-5) | B:M A:M H:M | 10 / 9 / 10 | project deep-dive, behavioural | ledger confirmed, no unresolved high-risk claim, 3 flagships chosen with rationale |
| M2 Narrative spine | 1.0 / 0.75 | P01-P05 (5) | G-5 | B:M A:M H:M | 12 / 11 / 11 | voice drill, project deep-dive | 3-min take with all 8 parts, structure >= 3.5, two sessions 48 h apart |
| M3 Multi-depth | 1.5 / 1.0 | P01-P05 (5) | G-5 | B:M A:M H:M | 10 / 9 / 10 | voice drill | 30 s, 3 min, 10 min takes consistent with ledger, within time; non-technical version passes |
| M4 Decisions and why-ladder | 1.75 / 1.25 | P01-P08 (8) | G-5 | B:M A:M H:M | 16 / 15 / 15 | voice drill (strict), project deep-dive | why-ladder passed on 2 projects, no contradictions, senior bar in 2 sessions |
| M5 Technology probes | 2.0 / 1.25 | P01-P08 (8; drawn from listed tech) | G-5 per technology | B:M A:M H:M | 14 / 15 / 15 | voice drill, project deep-dive | cheat card plus voice drill at senior bar on top 8 of 10 technologies; remaining 2 reworded or flagged |
| M6 Impact | 1.0 / 0.75 | P01-P05 (5) | G-5 | B:M A:M H:M | 8 / 9 / 8 | voice drill, behavioural | metric card per claim, 3-probe "how do you know" drill passed |
| M7 Contribution, failure, NDA | 1.25 / 0.75 | P01-P05 (5) | G-5 | B:M A:M H:M | 9 / 8 / 8 | behavioural, project deep-dive | contribution map, one failure story and one NDA-safe abstraction at >= 3.5 |
| M8 Own architecture | 1.25 / 0.75 | P01-P06 (6) | G-5 | B:M A:M H:M | 14 / 14 / 14 | canvas, voice drill | blank-canvas 6-min draw plus 3 live probes, no major diagram-story gap |
| M9 Archetype workshops | 1.25 / 0.75 | P01-P05 (5) | G-5 | B:M A:M H:M | 7 / 10 / 9 | project deep-dive (full mock), voice drill | two mocks, 3 repairs logged, re-run shows targeted claims no longer high risk |

Weights sum to 100 per target (B 100, A 100, H 100). Rationale `[judgement]`: project deep-dive rounds and "why did you choose that" probes dominate senior loops; AI-engineer loops weight tech probes and latency stories slightly higher; backend loops weight own-architecture and decisions. Calibrate after the 3-learner pilot. Interview-weighted time: budget hours in proportion to weights, but never below the Deeper minimum for M4, M5 and M8.

Company-style tags: IT-services and Big 4 `M` for M2, M3, M6, M7 (role and delivery framing); global-product and Indian-product `M` for M4, M5, M8 (depth and scale); AI-startup `M` for M5, M8, M9 (what you shipped). Cross-tag `S` otherwise.

### LF-3 Experience bridge ("from what you already know")

Generic mapping for L3/L4 learners; real project specifics stay private to the learner (R-C2-5).

| Module | You already know (typical senior backend/AI profile) | Gap between using it and explaining at senior depth |
|---|---|---|
| M1 | Years of delivered projects on APIs, SQL, Redis, Docker, AWS Lambda/S3, LLM APIs, voice agents | the resume lists everything but each line is a probe; most seniors cannot rank their own claims by defensibility |
| M2 | You can describe a system you built for an hour | converting an hour into a 3-minute problem-first spine with constraints and one failure |
| M3 | You explain designs to teammates daily | holding facts constant across 30 s, 3 min and 10 min and to non-technical stakeholders |
| M4 | You made many calls under deadline (framework, queue, DB) | reconstructing honest rationale and naming the rejected alternative and its cost; surviving level 4 (evidence) |
| M5 | Daily use of FastAPI/Django/Flask, Postgres/MySQL, Redis, Docker, Lambda/S3, MQTT, LLM APIs | used vs operated vs designed with; "one level below the API" (event loop, MVCC, eviction, QoS handshake, token accounting) |
| M6 | You saw dashboards and latency graphs | baseline, window, method and attribution; saying "I did not instrument it" without losing credibility |
| M7 | Team delivery across layers; NDAs | precise I/we; project-specific failure narrative; abstraction that keeps technical substance |
| M8 | You sketch architectures often | drawing the built system (not the ideal), tracing one request, failure paths and 10x answers live, consistent with speech |
| M9 | You can do ad hoc mocks | using a risk report to repair claims and avoid script over-fitting |

### LF-4 Senior-bar framing (module level; per-question notes are in section 4)

| Module | Mid | Senior | Staff |
|---|---|---|---|
| M1 | lists everything, defends nothing | ranks claims, repairs or removes weak ones | links claims to a role thesis; mentors others' resumes |
| M2 | stack tour | problem-first spine with constraints and role in the first minute | frames business value and what the org learned |
| M3 | one memorised monologue | three nested, consistent depths; re-enters after interruption | adapts audience live including non-technical |
| M4 | "best practice" | options, criteria, accepted cost, evidence | reversibility, review triggers, decisions as policy |
| M5 | API recital | one level below, failure behaviour, honest boundary | cross-technology trade-offs, operational and cost view |
| M6 | round numbers | baseline, method, attribution | confounders, new costs, business translation |
| M7 | "we" or "I did all" | precise by layer, blameless failure, safe abstraction | systemic prevention, mentoring and bus-factor |
| M8 | boxes | request trace, trust boundaries, failure paths | built vs ideal, cost-ordered evolution |
| M9 | treats mock as guarantee | repairs by evidence and re-runs | builds a repeatable prep routine across rounds |

### LF-5 Adaptive hooks

- **Mastery thresholds:** module 80% (G-7); readiness retake at 85% spaced >= 7 days (G-6); course final 80%.
- **Remediation links:** M1 -> C1.M1-M3 (story basics); M2/M3 -> C1.M9 (spoken narrative) and C3.M1 (structure habit); M4 -> A6.M1 (trade-off vocabulary); M5 -> owner lessons in the depth-probe packs (A2, A4, A5, B2, B6, A7); M6 -> A8.M10 (measurement method); M7 -> C1 failure and conflict lessons; M8 -> A6.M10 (canvas craft) and A5.M10 (layering and boundaries); M9 -> M1, M4, M5 by report finding.
- **Challenge extensions** when mastery >= 90% on first attempt: adversarial probes, 10x variants, cross-session consistency audit, company-style re-weighting profile.
- **Reordering:** the adaptive engine may reorder M5 to the front for learners whose ledger shows many technologies, and may defer M8 until the canvas free-form mode is available to the learner; M1 must precede M9; M4 precedes M8 for L1-L2. M9 is never skipped.
- **Dependencies (boss graph):** C1.M1-M3 required before C2.M1 (solid edge; C1 continues as a parallel thread); A5, A6 and B6 vocabulary recommended (dotted edges); B2 recommended for LLM vocabulary; C2 is required before C4.M5-M9.

### LF-6 Applied labs (one production-framed lab per module, generic archetypes)

| Module | Lab |
|---|---|
| M1 | Audit a generic resume for an ARC-1 style API-with-assistant profile; rank 12 claims and repair 4 |
| M2 | Convert a rambling transcript of an ARC-3 IoT access-control project into the 8-part spine |
| M3 | Produce 30 s, 3 min and non-technical versions of an ARC-4 field-service platform |
| M4 | Reconstruct a decision record for a queue choice in an ARC-2 voice platform when you did not make it alone |
| M5 | Build cheat cards for five technologies from an ARC-1 stack and drill probes 4-8 |
| M6 | Rewrite five impact bullets for an uninstrumented SaaS project with a metric card and a no-numbers answer |
| M7 | Write an NDA-safe abstraction of a client enterprise project that preserves patterns and failure modes |
| M8 | Blank-canvas draw of an ARC-2 or ARC-3 system in 6 minutes and answer "broker drops" and "10x load" |
| M9 | Two full mocks on two archetypes; act on the qualitative risk report and re-run |

### LF-7 Currency and quality bar (dated October 2026)

| Module | Memorable visual or analogy | Top 3 misconceptions handled |
|---|---|---|
| M1 | "Resume as an attack surface": each line is an entry point; the interviewer picks the least defended | more keywords help; deleting weak claims is cheating-proof; titles equal scope |
| M2 | Elevator with eight floors (spine boxes with time budgets) | tech first is impressive; failures hurt credibility; longer is better |
| M3 | Nested onion where inner facts never change | memorised script is safest; depth equals length; non-technical means dumbing down |
| M4 | The why-ladder: five rungs, each rung must hold weight | "best practice" is a reason; I must pretend I chose it; admitting constraint is weak |
| M5 | Depth meter per skill: used, operated, designed | claiming more earns more; QoS 2 means no duplicates end-to-end; knowing the API equals knowing the tool |
| M6 | Metric card with baseline, window, method, share | round numbers sound credible; percentages need no baseline; no numbers means no impact |
| M7 | Contribution map by layer | "we" is humble; failure stories need drama; NDA means silence |
| M8 | Drawing order: actors, entry, services, data, async, externals | ideal design is allowed; more boxes show more skill; failure paths are optional |
| M9 | Repair loop: fix, learn or remove | high score is a guarantee; scripts beat understanding; deleting weak claims is progress |

Readiness (course level): all module readiness criteria met; final voice mock at Senior bar in two sessions 48 h apart; Interview Master project deep-dive and canvas modes at Senior bar (G-6); no open high-risk claim. Rehearsal link: Interview Master project deep-dive mode (primary), voice drill, canvas, behavioural (M7).

## 9. Open items for the boss (real ones only)

1. **Canvas free-form delivery date.** M8 needs P-19 free-form mode and graph export by closed beta; confirm date or accept the structured-text fallback.
2. **Calibration labelling capacity.** The 30-transcript protocol needs two SME labellers; confirm who (Content Lead panel) and when, since the claim-risk band stays off until then.
3. **Ledger onboarding placement.** Confirm the Profile Facts Ledger confirmation step ships in onboarding (not only inside C2) so C1 and Interview Master can use it from day one.

## 10. Boss compliance log

| ID | Applied |
|---|---|
| G-1 | Ownership notes row added to the module table; pointers to C1, C4, A6, C3 stated; no labs or new items for pointer topics |
| G-2 | Effort 12 / 8 h plus 2.5 h assessment (boss effort table); Simpler/Deeper semantics sentence added; module hours sum to header |
| G-3 / G-4 / G-5 | Tags `B:M A:M H:M`, L1-L4 paths and G-5 bands in LF-1 and LF-2 |
| G-6 / G-7 | Readiness (Senior bar, two sessions 48 h apart) vs badge ("Meets") separated; mastery 80% |
| G-9 | Hyphenated tags; `behav.ownership` demoted to secondary (C1 owns) |
| G-10 | Coding bucket relabelled to applied artefact tasks x4; IDs `Q-C2-M<n>-<nn>`; goal tags and senior-bar notes on all 27 seeds |
| G-11 | Sections LF-1 to LF-7 in section 8 |
| G-12 | Dated October 2026; no invented figures; MQTT facts verified |
| R-C2-1 | Profile Facts Ledger is the single claim ledger; consent and deletion note in header |
| R-C2-2 | M7 reuses C1 failure and conflict stories by reference; separate tags |
| R-C2-3 | Canvas free-form mode confirmed; diagram-vs-story consistency rubric defined (qualitative, no rule IDs) |
| R-C2-4 | Qualitative feedback only until calibration; 30-transcript protocol documented in section 5 |
| R-C2-5 | Generic fictional archetypes only; real projects private learner data |
| R-C3-3 | M3 owns multi-depth including non-technical audiences; C3.M2 holds a pointer |
| R-C1-1 / R-C1-4 | C2 owns project-level narrative only; ledger shared with C1 and C4 |
| O-105 / O-106 / O-109 | Ownership recorded; own-architecture defence references A6.M10 and canvas v0.1 |
| P-19 / P-20 | Canvas and ledger capabilities referenced; fallback noted |
| Boss C2 items 1-9 | 1 appendix and L4 skip; 2 ledger; 3 M3 and M7; 4 M8 rubric; 5 section 5; 6 and 7 archetype policy and bridge; 8 depth-probe packs; 9 relabel |

| Track Lead C (2026-10-05) | Dependencies line corrected to C1.M1-M3 required; Q7.1 retagged to an owned tag; outcome 6 credit tag fixed; MQTT remediation pointer moved to A6.M9; M8 remediation pointer corrected; difficulty scale stated. See `_reviews/TrackLead_C_Report.md` |
