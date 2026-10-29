---
name: Skillora Learning 1.0 — ADR Register & Spike Plan
version: 1.0
date: 2026-10-05
status: Draft for founder/tech-lead decision. Recommendations are defaults, not final until an ADR is accepted.
purpose: List the architecture decisions that must be recorded before and during 1.0 build, with a recommended default, options, decision owner, deadline, and the spikes that produce the evidence
related: ../01_Scope/Skillora_Learning_Release_1_0_Web_Scope_and_Founder_Curriculum.md, ../../01_High_Level_Design/04_Engineering_Standards/Skillora_Learning_Engineering_Quality_and_Operations_Specification.md, ../05_Integration/Platform_Capability_Requirements.md, ../05_Integration/Placement_Test_Blueprint.md, ../05_Integration/Content_Data_Model_and_Import_Schema.md
billing_update: 2026-10-22 — section 2B adds ADR-036 to ADR-040 and section 3B adds spikes S18 and S19 for Billing Lite and the free trial (see ../01_Scope/Skillora_Learning_Release_1_0_Billing_Lite_and_Trial_Specification.md)
integration_update: 2026-10-05 — curriculum is final at 19 courses / 192 modules / about 294 h Deeper path; sections 2A and 3A add proposed ADR-023..035 and spikes S7..S17 derived from the curriculum (full rationale in ../05_Integration/Platform_Capability_Requirements.md section 13)
---

# 1.0 ADR Register and Spike Plan

## 1. HOW TO USE

Each decision becomes a short ADR (Context → Options → Decision → Consequences → Review date) stored with the code. Status values: **Proposed → Accepted → Superseded**. "Default" is the recommendation already stated in the Engineering spec or this document; change it only with evidence from a spike.

## 2. ADR REGISTER

| ID | Decision | Default (recommended) | Evidence needed / spike | Needed by |
|---|---|---|---|---|
| ADR-001 | **Backend shape** | Python modular monolith (Django + DRF) for core/admin; FastAPI async services for realtime voice, AI gateway, sandbox orchestrator | None; matches founder expertise | Week 1 |
| ADR-002 | **Web frontend** | TypeScript + React/Next.js; Admin as separate bundle on same design system | S6 (canvas/IDE perf) | Week 1 |
| ADR-003 | **Database and tenancy** | PostgreSQL, row-level security, tenant-ready schema, JSONB for flexible content; pgvector first | S5 (retrieval quality) | Week 2 |
| ADR-004 | **Hosting and region** | AWS ap-south-1 (Mumbai), DR ap-south-2; ECS/Fargate; Terraform | Cost model (§5) | Week 2 |
| ADR-005 | **Code/SQL sandbox technology** | Isolated microVM/gVisor-class sandbox with per-attempt disposable PostgreSQL/MySQL databases | S3 | Month 1–2 |
| ADR-006 | **Voice pipeline architecture** | Browser mic → WebSocket/WebRTC → streaming STT → interview engine → streaming TTS; barge-in; provider abstraction; text fallback | S1, S2 | Month 4 |
| ADR-007 | **STT/TTS/LLM providers (primary + fallback)** | Chosen from bake-off by accuracy on Indian English, technical-term accuracy, p50/p95 latency, cost/minute, data terms (India region, retention) | S1 | Month 4–5 |
| ADR-008 | **Interview scoring approach** | Rubric-based LLM scoring with evidence quotes + deterministic checks; SME-labelled calibration set; versioned rubrics/prompts; human sampling | S4 | Month 4 |
| ADR-009 | **Adaptive engine model** | Per-competency estimate with uncertainty + coverage constraints; rule-based fallback; fully explainable choices | S4 | Month 6 |
| ADR-010 | **System-design canvas engine** | Evaluate graph-based canvas libraries (e.g. React Flow-class vs tldraw-class vs custom); typed graph schema as the contract; text-mode accessible alternative | S2b | Month 3–4 |
| ADR-011 | **Design evaluator** | Rule engine (SPOF, missing cache/queue, idempotency, auth, observability) + LLM rubric on graph and transcript | S2b | Month 5 |
| ADR-012 | **AI Gateway** | Single gateway: provider abstraction, quotas/budgets, prompt registry, logging with PII scrubbing, caching, fallbacks | S1 | Month 2 |
| ADR-013 | **Content model and publishing** | Course → Module → Lesson with versioned drafts/review/publish; diagrams pre-rendered to SVG at publish; knowledge base as admin data | None | Month 2 |
| ADR-014 | **Question bank and company packs data model** | Purpose-tagged practice banks; company pack = versioned bundle (rounds, rubric weights, questions, themes, sources log, review date) | None | Month 3 |
| ADR-015 | **Auth** | Email + Google (OIDC), session security, MFA for admin; mobile OTP deferred | None | Week 3 |
| ADR-016 | **Entitlements and flags** | Central entitlement service with plans, quotas and credit ledger (Launch-Free, Free, Trial, Sprint, Pro 90, Pro Annual); paywall components behind flags; extended by ADR-036 to ADR-040 | None | Month 2 |
| ADR-017 | **Observability and SLOs** | OpenTelemetry, Sentry, SLO dashboards including voice latency and STT/TTS error rates | None | Month 2 |
| ADR-018 | **Cost guardrails** | Per-session and monthly budgets per user and global; Admin-visible cost per interview minute | S1 results | Month 5 |
| ADR-019 | **Data retention and audio** | Default: audio short retention then transcript only; learner-controlled; deletion on request; no voice biometrics | Counsel review | Month 3 |
| ADR-020 | **Search** | PostgreSQL full-text first | None | Month 3 |
| ADR-021 | **Release and environments** | dev/staging/prod, preview environments, GitHub Actions, feature flags, staged rollouts | None | Week 3 |
| ADR-022 | **Testing strategy for AI features** | Eval harness with labelled sets (STT accuracy, scoring agreement, canvas evaluation agreement, adaptive coverage), regression gates in CI | S4 | Month 4 |

## 2A. PROPOSED ADDITIONS FROM THE FINAL CURRICULUM (INTEGRATION, STATUS: PROPOSED)

The 19 course syllabi (192 modules, about 294 h Deeper path, routes of about 210 h Backend / 215 h AI Engineer / 250 h Hybrid) assume platform capabilities that ADR-001 to ADR-022 do not yet cover. Proposed entries (defaults and evidence in `../05_Integration/Platform_Capability_Requirements.md` section 13):

| ID | Decision | Default (recommended) | Evidence | Needed by |
|---|---|---|---|---|
| ADR-023 | Runtime catalogue, image pin manifest and example CI (Python 3.14 default, 3.13/3.15 and 3.14t selectable; PG 18, MySQL 8.4 LTS) | One manifest; CI runs every lesson example and lab solution | None | Month 1 |
| ADR-024 | LLM Lab Harness architecture and `LLMClient` contract | Record/replay plus scripted mock; shared fixture repository | S9 | Month 3 |
| ADR-025 | Voice Lab Harness and speech fixture policy | Fake streaming STT/TTS/LLM, seeded jitter; consent-cleared or synthetic audio | S10, S1 | Month 4 |
| ADR-026 | Hidden-test engine and complexity-aware grading | Test categories, op-count proxies, import allow-lists, hint ledger | S11 | Month 3 |
| ADR-027 | Placement and route engine | Rule-based v1 for M2, Bayesian module estimates v2 for M5; CI reconciliation to 210/215/250 h | S14 | Month 4 |
| ADR-028 | Curriculum import, tag/ID registries, `ref` enforcement, dated data files, release snapshots | JSON/CSV bundle per the import schema | S15 | Month 2 |
| ADR-029 | Canvas vocabulary versioning and governed rule registry (39 rules) | Semver vocabulary; rules as data with good/weak test graphs | S17, S2b | Month 3 |
| ADR-030 | Profile Facts Ledger and sensitive pay data | Learner-controlled; encrypted; session-scoped by default | Counsel review | Month 3 |
| ADR-031 | Mastery, readiness (G-6) and credential evaluation engine | Thresholds as configuration (80 module, 85 readiness) | None | Month 4 |
| ADR-032 | Publication gates (human professional review) and calibration-gated feature flags | Review state plus flags tied to calibration metrics | None | Month 3 |
| ADR-033 | Database lab engines, two-session mode, SQL IDE and plan viewer | Template-based per-attempt PG 18 and MySQL 8.4; limited-privilege roles | S8 | Month 2 |
| ADR-034 | Interactive visual pipeline (step-throughs) | Declarative spec, static fallback | S16 | Month 4 |
| ADR-035 | Cloud/devops lab approach | Static validators plus moto-style emulator; no live cloud | S7, S13 | Month 5 |

Amendments to existing entries: **ADR-005** also covers network allow-lists and the Lab Service; **ADR-009** has its placement-time inputs defined by ADR-027 and must not block placement v1; **ADR-013** is extended by ADR-028 (Simpler/Deeper variants, pointer/shared lessons, release snapshots); **ADR-016** gains the curriculum flags listed in the capability document section 12.1; **ADR-022** gains the labelled sets in section 11 of that document.

## 2B. BILLING LITE AND TRIAL (CONFIRMED DIRECTION 2026-10-22; STATUS: PROPOSED UNTIL ACCEPTED)

Source: `../01_Scope/Skillora_Learning_Release_1_0_Billing_Lite_and_Trial_Specification.md`.

| ID | Decision | Default (recommended) | Evidence | Needed by |
|---|---|---|---|---|
| ADR-037 | **Plans, passes and quota model** | Entitlement service holds plan, window, per-feature quotas and a credit ledger; passes queue; monthly quota reset; prices and quotas in the Admin price book, versioned; existing passes keep their terms | None | Month 2 |
| ADR-038 | **Free trial policy and abuse controls** | 7 days from first activity, +7 on activation (placement test, one lesson, one mock); one trial per verified identity and device; disposable-email block; IP rate limits; no card | S19 | Month 4 |
| ADR-039 | **AI budget pools and cost caps** | Separate trial and paid pools at the AI Gateway; per-learner and per-session caps; degrade order (cheaper model, shorter output, text fallback, block); waitlist when the trial pool is exhausted; kill switch | S19 | Month 4 |
| ADR-040 | **GST, invoicing and refunds** | Tax-inclusive prices at 18%; PDF invoices with GSTIN fields and number series; admin-initiated refunds through the gateway under a stated policy; accountant and counsel sign-off before first sale | Accountant and counsel review | Month 4 |


## 2C. PUBLIC PROFILE LITE (CONFIRMED DIRECTION 2026-10-22; STATUS: PROPOSED UNTIL ACCEPTED)

Source: `../01_Scope/Skillora_Learning_Release_1_0_Public_Profile_Lite_Specification.md`.

| ID | Decision | Default (recommended) | Evidence | Needed by |
|---|---|---|---|---|
| ADR-041 | **Public profile, share images and embeds** | Server-rendered profile at `/u/{handle}` cached at the edge; opt-in versioned consent; section-level visibility enforced in one place for page, image, embed and JSON; `noindex` by default; on-demand preview images and SVG badges cached; verification pages as the single source of truth; no recruiter-facing API or search; moderation queue and takedown | S20 | Month 8 |

Amendment: **ADR-031** (credential engine) exposes verified credentials to the profile; **ADR-019** (retention) covers profile data deletion and cache purge within 5 minutes.

## 2D. AUDIENCE AND ELIGIBILITY (CONFIRMED DIRECTION 2026-10-22; STATUS: PROPOSED UNTIL ACCEPTED)

Source: `../01_Scope/Skillora_Learning_Release_1_0_Audience_and_Eligibility_Policy.md`.

| ID | Decision | Default (recommended) | Evidence | Needed by |
|---|---|---|---|---|
| ADR-042 | **Age and eligibility gate** | Adult-only (18+, Higher Secondary completed): date of birth checked once and discarded (keep birth year and an 18+ flag), fixed education list, signed eligibility statement at sign-up and checkout, no data kept for declined attempts, underage detection and deletion runbook; **no ID, selfie or video collection in 1.0; age assurance by declaration, automatic plausibility checks, reconfirmation and fast removal; an optional consented digital age check (age flag only, never documents) only if S21 and counsel clear it**; no guardian or parental-consent flows anywhere | Counsel review of wording and reasonable-steps standard | Month 2 |

Amendment: **ADR-015** (auth) and **ADR-016** (entitlements) require the eligibility flag before any account, trial or purchase; **ADR-019** (retention) covers 7-day deletion of underage accounts.

## 3. SPIKE PLAN

| ID | Spike | Question answered | Time-box | Exit criteria |
|---|---|---|---|---|
| **S1** | **Voice provider bake-off** | Which STT/TTS/LLM combination meets latency and accuracy on Indian English and technical terms within budget? | 2 weeks | Test set of ≥ 100 utterances across accents, with technical vocabulary (FastAPI, asyncio, pgvector, Kubernetes, idempotency…); measure WER, term accuracy, p50/p95 end-of-speech → first audio, barge-in behaviour, cost per minute, data/region terms; pick primary + fallback; document in ADR-007 |
| **S2** | **Realtime session prototype** | Does the browser voice loop feel natural? | 2 weeks (after S1) | Working mic → STT → LLM → TTS loop with barge-in, live transcript, reconnect; p50 ≤ ~1.5 s on typical broadband; text fallback demonstrated |
| **S2b** | **Canvas prototype** | Which canvas approach supports rich diagrams, typed graph export, accessibility and good performance? | 2 weeks | Palette, connectors, groups, graph export, undo; 200-node stress test; screen-reader/text-mode alternative |
| **S3** | **Sandbox hardening** | Can untrusted Python and SQL (PostgreSQL + MySQL) run safely and cheaply with per-attempt databases? | 3 weeks | Resource limits, no network, fork-bomb/mem/time tests, startup < target, cost per run, escape-attempt tests |
| **S4** | **Scoring calibration** | Does LLM rubric scoring agree with SME judgement? Does the adaptive engine cover must-have areas? | 3 weeks | ≥ 50 SME-labelled answers across Tracks A/B/C; agreement metric target set with SMEs; variance across repeated runs measured; prompt/rubric versioning in place |
| **S5** | **Retrieval and tutor quality** | Is pgvector sufficient for tutor and content search at expected scale? | 1 week | Recall@k on a labelled query set; latency; fallback plan |
| **S6** | **Web performance baseline** | Do IDE + canvas + player meet web budgets on mid-range laptops/phones in a browser? | 1 week | Budgets from Engineering spec §3.2 applied to web |

## 3A. PROPOSED SPIKES FROM THE FINAL CURRICULUM (STATUS: PROPOSED)

The boss matrix names its spikes S-1 to S-7; they are renamed here to continue the register numbering and avoid a clash with S1 to S6.

| ID | Spike | Time-box | Exit criteria | Boss ID |
|---|---|---|---|---|
| **S7** | AWS emulation: moto-style versus LocalStack | 1 week | Feature and licence matrix; default chosen before A7.M4 and M5 labs are authored | S-1 |
| **S8** | Two-session database mode and lock visualisation | 2 weeks | Scripted interleaving for PG and MySQL, lock-wait and deadlock views, cost per session | S-2 |
| **S9** | LLM Lab Harness build and tokeniser vocabulary licence check | 2 weeks | Record/replay, scripted mock with faults and streaming, reasoning-state round trip; B2 labs run; vocabulary redistribution answered | S-3 |
| **S10** | Indian-English technical speech fixture set | 1 week, alongside S1 | At least 100 utterances, consent and licence documented, shared with S1 | S-4 |
| **S11** | Hidden-test engine: categories, op-count proxies, import allow-lists | 2 weeks | O(n log n) versus O(n^2) separated on sandbox hardware; per-category catch rate; allow-list enforced | S-5 |
| **S12** | Lab Service legal and security review | 1 week | Go or no-go; fallback tasks defined | S-6 |
| **S13** | Static validators for Kubernetes, Terraform, IAM, CI YAML | 2 weeks | Validators run against fixtures; known-bad corpus detected | S-7 |
| **S14** | Placement calibration and route reconciliation | 2 weeks after the 3-learner pilot | Prior strength, weights, thresholds and time boxes fixed; all-study routes within 10 percent of 210/215/250 h | new |
| **S15** | Curriculum import dry-run | 1 week | All 19 syllabi parse to the bundle; ID and tag normalisation report; volume ledger matches (192 modules, 6,336 bank targets) | new |
| **S16** | Mermaid-to-SVG and interactive visual pipeline | 1 week, with S6 | Publish-time rendering with alt text; three interactive prototypes; web budgets met | new |
| **S17** | Rule engine on the typed graph | 2 weeks after S2b | 39 registry rules evaluated on 78 test graphs; stated-non-goal suppression; evidence links | new |

### 3B. BILLING LITE AND TRIAL SPIKES (ADDED 2026-10-22)

| ID | Spike | Time-box | Exit criteria |
|---|---|---|---|
| **S18** | Payment gateway selection and checkout prototype | 1 week | Compare 2 to 3 Indian gateways on fees, UPI success rate, settlement time, refund API, webhook quality, onboarding requirements (entity, KYC) and test mode; hosted checkout end to end in test mode with a signed, idempotent webhook granting an entitlement; return-from-UPI-app recovery works; gateway chosen and ADR-036 updated |
| **S20** | Profile pages, share images and embeds | 3 to 4 days, with S6 | Edge-cached server-rendered profile meets LCP under 2.5 s on 4G; on-demand 1200×630 image generation cached with a cost per 1,000 images measured; SVG badge renders in GitHub and two email clients; one privacy-switch change purges cached page, image and embed within 5 minutes; ADR-041 updated |

Suggested placement (relative weeks): S18 weeks 8 to 9 (before Billing Lite build at M4); S19 immediately after S1 and again in the paid beta; S15 weeks 2 to 3; S8 weeks 2 to 6 beside S3; S9 and S11 weeks 3 to 8; S10 with S1; S7 and S13 weeks 8 to 10 (before A7 authoring); S16 with S6; S17 immediately after S2b; S14 after the pilot (about week 12 onward).

## 4. SEQUENCING

```mermaid
gantt
    title Spike and ADR sequencing (relative weeks)
    dateFormat  X
    axisFormat  W%s
    section Decisions
    ADR-001..004, 015, 021 (stack, hosting, auth, CI/CD)   :0, 3
    section Early spikes
    S3 Sandbox hardening                                   :2, 5
    S1 Voice provider bake-off                             :4, 6
    S6 Web performance baseline                            :4, 5
    section Voice and design
    S2 Realtime session prototype                          :6, 8
    S2b Canvas prototype                                   :8, 10
    S4 Scoring calibration                                 :10, 13
    S5 Retrieval and tutor quality                         :10, 11
    section Gate
    ADR review: accept or revise all spike-dependent ADRs  :13, 14
```

The voice bake-off moves earlier than the roadmap's month 4–5 if the founder wants to practise by voice sooner; it needs only the AI Gateway skeleton.

## 5. COST MODEL TO PRODUCE (INPUT TO ADR-018)


## 6. DEFINITION OF DONE FOR THIS KICKOFF

- [ ] ADR-001–004, 015, 016, 021 accepted.
- [ ] S1 and S3 complete with written results.
- [ ] Provider primary/fallback chosen; per-minute cost known.
- [ ] A cost cap policy is configured as a default.
- [ ] Spike findings updated in the Production Readiness Review risk list.
- [ ] Proposed ADR-023, ADR-028 and ADR-033 reviewed and accepted or revised by Month 2; S15 (curriculum import dry-run) and S8 started.
- [ ] Billing Lite: ADR-036 and ADR-037 reviewed by Month 3, ADR-038 to ADR-040 by Month 4; S18 and S19 complete; gateway chosen; accountant and counsel engaged.
- [ ] Proposed ADR-024, ADR-026, ADR-029, ADR-030 and ADR-032 reviewed by Month 3 (before Track B authoring and Track A coding labs depend on them).
