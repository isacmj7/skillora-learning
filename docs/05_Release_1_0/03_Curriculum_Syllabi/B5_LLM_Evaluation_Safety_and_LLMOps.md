---
name: B5 — LLM Evaluation, Safety and LLMOps
course_id: B5
track: B (AI Engineer)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-16 (see _reviews/Boss_Final_Approval.md)
---

# B5 — LLM Evaluation, Safety and LLMOps (Track B)

Version 2.0 (wave-2 upgrade). Last reviewed: 2026-10-06 (October 2026; track-lead B review). AI/LLM content is volatile: tool versions, regulatory dates and framework APIs change quickly; no model names appear in lessons (provider notes only, G-8). Facts verified by web search on 2026-10-05 carry a dated source note; items still marked **[verify]** need the exact check stated beside them.

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | B5 — LLM Evaluation, Safety and LLMOps / Track B (AI Engineering) |
| Audience | Senior Python backend engineers (L3–L4) moving into AI Engineer roles; already ship LLM features via Claude/OpenAI APIs and want to prove they are reliable, safe and operable |
| Prerequisites | B2 (LLM Application Engineering) is required (solid edge B2 to B5), especially B2.M10 (guardrails and injection preview, at most 1 h) and B2.M11 (mini eval set). B1.M10 (experiment design and the statistics lesson: bootstrap confidence intervals, power, multiple comparisons) is the owner of statistics; B5 only applies it. Working Python, pytest, HTTP services. B3 (RAG) and B4 (Agents) are recommended (dotted edges) for M2 and M4 |
| Placement skip | Per-module placement bands follow G-5 (85 or more skip lessons and take the mastery check; 70-84 skim; 50-69 study; under 50 deep-study); details in section 8 (LF-2). No band waives the course final assessment. M4 (injection and red-team) and M8 (release engineering) carry the most interview weight; their mastery checks are never waived, and a skip there still requires the timed lab check |
| Outcomes (measurable, interview competency in brackets) | 1. Design an eval suite for an LLM feature: golden set construction, slicing, regression gating, offline vs online split (`llm.eval-design`). 2. Select and justify metrics per task type and report them with uncertainty (`llm.metrics`). 3. Build an LLM-as-judge, name its biases, and calibrate it against human labels with agreement statistics and confidence intervals (`llm.judge`). 4. Threat-model an LLM application and implement layered defences against direct and indirect prompt injection and jailbreaks (`llm.security`). 5. Design PII handling and hallucination mitigation for a production pipeline (`llm.privacy`, `llm.grounding`). 6. Architect a guardrail and content-safety layer with explicit latency, cost and false-positive trade-offs (`llm.guardrails`). 7. Instrument, monitor and detect drift for an LLM service, and operate versioned prompt/model rollouts (canary, shadow, A/B) with rollback (`llm.llmops`). 8. Run an AI incident end to end and explain governance obligations at awareness level (India DPDP, EU AI Act) (`llm.incident`, `llm.governance`) |
| Estimated effort | **16 h Simpler / 11 h Deeper** plus 2.0 h assessment allowance (final mastery check, timed task, canvas task, voice mock, revision loops), matching the boss effort table. Simpler = L1-L2 route with full scaffolding; Deeper = L3-L4 route with foundations skimmed per placement, hours net of typical skip-ahead (so Deeper is shorter in wall-clock time but harder per hour). Module hours (S/D): M1 2.0/1.5, M2 1.5/1.0, M3 2.0/1.5, M4 2.5/1.5, M5 1.5/1.0, M6 1.5/1.0, M7 1.75/1.25, M8 2.0/1.5, M9 1.25/0.75 = 16.0/11.0 |
| Interview coverage tags | Owned (credit issued here): `llm.eval-design`, `llm.metrics` (LLM-eval usage), `llm.judge`, `llm.security`, `llm.privacy`, `llm.grounding`, `llm.guardrails`, `llm.llmops`, `llm.incident`, `llm.governance`, `llm.responsible-ai`. Secondary (no credit): `design.ai-systems` (owned by A6), `ml.experiment-design` (owned by B1.M10) |
| Badge and certificate | Course badge "LLM Reliability Engineer (B5)": all 9 module badges (module mastery 80%, G-7), final mastery check at 80% or more, voice mock at "Meets" (average 3.0 or more, none under 2), and a portfolio artefact set (eval repo with CI gate and confidence intervals, judge calibration report, threat model, rollout runbook). Interview readiness is a separate, stricter criterion (G-6, section 8 LF-2). Certificate states version and review date. Evidence is stored as code submissions and graded rubric output |
| Authoring owner | SME agent B5 (wave 2, v2.0) |
| Reviewer | SME reviewer role: AI safety and LLMOps practitioner from the Content Lead's SME panel (to be named); separate human legal reviewer for the gated lessons M5 (Aadhaar and sector handling) and M9 (EU AI Act, DPDP, regulator guidance), per PCR-069 |
| Review cadence | Every 3 months (fast-moving); regulatory and tooling items re-verified at each review |

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice tasks | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Eval design and golden sets | Design an eval strategy for an LLM feature including dataset sourcing, slicing, offline vs online roles and a regression gate | **Lesson M1.0 / X5 (20 min, first thing in every B-track lab): LLM Lab Harness orientation** (record and replay fixtures, scripted mock model, budget meter, recorded-response mode is graded, live mode optional behind gateway caps); why "vibes" fail; eval pyramid (unit-style assertions, golden set, judge-scored, human, online); sourcing cases from production logs, SME-written, synthetic with review; coverage by slice (intent, language incl. Hinglish, length, difficulty, adversarial); dataset versioning and contamination/leakage; train/dev/test discipline for prompts (overfitting prompts to the golden set); regression suite vs capability suite; eval harness design (inputs, expected, graders, run config, seed, cached model outputs); statistics applied (bootstrap confidence intervals, paired comparison, resampling at the case level rather than the run level when cases are run several times; concepts owned by B1.M10, here only applied to eval pass rates); offline vs online evals | Eval pyramid; golden set lifecycle loop (log, triage, label, add, version); offline-to-online funnel | Build a pytest-based harness in Python over a 40-case golden set for a support-ticket classifier plus summariser; add slice reporting; add bootstrap CI; write a gating threshold with justification | Golden set of 20 hand-picked happy paths; tuning prompts until the set passes, then claiming quality; one aggregate number with no slices; ignoring nondeterminism (single run); no dataset versioning |
| M2 | Metrics and task-specific evaluation | Choose, compute and interpret metrics per task type and report variance honestly | Exact match, F1, precision/recall for extraction and classification; schema validity and field-level accuracy; BLEU/ROUGE limits and why they mislead for open generation; embedding similarity (BERTScore style) caveats; RAG metrics (context precision/recall, faithfulness, answer relevance; retrieval hit rate, MRR, nDCG) as cross-reference to B3; pass@k and best-of-n for code/agents; agent metrics (task success, steps, tool-call accuracy, cost per success) cross-reference B4; refusal and over-refusal rate; latency percentiles and cost per successful task; multiple-run variance, temperature 0 is not deterministic; paired comparison and significance for prompt A vs B; Goodhart's law | Metric chooser decision tree; confusion matrix with business-cost overlay; variance across N runs plot | Compute field-level F1 for an invoice extractor; compare two prompts with paired bootstrap; produce a metric card (what it measures, blind spots) for three tasks | Quoting BLEU for chat quality; accuracy on imbalanced classes; comparing prompts on one run; ignoring cost of false negatives vs false positives; treating judge score as ground truth |
| M3 | LLM-as-judge and human evaluation | Build a judge, expose its failure modes and calibrate it against human labels | Pointwise vs pairwise vs reference-based judging; rubric design with anchors and chain-of-thought-then-score; known biases (position, verbosity, self-preference, formatting, sycophancy to stated opinions); mitigation (swap order, multiple judges, ensembles, structured rubrics, reference answers, score decomposition); judge model choice and cost; calibration: collect 100 or more human labels, measure agreement (Cohen's kappa, Krippendorff's alpha, Spearman for ordinal; report raw percent agreement and the label distribution beside kappa, because kappa can look low when one label dominates), inspect disagreements, iterate rubric; human evaluation design (guidelines, annotator training, inter-annotator agreement, adjudication, annotation tooling, sampling); when not to use a judge (math, factual verification without references, safety edge cases); judge drift when the judge model is updated; pin and version the judge | Judge pipeline diagram; position-bias swap test step-through; calibration loop; agreement vs rubric-iteration chart | Implement pairwise judge with order swap; compute kappa against provided human labels; find three systematic judge errors and fix rubric; write annotation guideline for a 3-point helpfulness scale | "Use a model to grade a model, done"; no human anchor; same model as generator and judge; a single 1-10 score without rubric; ignoring position bias; reporting judge scores without agreement stats; changing the judge model silently and comparing runs across it |
| M4 | Red-teaming, prompt injection and jailbreak defence | Threat-model an LLM app and implement layered mitigations that assume the model can be tricked | Threat modelling (assets, trust boundaries, attacker goals); direct injection vs indirect injection (web pages, emails, documents, tool outputs, RAG chunks); jailbreak families (role-play, encoding/obfuscation, multi-turn escalation, many-shot, multilingual) described at defender level; data exfiltration paths (markdown image rendering, link unfurling, tool calls to attacker URLs); agent-specific risks (excessive agency, confused deputy, tool poisoning, MCP server trust); OWASP Top 10 for LLM Applications (a 2026 edition was published on 3 August 2026 on the OWASP GenAI Security Project site, verified 2026-10, so teach the risk classes rather than fixed IDs; 2025 list for reference: LLM01 Prompt Injection, LLM02 Sensitive Information Disclosure, LLM03 Supply Chain, LLM04 Data and Model Poisoning, LLM05 Improper Output Handling, LLM06 Excessive Agency, LLM07 System Prompt Leakage, LLM08 Vector and Embedding Weaknesses, LLM09 Misinformation, LLM10 Unbounded Consumption; secondary summaries of the 2026 edition report reordering and retitling of some entries but disagree with each other, so lessons and assessment items use the risk-class names and never the 2026 numbers until read from the primary document **[verify: exact 2026 numbering and titles against the OWASP PDF on genai.owasp.org]**); agent-specific guidance and MCP server trust (treat tool descriptions and server outputs as untrusted input; tool poisoning) (OWASP Top 10 for Agentic Applications for 2026, ASI01 to ASI10, published 9 December 2025, and the OWASP Practical Guide for Secure MCP Server Development, February 2026; verified 2026-10 on genai.owasp.org); defences: privilege separation, least-privilege tools, human confirmation for side effects, untrusted-content delimiting and its limits, input/output classifiers, output sanitisation and URL allowlists, dual-LLM or quarantined-model patterns, egress controls, rate limiting; "no prompt-only defence is sufficient"; red-team process: scoping, attack library, automated attack generation, scoring attack success rate, regression suite of past attacks; responsible disclosure; bug-bounty style reporting | Trust-boundary diagram for RAG agent; indirect injection sequence diagram; defence-in-depth layers; exfil path step-through | Given a vulnerable FastAPI RAG-plus-tools demo (provided in sandbox with synthetic data only), find three injection paths, patch them with tool privilege reduction and output sanitisation, and add attack cases to a regression suite; compute attack success rate before and after | "Tell the model to ignore malicious instructions"; relying on a single classifier; trusting tool or RAG output; giving agents broad credentials; thinking jailbreak and prompt injection are the same; fixing one payload instead of the class |
| M5 | PII, privacy and hallucination mitigation | Design data-protection and grounding controls for an LLM pipeline | PII taxonomy (direct identifiers, quasi-identifiers; India context: Aadhaar, PAN, phone, UPI IDs; Aadhaar-number handling is restricted by its own legal regime, so the lesson teaches detect-and-never-store as a design stance and flags exact legal rules **[verify: human or legal review of Aadhaar and sector-regulator handling rules]**); detection (regex plus validators such as checksum, NER, Presidio-style tooling (Microsoft Presidio is an actively maintained open-source PII detection and anonymisation SDK; latest PyPI release presidio-analyzer 2.2.364 on 2026-07-22; verified 2026-10)); redaction vs tokenisation (reversible pseudonymisation) vs masking; where PII flows (prompts, logs, traces, eval datasets, vector stores, fine-tuning data, caches, provider retention); provider data-use and retention terms, zero-retention options, regional processing (taught as a due-diligence checklist; terms differ per provider and change, so no provider claims are stated in lessons and the dated Provider Notes sidebar carries them); log hygiene and retention; right-to-erasure implications for vector stores and caches; secrets in prompts. Hallucination: taxonomy (intrinsic vs extrinsic, fabricated citations, stale facts); mitigations (retrieval grounding, citation with span verification, constrained decoding and schema, abstention and "I don't know" calibration, self-consistency, verifier or NLI check, tool use for facts, lower-risk UX framing, human review for high stakes); measuring hallucination (faithfulness against sources, unsupported-claim rate, citation precision) | PII flow map across the stack; redact-process-restore sequence; grounding pipeline with verifier | Write a redaction middleware for FastAPI with reversible tokens and unit tests including Indian identifiers (synthetic); add a citation-verification step to a RAG answer and measure unsupported-claim rate on a labelled set | "Temperature 0 stops hallucination"; redacting prompts but logging raw output; PII in eval datasets and traces; trusting the model's own confidence statements; citations that are not verified against sources |
| M6 | Guardrails and content safety architecture | Design a guardrail layer with measured effectiveness and explicit trade-offs | Guardrail placement (input, retrieval, tool-call, output, conversation-level); rules vs classifiers vs LLM-based checks; content-safety categories and policy definition (what the product allows is a policy decision, not a model default); provider moderation endpoints and open-weight safety classifiers (specific families and versions live in the dated Provider Notes sidebar, not in lessons); frameworks (NeMo Guardrails, PyPI latest 0.24.1 on 2026-09-16 (verified 2026-10) and still pre-1.0, so treat as fast-moving with `[verify: beta label in current NVIDIA docs]` rather than a production default; Guardrails AI validators actively maintained; web sources 2026-10-05) vs hand-rolled (hand-rolled deterministic checks plus one classifier is the default teaching stance); schema and business-rule validation; topic and scope enforcement; refusal design and over-refusal measurement; fail-open vs fail-closed decisions; latency and cost budget of guardrails (parallelise checks, short-circuit, small models, streaming output moderation with buffering); evaluating guardrails (precision and recall on safe and unsafe sets, per category, per language including code-mixed Hindi-English); multilingual and low-resource gaps; guardrail drift and appeals/feedback loop | Guardrail placement map; latency waterfall with parallel checks; precision-recall tradeoff slider | Add input and output guardrails to an LLM endpoint with async parallel checks; measure added p95 latency; build a labelled safe/unsafe set (provided, synthetic) and report per-category precision and recall; implement fail-closed behaviour with a graceful fallback message | Guardrail equals "moderation API call"; no measurement of false positives; ignoring streaming; English-only test sets; fail-open on timeout for high-risk categories without a decision record |
| M7 | Observability, monitoring and drift | Instrument an LLM service and define alerts and dashboards that detect quality, safety, cost and latency regressions | Tracing (spans for retrieval, prompt build, model call, tool calls, guardrails; OpenTelemetry GenAI semantic conventions (every gen_ai.* attribute is at Development status as of 2026-10-05; they moved to a dedicated repository in June 2026 and attribute names changed several times in 2026, so wrap them in an internal naming layer and do not build long-term dashboards directly on raw names)); tooling landscape (Langfuse, LangSmith, Arize Phoenix, Helicone, Weights and Biases Weave, Datadog and cloud-native options; ownership is shifting: Langfuse was acquired by ClickHouse in January 2026 and Dynatrace announced an agreement to acquire Arize in August 2026 and completed the acquisition on 1 October 2026 (Dynatrace announcement, verified 2026-10; Phoenix stays open source), so vendor-risk and exit plan are part of build vs buy) and build vs buy; what to log and what not to log (PII, retention, sampling); metrics: latency (TTFT, tokens per second, end-to-end p50/p95/p99), error and rate-limit rates, token counts, cost per request, cost per successful task, cache hit rate, refusal rate, guardrail trigger rate; online quality: user feedback (thumbs, edits, regenerate), implicit signals, sampled online judge scoring, human review queues; drift types (input distribution, model/provider silent updates, retrieval corpus drift, prompt-template drift, judge drift, user-behaviour drift); drift detection (embedding clustering, topic distribution shift, PSI/KL on low-dimensional features such as length, language and topic proportions (suitable); PSI or KL directly on raw high-dimensional embeddings is unreliable, so use distance-to-reference-centroid, clustering proportions or a classifier two-sample test (verified 2026-10 against practitioner sources, which recommend centroid cosine drift, maximum mean discrepancy or a domain classifier for embeddings), canary eval set run on a schedule); alert design (symptom-based, SLOs, error budgets for quality); cost and latency dashboards | Trace waterfall for a RAG agent request; dashboard wireframe (quality, safety, cost, latency); drift detection pipeline | Instrument a FastAPI LLM service with OpenTelemetry spans and token/cost metrics; build a scheduled canary-eval job that alerts on score drop; design (on canvas) a four-panel dashboard with alert thresholds | Monitoring only HTTP 200 and latency; logging full prompts with PII forever; no way to detect silent provider model change; alerting on averages not percentiles; no link between traces and eval datasets |
| M8 | Versioning, rollout and LLMOps pipelines | Operate prompts and models as versioned artefacts with eval-gated CI/CD and safe rollout. **Boundary with B7.M9:** B5.M8 owns LLM release engineering (prompt, model-snapshot, retrieval and judge versioning, canary, shadow, A/B, rollback, kill switches); B7.M9 owns the model-artefact lifecycle (registry, lineage, classical drift on trained models) and points here for rollout | Prompt as code vs prompt registry; semantic versioning of prompt, model, retrieval config, tool schemas, judge and dataset as one "release bundle"; pinning model snapshots vs floating aliases; provider deprecation handling and migration playbook; CI: run offline eval on PR, compare with baseline using paired stats, block on regression thresholds, cost of CI evals (caching, sampling, tiered suites); rollout strategies: shadow, canary, A/B, interleaving, feature flags, progressive delivery with automatic rollback triggers; online experiment design for LLMs (metric choice, guardrail metrics, sample size, novelty effects, nondeterminism, user stickiness); model upgrade checklist; fallback chains and multi-provider failover (pointer to B2.M8); cost controls (budgets, per-tenant quotas, routing small-to-large, caching); gateway pattern; rollback and kill-switch design; environments (dev, staging with replayed traffic, prod) | Release bundle diagram; CI eval gate flow; canary rollout timeline with rollback trigger; A/B experiment flow | Build a GitHub Actions style pipeline (YAML provided; the sandbox does not run a real GitHub Actions runner: a CI-YAML linter plus a local step runner executes the eval script, per platform capability P-11) that runs the eval suite on a prompt change and fails on regression; implement a canary router with feature flag and automatic rollback on guardrail-metric breach in a FastAPI service; write a model-migration plan | Editing the prompt in production console; floating model alias; changing prompt, model and retrieval together then not knowing what caused a change; A/B test with no guardrail metrics; eval gate with no confidence intervals leading to flaky CI; no rollback path |
| M9 | Incident response, governance and responsible AI | Run an AI incident from detection to post-mortem and explain governance obligations at awareness level | AI incident taxonomy (harmful output, data leak, injection exploit, hallucination causing harm, bias, runaway cost, provider outage, silent regression); severity matrix; detect-triage-contain-eradicate-recover-learn flow; containment levers (kill switch, feature flag, tighten guardrails, disable tools, route to fallback, rate limit); evidence preservation vs privacy; communication and escalation; blameless post-mortem producing new eval cases and guardrails; incident registry. AI governance only (this module owns AI-specific governance; DPDP Act, GDPR basics, retention, consent, residency and breach notification live in shared lesson X1 owned by A8.M9.4 and are linked, 10-minute pointer only, preview if A8 not yet taken): EU AI Act (risk tiers, prohibited practices (applicable since February 2025), high-risk obligations, general-purpose AI model obligations (applicable since August 2025), Article 50 transparency; later dates as of 2026-10-05 per law-firm and EU-institution-derived web sources: the Digital Omnibus on AI was adopted in June 2026, published in the Official Journal on 24 July 2026 as Regulation (EU) 2026/1744 and in force from 27 July 2026, deferring Annex III standalone high-risk obligations to 2 December 2027 and Annex I product-embedded systems to 2 August 2028, while Article 50 transparency duties stay on 2 August 2026 with a grace period to 2 December 2026 for watermarking of systems already on the market **[verify: dates match law-firm summaries; article-level wording on EUR-Lex not read directly]**); NIST AI RMF (voluntary risk framework: govern, map, measure, manage) and ISO/IEC 42001 (certifiable AI management-system standard) at awareness level (AI RMF 1.0 is the published version; a revision was directed in July 2025 and a secondary tracker reports no draft by late September 2026 **[verify: NIST page not read directly, recheck next review]**); DPDP Rules phasing awareness only via X1 (Rules notified 14 November 2025; Consent Manager provisions after 12 months, most obligations after 18 months, around May 2027; PIB confirms notification on 14 November 2025 with an 18-month phased period, verified 2026-10; RBI's FREE-AI committee report of 13 August 2025 is a recommendations report and SEBI consulted on AI/ML guidelines in June 2025 and said in 2026 that final guidelines are coming **[verify: binding status of RBI, SEBI and IRDAI AI rules needs human legal review]**); model cards, system cards, data sheets, audit trails, human oversight, DPIA-style assessments, vendor due diligence; responsible AI: fairness testing across slices (language, region, gender, caste/religion proxies in Indian context), bias evaluation approaches, transparency to users, accessibility, environmental cost awareness, human-in-the-loop for consequential decisions. This is awareness, not legal advice | Incident lifecycle swimlane; severity matrix; regulatory landscape map (awareness); responsible-AI checklist | Tabletop exercise: given a scenario of a chatbot leaking another user's data via cache key bug, write the incident timeline, containment steps, notification decision points and post-mortem with three new eval/guardrail items; fill a lightweight AI risk register and model card for the course project | Treating compliance as "legal's job"; no kill switch; incident response that ends at the hotfix; claiming certainty on regulation dates (they moved in 2026); fairness evaluated only on aggregate; describing DPDP or EU AI Act as identical to GDPR |

### Ownership notes, mental models and misconception handling (one row per module)

| Module | OWNS | POINTS | Memorable visual or analogy | Top misconceptions handled |
|---|---|---|---|---|
| M1 | General eval methodology, golden sets, harness design, offline/online split, regression gating; X5 harness orientation | statistics concepts -> B1.M10; RAG retrieval golden sets -> B3.M8 | Smoke detector versus fire drill: the eval pyramid is the detectors (cheap, always on), the human audit is the drill | "Temperature 0 means deterministic"; "a high pass rate on my own examples proves quality"; "one run is enough" |
| M2 | Metric selection, variance, paired comparison, cost and latency as metrics | RAG metric definitions -> B3.M8; agent trace metrics -> B4.M8 | Metric card as a nutrition label (what it measures and what it hides) | "BLEU measures chat quality"; "accuracy is fine on imbalanced classes"; "3 points higher means better" |
| M3 | LLM-as-judge design, biases, calibration, human evaluation | RAG-faithfulness judge details -> B3.M8; agent judge usage -> B4.M8 | The judge is a thermometer that must be checked against a reference thermometer before you trust a fever reading | "A strong judge model is ground truth"; "kappa 0.6 is a universal pass"; "same model as generator is fine" |
| M4 | Prompt injection, jailbreak classes, red-teaming, OWASP LLM Top 10, agent privilege design for untrusted content | agent tool-permission design -> B4.M9; generic threat modelling -> A8.M9; OWASP API -> A8.M7; injection basics -> B2.M10 | Confused deputy as a bank clerk who obeys any note slipped into the paperwork; defence is giving the clerk a smaller cash drawer, not a better lecture | "Delimiters stop injection"; "injection equals jailbreak"; "patch the payload, not the class" |
| M5 | PII flow mapping and redaction in LLM pipelines, hallucination mitigation and measurement | DPDP, GDPR basics, retention, erasure law -> X1 (A8.M9.4); RAG retrieval fixes -> B3 | Plumbing diagram: PII is water, find every pipe and every tap (logs, traces, vector store, cache) | "Redacting the prompt is enough"; "temperature 0 stops hallucination"; "model confidence is calibrated" |
| M6 | Guardrail architecture, placement, latency and cost, evaluation of guardrails | provider moderation specifics -> Provider Notes sidebar (B2 template X4) | Airport security lanes: cheap scan first, deeper check only for flagged risk, with a decision on what happens when the scanner is down | "A guardrail is a moderation API call"; "fail-open is harmless"; "English-only test sets suffice" |
| M7 | LLM observability, online quality signals, drift, quality SLOs | generic metrics, traces, alerting -> A7.M9; voice observability -> B6; agent traces -> B4.M8 | Flight recorder plus cockpit gauges: traces replay what happened, SLO gauges say when to worry | "200 OK means healthy"; "log everything"; "PSI on raw embeddings is a drift test" |
| M8 | LLM release engineering, eval-gated CI, shadow, canary, A/B, rollback, kill switch, cost governance | experiment statistics -> B1.M10; model-artefact lifecycle -> B7.M9; provider failover -> B2.M8 | Bundle as a sealed shipping container with one label; you roll back containers, not loose parts | "Floating alias is fine"; "prompt edits are not deployments"; "A/B without guardrail metrics" |
| M9 | AI incident response for LLM features, AI-specific governance (EU AI Act, NIST AI RMF, ISO/IEC 42001), model cards, responsible AI and fairness testing | DPDP, GDPR, breach law -> X1; voice consent -> B6 | Aviation incident model: contain first, then investigate, then change the checklist so it cannot recur | "Compliance is legal's job"; "EU AI Act equals GDPR"; "fix the prompt first" |

## 3. Per-module detail blocks

Question-bank targets per module follow the authoring template: 12 MCQ/concept, 6 code-output/predict, 4 coding tasks (hidden tests), 8 verbal with follow-up trees, 3 scenario/design. Mastery check: 6–10 items drawn from the bank, pass at 80%, revision loop on misses.

### M1 — Eval design and golden sets
- Hours Simpler / Deeper: 2.0 / 1.5. Lesson IDs: M1.1 to M1.5 (lessons below in order).
- Lessons: (0) X5 LLM Lab Harness orientation (20 min); (1) Why LLM features need evals and what an eval pyramid looks like; (2) Building a golden set: sourcing, labelling, slicing, versioning; (3) Harness design and run reproducibility; (4) Statistics applied to evals: variance, bootstrap CIs and paired comparison on pass rates (concepts from B1.M10; no re-teaching of theory); (5) Offline vs online roles and regression gates.
- Simpler: stay with a classification-style task and exact-match graders; use provided bootstrap helper. Deeper: design eval for multi-turn and tool-using features; leakage and contamination audit; cost-tiered suites.
- Mastery focus: offline vs online, slicing, overfitting to dev set, flaky gate diagnosis.
- Badge: harness with 40+ case golden set, slice report, CI-style gate script, written justification of threshold.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

### M2 — Metrics and task-specific evaluation
- Hours Simpler / Deeper: 1.5 / 1.0. Lesson IDs: M2.1 to M2.5 (lessons below in order).
- Lessons: (1) Metric families and the chooser tree; (2) Extraction and classification metrics with business cost; (3) Generation, RAG and agent metrics; (4) Variance, significance and Goodhart; (5) Cost and latency as first-class metrics.
- Simpler: precompute metrics with provided functions and interpret. Deeper: implement nDCG and paired bootstrap from scratch; discuss metric gaming.
- Mastery focus: picking the metric for a scenario, reading a confusion matrix, spotting invalid comparisons.
- Badge: metric cards for three tasks and a correct paired comparison report.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

### M3 — LLM-as-judge and human evaluation
- Hours Simpler / Deeper: 2.0 / 1.5. Lesson IDs: M3.1 to M3.5 (lessons below in order).
- Lessons: (1) Judge designs and rubric writing; (2) Biases and bias tests; (3) Calibration with human labels and agreement stats; (4) Human evaluation operations; (5) Judge governance: pinning, drift, cost, when not to judge.
- Simpler: use a provided rubric and labelled set; compute kappa. Deeper: ensemble and jury designs, ordinal agreement, active selection of cases for human review.
- Mastery focus: bias names and mitigations, interpreting kappa, deciding judge vs human vs programmatic check.
- Badge: calibration report with kappa before and after rubric iteration and a bias-test result.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

### M4 — Red-teaming, prompt injection and jailbreak defence
- Hours Simpler / Deeper: 2.5 / 1.5. Lesson IDs: M4.1 to M4.5 (lessons below in order).
- Lessons: (1) Threat modelling an LLM application; (2) Direct vs indirect injection and exfiltration paths; (3) Jailbreaks and model-level safety (defender view); (4) Layered defences and agent privilege design; (5) Running a red-team programme and regression attack suite.
- Simpler: guided patching of the vulnerable demo with hints. Deeper: design a quarantined-model pattern; attack-success-rate analysis; multi-turn and multilingual attack coverage.
- Mastery focus: classify an attack, choose the right layer, explain why prompt-only defence fails, least privilege for tools.
- Badge: threat model document, patched demo with attack regression suite, before/after attack success rate. All attack material is synthetic and defensive in framing.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

### M5 — PII, privacy and hallucination mitigation
- Hours Simpler / Deeper: 1.5 / 1.0. Lesson IDs: M5.1 to M5.5 (lessons below in order).
- Lessons: (1) PII taxonomy and flow mapping; (2) Detection and redaction/tokenisation patterns; (3) Provider terms, retention, logs and vector-store erasure; (4) Hallucination taxonomy and mitigations; (5) Measuring faithfulness and abstention quality.
- Simpler: use provided detectors and focus on pipeline placement. Deeper: reversible tokenisation with per-tenant keys; erasure design for derived data; NLI-based verifier tradeoffs.
- Mastery focus: where PII leaks, redaction vs tokenisation, which hallucination mitigation fits which risk.
- Badge: redaction middleware with tests; grounding verifier with measured unsupported-claim rate.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

### M6 — Guardrails and content safety architecture
- Hours Simpler / Deeper: 1.5 / 1.0. Lesson IDs: M6.1 to M6.5 (lessons below in order).
- Lessons: (1) Policy first: defining what to block, allow and escalate; (2) Guardrail placement and technique selection; (3) Latency, cost and streaming; (4) Measuring guardrails: precision, recall, multilingual coverage; (5) Failure modes: fail-open vs fail-closed, over-refusal, drift.
- Simpler: wire a provided classifier; measure. Deeper: streaming output moderation with buffered windows; small-model distillation of guardrail checks; per-tenant policy.
- Mastery focus: tradeoff reasoning, placement choice, failure-mode decisions.
- Badge: guardrail layer with p95 overhead measurement and per-category precision/recall report.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

### M7 — Observability, monitoring and drift
- Hours Simpler / Deeper: 1.75 / 1.25. Lesson IDs: M7.1 to M7.5 (lessons below in order).
- Lessons: (1) Tracing an LLM request end to end; (2) Metrics, SLOs and dashboards for quality, safety, cost and latency; (3) Online quality signals and sampled judging; (4) Drift types and detection; (5) Alerting, privacy-safe logging and tool choice.
- Simpler: use the provided collector and focus on dashboard reading. Deeper: define a quality SLO with error budget; drift detection via embeddings; build vs buy analysis.
- Mastery focus: what to log, which metric detects which failure, silent provider update detection.
- Badge: instrumented service, scheduled canary-eval alert, dashboard design with thresholds.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

### M8 — Versioning, rollout and LLMOps pipelines
- Hours Simpler / Deeper: 2.0 / 1.5. Lesson IDs: M8.1 to M8.5 (lessons below in order).
- Lessons: (1) Release bundle and versioning strategy; (2) Eval-gated CI; (3) Shadow, canary and A/B rollout; (4) Model migration and deprecation handling; (5) Cost governance, gateway and kill switches.
- Simpler: run the provided pipeline and interpret outputs. Deeper: sequential testing and early stopping; interleaving; replay-based staging.
- Mastery focus: attribution of regressions to a bundle component, rollout choice, rollback triggers, flaky eval gates.
- Badge: working CI eval gate, canary router with automatic rollback, model-migration plan.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

### M9 — Incident response, governance and responsible AI
- Hours Simpler / Deeper: 1.25 / 0.75. Lesson IDs: M9.1 to M9.5 (lessons below in order).
- Lessons: (1) AI incident taxonomy and severity; (2) Response lifecycle and containment levers; (3) Post-mortems that feed evals; (4) AI governance awareness: EU AI Act tiers and dates, NIST AI RMF, ISO/IEC 42001, with a 10-minute pointer to X1 for DPDP and GDPR; (5) Responsible AI practices: fairness, transparency, oversight, documentation.
- Simpler: guided tabletop with prefilled timeline. Deeper: draft a full model card and risk register; fairness slice analysis on provided data.
- Mastery focus: containment ordering, what a post-mortem must produce, awareness-level regulatory accuracy and honest uncertainty.
- Badge: tabletop write-up, risk register, model card; no legal claims beyond awareness level.
- Question-bank targets: 12 / 6 / 4 / 8 / 3.

## 4. Seed interview questions

Format per question: ideal answer outline, common weak answers, follow-up probes, difficulty, competency tag, company-style cluster. Rubric anchors per answer (1 to 5, senior bar 3.5): 1 names the topic only; 2 lists generic items without mechanism; 3 meets, with correct mechanism and one trade-off; 4 adds failure modes, measurement and operations; 5 adds organisational policy, budgets and honest uncertainty. The per-question mid, senior and staff signals are in the seed annotations table below. These are style-of-question items, not questions from any named company.

### M1 — Eval design and golden sets

**Q1.1 "You have shipped a summarisation feature with no tests. How would you build an evaluation strategy from scratch?"**
- Ideal answer:
  - Start from user and business risk: define what "good" means (faithful, complete, concise, tone) and the cost of each failure.
  - Collect real inputs from logs (with consent and PII scrubbing), supplement with SME-written and adversarial cases; target dozens first, grow to hundreds.
  - Label with rubric and at least two annotators for a subset; slice by length, domain, language, difficulty.
  - Layer graders: cheap programmatic checks (length, format, banned content), judge-scored quality calibrated on human labels, periodic human review.
  - Build a repeatable harness with pinned config, cached outputs, multiple runs and confidence intervals.
  - Gate changes in CI against a baseline; keep a separate held-out test set to detect prompt overfitting.
  - Add online signals (feedback, edits) and feed production failures back into the golden set.
- Weak answers: "Try a few examples and eyeball"; "Use a standard benchmark"; "Ask a model to rate it" with no calibration.
- Follow-ups: How many cases do you need and how do you decide? How do you stop the golden set going stale? What would you do with only a day?
- Difficulty: Medium. Tag: `llm.eval-design`. Cluster: global-product, AI-startup.

**Q1.2 "Offline evals pass but users complain after a prompt change. What went wrong and how do you prevent it?"**
- Ideal answer:
  - Golden set is unrepresentative or stale vs production distribution; no coverage of the slice that regressed.
  - Overfitting of prompt to dev set; no held-out set.
  - Metrics do not capture what users value (e.g. tone, latency, verbosity).
  - Aggregate hid a slice regression; no per-slice reporting.
  - Online factors absent offline: multi-turn context, real retrieval, latency, tool failures.
  - Prevent: refresh set from logs, slice reporting, held-out set, shadow/canary with online metrics, complaints triaged into new eval cases.
  - Distinguish offline as a gate (necessary) and online as truth (sufficient).
- Weak: "Add more test cases"; blames users; says offline evals are useless.
- Follow-ups: How do you detect the stale set automatically? How would you triage the complaints into eval cases? What online metric would have caught this fastest?
- Difficulty: Medium. Tag: `llm.eval-design`. Cluster: global-product, Indian-product.

**Q1.3 "Your eval pass rate moves between 82% and 88% across identical runs. How do you make the CI gate trustworthy?"**
- Ideal answer:
  - Quantify variance: repeat N runs, compute standard deviation and CI; check if the movement is sampling noise from the model, the judge, or small sample.
  - Reduce sources: pin model snapshot, set temperature and seed where supported (note determinism is not guaranteed), cache model outputs, pin judge.
  - Increase statistical power: larger or stratified sample; paired comparison against baseline on the same cases.
  - Gate on lower bound of CI or on significant regression, not raw threshold.
  - Tiered CI: fast smoke set on every PR, full set nightly or on label.
  - Quarantine flaky cases; investigate cases that flip.
- Weak: "Set temperature to 0 and it will be deterministic"; "Just rerun until green".
- Follow-ups: How do you pick the regression tolerance? What if a flip is genuine ambiguity in the case? Cost implications of repeated runs?
- Difficulty: Hard. Tag: `llm.eval-design`, `llm.metrics`. Cluster: global-product, global-IT-consulting.

### M2 — Metrics and task-specific evaluation

**Q2.1 "Which metrics would you use for an LLM invoice-extraction pipeline, and why not just accuracy?"**
- Ideal answer:
  - Field-level precision, recall, F1 (per field and micro/macro), because most values are present and missing/wrong fields matter differently.
  - Schema validity rate and type/format validators (dates, currency, GSTIN-like patterns, checksum-validated; format specifics are examples only and use synthetic values).
  - Normalised exact match with tolerance rules (whitespace, currency formatting) defined up front.
  - Business-cost weighting: wrong amount worse than missing vendor note; add human-review rate and straight-through-processing rate.
  - Slice by vendor, template, scan quality, language.
  - Report confidence intervals and calibration of any model confidence used for routing to review.
- Weak: "Accuracy"; "BLEU"; ignores partial credit and cost.
- Follow-ups: How do you handle line items with variable counts? How would you set the human-review threshold? How would you evaluate OCR errors separately from LLM errors?
- Difficulty: Medium. Tag: `llm.metrics`. Cluster: IT-services, Big4.

**Q2.2 "Why are BLEU and ROUGE poor for evaluating chat or summarisation quality, and what do you do instead?"**
- Ideal answer:
  - They measure n-gram overlap with references; valid paraphrases score low, fluent unfaithful text can score high.
  - Insensitive to factuality, tone, instruction following; correlate weakly with human judgement for open generation.
  - Still useful for narrow tasks (translation baselines, trend tracking, extractive summaries) as a sanity signal.
  - Better: faithfulness checks against source (NLI or judge with evidence), rubric-based judge calibrated to humans, pairwise preference for A/B, task-specific programmatic checks.
  - Combine multiple metrics and always keep human spot checks.
  - Mention embedding similarity limits (topic similarity is not correctness).
- Weak: "They're outdated"; "Use perplexity"; cannot explain failure mode.
- Follow-ups: When would you still use ROUGE? How do you evaluate faithfulness for a long document? What goes wrong with embedding similarity?
- Difficulty: Easy–Medium. Tag: `llm.metrics`. Cluster: global-product, AI-startup.

**Q2.3 "Prompt B scores 3 points higher than prompt A on 100 cases. Do you ship B?"**
- Ideal answer:
  - Ask: same cases, same run conditions, how many runs, which metric and judge.
  - Paired analysis: per-case differences, bootstrap or sign test; CI on the difference.
  - Check slices: gains may hide regressions in important segments; check cost, latency, refusal rate and format validity.
  - Check practical significance vs noise at n=100; may need more cases.
  - Guard against prompt overfitting; confirm on held-out set.
  - Decide with decision rule defined before testing; stage via canary with online guardrail metrics.
- Weak: "Yes, 3 points is higher"; "Run it again once".
- Follow-ups: How many cases for a 3-point effect at your noise level? What if B is 3 points better but 20% costlier? How would you do a multiple-comparison correction when testing many prompt variants?
- Difficulty: Medium–Hard. Tag: `llm.metrics`, `llm.eval-design`. Cluster: global-product, AI-startup.

### M3 — LLM-as-judge and human evaluation

**Q3.1 "What are the main failure modes of LLM-as-judge, and how do you mitigate them?"**
- Ideal answer:
  - Position bias in pairwise comparisons: swap order, require consistency, treat inconsistent as tie.
  - Verbosity and formatting bias: rubric that penalises padding; length-controlled comparisons.
  - Self-preference: use a different model family for judging than generating; ensemble.
  - Rubric vagueness and score compression: anchored rubrics, decomposed criteria, reasoning-before-score, reference answers.
  - Limited capability on verification tasks (math, facts) without references or tools.
  - Prompt and model sensitivity; judge drift on provider update: pin snapshot, track judge scores on a frozen set.
  - Calibration against human labels with agreement statistics is the control for all of it.
- Weak: Lists one bias; says "strong frontier-model judges match humans" as a blanket claim.
- Follow-ups: How would you detect position bias empirically? What agreement is "good enough"? When would you refuse to use a judge?
- Difficulty: Medium. Tag: `llm.judge`. Cluster: global-product, AI-startup.

**Q3.2 "Walk me through calibrating a judge against human annotators."**
- Ideal answer:
  - Draw a stratified sample (100–300 items) covering score range and slices, including edge cases.
  - Write annotation guidelines with examples; train annotators; double-annotate a subset to measure inter-annotator agreement (kappa or alpha); resolve disagreements by adjudication.
  - Run the judge; compute agreement with the adjudicated labels (Cohen's kappa for categorical, Spearman or weighted kappa for ordinal), plus confusion matrix and bias direction.
  - Inspect disagreement clusters, revise rubric or add few-shot examples, re-run on a fresh split to avoid overfitting.
  - Set an acceptance bar relative to human-human agreement (judge should approach the ceiling set by humans).
  - Re-calibrate on schedule or whenever judge, rubric or product behaviour changes; use sampled ongoing human checks.
- Weak: "Compare average scores"; "Check a few examples"; no concept of human ceiling.
- Follow-ups: Humans disagree with each other at kappa 0.4. What now? How would you reduce annotation cost? How do you avoid overfitting the rubric to the calibration set?
- Difficulty: Hard. Tag: `llm.judge`. Cluster: global-product, AI-startup.

**Q3.3 "When would you choose human evaluation over an LLM judge or programmatic metric?"**
- Ideal answer:
  - Establishing ground truth and calibrating automated graders.
  - Subjective or culturally sensitive quality (tone, local language nuance, brand voice), safety-sensitive edge cases, new failure modes.
  - High-stakes domains (health, finance, legal) requiring expert review.
  - Use programmatic checks wherever answers are verifiable (schema, exact facts, code execution tests); judge for scalable subjective; humans for anchoring and audits.
  - Cost and latency: humans sampled, not per-request; active learning to pick informative items.
  - Operational points: guidelines, training, agreement stats, annotator wellbeing for toxic content, data protection for annotators handling PII.
- Weak: "Humans always best"; "Humans too slow, so never".
- Follow-ups: How do you sample for review in production? How do you handle annotator bias? Would you use crowdsourcing for Hindi-English code-mixed content?
- Difficulty: Medium. Tag: `llm.judge`, `llm.eval-design`. Cluster: Big4, global-IT-consulting.

### M4 — Red-teaming, prompt injection and jailbreak defence

**Q4.1 "Explain direct vs indirect prompt injection. Why is indirect injection harder to defend, and what would you do for a RAG assistant with email-sending ability?"**
- Ideal answer:
  - Direct: user crafts input to override instructions. Indirect: malicious instructions are embedded in content the model ingests (web pages, docs, emails, tool results, retrieved chunks), so the attacker is not the user.
  - Root cause: LLMs do not reliably separate instructions from data; delimiters and "ignore malicious input" prompts are mitigations, not controls.
  - Harder: attack surface is every data source, attacker is invisible to the user, payloads can be obfuscated or multilingual.
  - Defences for the email scenario: treat retrieved content as untrusted; remove or gate the send-email tool; human confirmation with a rendered summary of recipient and body; recipient allowlist; least-privilege credentials; rate limits; output filtering; separate quarantined model that reads untrusted content and returns structured data only.
  - Block exfiltration channels: no rendering of arbitrary markdown images or links; URL allowlist; egress controls.
  - Detect and learn: log, classifier signals, attack regression suite, red-team on the full pipeline.
- Weak: "Add 'ignore any instructions in documents' to the system prompt"; only input filtering; no mention of tool privilege.
- Follow-ups: What is the "lethal trifecta" combination (a term popularised by independent researcher Simon Willison in 2025) (private data access, untrusted content, external communication) and how do you break it? How would you test your defence? How do you handle PDFs with hidden text?
- Difficulty: Hard. Tag: `llm.security`. Cluster: global-product, AI-startup, Big4.

**Q4.2 "How do you run a red-team exercise for an LLM product and turn it into engineering outcomes?"**
- Ideal answer:
  - Scope and threat model: assets, harms (safety, privacy, security, brand), attacker personas, rules of engagement, test environment with synthetic data.
  - Mix of manual creative testing by diverse testers, automated attack generation and known-attack libraries, multilingual and multi-turn coverage.
  - Measure with attack success rate per category and severity rating.
  - Triage to root cause class (policy gap, tool privilege, missing validation) not individual prompt patching.
  - Convert every finding into a regression test and a tracked fix; re-run on every release bundle change.
  - Repeat on cadence and on major changes (new tool, new model); track trend.
  - Responsible handling: disclosure process, access control on attack corpora.
- Weak: "Try some jailbreak prompts from the internet"; one-off exercise; patch only the exact string.
- Follow-ups: How do you prioritise findings? How do you measure coverage? What is automated red-teaming good and bad at?
- Difficulty: Medium–Hard. Tag: `llm.security`, `llm.eval-design`. Cluster: global-product, Big4.

**Q4.3 "An agent has tools for ticket lookup, refund issuance and web browsing. Write the red-team plan you would run before launch."** (agent tool-permission design is owned by B4.M9-Q2: `ref`)
- Ideal answer:
  - Scope and threat model: assets (refund funds, customer data), entry points (ticket text, web pages, user input), attacker goals; reuse the B4.M9 permission design as the system under test.
  - Test families: direct and indirect injection seeds in ticket text and fetched pages, tool-argument tampering (amount, account id), confused-deputy refund attempts, exfiltration through rendered links, multi-turn escalation.
  - Method: curated attack corpus plus generated variants, pass/fail oracles on tool-call logs (no unauthorised refund call, no egress to non-allowlisted hosts), severity ratings and an attack-success-rate threshold for launch.
  - Process: findings to fixes loop, regression suite in CI, owner sign-off, re-test on every tool or prompt change; incident link to the B5.M9 playbook.
- Weak: "Ask the model to be careful"; one-off manual poking with no pass criteria.
- Follow-ups: How do you set the launch threshold for attack success rate? How do you keep the suite from going stale? Who signs off residual risk?
- Difficulty: Hard. Tag: `llm.security`, `design.ai-systems`. Cluster: global-product, global-IT-consulting.

### M5 — PII, privacy and hallucination mitigation

**Q5.1 "Design PII handling for a customer-support LLM assistant used by Indian customers."**
- Ideal answer:
  - Map data flows: client to API to prompt builder to provider to response to logs/traces/analytics/vector store/eval sets.
  - Data minimisation: send only fields needed; redact or tokenise identifiers (phone, email, account numbers, Aadhaar-like and PAN-like numbers) before the model call when the task does not need them, and restore after; labs use synthetic identifiers only, and the Aadhaar and PAN handling rules are reviewed by a human before the lesson publishes.
  - Detection: regex with validators plus NER; evaluate recall on a labelled synthetic set because misses matter more than false positives.
  - Provider controls: contractual data-use terms, zero-retention options, regional processing where available (checklist, per provider, dated Provider Notes); encrypt in transit and at rest.
  - Logging: redact before logging, short retention, access control, sampling; separate raw store with stricter controls if needed.
  - Lifecycle: consent and purpose limitation under DPDP awareness; deletion requests propagate to derived stores (vector index, caches); do not train or tune on raw PII without basis.
  - Test: PII leakage tests in eval suite; incident plan for leaks.
- Weak: "Ask the model not to repeat PII"; only mentions encryption; forgets logs and vector stores.
- Follow-ups: How do you evaluate redaction recall? What breaks when you tokenise names in a summarisation task? How do you honour deletion in a vector DB?
- Difficulty: Medium–Hard. Tag: `llm.privacy`, `llm.governance`. Cluster: IT-services, Big4, Indian-product.

**Q5.2 "How do you reduce and measure hallucination in a RAG-based answer feature?"**
- Ideal answer:
  - Distinguish retrieval failure (right chunk not retrieved) from generation failure (ignores or contradicts context) from knowledge-gap fabrication.
  - Mitigations: improve retrieval quality (cross-reference B3), instruct to answer only from context with explicit abstention, require citations mapped to spans, verify citations post hoc (NLI or judge against source text), constrained output schema, lower temperature as a minor factor, tool use for facts and calculations, route high-risk queries to humans.
  - UX: show sources, confidence-appropriate wording, easy feedback.
  - Measure: faithfulness/unsupported-claim rate, citation precision/recall, abstention precision (refuses when it should), answer correctness on labelled set; track online with sampled judging.
  - Calibrate the verifier against human labels; keep an adversarial set (unanswerable questions, conflicting sources).
- Weak: "Use a better model"; "Set temperature 0"; self-reported confidence as measure.
- Follow-ups: How do you test abstention? What if the sources conflict? What does a verifier cost in latency and how would you reduce it?
- Difficulty: Medium. Tag: `llm.grounding`. Cluster: global-product, AI-startup, global-IT-consulting.

**Q5.3 "A user asks you to delete their data. Which parts of your LLM system store personal data, and how do you handle erasure?"**
- Ideal answer:
  - Inventory: primary DB, conversation history, prompt/response logs, traces and analytics, vector embeddings of their content, caches (response, prompt), eval datasets built from logs, backups, provider-side retention, fine-tuning datasets and any fine-tuned weights.
  - Design for erasure up front: tenant/user IDs on all derived records, deletion workflows with job tracking, TTLs on logs and caches, exclude user-linked data from eval sets unless anonymised.
  - Vector stores: delete by metadata filter; handle index compaction; embeddings can leak content so treat as personal data.
  - Backups: expire by retention policy; document the approach.
  - Fine-tuned models: cannot easily unlearn; avoid tuning on identifiable data or retrain/rotate on schedule.
  - Provider: rely on retention terms and deletion APIs (per Provider Notes, dated); keep records of deletion.
  - Legal basis and exceptions need legal input; engineering provides the mechanics and audit evidence.
- Weak: "Delete the DB row"; forgets embeddings, logs and caches.
- Follow-ups: How would you verify deletion completeness? What about data in a shared cache keyed by prompt hash? What would you do about analytics aggregates?
- Difficulty: Hard. Tag: `llm.privacy`, `llm.governance`. Cluster: Big4, global-IT-consulting, Indian-product.

### M6 — Guardrails and content safety architecture

**Q6.1 "Where would you place guardrails in an LLM pipeline and what does each layer catch?"**
- Ideal answer:
  - Input: size limits, rate limits, PII detection, injection and policy classifiers, language/scope checks.
  - Retrieval/context: access-control filtering (never retrieve what the user cannot see), source trust labels, injection scanning of chunks.
  - Model call: system policy, structured output constraints, tool allowlists.
  - Tool-call layer: argument validation, authorisation, rate limits, confirmation for side effects, deterministic business rules.
  - Output: schema validation, safety classifiers, PII leakage check, grounding/citation check, URL and markdown sanitisation.
  - Conversation level: escalating-risk detection across turns, abuse throttling.
  - Observe and feed back: log triggers, review false positives.
  - Principle: deterministic controls for hard constraints; probabilistic classifiers for fuzzy ones; do not depend on one layer.
- Weak: "Use a moderation API on the output"; single-layer thinking.
- Follow-ups: Which checks can run in parallel with the model call? How do you moderate streamed output? What do you do when the classifier times out?
- Difficulty: Medium. Tag: `llm.guardrails`. Cluster: global-product, IT-services, global-IT-consulting.

**Q6.2 "Your guardrails block 6% of legitimate requests but catch most attacks. How do you reason about and fix this?"**
- Ideal answer:
  - Frame as precision/recall by category and the business cost of each side; measure on a labelled safe set and unsafe set, per language and slice.
  - Find which rule or classifier causes false positives; sample blocked traffic for human review (with privacy care).
  - Fixes: threshold tuning per category, rule refinement, context-aware checks, two-stage design (cheap filter then stronger check), allow-list known-safe patterns, better refusal UX with a way to rephrase or escalate.
  - Policy clarity: some blocks reflect ambiguous policy; fix policy first.
  - Track over-refusal rate as a first-class metric with alert; run A/B on thresholds.
  - Be careful about tuning only to English; code-mixed inputs often misfire.
- Weak: "Lower the threshold"; "Accept it, safety first"; no measurement plan.
- Follow-ups: How do you get labelled safe data representative of production? What threshold per category for self-harm vs profanity? How would you detect guardrail drift?
- Difficulty: Medium–Hard. Tag: `llm.guardrails`, `llm.metrics`. Cluster: global-product, Indian-product.

**Q6.3 "Guardrails add 400 ms to every request. The product team says that is too much. What do you do?"**
- Ideal answer:
  - Profile each check; find the critical path.
  - Parallelise input checks with model call (cancel generation if a check fails); run cheap rules first and short-circuit.
  - Use small specialised classifiers or distilled models locally instead of large LLM-as-guard calls; batch and cache repeated checks.
  - Stream output with sliding-window moderation rather than wait for the full answer; accept small risk window where policy permits, hold back for high-risk categories.
  - Risk-tier requests: lighter checks for low-risk flows, stronger for high-risk or tool-calling flows.
  - Make explicit fail-open/closed decisions per category and document them.
  - Measure with p95/p99 and report quality tradeoffs, not just mean latency.
- Weak: "Remove the guardrails"; "Use faster hardware" only.
- Follow-ups: What risk do you accept by streaming before moderation completes? How do you decide to cancel generation? How do you test the tiered approach?
- Difficulty: Medium. Tag: `llm.guardrails`, `llm.llmops`. Cluster: global-product, AI-startup.

### M7 — Observability, monitoring and drift

**Q7.1 "What would you monitor for a production LLM service beyond standard API metrics?"**
- Ideal answer:
  - Per-request traces with spans for retrieval, prompt assembly, model call, tool calls, guardrails; correlation ID linked to prompt/model/dataset version.
  - Latency: TTFT, tokens per second, end-to-end p50/p95/p99; error, timeout and rate-limit rates per provider.
  - Cost: input/output tokens, cost per request and per successful task, cache hit rate, per-tenant spend and budget alerts.
  - Quality: user feedback, regeneration/edit rates, sampled judge scores, task-success proxies, abstention and refusal rates, format-validity rate.
  - Safety: guardrail trigger rates by category, injection-detector hits, PII detections in outputs.
  - Drift: input topic/length/language distribution, retrieval hit rates, canary eval scores on a fixed set.
  - Privacy: redacted logs, sampling and retention.
  - Alerts on symptoms with SLO and error-budget thinking.
- Weak: "Latency and errors"; "Log everything including prompts".
- Follow-ups: How do you detect a silent provider model change? What is your quality SLO? How long do you retain prompts and why?
- Difficulty: Medium. Tag: `llm.llmops`. Cluster: global-product, global-IT-consulting, IT-services.

**Q7.2 "Quality seems to have degraded over the last two weeks but nothing was deployed. How do you investigate?"**
- Ideal answer:
  - Confirm with data: compare canary eval scores over time; slice user feedback and judge scores by date and segment.
  - Candidate causes: provider changed a floating model alias or backend; input distribution shifted (new user segment, language, seasonal topic); retrieval corpus changed or index staleness; upstream tool/API change; judge or rubric drift confusing the metric; caching serving stale answers; rate-limit-induced fallbacks to a weaker model.
  - Bisect with traces: compare the same replayed inputs across date-pinned model snapshots, retrieval configs and prompt versions.
  - Check fallback and routing metrics.
  - Mitigate: pin snapshots, roll back config, add the failing cases to the golden set, add drift alert.
  - Postmortem: why detection was slow.
- Weak: "Retrain the model"; "Check the code" only; no systematic hypothesis list.
- Follow-ups: How would you prove it is provider-side? Which dashboard panel would have caught it earlier? How do you separate input drift from model drift?
- Difficulty: Hard. Tag: `llm.llmops`. Cluster: global-product, AI-startup.

**Q7.3 "What would you log in LLM traces, and how do you balance debuggability with privacy and cost?"**
- Ideal answer:
  - Always log metadata: IDs, versions, token counts, latency, model, tool names, guardrail decisions, cost.
  - Content logging: redacted prompts/outputs; sampling (e.g. all errors and flagged, small random sample of rest); tiered retention (short for raw, longer for metadata).
  - Access control, audit on who reads traces; encryption.
  - Per-tenant opt-out, contractual constraints, regional storage.
  - Cost: trace storage and ingestion pricing; sampling and truncation of large contexts; store hashes plus pointers for big blobs.
  - Debuggability tactics: deterministic replay via stored versions and redacted inputs; synthetic replays.
  - Consider vendor vs self-hosted tracing (data residency).
- Weak: "Log everything"; "Do not log prompts" without alternative.
- Follow-ups: How do you debug a bad answer if you cannot see the prompt? How do you ensure redaction runs before the trace exporter? What retention would you choose?
- Difficulty: Medium. Tag: `llm.llmops`, `llm.privacy`. Cluster: Indian-product, Big4, global-product.

### M8 — Versioning, rollout and LLMOps pipelines

**Q8.1 "How do you version and release prompts and models safely?"**
- Ideal answer:
  - Treat prompt, model snapshot, decoding params, retrieval config, tool schemas, guardrail config and judge/dataset versions as a release bundle with one identifier recorded on every trace.
  - Store in version control with code review; optional registry for non-engineer editing with approvals; no direct prod edits.
  - Pin model snapshots; avoid floating aliases in prod; track provider deprecation dates.
  - CI: offline eval on every change with paired comparison and thresholds; separate fast and full suites.
  - Rollout: shadow then canary (1–5%) with online guardrail metrics, then ramp; automatic rollback triggers; kill switch.
  - Change one major component at a time where possible for attribution.
  - Keep release notes and a model-migration checklist.
- Weak: "Keep prompts in a config file"; "Always use latest model".
- Follow-ups: How do you handle a provider deprecating a model in 60 days? Who approves prompt edits? How do you attribute a regression to one component?
- Difficulty: Medium. Tag: `llm.llmops`. Cluster: global-product, global-IT-consulting, IT-services.

**Q8.2 "Design an A/B test to compare two prompt versions in production."**
- Ideal answer:
  - Hypothesis and primary metric (task success proxy, edit rate, resolution rate), plus guardrail metrics (safety triggers, latency, cost, refusal).
  - Randomise at user or session level with stickiness to avoid cross-contamination in multi-turn flows.
  - Power analysis from baseline variance; fixed horizon or sequential testing with correction; run through full weekly cycle.
  - Judge-scored quality on a sample as secondary, not the sole decision basis; human review of a sample.
  - Handle nondeterminism and novelty effects; segment analysis.
  - Pre-register decision rule; ramp gradually with auto-stop on guardrail breach.
  - Ethical/privacy considerations: consent and disclosure where needed.
- Weak: "Send half the traffic and see which gets more thumbs up"; no guardrail metrics; peeking at results.
- Follow-ups: Thumbs feedback is 2% of traffic: what bias? When would you use interleaving or shadow instead? How long do you run?
- Difficulty: Hard. Tag: `llm.llmops`, `llm.metrics`. Cluster: global-product, AI-startup.

**Q8.3 "Your CI eval suite costs significant money and takes 40 minutes. How do you make it practical?"**
- Ideal answer:
  - Tier suites: smoke (a few minutes, key regression cases, deterministic graders first), full (nightly or pre-release), deep (weekly with human audit).
  - Run only impacted suites based on changed components (prompt X affects suite X).
  - Cache model outputs keyed by input and bundle hash; reuse baseline results rather than rerun.
  - Cheaper judges for screening and stronger judge for final decisions, validated by calibration.
  - Parallelise with rate-limit aware workers; batch APIs for non-urgent runs (discount levels vary by provider and are read from the dated price data file, never hard-coded).
  - Sample intelligently (stratified, focus on historically flaky/high-risk cases).
  - Track cost per run in dashboards; set budgets.
  - Keep statistical rigour: smaller samples widen CIs, so gate on larger effects in fast tier.
- Weak: "Delete half the tests"; "Run evals manually before release".
- Follow-ups: What do you cache and how do you invalidate? How do you ensure the cheap judge agrees with the strong judge? What does a failing smoke test trigger?
- Difficulty: Medium. Tag: `llm.llmops`, `llm.eval-design`. Cluster: global-product, global-IT-consulting.

### M9 — Incident response, governance and responsible AI

**Q9.1 "Your chatbot just told a customer something harmful and it is going viral. Walk me through the first hour."**
- Ideal answer:
  - Declare incident, assign incident commander and comms owner, open a timeline.
  - Contain first: feature flag to disable or restrict the feature, route to fallback or human agents, tighten guardrail on the offending category, disable risky tools, block the exploit pattern as a stopgap.
  - Preserve evidence: capture trace, bundle version, inputs (respecting privacy), and reproduction steps.
  - Assess scope: search logs for similar outputs, count affected users, check whether data exposure or other policy harm occurred.
  - Communicate: internal escalation (security, legal, leadership), customer-facing statement as appropriate; follow notification obligations where personal data involved (statutory clocks differ: the DPDP Rules breach provision expects intimation to affected Data Principals and to the Board without delay and a fuller report to the Board within 72 hours once phased in around May 2027; legal confirms the rule number and clocks).
  - Root cause and fix after containment; add the case to eval and red-team suites; add monitoring.
  - Blameless post-mortem with action items, owners, deadlines.
- Weak: Starts with "fix the prompt"; no containment or comms; blames the model vendor.
- Follow-ups: Who can pull the kill switch and do they need approval? How do you decide when to restore the feature? How would you prevent recurrence systemically?
- Difficulty: Medium. Tag: `llm.incident`. Cluster: global-product, Big4, IT-services.

**Q9.2 "What should an engineer know about the EU AI Act and India's DPDP Act when building LLM features?"**
- Ideal answer (awareness level, hedged, not legal advice; DPDP and GDPR specifics are owned by X1 in A8.M9.4, so the DPDP points below stay a short awareness paragraph and the single `ref` is to X1):
  - EU AI Act: risk-based classification (prohibited, high-risk, limited/transparency, minimal); obligations differ by role (provider vs deployer); transparency duties such as disclosing AI interaction and labelling synthetic content; general-purpose AI model obligations mostly sit with model providers; phased application dates changed in 2026: the Digital Omnibus on AI (in force 27 July 2026, per web sources 2026-10-05) pushed Annex III high-risk duties to 2 December 2027 and Annex I to 2 August 2028, while Article 50 transparency duties stayed on 2 August 2026 with a watermarking grace period to 2 December 2026; confirm against the Official Journal.
  - Engineering implications: classify the use case early, keep documentation (intended purpose, data, evals, limitations), logging and traceability, human oversight design, accuracy/robustness testing, incident reporting processes for high-risk systems.
  - DPDP Act 2023: consent and notice, purpose limitation, data minimisation, security safeguards, breach notification, data principal rights (access, correction, erasure, grievance), obligations of data fiduciaries and processors, children's data constraints; Rules notified 14 November 2025 with phased commencement (about 12 and 18 months, so most duties bite around May 2027).
  - Engineering implications: consent capture and withdrawal flows, purpose tags on data, deletion workflows, vendor/processor agreements, retention limits, cross-border transfer awareness (via X1).
  - Differences vs GDPR exist; do not assume equivalence; involve legal counsel for classification and obligations.
  - Frameworks like NIST AI RMF (voluntary) or ISO/IEC 42001 (certifiable management system) provide structure.
- Weak: "Not my job"; claims precise legal conclusions with false confidence; conflates both with GDPR.
- Follow-ups: How would you classify an internal HR-screening assistant? What evidence would you keep for an audit? Which of these requirements would you build into the platform vs per-feature?
- Difficulty: Medium–Hard. Tag: `llm.governance`. Cluster: Big4, global-IT-consulting, IT-services.

**Q9.3 "How do you test an LLM feature for bias and unfair outcomes, in an Indian context?"**
- Ideal answer:
  - Identify protected or sensitive attributes relevant to the use case and region: gender, religion, caste proxies (surnames, locations), region, language, accent for voice, disability, age, socioeconomic markers.
  - Build counterfactual test sets: swap names/attributes while holding content constant; compare outcomes (scores, tone, refusal, recommendations); slice real-traffic metrics by language and region.
  - Include multilingual and code-mixed inputs; evaluate performance gaps across languages (quality, refusal, latency, cost via tokenisation overhead).
  - Metrics: disparity in outcome rates, error-rate differences, judge-scored tone differences with human validation; statistical significance.
  - Mitigations: prompt and policy changes, retrieval/data balance, human oversight for consequential decisions, avoid using LLMs for high-stakes decisions without audit.
  - Document findings in model card, monitor in production, collect feedback channels.
  - Acknowledge limits: no single fairness metric; tradeoffs between definitions; context decides.
- Weak: "Models are trained to be unbiased"; "Remove names from prompts" only.
- Follow-ups: How would you handle a hiring-screening use case? Tokenisation makes Hindi costlier: is that a fairness issue and what do you do? How would you source test names without stereotyping?
- Difficulty: Medium–Hard. Tag: `llm.responsible-ai`. Cluster: Big4, global-product, AI-startup.

### Seed annotations (G-10): goal tags, senior-bar notes and staff-level upgrade probes

Goal tag format `B:x A:x H:x` (B = Senior Python Backend, A = AI Engineer, H = hybrid; M = MUST, S = SHOULD, N = NICE).

| Q | Goal tag | Senior-bar note (mid, senior, staff) |
|---|---|---|
| Q1.1 | B:N A:M H:M | Mid lists a golden set; senior adds slicing, held-out set, tiered graders and online loop; staff defines the quality SLO and who owns the dataset |
| Q1.2 | B:N A:M H:M | Mid says add cases; senior diagnoses set-versus-production drift and overfitting; staff builds the complaint-to-eval-case pipeline with SLA |
| Q1.3 | B:N A:M H:M | Mid reruns; senior quantifies variance and gates on CI; staff budgets power versus CI cost and quarantines flaky cases by policy |
| Q2.1 | B:N A:M H:M | Mid says F1; senior adds field-level cost weighting and review routing; staff ties metrics to straight-through-processing economics |
| Q2.2 | B:N A:M H:S | Mid says outdated; senior explains overlap failure and replacements; staff defines a metric portfolio with validation against humans |
| Q2.3 | B:N A:M H:M | Mid compares means; senior uses paired bootstrap, slices, cost; staff pre-registers decision rules and corrects for many variants |
| Q3.1 | B:N A:M H:S | Mid names one bias; senior lists several with tests; staff sets judge governance (pinning, frozen drift set, retirement) |
| Q3.2 | B:N A:M H:S | Mid compares averages; senior uses kappa and human ceiling; staff plans ongoing calibration budget and active sampling |
| Q3.3 | B:N A:S H:S | Mid says humans best; senior allocates grader types by verifiability; staff designs annotation operations and annotator safeguards |
| Q4.1 | B:S A:M H:M | Mid adds a prompt line; senior breaks the lethal trifecta and gates side effects; staff owns the org threat model and attack-regression release gate |
| Q4.2 | B:S A:M H:S | Mid runs jailbreak lists; senior triages by root-cause class and automates regression; staff sets cadence, coverage metrics and disclosure policy |
| Q4.3 | B:S A:M H:M | Mid relies on manual poking; senior designs attack families and tool-log oracles; staff sets launch thresholds, regression CI and sign-off |
| Q5.1 | B:S A:M H:S | Mid redacts prompts; senior maps all flows including logs; staff adds redaction recall SLO and tenant-key design |
| Q5.2 | B:N A:M H:S | Mid says better model; senior separates retrieval from generation failure; staff defines hallucination error budget by risk tier |
| Q5.3 | B:S A:S H:S | Mid deletes a row; senior inventories derived stores; staff designs erasure verification and retention by default |
| Q6.1 | B:S A:M H:S | Mid names output moderation; senior places layers and deterministic versus probabilistic split; staff sets per-tenant policy as configuration |
| Q6.2 | B:S A:M H:S | Mid lowers threshold; senior does per-category precision and recall plus policy fix; staff runs threshold A/B with cost model |
| Q6.3 | B:S A:M H:S | Mid removes checks; senior parallelises and tiers; staff states accepted risk window and fail-open decision record |
| Q7.1 | B:S A:M H:S | Mid lists latency and errors; senior adds quality, safety, cost and drift signals; staff defines quality SLOs and error budgets |
| Q7.2 | B:S A:M H:S | Mid checks code; senior builds hypothesis list and replays pinned snapshots; staff adds detection-gap post-mortem and canary schedule |
| Q7.3 | B:S A:M H:S | Mid logs all or none; senior samples with redaction and tiered retention; staff designs access audit and residency |
| Q8.1 | B:N A:M H:M | Mid uses a config file; senior bundles and pins; staff builds the change-management and attribution process |
| Q8.2 | B:N A:M H:M | Mid splits traffic; senior adds guardrail metrics, stickiness, power; staff handles sequential testing and interference |
| Q8.3 | B:N A:M H:M | Mid trims tests; senior tiers, caches, samples; staff sets CI cost budget as a tracked metric |
| Q9.1 | B:S A:S H:S | Mid fixes the prompt; senior contains first and preserves evidence; staff pre-authorises the kill switch and rehearses it |
| Q9.2 | B:N A:S H:S | Mid says legal's job; senior classifies the use case and states uncertainty; staff builds reusable compliance evidence in the platform |
| Q9.3 | B:N A:S H:S | Mid removes names; senior builds counterfactual sets and language-gap analysis; staff sets fairness review gates with documented trade-offs |

Staff-level upgrade probes (two per module, added to the follow-up trees):

| Module | Probe 1 | Probe 2 |
|---|---|---|
| M1 | Define a quality SLO for this feature and say who is paged when it burns | How would you retire or refresh golden cases without losing regression history? |
| M2 | Design a metric portfolio that resists Goodhart pressure from a team bonus tied to one number | How do you validate that a new metric tracks user outcomes? |
| M3 | Set a calibration budget for a quarterly judge refresh and justify it against incident cost | How would you detect that your human annotators drifted? |
| M4 | Define the org-wide rule for which agents may combine private data, untrusted input and outbound channels | How would you measure red-team coverage, not just attack success rate? |
| M5 | Set a redaction-recall target by data class and the fallback when it is missed | How would you prove erasure completeness to an auditor? |
| M6 | Write the decision record for fail-open versus fail-closed per risk category | How would you let tenants tune policy without weakening the platform floor? |
| M7 | Set a hallucination error budget and the policy when it is exhausted | How do you stay vendor-neutral given tracing vendor consolidation and unstable semantic conventions? |
| M8 | Describe the review and approval path for a prompt change by a non-engineer | When is a rollback worse than a forward fix for a model regression? |
| M9 | Write the incident-response runbook for model regressions, including who can invoke the kill switch | How would you present AI-governance posture to a Big 4 client audit without overclaiming? |

Seed question total: 27 verbal seeds (3 per module, 9 modules), each with the annotations above; question banks follow template targets per module (12 / 6 / 4 / 8 / 3) with IDs `Q-B5-Mn-nn`. Items about statistics concepts or RAG metric definitions are `ref` items pointing to B1.M10 and B3.M8 (at most one per module, never re-authored). Judge-human agreement figures are never stated in items without a cited source; agreement items use the supplied calibration sets.

## 5. Final assessment

- **Mastery check (30 items):** weighted sample across modules (about 3-4 per module): concept MCQs, predict-the-result items (read a confusion matrix, interpret a kappa with its confidence interval, read a canary dashboard), and two multi-step scenario items. Pass at 80%; weak modules trigger the revision loop.
- **Timed coding task (90 minutes, IDE, LLM Lab Harness in recorded-response mode):** Given a small FastAPI LLM service (scripted mock provider, synthetic data), (a) add a golden-set eval with paired bootstrap comparison and a CI-lower-bound regression gate, (b) add a calibrated judge using the provided human labels and report kappa with uncertainty, (c) add an input and output guardrail with a tool-privilege fix for an injection path, and (d) add trace spans and a cost metric. Hidden tests grade correctness (70 percent threshold, G-7); a rubric grades statistical reporting and design notes.
- **Canvas system-design task (45 minutes):** "Design the safety, evaluation and rollout architecture for an enterprise RAG agent that can send emails and issue refunds across tenants, with an India DPDP awareness constraint." The rule engine must show no findings on R-AI-1 (fallback and degraded mode on LLM path), R-AI-2 (per-tenant budget and metering), R-AI-3 (ingestion and staleness plan), R-AI-4 (injection guard and least-privilege tool boundary) and R-PII-1 (redaction path before an external model); plus R-AGENT-1 and R-TENANT-2 where used. Rubric (overall 3.5 or more, no dimension under 3): trust boundaries, layered defences, eval pipeline, monitoring and drift, rollout and rollback, incident response, privacy and governance, cost and latency.
- **Assessment time note:** the 2.0 h allowance in the boss effort table is a `[judgement]` average that includes revision loops; one sitting of all four parts is about 3 h (mastery check about 30 minutes, timed task 90, canvas 45, voice 20), so parts are scheduled across sittings and the allowance is flagged for calibration after the pilot.
- **Voice mock round (20 minutes):** three questions drawn from the seed bank with probing follow-ups; scored per competency tag with evidence quotes. Badge: "Meets" (average 3.0, none under 2). Readiness uses the Senior bar in section 8.
- **Certificate:** issued when all four parts reach threshold; portfolio evidence linked.

## 6. Course-level risks and freshness notes

Facts below were checked by web search on 2026-10-05 against secondary sources (law-firm, vendor and trade summaries) unless stated; primary-source confirmation is listed as a remaining check.

| Item | State as of October 2026 | Remaining check |
|---|---|---|
| OWASP Top 10 for LLM Applications | 2026 edition published 3 August 2026 (OWASP GenAI Security Project); 2025 list kept in M4 for reference; Agentic Top 10 (ASI01 to ASI10) published 9 December 2025; verified 2026-10 | Exact 2026 numbering from the OWASP PDF **[verify]** |
| EU AI Act dates | Digital Omnibus on AI adopted June 2026, published in the Official Journal 24 July 2026 (Regulation (EU) 2026/1744), in force 27 July 2026; Annex III high-risk to 2 December 2027; Annex I to 2 August 2028; Article 50 transparency stays 2 August 2026, watermarking grace to 2 December 2026 | Read article text on EUR-Lex **[verify]** |
| DPDP Rules | Notified 14 November 2025; Consent Managers after 12 months; most obligations after 18 months (about May 2027); breach intimation then report within 72 hours | Binding status of sector regulator AI guidance (RBI, SEBI, IRDAI) and Aadhaar-handling legal review by a human **[verify]** |
| OTel GenAI conventions | All gen_ai.* at Development status; moved to a dedicated repository in June 2026; names changed repeatedly | None; re-check each review |
| Tooling | Langfuse acquired by ClickHouse (January 2026); Dynatrace completed its Arize acquisition on 1 October 2026; NeMo Guardrails latest PyPI release 0.24.1 on 2026-09-16 (beta status not rechecked); Guardrails AI and Presidio actively maintained | Re-check NeMo status each review |
| NIST AI RMF and ISO/IEC 42001 | Awareness-level descriptions only | NIST page not read directly; revision pending per secondary tracker **[verify]** |
| PSI or KL on raw embeddings | Treated as unreliable; use centroid distance, cluster proportions, MMD or classifier two-sample test; verified 2026-10 against practitioner sources | None |
| Provider zero-retention, regional processing, batch discounts | Not asserted; taught as checklist with dated Provider Notes and price data file | Provider Notes owners refresh quarterly |

- **Technical cautions:** "temperature 0 is deterministic" is false in general; kappa thresholds are conventions, not standards; judge-human agreement figures from papers are never quoted without a source; attack content teaches classes and defences, not payload lists.
- **Safety of content:** all attack and harmful examples are synthetic and defensive, inside the sandbox; no real PII; no operational harmful instructions.
- **Sandbox:** uses the shared LLM Lab Harness (P-07; scripted mock provider, record and replay, budget meter, shared fixture repository), recorded-response mode graded (R-B5-4); live mode optional behind AI Gateway caps and never gating; vulnerable RAG-plus-tools demo is a harness fixture, not a Lab Service dependency.
- **Overlaps resolved:** B2.M10 preview (at most 1 h, MCQ-only attack items), B3.M8 RAG metric definitions, B4.M8 and B4.M9 agent evaluation and tool permissions, B7.M9 artefact lifecycle, A7.M9 generic observability, A8.M9 threat modelling and X1 compliance primer.
- **Review cadence:** every 3 months; pilot with at least 3 learners; calibrate difficulty from pilot data.

## 7. Calibration datasets (specification, R-B5-5)

Owner: Content Lead (founder plus SME panel). Three fully synthetic labelled sets, for two human labellers, stored as platform calibration sets; no real user data; attack content defensive and synthetic.

| Set | Size | Labels | Purpose |
|---|---|---|---|
| CAL-1 Summarisation quality | 60-100 source and summary pairs, with injected faithfulness errors, omissions and padding | 3-point helpfulness plus binary faithfulness; two labellers, adjudication | Judge calibration, position and verbosity bias tests (M3) |
| CAL-2 RAG faithfulness | 60-100 question, context, answer triples with supported, unsupported, contradicted and unanswerable cases | supported or unsupported per claim | Hallucination verifier and abstention measurement (M5), used by B3.M8 by reference |
| CAL-3 Safety refusal | 60-100 prompts spanning clearly safe, borderline and clearly unsafe categories in English and Hindi-English code-mixed text, plus synthetic indirect-injection documents | allow, refuse, escalate per policy document | Guardrail precision and recall, over-refusal (M6), attack regression (M4) |

Labelling guidelines (one page each): definitions with two anchored examples per label, borderline rules, "when unsure choose escalate", annotator wellbeing note for CAL-3, double-annotation of at least 30 percent, agreement reported with confidence intervals before the set is released, and a version stamp per set. Learners compute their own agreement statistics from the provided labels; no published agreement benchmark is quoted.

## 8. Learner-Fit Appendix

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 (new to LLM evals or security) | Simpler, 16 h; take B2 and B1.M10 first (deep-study bands trigger linked remediation) | Provided helpers, guided patching, prefilled tabletop | none | none until module mastery 80 |
| L2 (junior-mid, ships LLM features with guidance) | Simpler, about 14 h after skimming known lessons | Provided metrics, build harness and judge with hints | lessons already known per placement | one per module after pass |
| L3 (experienced, ships but cannot always explain) | Deeper, about 10.5 h | Build from scratch: harness, paired bootstrap, calibration, canary router | foundations (X5 may be skimmed once familiar) | M3 jury design, M4 quarantined-model pattern, M8 sequential testing |
| L4 (senior or owner) | Deeper, about 8-9 h, go straight to mastery checks where scored | Timed labs, canvas, staff probes | all lessons scoring 85 or more | all challenge items, plus staff probes in voice drills |

### LF-2 Module matrix

Tags: B = Senior Python Backend, A = AI Engineer, H = hybrid. Weights are interview-weight percent per target, summing to 100 per target, rationale: judgement from how senior AI-engineer loops probe evaluation and safety versus backend loops that only touch observability and security basics `[judgement]` (calibrate after pilot). Diagnostics: 6 placement items per module (`placement`, drawn from MCQ and predict buckets), IDs `Q-B5-Mn-p01` to `Q-B5-Mn-p06` (G-10 placement-tagged items from the MCQ and predict buckets, no extra authoring); bands per G-5 (85 skip, 70-84 skim, 50-69 study, under 50 deep-study).

| Module | Tags B / A / H | Weight B / A / H (%) | Rehearsal modes | Module readiness criterion (G-6 specialised) |
|---|---|---|---|---|
| M1 Eval design | B:N A:M H:M | 5 / 14 / 12 | voice drill, design canvas | Retake 85 after 7 days; build a gated golden-set harness with CI in timed lab; voice Senior bar on `llm.eval-design` in two sessions 48 h apart |
| M2 Metrics | B:N A:M H:M | 5 / 11 / 9 | voice drill, coding | Retake 85; paired comparison report correct in timed task; voice Senior bar on `llm.metrics` |
| M3 Judge | B:N A:M H:S | 3 / 12 / 8 | voice drill, coding | Retake 85; kappa with CI computed and interpreted; bias tests run; voice Senior bar on `llm.judge` |
| M4 Injection and red-team | B:N A:M H:M | 22 / 16 / 17 | voice drill, canvas, coding | Retake 85; attack success rate reduced in lab with regression suite; canvas rubric 3.5 with R-AI-4; voice Senior bar on `llm.security` |
| M5 PII and hallucination | B:N A:M H:S | 18 / 12 / 14 | voice drill, canvas | Retake 85; redaction middleware tests pass; voice Senior bar on `llm.privacy` and `llm.grounding` |
| M6 Guardrails | B:N A:M H:S | 14 / 12 / 12 | voice drill, canvas, coding | Retake 85; p95 overhead measured and per-category precision and recall reported; voice Senior bar on `llm.guardrails` |
| M7 Observability | B:N A:M H:S | 16 / 10 / 13 | voice drill, canvas | Retake 85; canary-eval alert and dashboard design rubric 3.5; voice Senior bar on `llm.llmops` |
| M8 Release engineering | B:N A:M H:M | 12 / 9 / 11 | voice drill, canvas, coding | Retake 85; CI gate with CI bounds plus canary rollback working; voice Senior bar on `llm.llmops` |
| M9 Incident and governance | B:N A:S H:S | 5 / 4 / 4 | voice drill, behavioural | Retake 85; tabletop rubric 3.5; voice Senior bar on `llm.incident`, `llm.governance` with honest uncertainty about dates |

Module tags follow the boss ruling (section 8 item 1: M1-M4 and M8 `A:M H:M B:N`, M3 `H:S`, M5-M7 `A:M H:S B:N`, M9 `A:S H:S`). The high Backend interview weights on M4-M7 are therefore interview-weight statements for question selection, not route-hour weights (Backend learners take only the NICE primers B5.M4 and M5, per the study-route doc). Company-style clusters follow the Company Pack map (B5 is NICE for IT-services and Indian-product, SHOULD for global-IT consulting, Big4 and global-product, MUST for AI-startup at course level): Big4 pulls M5 and M9; global-IT consulting pulls M9; global-product pulls M1-M3; AI-startup pulls M1-M4 and M7. Indian-product and IT-services loops still probe privacy and code-mixed guardrails (M5, M6) as question-selection weight, not as route hours.

### LF-3 Experience bridge ("you already know" to "senior-depth gap")

| Module | You already know | Gap between using and explaining at senior depth |
|---|---|---|
| M1 | pytest, CI, LLM API calls, shipped chatbots and voice agents with manual testing | Golden sets built from real tickets, slices, held-out set against prompt overfitting, nondeterminism, CI gates |
| M2 | Precision and recall from backend monitoring and search tuning | Metric choice per task, cost-weighted errors, paired comparisons, Goodhart |
| M3 | Code review and QA sign-off | Judge as an instrument: bias tests, kappa against humans, pinning and drift |
| M4 | OWASP web (SQLi, SSRF), authz in FastAPI and Django, least-privilege on AWS IAM | Instructions and data share a channel, so privilege separation replaces sanitisation; indirect injection through RAG chunks and tool results |
| M5 | PII handling, logging hygiene, encryption, S3 lifecycle rules | PII in prompts, traces, vector stores and caches; erasure for derived data; verifying grounding |
| M6 | API gateways, WAFs, validation (Pydantic) | Probabilistic classifiers, measured precision and recall, streaming moderation, fail-open decisions |
| M7 | Prometheus, Grafana, CloudWatch, OpenTelemetry, Redis cache metrics | Quality and safety signals beside latency, silent provider changes, SLOs for quality, privacy-safe traces |
| M8 | Feature flags, canaries, blue-green on AWS, Docker, CI | Release bundle of prompt, model, retrieval and judge; eval-gated CI with statistics; A/B on nondeterministic outputs |
| M9 | On-call, post-mortems, SOC2 awareness | AI incident taxonomy, kill switches, model cards, EU AI Act tiers and honest uncertainty |

Applied lab bridge (founder-experience pattern: LLM features shipped without systematic evals): build a golden set from real support tickets (synthetic stand-ins), add a CI gate, then ship through a canary (M1, M8).

### LF-4 Senior-bar framing

| Module | Mid signal | Senior signal | Staff signal |
|---|---|---|---|
| M1 | Runs a test set, reports one pass rate | Slices, held-out set, repeated runs with CIs, gated CI | Defines the quality SLO, owns the dataset lifecycle and who is paged |
| M2 | Quotes accuracy or BLEU | Cost-weighted field metrics, paired comparison, variance | Metric portfolio resistant to Goodhart, validated against user outcomes |
| M3 | Uses a model to grade | Calibrated, bias-tested, pinned judge | Governs judge lifecycle, budgets human calibration |
| M4 | Prompt hardening | Layered controls, least privilege, regression attacks | Org rules for the lethal-trifecta combination, red-team cadence and release gate |
| M5 | Redacts the prompt only | Flow-mapped redaction across logs, traces, vector stores and caches; grounding verifier | Redaction-recall targets by data class, erasure verification for auditors |
| M6 | Adds a moderation API call | Layered, measured guardrails with per-category precision and recall and fail-open decisions | Per-tenant policy over a platform floor, decision records for accepted risk |
| M7 | Dashboards on latency and errors | Quality, safety, cost and drift signals; canary eval for silent provider change | Quality SLOs and error budgets, vendor-neutral instrumentation |
| M8 | Edits the prompt in place | Bundled releases, eval-gated CI, canary with automatic rollback | Change-management and attribution process, CI cost budgets, rollback-versus-forward-fix policy |
| M9 | Fixes the prompt after an incident | Contains first, preserves evidence, feeds evals | Pre-authorised kill switch, rehearsals, governance evidence reusable across features |

### LF-5 Adaptive hooks

- Mastery: module 80 percent; readiness 85 percent on a retake spaced 7 days apart.
- Remediation links: M1-M2 to B1.M10 (statistics) and B2.M11; M3 to M1 and B1.M10; M4 to B2.M10, A8.M7 and A8.M9; M5 to X1 (A8.M9.4) and B3; M6 to M4; M7 to A7.M9 and M1; M8 to M1, M2 and B1.M10; M9 to M4 and X1.
- Challenge extensions unlock at module score 90 or more (see LF-1 L3 column).
- Adaptive engine may reorder: M5, M6, M7 in any order after M4; may defer M9 for Route A (Backend). M1 to M3 stay in order; M4 precedes M6; M8 follows M1 and M2.
- X5 harness orientation may be skipped only if the learner has completed the same harness in B2 labs.

### LF-6 Applied labs (production-framed, one per module)

| Module | Lab |
|---|---|
| M1 | Ticket-summary assistant: build a golden set from synthetic support tickets, harness, slice report and gate |
| M2 | GST-invoice extraction service: field-level F1 with cost weighting and paired prompt comparison |
| M3 | Calibrate a judge for support-reply helpfulness against CAL-1 labels and fix three systematic errors |
| M4 | Patch an enterprise RAG assistant with email and refund tools against three injection paths; report attack success rate before and after |
| M5 | FastAPI redaction middleware with reversible tokens and Indian-format synthetic identifiers; verify citations on CAL-2 |
| M6 | Guardrails on a customer-chat endpoint with parallel checks, p95 overhead and per-category results on CAL-3, including Hindi-English prompts |
| M7 | Instrument an IoT-support voice-and-chat backend with spans, token cost metric and a scheduled canary eval |
| M8 | SaaS prompt change flowing through CI gate, canary router with automatic rollback and a model-migration plan |
| M9 | Tabletop: cross-user data leak through a cache-key bug; produce timeline, notification decisions, risk register, model card |

### LF-7 Currency and quality bar

| Module | Dated stamp | Memorable visual or analogy | Top 3 misconceptions handled |
|---|---|---|---|
| M1 | 2026-10-05 | Eval pyramid as smoke detectors and fire drills | single run; temperature 0; own-example pass rate |
| M2 | 2026-10-05 | Metric card as nutrition label | BLEU for chat; accuracy on imbalance; unpaired comparison |
| M3 | 2026-10-05 | Thermometer calibrated against a reference | judge equals truth; same family judge; one number without kappa |
| M4 | 2026-10-05 (OWASP 2026 edition published 3 Aug 2026; exact numbering `[verify]`) | Bank clerk and slipped notes | delimiters suffice; injection equals jailbreak; payload patching |
| M5 | 2026-10-05 | PII plumbing diagram | prompt-only redaction; temperature 0; confidence as truth |
| M6 | 2026-10-05 (tool versions per sources) | Airport security lanes | moderation call equals guardrail; fail-open harmless; English-only |
| M7 | 2026-10-05 (OTel GenAI at Development) | Flight recorder plus gauges | 200 OK is healthy; log everything; PSI on embeddings |
| M8 | 2026-10-05 | Sealed shipping container | floating alias; prompt edits are not deploys; A/B without guardrails |
| M9 | 2026-10-05 (EU AI Act omnibus dates) | Aviation incident model | compliance is legal's job; EU AI Act equals GDPR; fix prompt first |

## 9. Open items for boss

1. Confirm the CAL-1 to CAL-3 labellers and review schedule with the Content Lead (R-B5-5 owner) before M3, M5 and M6 labs are built.
2. The Arize acquisition closing and NeMo Guardrails status may change tooling examples; approve naming vendors only in dated Provider Notes (G-8 tier language).
3. Primary-source legal review (EU AI Act Official Journal text, DPDP Aadhaar handling, RBI, SEBI, IRDAI guidance) needs a human reviewer before M5 and M9 publish.
4. Boss final (2026-10-06): the 2.0 h assessment allowance stays (FR-03); real assessment parts may reach about 3 h; measure in the pilot. Not a build blocker.

## 10. Boss compliance log

| Decision ID | Applied |
|---|---|
| G-1 | Ownership notes table added; pointers to B1.M10, B3.M8, B4, B7.M9, A7.M9, A8.M9 and X1 are short |
| G-2, section 5 | Header hours 16 S / 11 D, assessment 2.0 h, per-module hours sum to header, S and D semantics stated |
| G-3, G-5, G-6, G-7 | Tags, bands, readiness and thresholds in LF-2 and header |
| G-8, G-12 | No model names; dated provider-note approach; freshness table dated 2026-10-05 |
| G-9 | Owned versus secondary tags separated; legacy `sd.ai-feature` replaced by `design.ai-systems` (secondary, owner A6) |
| G-10 | Seed annotations with goal tags and senior-bar notes; two staff probes per module; question IDs `Q-B5-Mn-nn` |
| G-11 | LF-1 to LF-7 in section 8 |
| G-13, R-B5-2, P-07 | X5 lesson M1.0 and harness-based labs |
| R-B5-1 | B2.M10 preview acknowledged; B5 deep treatment |
| R-B5-3 | M9 owns AI governance only; X1 linked |
| R-B5-4 | Recorded-response mode graded; live optional |
| R-B5-5 | Section 7 calibration set specifications |
| Section 8 B5 items 1-10 | Tags (1), ownership (2), X5 (3), statistics referencing B1.M10 and no unsourced agreement figures (4), M9 scope and dates (5), calibration sets (6), canvas rules R-AI-1..4 and R-PII-1 (7), experience bridge (8), staff SLO framing (9), tool landscape verified (10) |

- Track lead B review (2026-10-06): see `_reviews/TrackLead_B_Report.md`. Applied: fixed garbled data-minimisation bullet in Q5.1; replaced unverified OWASP 2026 renumbering claims with risk-class teaching and a narrow [verify]; added kappa-prevalence and case-level resampling points; added EU AI Act in-force dates for prohibited practices and GPAI and X1 ref note on Q9.2; hedged DPDP breach-clock wording; named SME and legal reviewer roles; added rubric anchors; added assessment-time note; aligned cluster weights to Company Pack map; split LF-4 rows per module.
