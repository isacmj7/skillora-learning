---
name: B7 — Fine-tuning, Model Selection and MLOps Basics
course_id: B7
track: B (AI Engineer)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-18 (see _reviews/Boss_Final_Approval.md)
---

# B7 — Fine-tuning, Model Selection and MLOps Basics (Track B)

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | B7 — Fine-tuning, Model Selection and MLOps Basics / Track B (AI Engineer Mastery) |
| Audience | Senior Python backend engineers moving into AI Engineer roles (L3–L4), and L1–L2 learners via the Simpler path. Assumes daily use of hosted LLM APIs, not hands-on training. Interview depth, not research depth: the learner must reason, decide and defend, not derive. Training is reasoned about, never run on a GPU (R-B7-1); labs are CPU-only tiny models, calculators, config reviews, recorded training logs and a fixture dry-run. |
| Prerequisites | B1 (ML foundations: overfitting, train/val/test, metrics, transformers and the KV-cache formula, owned by B1.M9), B2 (LLM application engineering, including `LLMClient` adapter and token accounting) are required (solid edges B1 to B7 and B2 to B7); A7 (Docker, CI/CD, observability) is helpful. B3 and B5 recommended before M1 and M5 (B5 owns the eval harness) but not required. |
| Placement skip | Per-module diagnostics (Appendix section 8.1) decide skip / skim / study / deep-study by score band. A skip means going straight to the mastery check; it never waives the final assessment. M1 (decision framework) and M6 (selection and routing) can be skimmed but never fully skipped: highest interview weight. |
| Ownership (binding, Boss matrix) | B7 OWNS: prompt vs RAG vs fine-tune framework (M1), all fine-tuning mechanics (M2–M4), tuned-model evaluation only (M5), model selection, routing, open-weight vs hosted (M6), serving and GPU memory arithmetic with calculators (M7–M8), model-artefact lifecycle (M9). B7 POINTS TO: B3.M1 (RAG vs long context), B1.M9 (transformers, KV-cache formula), B1.M7 (optimiser-memory intuition), B2.M2 (per-request model/effort choice, token accounting), B5 (eval harness, judge design, LLM observability, LLM release engineering B5.M8), A7 (generic CI/CD, FinOps levers A7.M10), A8.M9.4 compliance primer X1. |
| Outcomes (each tied to a competency) | 1. Choose between prompting, RAG, fine-tuning, or a combination for a stated requirement and defend it with cost, risk and maintenance arguments (`llm.ft-vs-rag`). 2. Describe a dataset preparation pipeline for SFT: sourcing, cleaning, dedup, formatting, splits, leakage and PII controls (`ml.data-prep`). 3. Explain SFT, LoRA, QLoRA and PEFT mechanics, key hyperparameters and failure modes, and sketch a training run on one GPU (`ml.finetune-concepts`). 4. Explain preference tuning (RLHF, DPO family) and distillation at overview level and say when each is justified (`ml.alignment-distill`). 5. Design an evaluation plan for a tuned model that detects regressions, forgetting and overfitting before rollout (`llm.eval-tuned`). 6. Build a model selection and routing policy balancing cost, quality, latency, data residency and licensing, including open-weight vs hosted (`llm.model-routing`; B2 owns `llm.model-selection` per-request choice). 7. Explain serving internals at concept level (batching, KV cache, quantisation, paged attention, vLLM-style engines) and do capacity and cost estimates (`ml.serving`, `ml.gpu-cost`). 8. Describe an MLOps lifecycle: experiment tracking, model registry, data/model versioning, CI/CD for ML, monitoring and drift, rollback (`mlops.lifecycle`). |
| Effort | 16 h Simpler / 11 h Deeper, plus 2.0 h assessment allowance (Boss effort table, unchanged). Deeper = path after skip/skim decisions and L3–L4 compression; Simpler = full path with extra worked examples. Per-module split in Appendix 8.4. |
| Goal-role weight (course level) | Senior Python Backend: NICE. AI Engineer: SHOULD. Python+AI hybrid: NICE (Boss goal-tag table). Module-level tags in Appendix 8.3. |
| Interview coverage tags | `llm.ft-vs-rag`, `ml.data-prep`, `ml.finetune-concepts`, `ml.alignment-distill`, `llm.eval-tuned`, `llm.model-routing`, `ml.serving`, `ml.gpu-cost`, `mlops.lifecycle`, `mlops.compliance` |
| Badge and certificate | Course badge "Model Strategy and MLOps Practitioner": all 9 module badges, final assessment at 80% or more, plus evidence of the capstone design (decision memo and routing and serving plan) scored against the rubric. Certificate states interview-readiness at concept depth, not production certification. |
| Authoring owner | SME agent B7 |
| Reviewer | SME reviewer role: ML/LLM engineer experienced in fine-tuning and model serving from the Content Lead's SME panel (to be named before publish); separate human legal/compliance read for the provider-terms and regulator `[verify]` items (section 6) |
| Review cadence | Every 3 months (fast-moving). Model names, prices, licences and tool versions carry dated notes; stable concepts (LoRA maths, batching, KV cache) reviewed every 12 months. Last reviewed: 2026-10-06 (facts dated 2026-10-05). |

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice tasks | Pitfalls and misconceptions |
|---|---|---|---|---|---|---|
| M1 | Prompt vs RAG vs fine-tune: the decision (B7 owns this framework; B3.M1 owns RAG vs long-context tree, B2.M7 keeps context management; both point here) | Given a requirement, pick and justify the cheapest approach that meets it, with a stated escalation path | Failure taxonomy (knowledge gap vs behaviour/format/style gap vs capability gap); prompting and few-shot first; long context vs RAG (one slide, pointer to B3.M1); RAG for fresh, private, attributable knowledge; fine-tune for style, format, latency/cost via smaller model, narrow skills; combos (RAG plus tuned model, tuned embedder or reranker); build-vs-buy; cost of ownership (data, eval, retrain, serving) | Decision tree (failure type to technique); cost-of-ownership stacked bar | Triage 6 written scenarios into approach plus escalation trigger; write a one-page decision memo; two decision-memo scenarios: (a) regulated BFSI claims assistant with data-residency limits, (b) high-volume (tens of millions of calls per month) classification and reply drafting with tight unit cost; staff-level probe: "make the case for NOT fine-tuning" | "Fine-tuning teaches the model new facts reliably" (weak for knowledge, good for behaviour); "RAG and fine-tuning are alternatives, not complements"; skipping evals before deciding; ignoring maintenance when the base model is deprecated |
| M2 | Dataset preparation | Specify and build a clean, leak-free SFT dataset with documented provenance | Data sources (logs, expert-written, synthetic); instruction/chat formats and chat templates; quality over quantity; dedup (exact, near-dup), filtering, label noise; train/val/test splits and leakage; class and topic balance; PII and licence checks; consent and retention; synthetic data risks (model collapse, style homogeneity); dataset versioning and cards | Data pipeline flow with quality gates; leakage example step-through | In IDE: write a Python pipeline (Pandas) that dedups, redacts PII with regex, applies a chat template and splits by conversation/user, with hidden tests for leakage | Splitting by row instead of by conversation/user; training on production logs without consent; mismatched chat template at train vs inference; "more data always helps"; evaluating on data seen in training |
| M3 | SFT, LoRA, QLoRA and PEFT | Explain how parameter-efficient tuning works and configure a sensible run | Full fine-tune vs PEFT; SFT loss on assistant tokens only (loss masking); LoRA: low-rank update W + BA, rank r, alpha, target modules, adapter merge; QLoRA: 4-bit base (NF4), paged optimisers, higher memory saving, slight quality/speed cost; other PEFT (prefix, prompt tuning, adapters, DoRA) at name level; memory budget (weights, gradients, optimiser state, activations; rule of thumb, labelled as such: a mixed-precision Adam full fine-tune costs about 16 bytes per parameter before activations, so roughly 112 GB for a 7B model, which is why LoRA and QLoRA fit on one GPU); hyperparameters (learning rate, epochs, batch size, gradient accumulation, warmup); catastrophic forgetting; multi-adapter serving; hosted fine-tuning APIs vs self-run; tooling names (Hugging Face Transformers/PEFT/TRL; TRL 1.x as of Oct 2026, pin versions, freshness box); optimiser-memory intuition stays conceptual in B1.M7, here only applied to the 7B/8B budget; hosted fine-tuning shown via fixture dry-run (create job, poll, evaluate), no paid API lab (R-B7-4) | LoRA matrix diagram; GPU memory budget stacked bar (full vs LoRA vs QLoRA) | Compute memory estimate for a 7B/8B model across full, LoRA, QLoRA; read and critique a provided training config (YAML) and loss curves; skeleton script reviewing a PEFT setup (runs on tiny model on CPU in sandbox) | "LoRA trains a few layers" (it adds low-rank adapters, base frozen); thinking QLoRA is a different algorithm rather than LoRA on a quantised base; high rank equals better; too many epochs overfit small datasets; forgetting to match tokenizer and template |
| M4 | Preference tuning and distillation (overview) | Explain when and why to go beyond SFT and compare the main methods | SFT then preference stage; RLHF pipeline (reward model, PPO) at concept level; DPO and relatives (IPO, KTO, ORPO) as simpler offline preference methods; RLAIF and constitutional-style feedback; RL with verifiable rewards for reasoning (GRPO-style group-relative methods) at name level. Freshness box (Oct 2026): Hugging Face TRL 1.0.0 was published on 2026-03-30 as a unified stack covering SFT, reward modelling, DPO and GRPO, and was at 1.14.x by late September 2026 with LoRA/QLoRA via PEFT; teach concepts, not API signatures, and pin the version in the sandbox; the long tail of DPO variants is still moving, so names beyond DPO and GRPO stay at name level; hosted providers may offer preference or reinforcement-style tuning in addition to SFT, with availability, model coverage and terms varying per provider (dated provider note, no paid-API lab, R-B7-4); preference data collection and noise; reward hacking, over-optimisation; distillation (teacher to student, logit vs output/synthetic-data distillation), licensing limits on using model outputs to train other models; when a small distilled model beats a big prompted one | Pipeline diagram SFT to preference to eval; teacher-student distillation flow | Design a preference-data collection UI/flow on paper; write a distillation plan with cost break-even calculation | Saying RLHF is "just more fine-tuning" without a reward signal; assuming DPO needs no reference model or no data quality; ignoring provider terms for distilling from hosted outputs; distilling without a task-specific eval |
| M5 | Evaluating tuned models (tuned-model specifics ONLY; B5 owns the eval harness, judge design and calibration, A/B-canary-shadow-rollback mechanics (B5.M8); B1.M10 owns statistics) | Design a regression-safe evaluation plan and promotion gate for a tuned model | Before/after comparison against prompted-base and production baselines; held-out golden set built by conversation/time split from M2; forgetting checks (general and safety suites reused from B5); overfitting and memorisation signs (train/val gap, verbatim regurgitation probes); format validity rate; win-rate with paired comparison and sample-size intuition (pointer to B1.M10); small tuned model vs large prompted baseline at equal cost; one-paragraph judge caveat with pointer to B5.M3 | Before/after scorecard; train/val loss divergence chart | In harness (B5 shared LLM Lab Harness, X5): plug two recorded model-output sets into the B5 harness and write the tuned-model gate (task metric, forgetting delta, schema validity, win-rate CI) as a Python function with hidden tests; no new generic harness is built here | Only evaluating on training-like data; trusting perplexity or loss as task quality; no baseline comparison against prompted base model; judge model from same family inflating scores; skipping safety regression |
| M6 | Model selection, routing, open-weight vs hosted | Produce a defensible model portfolio and routing policy for a workload | Selection criteria: quality on your task, latency (TTFT and tokens/s), price per million tokens, context length, modality, tool use, rate limits, availability and SLAs; public benchmarks vs private evals; model families (closed hosted vs open-weight) at category level, with dated examples in freshness boxes (Oct 2026); routing patterns (static by task, cascade with confidence/escalation, classifier or learned router, fallback on failure), prompt caching and batch APIs for cost; open-weight vs hosted: control, data residency, customisation, ops burden, TCO crossover; licensing (permissive, community licences with usage thresholds or restrictions, "open weights" is not "open source") and licence literacy plus provider data and training terms only (compliance depth is X1 in A8.M9.4: DPDP Act, GDPR, residency; one pointer slide here); per-request model/effort choice stays B2.M2; vendor lock-in and provider abstraction | Cost-quality-latency scatter with Pareto frontier; cascade router flow | Given a price and latency table and an eval table, choose models and routing thresholds to meet a budget; write a licence/compliance checklist for 3 candidate models | Picking by leaderboard rank alone; comparing price per token without tokens per task; calling any open-weight model "free" or "open source"; ignoring data residency; routing without an escalation quality check; no fallback for provider outage |
| M7 | Serving and inference optimisation | Explain how LLM inference serving works and tune a serving setup conceptually | Prefill vs decode (compute-bound vs memory-bandwidth-bound); KV cache (reuse the formula derived in B1.M9; here it is applied only: concurrency and max-context calculators, no re-derivation); static vs continuous (in-flight) batching; paged attention idea, prefix caching; throughput vs latency trade-off, TTFT, inter-token latency; quantisation (FP16/BF16, INT8, FP8, INT4, GPTQ/AWQ at name level, quality vs memory vs speed); speculative decoding concept; tensor/pipeline parallelism at concept level; vLLM-style engines and alternatives (TGI, TensorRT-LLM, SGLang, llama.cpp for CPU/edge) at positioning level. Freshness box (Oct 2026): vLLM was at 0.31.0 on 2026-10-05 (PyPI) and releases roughly every two to three weeks; prefix caching is on by default in the V1 engine, chunked prefill and several speculative-decoding drafters (EAGLE-family and newer) are supported; feature names churn monthly, so lessons teach the mechanism and the config trade-off, not flag names; autoscaling, queueing, streaming, multi-LoRA serving; managed endpoints vs self-hosting | Prefill/decode timeline; KV cache growth chart; continuous batching step-through | Calculator: max concurrent sequences for a given model config and GPU memory (using B1.M9 formula); config review of a provided vLLM-style serving config (max batch tokens, memory utilisation, quantisation, prefix caching); Python load-test script against a mock streaming endpoint (harness) measuring TTFT and throughput at different concurrency; all CPU-only | Confusing batch size with throughput linear scaling; forgetting KV cache dominates memory at long contexts; "quantisation is free"; ignoring queueing delay in p99; assuming GPU utilisation 100% means efficient |
| M8 | GPU and cost basics | Estimate GPU needs and cost per request, and choose build vs buy | GPU types by class (consumer, data centre, memory size and bandwidth matter most) without relying on specific prices (price table is a dated learner-visible data file); fitting a model: params times bytes per param plus KV cache plus overhead (for mixture-of-experts models memory scales with total parameters while per-token compute scales with active parameters, so "active parameters" never sizes the GPU); training vs inference cost; on-demand vs reserved vs spot, utilisation as the main driver; cost per million tokens self-hosted vs API; break-even analysis; India angle: GPU availability and region/data residency choices, INR budgeting and egress costs; right-sizing, batching, caching, quantisation, smaller model as cost levers; FinOps tagging | Break-even chart (API vs self-host by volume); memory fit table | Python cost model notebook: inputs (traffic, tokens, GPU price, utilisation) to cost per 1M tokens, with sensitivity analysis | Quoting GPU cost without utilisation; ignoring idle capacity and engineering time; forgetting KV cache in fit estimates; sizing a mixture-of-experts model by active instead of total parameters; assuming spot is safe for latency-critical serving |
| | **Ownership notes (G-1)** | | **OWNS:** prompt vs RAG vs fine-tune framework (M1); fine-tuning mechanics (M2-M4); tuned-model evaluation only (M5); model selection, routing, open-weight vs hosted, licence literacy (M6); serving and GPU memory arithmetic with calculators (M7-M8); model-artefact lifecycle: registry, lineage, reproducibility, classical drift (M9). **POINTS:** RAG vs long-context -> B3.M1 (M1); KV-cache formula, transformers -> B1.M9 and optimiser-memory intuition -> B1.M7 (M3, M7); statistics -> B1.M10 (M5); eval harness -> B5.M1-M2 and judge design -> B5.M3 (M5); per-request model/effort and token accounting -> B2.M2 (M6); compliance primer -> X1 (A8.M9.4) (M2, M6); LLM observability -> B5.M7 and LLM release engineering -> B5.M8 (M9); generic CI/CD -> A7; infrastructure cost levers -> A7.M10 (M8) | | | | |
| M9 | MLOps basics: model-artefact lifecycle (registry, lineage, reproducibility, classical drift). Boundary line: B5.M8 owns LLM release engineering (prompt and model versioning in the app, canary, shadow, A/B, rollback gates); B7.M9 owns the model artefact and its lineage; generic CI/CD and observability stay A7 | Describe and sketch a minimal model-artefact lifecycle for a tuned or routed LLM system, and say where B5.M8 takes over | Experiment tracking (params, metrics, artefacts; MLflow or Weights and Biases at category level, claims limited to categories); reproducibility (seeds, pinned deps, container images, config in code); data and model versioning (DVC, object storage with hashes, Hugging Face Hub style repos, dataset cards); model registry and stages (candidate, staging, production), lineage, approval gates; CI/CD for ML (tests, data validation, eval gate, build image, deploy canary, rollback); continuous training vs scheduled retraining triggers; monitoring (latency, errors, cost, quality sampling, data and concept drift, prompt/input drift); incident playbook; governance (model cards, audit trail, access control, licence record); LLMOps overlap with B5 | MLOps lifecycle loop diagram; CI/CD pipeline with eval gate | Write a CI workflow (YAML) sketch with a gate that blocks promotion when eval drops; log a run with a tracking API stub and register a model version in a mock registry (Python) | Treating notebooks as the pipeline; no lineage from model to data and code; promoting on training metrics; monitoring only infra, not quality; retraining on a schedule with no eval gate |

## 3. Per-module detail blocks

Question-bank targets per module (from the standard): 12 MCQ/concept, 6 code-output/predict, 4 coding/analysis tasks with hidden tests, 8 verbal with follow-up trees, 3 scenario/design. Mastery check: 6–10 items, 80% threshold, revision loop on misses. For modules where code is mostly analysis (M4, M6), "coding tasks" are calculation, config-review or policy-as-code tasks.

**M1 — Prompt vs RAG vs fine-tune**
- Lessons: (1) Diagnose the failure: knowledge, behaviour or capability; (2) Prompting, few-shot and long context as the baseline; (3) RAG vs fine-tuning vs both; (4) Cost of ownership and escalation triggers.
- Simpler: decision tree with worked examples. Deeper: quantify break-even, hybrid architectures, tuned embedder/reranker.
- Mastery focus: classify scenarios; justify with cost, freshness, attribution, latency; escalation criteria.
- Badge: 80% on mastery check plus an accepted decision memo for a given scenario.

**M2 — Dataset preparation**
- Lessons: (1) What good SFT data looks like; (2) Cleaning, dedup, filtering; (3) Formats, chat templates, loss masking inputs; (4) Splits, leakage, PII and consent; (5) Synthetic data and dataset versioning.
- Simpler: single-turn instruction data. Deeper: multi-turn, tool-call data, near-dup via MinHash idea, contamination checks.
- Mastery focus: leakage detection, template consistency, privacy.
- Badge: mastery check plus passing hidden tests on the data pipeline task.

**M3 — SFT, LoRA, QLoRA, PEFT**
- Lessons: (1) SFT mechanics and loss masking; (2) LoRA from first principles; (3) QLoRA and memory budgeting; (4) Hyperparameters and failure modes; (5) Adapters in practice: merge, swap, multi-adapter serving, hosted tuning.
- Simpler: conceptual diagrams and rules of thumb. Deeper: parameter-count arithmetic, optimiser-state memory, rank/alpha scaling.
- Mastery focus: memory estimates, config critique, when LoRA is insufficient.
- Badge: mastery check plus correct memory-estimate and config-review tasks.

**M4 — Preference tuning and distillation**
- Lessons: (1) Why SFT is not enough; (2) RLHF pipeline; (3) DPO family and preference data; (4) Distillation approaches and legality; (5) When not to bother.
- Simpler: analogies and pipeline diagrams. Deeper: DPO objective intuition (policy vs reference, beta), reward hacking, KL regularisation.
- Mastery focus: method selection, data requirements, risk.
- Badge: mastery check plus distillation break-even plan.

**M5 — Evaluating tuned models**
- Lessons: (1) Eval design for tuned models (golden set, baselines); (2) Forgetting, overfitting, memorisation, safety regressions; (3) Win-rate, paired comparison and tuned-small vs prompted-large; (4) Promotion gate and report (rollout mechanics point to B5.M8; judge design points to B5.M3).
- Simpler: checklist and gate template. Deeper: confidence intervals (pointer B1.M10), slice analysis, contamination checks.
- Mastery focus: baselines, gates, leakage, forgetting.
- Badge: mastery check plus passing hidden tests on the tuned-model gate function (runs on the shared B5 harness).

**M6 — Model selection and routing**
- Lessons: (1) Selection criteria and private evals; (2) Cost, latency and quality trade-offs; (3) Routing and cascade patterns; (4) Open-weight vs hosted; (5) Licensing, compliance and residency.
- Simpler: scorecard template. Deeper: router training idea, confidence calibration, TCO crossover, multi-region failover.
- Mastery focus: policy design, licence literacy, compliance reasoning.
- Badge: mastery check plus accepted routing policy and compliance checklist.

**M7 — Serving and inference optimisation**
- Lessons: (1) Prefill, decode and the KV cache; (2) Batching: static, dynamic, continuous; (3) Quantisation and its trade-offs; (4) vLLM-style engines, paged attention, prefix caching, speculative decoding; (5) Scaling, autoscaling and SLOs.
- Simpler: timelines and rules of thumb. Deeper: KV cache formula, memory-bandwidth arithmetic, parallelism trade-offs.
- Mastery focus: bottleneck reasoning, capacity maths, latency metrics.
- Badge: mastery check plus correct capacity calculation and load-test analysis.

**M8 — GPU and cost basics**
- Lessons: (1) Fit estimation; (2) Training vs inference cost; (3) Pricing models and utilisation; (4) Break-even and build vs buy; (5) Cost levers and FinOps.
- Simpler: spreadsheet-style worked examples. Deeper: sensitivity analysis, amortised engineering cost.
- Mastery focus: calculation accuracy, assumptions stated.
- Badge: mastery check plus cost model task with hidden tests.

**M9 — MLOps basics**
- Lessons: (1) Reproducibility and experiment tracking; (2) Data and model versioning; (3) Registry, lineage, approvals; (4) CI/CD for ML with eval gate (generic pipeline mechanics point to A7; rollout, canary, rollback of LLM features point to B5.M8); (5) Classical drift (data, concept) and retraining triggers; LLM quality monitoring points to B5.M7; (6) Governance and documentation (model cards, licence record).
- Simpler: tool-agnostic lifecycle. Deeper: continuous-training triggers, drift statistics, lineage queries.
- Mastery focus: lifecycle ordering, gates, rollback.
- Badge: mastery check plus CI sketch and registry exercise.

## 4. Seed interview questions

Format per question: ideal answer outline; common weak answers; follow-ups; difficulty; competency tag; company-style cluster. All are style-of-question items. Bank IDs follow G-10: Qn.k here is `Q-B7-Mn-0k`; each seed inherits the module goal tag (8.3) and the module senior-bar line (8.7); these 27 seeds are the starter set for the 8-verbal-per-module target.

### M1 — Prompt vs RAG vs fine-tune

**Q1.1 "Our support bot gives outdated policy answers and sometimes the wrong tone. Do we fine-tune?"**
- Ideal: separate two problems: stale knowledge (retrieval problem) and tone (behaviour); RAG for fresh and attributable knowledge with citation; prompt and few-shot for tone first; fine-tune tone only if prompting fails or tokens/latency cost matters; measure with an eval set before and after; consider hybrid; define escalation trigger and owner of retraining; mention maintenance when policies change (RAG updates instantly, tuned model needs retrain).
- Weak: "Always fine-tune on company docs"; "just use RAG for everything"; no evals; ignoring cost.
- Follow-ups: How would you prove prompting is insufficient? What data would tone-tuning need? How do you keep citations accurate?
- Difficulty: medium. Tag: `llm.ft-vs-rag`. Cluster: global-product.

**Q1.2 "When is fine-tuning the right call for a production LLM feature?"**
- Ideal: stable narrow task with enough quality labelled data; need consistent format or style; distil to a smaller cheaper faster model; latency or token cost at volume; private on-prem requirement; baseline prompted model measured and insufficient; evals show lift; ownership of data, retraining and serving accounted for; fine-tuning poor for injecting volatile knowledge; model deprecation risk.
- Weak: "When the model is not accurate"; "to add knowledge"; no cost discussion.
- Follow-ups: What lift justifies the effort? What happens when the base model is retired? Can you combine with RAG?
- Difficulty: medium. Tag: `llm.ft-vs-rag`. Cluster: AI-startup.

**Q1.3 "A RAG baseline for your 5,000-document assistant is built and measured. What evidence would make you escalate to fine-tuning?"** (the long-context vs RAG vs fine-tune comparison is owned by B3 Q1.2 and B2: `ref`)
- Ideal: only after a measured RAG baseline; classify the residual failures (facts missing vs format, tone or task behaviour); fine-tune only for behaviour, style or latency/cost reasons, not facts; hybrid with a tuned reranker or embedder; escalation trigger stated as a metric threshold on the golden set; break-even versus prompt and retrieval improvements; ACL and freshness constraints keep knowledge in retrieval.
- Weak: choose fine-tuning by habit; no baseline; tries to teach facts by tuning.
- Follow-ups: Which failure class says "do not tune"? What is the cheapest next experiment? How do you document the decision in the memo?
- Difficulty: senior. Tag: `llm.ft-vs-rag`. Cluster: global-IT-consulting.

### M2 — Dataset preparation

**Q2.1 "Walk me through preparing a dataset to fine-tune a model for ticket classification and reply drafting."**
- Ideal: define task and success metric first; source (historic tickets, expert-edited replies); consent and PII redaction; dedup and near-dup; filter low-quality or wrong replies; label review sample with inter-annotator agreement; chat template matching inference; split by customer/thread with a time-based test; document in dataset card and version it; keep a frozen golden set; plan for ongoing refresh.
- Weak: "Export everything and train"; random row split; no PII mention.
- Follow-ups: How many examples do you need and how would you find out? How do you detect label noise? What if the best replies are only 5% of data?
- Difficulty: medium. Tag: `ml.data-prep`. Cluster: IT-services.

**Q2.2 "How can data leakage happen in LLM fine-tuning, and how do you prevent it?"**
- Ideal: same conversation or customer across splits; near-duplicates across train/test; test items posted in prompt templates or public benchmark contamination; temporal leakage; synthetic data generated from test prompts; prevent via group and time splits, hash-based dedup across splits, contamination search against benchmarks, frozen golden set access control, audit logs.
- Weak: only mentions "don't train on the test set".
- Follow-ups: How do you detect near-dup leakage at scale? Why does leakage hurt more in small datasets? How do you treat templated tickets?
- Difficulty: medium. Tag: `ml.data-prep`. Cluster: Big4.

**Q2.3 "Can we use customer chat logs to fine-tune? What are the risks?"**
- Ideal: lawful basis and consent, DPDP Act 2023 and GDPR purpose limitation, contract terms, PII and secrets scrubbing, retention and deletion (right to erasure vs memorisation), memorisation and extraction risk, tenant isolation (do not train a shared model on tenant A to serve B), human review, opt-out, vendor terms if hosted tuning, access control for datasets, audit; consider synthetic or anonymised data.
- Weak: "Anonymise names and it's fine"; no mention of consent or memorisation.
- Follow-ups: How do you honour a deletion request after training? What is multi-tenant risk? Do hosted tuning providers retain your data?
- Difficulty: senior. Tag: `mlops.compliance`. Cluster: Big4.

### M3 — SFT, LoRA, QLoRA, PEFT

**Q3.1 "Explain LoRA and why it saves memory."**
- Ideal: freeze base weights W; learn low-rank delta BA with r much smaller than dimension; trainable params shrink by orders of magnitude; gradients and optimiser states only for adapters; alpha/r scaling; target modules (attention projections, often MLP too); adapter merges into W at inference with no added latency or can be swapped; activations still cost memory, so gradient checkpointing helps; quality close to full tuning on many tasks but not all.
- Weak: "It trains fewer layers"; cannot explain why optimiser states matter.
- Follow-ups: How do you choose rank? Merge vs separate adapter in serving? When does LoRA underperform full fine-tuning?
- Difficulty: medium. Tag: `ml.finetune-concepts`. Cluster: AI-startup.

**Q3.2 "QLoRA vs LoRA: what changes and what do you pay?"**
- Ideal: base model quantised to 4-bit (NF4) and frozen; LoRA adapters kept in higher precision; dequantise on the fly for compute; paged optimisers for memory spikes; fits larger models on a single GPU; cost: slower step time, small quality gap, quantisation interactions when merging into a full-precision base; check eval, not assumption; hardware and library support caveats (check pinned bitsandbytes/PEFT versions).
- Weak: "QLoRA is just a faster LoRA".
- Follow-ups: How do you merge a QLoRA adapter safely? Estimate memory for an 8B model. When would you skip QLoRA?
- Difficulty: senior. Tag: `ml.finetune-concepts`. Cluster: global-product.

**Q3.3 "Your fine-tuned model got worse at general tasks and sometimes repeats training answers. Diagnose."**
- Ideal: catastrophic forgetting from high learning rate, too many epochs, narrow data; overfitting signals via val loss rising; memorisation; fixes: lower LR, fewer epochs, lower rank, early stopping, mix general data (replay), regularise, bigger and more diverse data, evaluate on general and safety sets; verify template and masking correctness; compare against prompted base.
- Weak: "Add more epochs"; only look at training loss.
- Follow-ups: How do you set early stopping? What do you log? How would you detect memorisation?
- Difficulty: senior. Tag: `ml.finetune-concepts`. Cluster: AI-startup.

### M4 — Preference tuning and distillation

**Q4.1 "What is RLHF and how does DPO differ?"**
- Ideal: SFT, then train reward model from human preference pairs, then optimise policy with RL (PPO) under KL penalty to the reference; complexity and instability; DPO optimises preference pairs directly with a classification-like loss using policy vs reference log-probabilities, no separate reward model or RL loop; simpler and cheaper, still needs quality pairs and a reference; limits: offline data distribution, over-optimisation, verbosity bias; newer variants (GRPO-style group-relative methods for verifiable rewards, further DPO variants) exist.
- Weak: "RLHF is humans retraining the model"; cannot name reward model or KL.
- Follow-ups: What is reward hacking? How do you collect preference pairs? When is SFT alone enough?
- Difficulty: senior. Tag: `ml.alignment-distill`. Cluster: global-product.

**Q4.2 "When would you distil a large model into a small one?"**
- Ideal: high-volume narrow task, stable requirements, cost and latency pressure; teacher produces labels or reasoning outputs, student trained via SFT on them (or logit matching with open teacher); filter teacher outputs by quality/verifier; evaluate student against teacher and against human golden set; accept ceiling on capability; terms-of-service restrictions on training with provider outputs; refresh when the teacher or task changes; break-even maths.
- Weak: "Train small model on big model answers" without eval or legal view.
- Follow-ups: How do you compute break-even? How do you stop copying teacher errors? What if ToS forbids it?
- Difficulty: medium. Tag: `ml.alignment-distill`. Cluster: Indian-product.

**Q4.3 "Your team wants RLHF for a customer-facing assistant. Do you agree?"**
- Ideal: ask what behaviour gap remains after prompting and SFT; evaluate cheaper options (better prompts, SFT on curated data, DPO on existing preference signals such as edits and thumbs); preference data cost and noise; safety and reward hacking; team maturity and infra; define metric uplift needed; likely recommend staged approach; guardrails and monitoring remain necessary.
- Weak: "Yes, RLHF gives best quality"; no costing.
- Follow-ups: What implicit feedback signals could you mine? How do you detect over-optimisation? Who labels?
- Difficulty: senior. Tag: `ml.alignment-distill`. Cluster: global-IT-consulting.

### M5 — Evaluating tuned models

**Q5.1 "How do you decide a fine-tuned model is ready to ship?"**
- Ideal: pre-agreed metrics and thresholds on a frozen golden set; baseline comparison to prompted base and to current production; format validity and task accuracy; general capability and safety regression suites; human review sample; latency and cost per request; slice analysis (languages, customer segments); shadow then canary with guardrail metrics; rollback plan; sign-off record in registry.
- Weak: "Loss went down"; "Looks good in a few examples."
- Follow-ups: How big should the golden set be? How do you handle metric disagreement? What triggers rollback?
- Difficulty: medium. Tag: `llm.eval-tuned`. Cluster: global-product.

**Q5.2 "You use a judge to compare a tuned model with its base model. Which judge risks are specific to tuned models, and what checks do you add?"** (general LLM-as-judge pitfalls and calibration are owned by B5.M3 / Q3.1: `ref`)
- Ideal: tuned-model-specific risks: judge shares a model family with the tuned or teacher model (self-preference, distillation leakage); tuned model learned the judge's style or rubric from synthetic training data; training-set contamination of the judge prompts; format overfitting that a judge rewards; mitigations: judge from a different family than the teacher, held-out judge prompts, paired comparison with order swap, programmatic checks and a small human-labelled slice, judge version pinned in the gate.
- Weak: "Judge models are accurate enough"; repeats the general bias list only.
- Follow-ups: How do you detect style overfitting? How do you pin the judge version in the promotion gate? Which tasks avoid judges?
- Difficulty: senior. Tag: `llm.eval-tuned`. Cluster: AI-startup.

**Q5.3 "What promotion criteria must a tuned small model meet before it replaces a hosted large model on live traffic?"** (rollout mechanics, canary and rollback are owned by B5.M8: `ref`)
- Ideal: criteria, not rollout steps: offline gate on the golden set with slice results and a stated margin versus the baseline; cost and latency targets met; fallback rate and invalid-output rate under thresholds; safety slice not worse than baseline; registered artefact with lineage and rollback target (B7.M9); sign-off by named owner; criteria written before the run.
- Weak: "Switch over on a weekend"; criteria chosen after seeing results.
- Follow-ups: What confidence signal do you use for fallback? Which slice would block promotion? Who owns the decision?
- Difficulty: senior. Tag: `llm.eval-tuned`. Cluster: IT-services.

### M6 — Model selection, routing, open-weight vs hosted

**Q6.1 "How do you choose a model for a new feature?"**
- Ideal: define task, quality bar, latency target (TTFT and total), volume and budget, data sensitivity and residency, context and modality needs, tool-use needs; shortlist 3–4 candidates across tiers; private eval set from real data; measure quality, latency, cost per task (not per token); check rate limits, SLAs, regional availability, deprecation policy; pilot; record decision and re-evaluate on cadence; avoid lock-in via abstraction.
- Weak: "Use the best on the leaderboard"; "Use whatever we already have".
- Follow-ups: What if two models tie on quality? How often do you re-evaluate? How do you handle a deprecation notice?
- Difficulty: medium. Tag: `llm.model-routing`. Cluster: global-IT-consulting.

**Q6.2 "Design a model router for a platform serving cheap FAQ queries and hard analytical ones."**
- Ideal: classify difficulty (rules, small classifier, or small LLM) and route to tiers; cascade: try small model, verify with validator or confidence, escalate on failure; cost, latency and quality per tier measured; fallbacks on provider errors and rate limits; caching (semantic and prompt) before routing; per-tenant policies; observability on route decisions and escalation rate; periodically audit misroutes; avoid router cost exceeding savings.
- Weak: "Send everything to the biggest model"; no escalation check; no monitoring.
- Follow-ups: How do you set confidence thresholds? What if the router is wrong on high-stakes queries? How do you A/B the router?
- Difficulty: senior. Tag: `llm.model-routing`. Cluster: Indian-product.

**Q6.3 "Open-weight model self-hosted vs hosted API for a regulated Indian BFSI client."**
- Ideal: data residency and sensitivity (DPDP Act 2023, sector regulator rules, specifics via X1 and counsel [verify: section 6 item 2; counsel check of DPDP Rules text and sector regulator documents]); control and auditability vs operational burden; quality gap on their task measured; licence terms (commercial use, usage thresholds, acceptable-use) read by legal; TCO with utilisation, GPUs, staff, security patching; hosted options with regional endpoints, zero retention and private networking; hybrid (sensitive workloads self-hosted, others hosted); exit plan; model risk management documentation.
- Weak: "Open-weight is cheaper"; "Hosted is always safer".
- Follow-ups: At what volume does self-hosting break even? What licence clauses worry you? How do you patch and update the model?
- Difficulty: senior. Tag: `mlops.compliance`. Cluster: Big4.

### M7 — Serving and inference optimisation

**Q7.1 "Given this serving config (layers, KV heads, head dimension, precision, context length, batch), compute the KV cache size and say what limits concurrency."** (KV-cache concept and formula are owned by B1.M9 / Q9.2: `ref`)
- Ideal: applies the B1 formula to the supplied numbers with units; adds model weights and runtime overhead to see free memory; derives the maximum concurrent sequences at the given context; compares the effect of grouped-query attention, KV quantisation and paged allocation on the answer; states which of prefill (compute-bound) or decode (bandwidth-bound) the config is limited by.
- Weak: "It caches responses"; formula without units; ignores weights memory.
- Follow-ups: Halve the context: what changes? What does paged attention solve? How does prefix caching help chat?
- Difficulty: senior. Tag: `ml.serving`. Cluster: global-product.

**Q7.2 "What is continuous batching and why do vLLM-style servers use it?"**
- Ideal: static batching waits for the slowest sequence and wastes GPU; continuous (iteration-level) batching adds and removes requests every decode step; higher throughput and better utilisation; combined with paged KV memory management; trade-off with per-request latency under load; scheduler policies, max batch tokens, preemption; tune for TTFT vs throughput; mention chunked prefill (supported by vLLM and SGLang as of Oct 2026).
- Weak: "Batch requests to speed things up" with no mechanism.
- Follow-ups: How does it affect p99 TTFT? What limits batch size? How would you set max concurrency?
- Difficulty: senior. Tag: `ml.serving`. Cluster: AI-startup.

**Q7.3 "What does quantisation do to a deployed model, and how do you validate it?"**
- Ideal: lowers precision of weights (and possibly activations/KV) to cut memory and increase speed; formats (INT8, FP8, INT4 methods such as GPTQ, AWQ) differ in hardware support and quality loss; larger models tolerate it better; may hurt long-context, maths, code or low-resource languages; calibration data matters; validate on private eval with latency and memory measured; staged rollout; keep full-precision fallback.
- Weak: "No loss in quality"; no validation plan.
- Follow-ups: Which tasks degrade first? Weight-only vs activation quantisation? How does it interact with LoRA adapters?
- Difficulty: medium. Tag: `ml.serving`. Cluster: IT-services.

### M8 — GPU and cost basics

**Q8.1 "Will a 70B model fit on one 80 GB GPU for serving? Walk through the estimate."**
- Ideal: weights = params times bytes (70B at FP16 about 140 GB; INT8 about 70 GB; 4-bit about 35 GB); add KV cache and runtime overhead; so FP16 does not fit, 4-bit may fit with limited concurrency/context, multi-GPU tensor parallelism otherwise; state assumptions; consider throughput target and quality impact of quantisation.
- Weak: guesses without arithmetic; ignores KV cache.
- Follow-ups: How does context length change the answer? What about two GPUs and interconnect? What would you test first?
- Difficulty: medium. Tag: `ml.gpu-cost`. Cluster: global-IT-consulting.

**Q8.2 "When is self-hosting cheaper than an API?"**
- Ideal: cost per million tokens self-hosted = GPU hourly cost divided by tokens per hour at realistic utilisation; add ops staff, redundancy, idle time, networking; API pay-per-use with no idle cost, includes updates; break-even at sustained high utilisation and predictable load; consider batch APIs, caching and smaller models first; sensitivity analysis; non-cost drivers (privacy, control, latency).
- Weak: "Self-hosting is cheaper at scale" without numbers.
- Follow-ups: What utilisation assumption is realistic? How do you account for peak vs average? What hidden costs get missed?
- Difficulty: senior. Tag: `ml.gpu-cost`. Cluster: Indian-product.

**Q8.3 "Your inference bill doubled this quarter. How do you investigate and reduce it?"**
- Ideal: break down by feature, tenant, model, input vs output tokens and cache hit rate; find regressions (prompt growth, retries, runaway agents, long contexts); levers: prompt trimming, caching, routing to smaller models, batch APIs, quantisation, rate limits and quotas, max tokens; set budgets and alerts; cost per successful task metric; verify quality after each lever.
- Weak: "Negotiate pricing" only; no attribution.
- Follow-ups: What metrics do you add? How do you protect quality? How do you apportion shared cost?
- Difficulty: medium. Tag: `ml.gpu-cost`. Cluster: IT-services.

### M9 — MLOps basics

**Q9.1 "What is the role of a model registry and what goes in it?"**
- Ideal: central catalogue of model versions with lineage (code commit, data version, config, base model, adapter), metrics and eval reports, stage and approval status, licence and model card, owner, deployment history; enables promotion, rollback, audit; integrates with CI/CD; access control; for LLM systems also record prompt and router config versions.
- Weak: "A folder in S3 with model files."
- Follow-ups: How do you tie a registry entry to a deployment? What approvals do you require? How do you register adapters?
- Difficulty: medium. Tag: `mlops.lifecycle`. Cluster: global-product.

**Q9.2 "Design CI/CD for a fine-tuned LLM service."**
- Ideal: code and data changes trigger pipeline; unit tests and data validation; training or adapter build job (on demand, not every commit); eval gate against golden set vs production baseline with thresholds; register artefact; build serving image with pinned deps; deploy to staging, smoke and load test; canary with metrics and auto-rollback; infra as code; secrets management; tracking and notifications; separation of training and serving pipelines.
- Weak: "Use Jenkins to deploy the model"; no eval gate.
- Follow-ups: How do you keep training reproducible? How do you version prompts and datasets? How do you control GPU cost in CI?
- Difficulty: senior. Tag: `mlops.lifecycle`. Cluster: IT-services.

**Q9.3 "A tuned classifier is in production. How do you detect classical drift and decide when to retrain?"** (LLM production monitoring is owned by B5.M7 / Q7.1: `ref`)
- Ideal: classical ML monitoring: input feature and embedding drift (PSI or similar), label or outcome delay and proxy metrics, prediction distribution shift, slice performance on a sampled labelled set; retraining triggers as thresholds on drift and quality, with a cost of retraining and a data-freshness check; versioned lineage linking model, data and eval; shadow-evaluate the candidate before promotion; privacy-aware logging.
- Weak: "CPU and memory and logs"; retrain on a calendar with no evidence.
- Follow-ups: How do you detect drift without labels? What stops a retrain from making things worse? Which trigger is the cheapest to automate?
- Difficulty: medium. Tag: `mlops.lifecycle`. Cluster: global-IT-consulting.

## 5. Final assessment

- **Mastery check:** 25 items across all modules (concept, calculation, predict-the-result, config critique), 80% pass (G-7), weighted toward M1, M6, M7 (highest interview weight). Whole-course test-out needs this check at 85% plus the voice mock at Senior bar (G-5).
- **Timed task (60 min, IDE, CPU-only):** given a JSONL dataset, a price/latency table and two recorded model-output sets, (a) detect leakage and clean the data, (b) apply the tuned-model promotion gate on the shared harness (B5 owns the harness), (c) compute cost per 1M tokens and break-even for self-hosting vs API. Hidden tests check correctness.
- **Fixture dry-run lab (no paid API, R-B7-4):** provider-shaped fixtures walk through create fine-tune job, poll status, read training log, evaluate result and decide promote/hold. Labelled "simulated" in the UI.
- **Design/canvas task (35 min):** "Design the model strategy and serving architecture for a high-volume enterprise assistant with a compliance constraint." Must show decision (prompt/RAG/tune), model portfolio and router, serving layer, registry and eval gate, monitoring pointer to B5, cost estimate, rollback pointer to B5.M8. Rubric items: trade-offs, failure modes, cost, compliance. The rule engine cites registry IDs only (for example R-AI-1 fallback on the LLM path, R-AI-2 or R-COST-2 budget and metering, R-OBS-1, R-TENANT-1 if multi-tenant); cost-model and licence reasoning are rubric-scored, not local rules.
- **Decision-memo scenarios (2, rubric-scored):** (1) regulated BFSI assistant; (2) high-volume classification and drafting. Each includes a "when not to fine-tune" section.
- **Voice mock round (20 min):** one decision question (M1), one serving or cost question (M7 or M8), one MLOps question (M9) with follow-ups, including a staff-level probe; scored on structure, accuracy, trade-off articulation and handling of unknowns. Badge bar "Meets" (average 3.0, none below 2); readiness bar Senior bar (G-6, 8.10).
- **Capstone evidence:** one-page decision memo plus routing and serving plan.

## 6. Course-level risks and freshness notes

Last reviewed: 2026-10-06 (version and date facts as of 2026-10-05). Review cadence: 3 months (AI content), 12 months (LoRA maths, batching, KV cache).

- Fast-changing: model families, prices, context lengths and licences; hosted fine-tuning availability per provider; serving engines and features; quantisation formats and hardware support (FP8/FP4); preference-method landscape; open-weight licence terms. Lessons teach categories and mechanisms; dated examples live in swappable freshness boxes.
- **Resolved by web check, 2026-10-05 (secondary sources, re-confirm at each review):** TRL 1.0.0 was published 2026-03-30 (SFT, reward modelling, DPO, GRPO) and 1.14.1 on 2026-09-29 (PyPI, verified 2026-10); LoRA/QLoRA through PEFT; vLLM 0.31.0 was published 2026-10-05 (0.29.0 on 2026-09-09, 0.30.0 on 2026-09-22; PyPI), with prefix caching and chunked prefill on by default in V1 per vLLM docs; PEFT latest 0.21.2 (2026-10-01). Speculative decoding (EAGLE-family drafters) in vLLM and chunked prefill in SGLang are long-documented mechanisms, so lessons state them at mechanism level only and name no flags or drafter versions; this removes the earlier "not rechecked" caveat (reviewer judgement, 2026-10-06).
- **[verify] (3 remain, each with its exact check; the inline marker in Q6.3 points to item 2):** (1) GPU price and availability in Indian cloud regions: before each quarterly publish, refresh the dated price data file from the public price pages of the cloud providers the course names and confirm that GPU instance families used in worked examples are still offered in an Indian region; never hard-code; (2) sector-specific Indian regulator guidance on AI/model risk and DPDP Rules operational details (owned by X1/A8.M9.4): counsel reads the DPDP Rules text as notified and the current RBI, SEBI and IRDAI documents, and confirms whether each is binding or advisory; (3) provider terms on training from outputs and on fine-tuning data retention: read each named provider's current data-usage, retention and output-use terms and record the date in the provider notes.
- Risk: drifting into research depth. Keep derivations to what an interviewer asks (LoRA rank maths, memory arithmetic, KV formula applied).
- Boundaries: B7 owns the decision framework, tuned-model evaluation, serving and GPU arithmetic, model-artefact lifecycle; B5 owns the eval harness, judges, LLM observability and LLM release engineering (B5.M8); A7 owns generic CI/CD, observability and infrastructure cost levers; A8.M9.4 owns compliance.
- No GPU in 1.0 (R-B7-1): state clearly to learners that training is reasoned about via calculators, tiny CPU models, recorded logs and config reviews.
- Not legal advice: licence content is literacy-level.

## 7. Open questions for the boss agent

All four earlier questions were ruled (R-B7-1 to R-B7-4): CPU-only, B5/B7 boundary set, compliance in X1, no paid hosted-tuning lab. New questions: (1) Should the shared harness (X5) expose a "tuned-model gate" template so B5 and B7 do not each ship a gate function? (2) Is a dated GPU price data file owned by A7.M10 so B7.M8 calculators can read it? (3) Accept the G-5 exception that M1 (decision framework) and M6 (selection and routing) are never fully skippable: a placement score of 85 or more is treated as Skim, and the mastery check is still required (section 1 Placement skip; 8.1). Recommended ruling: accept, because both carry the highest interview weight and the placement items are only 5 per module.

## 8. Learner-Fit Layer (template section 6)

Goal-tag legend: B = Senior Python Backend, A = AI Engineer, H = Python+AI hybrid. M MUST, S SHOULD, N NICE. Course-level: B:N A:S H:N (Boss table); module tags below are per Boss ruling.

### 8.1 Entry diagnostics (per module, 5-10 items feed the placement test; score on the module slice)

| Module | Sample diagnostic items (mix of MCQ, calculation, short verbal) | Skip | Skim | Study | Deep-study |
|---|---|---|---|---|---|
| M1 | Classify 3 failures (stale facts, wrong tone, weak reasoning) to a technique; why tuning is poor at facts; one escalation trigger; compare long-context vs RAG cost per query; what a decision memo must contain | not allowed (85-100% is treated as Skim) | 70-84% | 50-69% | under 50% |
| M2 | Spot leakage in a split description; why group split; chat-template mismatch effect; PII before training; near-dup vs exact dedup | 85-100% | 70-84% | 50-69% | under 50% |
| M3 | LoRA update shape W + BA; what is frozen; why optimiser state dominates full tuning; QLoRA vs LoRA; effect of too many epochs; loss masking | 85-100% | 70-84% | 50-69% | under 50% |
| M4 | RLHF stages; DPO vs RLHF; reference model role; reward hacking; distillation ToS risk | 85-100% | 70-84% | 50-69% | under 50% |
| M5 | Why loss is not task quality; baseline choices; forgetting check; win-rate on 40 samples is it reliable; promotion gate fields | 85-100% | 70-84% | 50-69% | under 50% |
| M6 | Cost per task vs per token; cascade failure mode; "open-weight" vs "open source"; selection steps; fallback design | not allowed (85-100% is treated as Skim) | 70-84% | 50-69% | under 50% |
| M7 | Prefill vs decode bound; apply KV formula for given config; continuous vs static batching; quantisation validation; p99 TTFT under load | 85-100% | 70-84% | 50-69% | under 50% |
| M8 | Fit a 70B model in 80 GB at 4-bit; cost per 1M tokens from GPU price and utilisation; break-even reasoning; spot risk; why KV cache and overhead must be added to weights in a fit estimate | 85-100% | 70-84% | 50-69% | under 50% |
| M9 | Registry contents; lineage fields; eval-gate position; classical drift types; where B5.M8 begins | 85-100% | 70-84% | 50-69% | under 50% |

Diagnostics (LF-2): each module has 5-6 placement items drawn from its MCQ/predict buckets, IDs `Q-B7-Mn-p01` onward (M3 has 6, all others 5; 46 in total, tagged `placement`, no extra authoring). Bands follow the G-5 defaults (85 or more, 70-84, 50-69, under 50). Band actions: Skip = mastery check only; Skim = key-idea summaries, worked examples and the applied lab, mastery check; Study = full Simpler or Deeper lessons; Deep-study = Simpler path, remediation links (8.5), then Deeper challenge after passing.

### 8.2 Level paths (L1 new, L2 junior-mid, L3 experienced, L4 senior/expert)

| Module | L1 | L2 | L3 | L4 |
|---|---|---|---|---|
| M1 | Simpler: decision tree, 3 worked cases; no memo | Simpler plus memo template | Deeper: break-even maths, hybrids; 2 memos | Deeper plus staff probe "when NOT to fine-tune"; memo defended aloud |
| M2 | Single-turn data, row-level checks | Group split, template, PII | Multi-turn, tool-call data, MinHash idea | Contamination audit; provenance and consent design |
| M3 | Diagrams, rules of thumb | Config reading, memory estimate | Parameter-count arithmetic, rank/alpha, failure diagnosis | Explain LoRA rank maths aloud; critique configs cold |
| M4 | Analogies, pipeline view | Compare methods in a table | DPO objective intuition (beta, reference), KL | Staged-recommendation reasoning against a team asking for RLHF |
| M5 | Checklist | Build gate function | Paired comparison, slices | Defend a gate against stakeholder pressure; contamination and judge caveats |
| M6 | Scorecard | Routing thresholds from tables | TCO crossover, router calibration | Portfolio memo with exit plan, licence reading, regional failover |
| M7 | Timelines, rules of thumb | Calculator use | KV and bandwidth arithmetic, parallelism trade-offs | Serving-config review; p99 reasoning under load |
| M8 | Spreadsheet examples | Cost model with sensitivity | Amortised engineering and idle cost | Defend a self-host vs API recommendation with assumptions |
| M9 | Tool-agnostic loop | Registry and CI sketch | Retraining triggers, lineage queries | Governance and audit design; boundary with B5.M8 |

Simpler/Deeper switch: L1-L2 default to Simpler, L3 starts Deeper after any Skim, L4 Deeper plus challenge. Time per module in 8.4.

Course-level summary (LF-1; Deeper hours are net of skip-ahead, G-2):

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 | Simpler, 16 h, all prerequisites (B1, B2) linked first | Worked examples, calculators, config reading; memo template | Nothing | None until first-attempt 95% |
| L2 | Simpler, about 14-16 h | Memo, gate function, cost model | Lessons already known after placement | Optional M1 and M6 |
| L3 | Deeper, about 11 h | Calculators and config reviews first, then timed task | Foundations in M2, M3, M9 after Skim | M3 parameter counts, M7 chunked-prefill trade-off |
| L4 | Deeper, about 8-11 h `[judgement]`; M1 and M6 never fully skipped | Staff probe, memo defended aloud, voice drill | Skip-to-mastery where scored 85% or more (M2, M3, M7-M9) | All Challenge items in 8.5 |

### 8.3 Goal tags (module and question level)

| Module | Goal tags | Company-style emphasis |
|---|---|---|
| M1 | B:N A:M H:S | AI-startup, global-product MUST; IT-services, Big4 SHOULD (solution decisions) |
| M2 | B:N A:S H:N | AI-startup SHOULD; Big4 (privacy angle) SHOULD |
| M3 | B:N A:S H:N | AI-startup, global-product SHOULD; others NICE |
| M4 | B:N A:S H:N | global-product SHOULD; others NICE |
| M5 | B:N A:S H:N | global-product, AI-startup SHOULD |
| M6 | B:N A:M H:S | IT-services, global-IT-consulting, Big4 MUST (model choice, BFSI); Indian-product SHOULD |
| M7 | B:N A:S H:S | AI-startup, global-product SHOULD; IT-services NICE |
| M8 | B:N A:S H:S | Indian-product, IT-services SHOULD (cost conversations) |
| M9 | B:N A:S H:N | global-product SHOULD; IT-services SHOULD for CI/CD framing |

Seed goal tags (G-10; the one-line senior-bar note for every seed is the module row in 8.7): Q1.1-Q1.3 `B:N A:M H:S`; Q2.1-Q2.3 `B:N A:S H:N`; Q3.1-Q3.3 `B:N A:S H:N`; Q4.1-Q4.3 `B:N A:S H:N`; Q5.1-Q5.3 `B:N A:S H:N`; Q6.1-Q6.3 `B:N A:M H:S`; Q7.1-Q7.3 `B:N A:S H:S`; Q8.1-Q8.3 `B:N A:S H:S`; Q9.1-Q9.3 `B:N A:S H:N`. Seed questions inherit the module goal tag and the module senior-bar line (G-10); senior-difficulty items in M1 and M6 are MUST for AI Engineer. Quick-win for a Senior Python Backend target: M1 and M6 only (about 3.6 h on the Deeper path, M1 1.8 plus M6 1.8) as a "can discuss AI trade-offs" layer `[judgement]`.

### 8.4 Interview-weighted time (hours, Simpler / Deeper; sums to 16 / 11, plus 2.0 assessment)

| Module | S | D | Rationale `[judgement]` |
|---|---|---|---|
| M1 | 2.5 | 1.8 | Appears in nearly every AI Engineer loop ("fine-tune or RAG?") |
| M2 | 1.5 | 1.0 | Asked as data-leakage and privacy probes; moderate |
| M3 | 2.0 | 1.4 | LoRA/QLoRA explanations are the most common training question |
| M4 | 1.0 | 0.75 | Overview depth; asked as "RLHF vs DPO" |
| M5 | 1.5 | 1.0 | Ship-readiness questions; harness depth lives in B5 |
| M6 | 2.5 | 1.8 | Model choice, routing and BFSI scenarios are very common in services and Big 4 |
| M7 | 2.0 | 1.4 | KV cache, batching, quantisation: core serving probes |
| M8 | 1.5 | 1.1 | Back-of-envelope GPU and cost questions |
| M9 | 1.5 | 0.75 | Registry, CI/CD gate; overlap with A7/B5 trimmed |

Interview-weight percent per target (G-11 LF-2; each column sums to 100; `[judgement]`, calibrate after the pilot; derived from the hours rationale above and from how often each topic is probed in target-role loops):

| Weight % | M1 | M2 | M3 | M4 | M5 | M6 | M7 | M8 | M9 | Sum |
|---|---|---|---|---|---|---|---|---|---|---|
| Backend (B) | 22 | 5 | 6 | 3 | 6 | 24 | 10 | 14 | 10 | 100 |
| AI Engineer (A) | 16 | 8 | 14 | 6 | 9 | 14 | 14 | 9 | 10 | 100 |
| Hybrid (H) | 18 | 6 | 10 | 5 | 8 | 18 | 12 | 10 | 13 | 100 |

Backend weights are high on M1, M6 and M8 because the Backend route takes only the NICE trade-off layer (M1 and M6, then M8 cost conversations).

### 8.5 Adaptive hooks

- Thresholds: mastery 80% (configurable); two misses in a topic cluster trigger the remediation link below; 95%+ on first attempt unlocks the Challenge item.
- Remediation links: M2 -> B1 (splits, leakage, overfitting); M3 -> B1.M7 (optimiser memory) and B1.M9 (transformers); M5 -> B5.M1-M3 (harness, judges) and B1.M10 (statistics); M6 -> B2.M2 (tokens, reasoning-model choice) and A8.M9.4 (X1); M7 -> B1.M9 (KV formula) and A7 (containers, scaling); M8 -> A7.M10 (cost levers); M9 -> A7 (CI/CD) and B5.M8 (release engineering).
- Challenge extensions: M1 staff probe memo; M3 derive adapter parameter counts for a given architecture; M6 router calibration with fixtures; M7 chunked-prefill TTFT vs throughput trade-off; M8 sensitivity analysis with spot interruptions.
- Reorderable: M2-M4 as a block (any order after M1); M7 and M8 may be swapped; M5 requires M2 (splits) and should follow M3. M1 first, M9 last, M6 may move earlier for hybrid and backend routes (`NICE` M1+M6 mini-route).

### 8.6 Experience bridge (From what you already know, L3-L4 with API-only experience)

| You already know | Maps to | Gap between using and explaining at senior depth |
|---|---|---|
| Calling LLM APIs, prompt iteration | M1 decision framework | Proving prompting is insufficient with a baseline eval, and costing ownership |
| Postgres/Redis capacity planning, caching | M7-M8 (KV cache, prefix cache, utilisation) | KV cache memory formula, batching mechanics and why GPU utilisation misleads |
| FastAPI services with queues and p99 SLOs | M7 TTFT, queueing, streaming | Reasoning about prefill/decode bounds and continuous batching, not just autoscaling |
| AWS bills, Lambda cost tuning | M6, M8 cost per task and break-even | Cost per successful task, utilisation and idle-GPU economics |
| Docker, CI/CD, registries | M9 | Eval gates, lineage of data+code+base model+adapter, drift beyond infra metrics |
| MQTT/IoT telemetry pipelines, schema validation, alert tuning | M2 data validation and M9 classical drift | Treating drift as a statistical signal with a retraining trigger and eval gate, not a dashboard alert |
| Voice agents with LLM routing (B6) | M6 routing and cascades | Router escalation quality, thresholds, and fallbacks on outage |

For API-only learners, the goal is to reason about training without running it: use calculators for memory and cost, and rehearse "explain LoRA rank maths and KV-cache sizing aloud" in voice drills.

### 8.7 Senior-bar framing (mid vs senior vs staff signals)

| Module | Mid-level answer | Senior answer | Staff answer |
|---|---|---|---|
| M1 | Picks RAG or tuning by habit | Separates failure types, baselines first, states escalation trigger and cost | Builds the organisational policy: evaluation-first gate, ownership of retraining, deprecation risk, argues when not to fine-tune |
| M2 | "Clean and split the data" | Group/time split, dedup across splits, PII and consent, dataset card | Data contracts and provenance across teams, deletion and memorisation policy, multi-tenant rules |
| M3 | "LoRA trains fewer parameters" | Frozen W, BA, optimiser state saving, rank/alpha, merge vs swap, QLoRA cost | Chooses tuning strategy per workload economics, multi-adapter serving, knows when LoRA underperforms |
| M4 | Names RLHF and DPO | Pipeline, KL and reference, data cost, staged recommendation | Org-level decision on investment, implicit-feedback mining, over-optimisation monitoring |
| M5 | Looks at loss and examples | Frozen golden set, baselines, forgetting, gate with thresholds | Gate governance, slice fairness, cost-quality frontier across a portfolio |
| M6 | "Best on leaderboard" | Private evals, cost per task, routing with escalation, licence and residency | Portfolio strategy, vendor exit plan, TCO crossover, deprecation handling at scale |
| M7 | "Batch to go faster" | KV, prefill/decode, continuous batching, quantisation validation | Capacity planning with SLOs, scheduler trade-offs, build-vs-managed endpoint decision |
| M8 | Guesses GPU needs | Arithmetic with stated assumptions, utilisation, break-even | Sensitivity, hidden costs, procurement and FinOps governance |
| M9 | "Store model in S3" | Registry with lineage, eval gate, drift types | Platform design across teams, audit and governance, separation of training and serving pipelines |

### 8.8 Memorable visual or analogy and misconception handling (per module)

| Module | Visual / analogy | Misconception handled (with the corrective step-through) |
|---|---|---|
| M1 | "Doctor, textbook, or training" decision tree: prompt = brief the doctor, RAG = hand them the chart, fine-tune = send them to residency | "Fine-tuning teaches facts": show a tuned model hallucinating a post-training policy while RAG answers correctly |
| M2 | Exam leakage: students sharing the same textbook chapter in study and test | Random-row split leakage: step-through that inflates test score then collapses on new customers |
| M3 | Sticky-note overlay on a printed manual (LoRA) over a locked book | "LoRA trains a few layers": animate frozen W plus BA delta and trainable-parameter counts |
| M4 | Taste-test tournament: SFT writes recipes, preference tuning ranks plates | "DPO needs no reference or data quality": show policy/reference log-prob ratio and noisy-label effect |
| M5 | Pre-flight checklist with a "no-go" light per item | "Loss tells quality": chart where val loss improves while win-rate drops |
| M6 | Pareto frontier map (cost vs quality) with a router as a toll booth | Leaderboard fallacy: same model ranked differently on private eval |
| M7 | Restaurant kitchen: prefill = prep, decode = plating, KV cache = tickets pinned to the pass; continuous batching = seating guests as tables free up | "Quantisation is free": long-context and low-resource-language degradation demo |
| M8 | Taxi vs owning a car: utilisation decides the winner | Cost without utilisation: break-even chart shifts with idle time |
| M9 | Passport with stamped lineage for each model version | "Notebook is the pipeline": lost-reproducibility incident replay |

### 8.9 Applied labs from real-world work (CPU-only, harness and calculators)

| Module | Lab (realistic scenario) |
|---|---|
| M1 | Insurance-claims support assistant: triage 6 requests to approach and write a decision memo; plus BFSI and high-volume memo scenarios |
| M2 | SaaS support tickets: Pandas pipeline that dedups, redacts PII, applies a chat template, splits by customer and time; hidden leakage tests |
| M3 | Tiny-model PEFT config review on CPU and 8B memory calculator across full/LoRA/QLoRA; critique a recorded training log |
| M4 | Distillation break-even for an IoT-alerts summariser; design a preference-collection flow from edit and thumbs signals |
| M5 | Tuned-vs-prompted comparison on recorded outputs: gate function with forgetting delta, schema validity and win-rate interval |
| M6 | Voice-assistant stack: choose models and cascade thresholds from price/latency/eval tables; licence checklist for 3 models |
| M7 | Capacity calculator and serving-config review for a support assistant; mock streaming endpoint load test for TTFT and throughput |
| M8 | Cost model notebook for an enterprise assistant with INR budget, utilisation and sensitivity table |
| M9 | Mock registry and CI YAML with eval gate; lineage query ("which data produced the model in production?") |

### 8.10 Interview rehearsal link and readiness criterion

Every row below is the topic-specific part of the G-6 readiness definition and is added to the G-6 base: module mastery check at 85% or more on a retake at least 7 days after the first pass; timed task passed; strict-mode voice drill at Senior bar (average 3.5 of 5, no competency below 3) in two sessions at least 48 hours apart; no open remediation flag on prerequisites. The badge ("Meets") bar is lower: average 3.0, none below 2.

| Module | Interview Master modes | Topic-specific readiness criterion (observable) |
|---|---|---|
| M1 | Voice drill, design canvas | Memo accepted; voice answers to Q1.1-Q1.3 each state an explicit escalation trigger |
| M2 | Voice drill, coding | Hidden tests pass; leakage verbal answer names group split, near-dup and contamination controls |
| M3 | Voice drill | Explains LoRA and QLoRA aloud in under 2 minutes with the memory estimate within 15% |
| M4 | Voice drill | One staged-recommendation answer (Q4.3) that costs the alternatives |
| M5 | Voice drill, design canvas | Gate function passes hidden tests (at least 70% of hidden tests); Q5.1 and Q5.3 state criteria before results |
| M6 | Voice drill, design canvas | Routing policy accepted; Q6.2 and Q6.3 cover escalation check, fallback and exit plan |
| M7 | Voice drill | KV-cache calculation correct within 10% on the timed item; Q7.1-Q7.2 name the bottleneck with units |
| M8 | Voice drill, coding | Break-even calculation correct with assumptions stated; Q8.1-Q8.2 include utilisation and KV cache |
| M9 | Voice drill, design canvas | CI sketch with gate accepted; boundary with B5.M8 stated correctly |

Course readiness (Interview Master): final mastery 80% or more (G-7), timed task passed (at least 70% of hidden tests plus stated assumptions), design canvas rubric overall 3.5 or above with no dimension below 3 and no critical rule findings (G-7), voice mock at Senior bar (average 3.5, no competency below 3) in two sessions at least 48 hours apart (G-6), decision memos accepted. For the course badge the voice mock bar is "Meets" (average 3.0, none below 2).

### 8.11 Quality-bar checklist

- Currency: facts dated October 2026; resolved items listed in section 6; remaining [verify] count 3, each with a named check (section 6).
- Original examples (BFSI, IoT, SaaS tickets, voice routing); no copied text or named-company questions.
- Staff-level probe added: "Make the case for NOT fine-tuning and describe the evidence that would change your mind."
- Ownership respected via pointers; no generic harness, judge, release-engineering or compliance depth duplicated here.
- Track lead B review (2026-10-06): see `_reviews/TrackLead_B_Report.md`. Applied: added G-1 Ownership notes row; MoE total-vs-active parameter and 16 bytes/param Adam rule of thumb; hosted preference-tuning provider note; G-6 readiness criteria and G-7 badge/test-out bars aligned (two sessions 48 h apart, 85% retake); LF-1 course-level table, M8 diagnostic item, placement ID and count note, MQTT experience-bridge row; Q-ID mapping note; [verify] items given exact checks and SGLang/EAGLE caveat resolved; last_reviewed 2026-10-06.
