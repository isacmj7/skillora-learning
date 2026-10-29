---
name: B6 — Real-time and Voice AI Systems
course_id: B6
track: B (AI Engineer)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-17 (see _reviews/Boss_Final_Approval.md)
---

# B6 — Real-time and Voice AI Systems (Track B)

Version 2.0 (wave-2 upgrade). Last reviewed: 2026-10-06; all version-sensitive facts are "as of October 2026". Fast-moving content (provider and model names, pricing, speech-to-speech APIs, frameworks): review every 3 months. Lessons use tier language and dated Provider Notes, not model names.

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | B6 — Real-time and Voice AI Systems / Track B (AI Engineer Mastery) |
| Audience | Senior Python backend engineers moving to AI Engineer roles; also L1-L4 learners generally. The reference learner has built a production-style voice agent, so for that learner the course turns lived experience into structured, defensible, interview-ready knowledge (vocabulary, numbers, trade-offs, failure modes) using "skim and defend" bands and the Defend-your-numbers mode (see section 8). Goal tags: Senior Python Backend `NICE`, AI Engineer `SHOULD`, hybrid `SHOULD`; `SHOULD` in the AI-startup company pack and `NICE` in the other cluster packs (Company_Pack_to_Curriculum_Map; effective tag is the max of role and cluster tag) `[judgement]` |
| Prerequisites | A2 (async, M2-M5 and M7) and B2 (LLM application basics incl. streaming, B2.M1-M8) are required (solid edges A2 to B6 and B2 to B6), HTTP/WebSocket familiarity. Placement skip: per module, using G-5 bands (>= 85 skip lessons and take the mastery check, 70-84 skim, 50-69 study, < 50 deep-study); no band waives the course final assessment |
| Outcomes (measurable) | 1. Draw and defend a cascaded (VAD, STT, LLM, TTS) vs speech-to-speech architecture and choose between them for a stated requirement set (`voice.architecture`). 2. Build a latency budget for a voice turn, name where latency hides, and propose measured fixes (`voice.latency`). 3. Choose WebSocket vs WebRTC vs SIP/PSTN ingress for a scenario and explain codecs, sample rates and framing consequences (`voice.transport`). 4. Explain VAD, endpointing, turn-taking and barge-in, including correct conversation-state truncation after an interruption (`voice.turn-taking`). 5. Design for Indian-English and multilingual speech: custom vocabulary, code-switching, evaluation sets, WER/CER (`voice.stt-quality`). 6. Design reliable, scalable stateful real-time services: reconnect, resume, text fallback, draining, autoscaling on sessions (`voice.reliability`). 7. Model cost per minute, set budgets and caps, and apply privacy, consent and security controls to audio (`voice.cost-privacy`). 8. Define an evaluation and test strategy (WER, latency percentiles, task success, simulated callers, regression) for a voice system (`voice.eval`) |
| Estimated effort | **15.5 h Simpler / 10.5 h Deeper** module time, plus about 2.0 h assessment allowance (final mastery check, voice mock, revision loops). Simpler = L1-L2 route with full scaffolding; Deeper = L3-L4 route, hours net of typical skip-ahead, so shorter in wall-clock but harder per hour. Per-module hours: M1 1.25/0.75, M2 1.25/0.75, M3 2.0/1.75 (includes the Challenge lesson: 1.5 h is its time cap, budgeted at about 0.5 h Deeper and 0.75 h Simpler `[judgement]`; the rest is optional off-route study, `B:N A:N H:N`), M4 1.5/1.0, M5 1.5/1.0, M6 1.5/1.0, M7 1.25/0.75, M8 1.5/1.0, M9 1.25/0.75 (includes the 20-minute compliance awareness lesson), M10 1.25/0.75, M11 1.25/1.0 (S/D) |
| Interview coverage tags | `voice.architecture`, `voice.latency`, `voice.transport`, `voice.turn-taking`, `voice.stt-quality`, `voice.tts`, `voice.telephony`, `voice.reliability`, `voice.cost-privacy`, `voice.eval` (B6 owns these tags and issues credit). Secondary only (no credit): `design.ai-systems` (voice platform design is owned by A6 library #17), `project.tradeoffs` (C2) |
| Badge and certificate | Module badges M1–M11 (each: mastery check at or above 80% plus the module practice artefact). Course badge "Voice AI Systems Engineer": all module badges, final assessment at or above 80%, the latency-budget workbook and the interview-voice-agent case study submitted, and a voice mock round at "Meets" (average >= 3.0, none < 2) with evidence quotes. Interview-ready on a topic uses the G-6 Senior bar (section 8). Certificate states scope and date, no hiring claims |
| Authoring owner / reviewer | Owner: Track B curriculum lead (B6 SME author role). Reviewers (roles to be named by Content Lead before publish): voice/telephony engineer for SME technical review; compliance reviewer for lesson 7.5 (gated); Interview Master voice team for fixtures. Track lead B review 2026-10-06 done |
| Review cadence | Every 3 months (providers, models, pricing, browser audio APIs); telephony and audio fundamentals content every 12 months |

### Practice environment note
The IDE runs Python only, so labs use the platform **Voice harness (P-08)** plus the shared LLM Lab Harness (P-07; start with orientation lesson X5 in B5.M1): audio fixtures (including Indian-English technical speech; consent-cleared speakers or synthetic speech only, owned by the Interview Master/voice team per R-B6-2, no learner audio), a fake streaming STT/TTS/LLM with configurable latency and jitter, an asyncio WebSocket server skeleton (FastAPI) and a metrics collector. **Harness-only in 1.0 (R-B6-1): no bring-your-own keys.** There is no real WebRTC or telephony in the sandbox. Every lab and number carries a label: **SIMULATED** (harness-generated, an assumption not a vendor claim) or **REAL** (taken from a documented real-world measurement, for example protocol arithmetic). Latency numbers shown in lessons are harness-generated values, never provider benchmarks. Design exercises use the canvas and cite registry rule IDs only (for example R-VOICE-1), never local rules.

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice tasks | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Voice agent anatomy: cascaded vs speech-to-speech | Choose and justify an architecture for stated requirements | Pipeline stages (capture, VAD, STT, dialogue/LLM, TTS, playback); cascaded vs native speech-to-speech (e.g. realtime audio-in/audio-out model APIs) trade-offs: latency, controllability, tool use, transcript/audit, cost, voice choice, language coverage; hybrid (speech-to-speech for conversation, cascaded for control); orchestration frameworks (for example Pipecat, LiveKit Agents; versions in a dated Provider Note) vs hand-rolled; "voice is a streaming dataflow with cancellation", not request/response | Pipeline dataflow diagram with cancel signals; decision matrix for cascaded vs S2S | Design exercise: pick architecture for a support bot, an interview coach and a phone IVR replacement; list what you lose with S2S | Treating voice as chat plus TTS; ignoring cancellation paths; saying S2S is always better or cascaded is obsolete; no mention of transcripts needed for scoring/audit |
| M2 | Audio fundamentals: capture, formats, codecs | Reason about sample rate, bit depth, framing, codecs and browser capture without guessing | PCM, sample rate (8/16/24/48 kHz), 16-bit, mono, bitrate arithmetic; frames (20 ms typical); Opus (WebRTC, wideband) vs G.711 mu-law/A-law (telephony, 8 kHz) vs AAC/MP3; resampling and its quality cost; browser capture: `getUserMedia`, `AudioWorklet` for raw PCM vs `MediaRecorder` (container chunks, not frame-aligned, higher latency); echo cancellation, noise suppression, auto gain; endianness, base64 overhead (about 33%) | Waveform to PCM to frames to codec diagram; bitrate calculator | Lab: convert fixture audio between 8 kHz mu-law and 16 kHz PCM, measure STT WER impact; compute bandwidth for 1000 concurrent sessions | Confusing sample rate with bitrate; sending MediaRecorder webm chunks to an STT expecting raw PCM; ignoring resampling; not knowing telephony is 8 kHz narrowband and why that hurts accuracy |
| M3 | Transport and streaming architecture | Select and implement WebSocket vs WebRTC vs SIP ingress and explain backpressure | WebSocket (TCP, ordered, head-of-line blocking, simple, server-side control); WebRTC (UDP/SRTP, DTLS, ICE/STUN/TURN, jitter buffer, packet-loss concealment, built-in AEC, SFU, adaptive bitrate); when each wins; **Challenge lesson (3.6): WebRTC internals at interview depth (ICE candidates, STUN vs TURN, SDP offer/answer, SFU vs MCU), at most 1.5 h, tagged `B:N A:N H:N`**; WebTransport (HTTP/3) now Baseline in all major browsers since Safari 26.4 (March 2026, see freshness notes); media server vs direct-to-provider streaming; SSE for text/LLM stream (protocol choice owned by A5.M9); asyncio design (mechanics owned by A2: here only the audio-specific application): producer/consumer queues, bounded queues, task cancellation, one task per direction; framing (binary frames, sequence numbers, timestamps); connection lifecycle (heartbeat, auth token in first message or short-lived URL token, not long-lived key in browser) | WebSocket vs WebRTC comparison table; asyncio task graph for one session; ICE connectivity flow | Lab: build FastAPI WebSocket duplex audio relay with bounded queues and cancellation; inject jitter and measure buffer growth. Design: when to add TURN and what it costs | "WebRTC is always lower latency" or "WebSocket is fine for everything" with no reasoning; unbounded queues; blocking calls in the event loop; leaking provider API keys to the browser; no heartbeat or reconnect story |
| M4 | VAD, endpointing, turn-taking and barge-in | Implement and reason about end-of-turn detection and interruptions | Energy-based vs model-based VAD (e.g. a small neural VAD versus the classic WebRTC VAD; specific model names live in the dated Provider Note); thresholds, hangover, pre-roll buffer so first syllable is not clipped; endpointing: silence timeout vs semantic/turn-detection models; trade-off between cutting users off and adding dead air; thinking pauses in interview answers (needs longer patience than customer support); barge-in: detect speech during playback, stop playback and flush TTS buffers, cancel LLM and TTS generation, **truncate assistant message in history to what was actually played**; echo problem: agent hearing itself (AEC, headphones, mute-while-speaking trade-offs); backchannels ("mm-hmm") and false interruptions (cough, noise); half-duplex vs full-duplex; commands such as repeat, slower | Turn-taking timeline with overlaps; barge-in sequence diagram; VAD threshold trade-off curve | Lab: implement a state machine (listening, user-speaking, thinking, agent-speaking) with VAD events; replay fixtures with pauses and interruptions and measure false cut-offs and missed barge-ins | Not truncating context after barge-in (model "remembers" saying things the user never heard); single fixed silence threshold for all contexts; ignoring echo; cancelling the LLM but not the TTS or playback queue; no pre-roll |
| M5 | STT and TTS in depth, Indian English and multilingual | Select, tune and evaluate STT/TTS for accents, jargon and languages | Streaming vs batch STT (interim vs final hypotheses, stability, word timestamps, diarisation); non-streaming (batch-oriented) models need chunking tricks; custom vocabulary: provider hotwords/keyterm boosting, phrase hints, post-STT correction with LLM or fuzzy lexicon, prompt biasing; technical terms (FastAPI, asyncio, Kubernetes, pgvector, "kube-ctl" vs "kubectl"); Indian-English accent variation, code-switching (Hinglish and other Indic mixing), Indic-language models and providers [verify: leaders are settled only by the fixture-set bake-off, not vendor claims]; telephony-quality audio; numbers, spelled-out letters, emails; TTS: streaming synthesis, time to first byte, sentence/clause chunking for LLM stream, prosody, SSML, pronunciation lexicons (acronyms, "SQL", "nginx"), voice cloning consent and ethics, text normalisation (numbers, dates, rupee amounts), language/voice selection | STT hypothesis timeline (interim to final); pipeline LLM-token-to-TTS chunking diagram; accent test matrix | Lab: build a technical-term lexicon and post-correction step, measure WER and term-recall on fixture set before/after; implement sentence-boundary chunker feeding fake TTS | Reporting a single WER without per-accent and per-term breakdown; assuming a vendor's headline WER holds on Indian-English telephony audio; treating interim transcripts as final; feeding whole LLM answer to TTS before speaking; ignoring text normalisation |
| M6 | Latency budgets: where latency hides | Build, measure and cut an end-to-end latency budget | Definition: user stops speaking to first agent audio byte heard; components: endpointing wait (often the largest and most overlooked), STT finalisation, network RTTs, LLM time to first token, first-sentence accumulation, TTS time to first byte, audio jitter buffer and device playback; budgets (the lab target is an assumption, for example 1.5 s p50 and 3 s p95, set per scenario in the workbook; all hop values harness-generated and labelled SIMULATED); percentiles not averages, tail latency, cold starts; techniques: streaming everywhere, speculative LLM start on interim transcripts (with cancellation), prompt caching, smaller or faster model for first reply, filler/acknowledgement audio ("let me think"), co-location of regions, connection reuse/keep-alive warm pools, avoid sync DB calls on the hot path, pre-fetch context, TTS first-chunk sizing; measurement: per-turn timestamps at every hop, tracing, synthetic probes; perceived vs real latency ("thinking" UI) | Stacked latency waterfall diagram; p50 vs p95 histogram; speculative-start sequence | Lab: instrument the harness with spans, produce a waterfall, hit a stated p95 target by choosing among provided optimisations and justify with numbers (latency-budget workbook) | Quoting LLM latency only; averages instead of percentiles; forgetting endpointing delay and playback buffering; optimising before measuring; speculative execution without cancellation or cost awareness; ignoring network distance between browser, server and providers |
| M7 | Telephony basics: SIP, PSTN, gateways | Explain how a phone call reaches a voice agent and what changes versus web voice | PSTN, SIP trunking, SIP signalling (INVITE, 180/200 OK, ACK, BYE) vs RTP media; SDP offer/answer and codec negotiation; G.711 (PCMU in North America/Japan, PCMA in India and Europe) and narrowband 8 kHz consequences; DTMF (RFC 4733 events vs in-band); SBC and gateways; CPaaS providers and Indian telecom providers [verify: Indian CPaaS and carrier specifics need current vendor documentation]; WebSocket media streams from CPaaS (e.g. mu-law 8 kHz frames) into the agent; call transfer (SIP REFER or warm transfer), hold, voicemail/answering-machine detection; jitter and packet loss on PSTN legs; concurrency limits and per-minute carrier cost; India context: voice compliance awareness lesson (7.5, at most 20 min, awareness not legal advice; pointer to X1): recording disclosure, TRAI telemarketing and DLT scope, AI-call disclosure, DPDP consent `[verify: TRAI rules are reported inconsistently by vendor blogs and the primary TRAI text was not read; see freshness notes]` | SIP call ladder diagram; web vs phone architecture side by side | Design exercise: add inbound phone support to a web voice agent (media path, codecs, transfer, DTMF, cost). Lab: decode mu-law stream into PCM and feed the pipeline | Thinking SIP carries audio (it signals; RTP carries media); ignoring 8 kHz limits on STT accuracy; no plan for DTMF or human handoff; no consent/recording disclosure; assuming web voice code works unchanged on phone |
| M8 | Reliability, error recovery and scaling stateful real-time services | Design failure handling and scale for long-lived sessions | Failure catalogue: mic permission denied/revoked, device change, network drop, provider timeout/5xx, rate limit, LLM stall, TTS failure, server restart/deploy; degradation ladder: retry with backoff, secondary provider, switch voice to text, resume at last question; session state externalised (Redis/DB) with session ID, idempotent resume, autosave; reconnect with sequence numbers and replay of missed events; heartbeats, timeouts per stage, circuit breakers, watchdog for "silence forever"; scaling: sessions are stateful and long-lived, sticky routing or session-affinity vs stateless workers with external state; autoscale on concurrent sessions and per-worker CPU/audio-loop lag, not request rate; graceful drain on deploy (stop new sessions, finish or migrate old), connection storms after outage, regional deployment, capacity planning (sessions per core), noisy-neighbour isolation, per-tenant quotas, load-test with synthetic callers | Degradation ladder diagram; session state machine with reconnect; scaling topology with drain | Lab: add reconnect/resume with sequence numbers to the WebSocket harness; chaos test with scripted drops. Design: capacity plan and deployment strategy for 2000 concurrent sessions | Treating voice sessions as stateless HTTP; scaling on RPS; killing pods mid-call on deploy; no text fallback; retry storms; losing user progress on reconnect |
| M9 | Cost, privacy, consent and security for voice | Model per-minute cost and apply privacy and security controls | Cost per minute = STT + LLM tokens + TTS characters/seconds + transport/media server + telephony + storage + compute; cascaded vs S2S cost curves; silence billing, idle sessions, max duration; cost levers (model routing, caching, shorter replies, endpointing, VAD gating so silence is not streamed to STT); budgets, quotas and kill switches at the gateway; admin-visible cost/minute; privacy (voice-specific consent only; the compliance primer is X1, owner A8.M9.4, and AI governance is B5.M9): explicit mic and recording consent, retention default short then transcript only, deletion on request, no voice biometrics, no emotion/personality inference, minimisation, encryption in transit (DTLS-SRTP, TLS) and at rest, access controls and audit for recordings, PII in transcripts, redaction, provider data-processing terms and zero-retention options [verify: check each provider's current data-processing terms], children and sensitive data; DPDP consent as it applies to audio (awareness, pointer to X1); security: token minting for browser sessions, origin checks, abuse and toll fraud on telephony, prompt injection via speech ("ignore your instructions" spoken or in audio played near mic), voice-clone/deepfake risk and spoofing, rate limits, TURN credential expiry | Cost stack waterfall; data-flow diagram with retention boundaries | Workbook: compute cost per minute and per 30-minute session for two architectures and set caps; design a consent and retention flow; threat model a voice endpoint | Quoting only LLM cost; no cap on session length; recording by default; storing raw audio forever; voice biometrics or emotion inference; long-lived provider keys in the browser |
| M10 | Evaluation and testing of voice systems | Define metrics and a test pyramid that catches voice regressions | Voice metrics owned here: per-stage latency percentiles, interruption success and false-barge-in rate, endpointing errors (cut-off rate, dead-air), WER sampling in production; LLM observability, traces and drift are a pointer to B5.M7, generic OTel and SLOs a pointer to A7.M9. WER (substitutions plus deletions plus insertions over reference words), CER for Indic/short tokens, term-recall for jargon, normalisation before scoring, per-accent/per-condition slices; latency percentiles per hop; interruption success and false-barge-in rate; endpointing errors (cut-off rate, dead-air); TTS quality (MOS-style human rating, pronunciation errors, intelligibility); task success/completion, turn count, escalation rate; conversational quality via rubric (LLM-as-judge: pointer to B5); audio-in test fixtures and golden transcripts; simulated callers (scripted TTS callers plus LLM-driven personas) with noise, accents, silence and interruptions; component tests with fake providers, contract tests per provider adapter, soak and load tests, canary and shadow comparison of providers; production monitoring (drop rate, reconnect rate, p95 latency, fallback rate, cost/min); bake-off methodology for choosing providers on accuracy, latency, barge-in, cost | Test pyramid for voice; evaluation dashboard mock; bake-off scorecard | Lab: compute WER with normalisation on fixtures, produce per-slice report, write a simulated-caller regression test that fails when p95 exceeds budget | One WER number as "accuracy"; evaluating on clean read speech; no normalisation (case, numerals, punctuation); judging task success by vibe; testing only the happy path; no regression suite when swapping providers |
| M11 | Latency-budget workbook and generic interview-voice-agent case study | Apply the course to a generic "interview voice agent" case and defend the numbers | Package the M6 latency-budget workbook (harness-generated hop values, assumptions labelled) with a cost model and failure ladder; generic interview-voice-agent case study built only from public-domain requirements (pre-flight check, live transcript, question state machine between STT and LLM, provider abstraction, barge-in cancel, text fallback on budget breach, resume at last question, consent defaults, per-session cost cap, strict vs coaching mode, long thinking pauses); requirement-to-module traceability M1-M10; what a bake-off must measure; **Defend-your-numbers mode (L4)**: five-level "why" on your own voice-agent decision and on every workbook number; story card from a real project. No unreleased product internals (R-B6-4); product specifics may be added later as an admin-managed note after sign-off. Platform-level voice design (canvas, scale to thousands of calls) is **A6 library #17** and is a pointer only | Annotated generic voice pipeline with latency and failure overlays; requirement-to-module traceability table | Workbook submission (budget, failure ladder, cost, eval plan, consent flow); then the five-level defence drill | Describing the product without numbers; no failure story; claiming results not measured; ignoring consent; unable to say what you would change |
| Own | Ownership notes | - | **OWNS:** voice pipeline technology, VAD, barge-in, transport choice for audio, STT/TTS quality, telephony, voice reliability, voice evaluation and metrics (O-89), latency method and workbook (O-90). **POINTS:** asyncio mechanics, TaskGroup, cancellation -> A2.M2-M5 and A2.M7; SSE vs WebSocket protocol choice -> A5.M9; LLM streaming, 429 handling, prompt caching, token budgets -> B2.M8-M9; cost template (tokens) -> X3 B2.M9; LLM observability -> B5.M7; generic OTel and SLOs -> A7.M9; compliance primer -> X1 (A8.M9.4); AI governance -> B5.M9; LLM-as-judge -> B5; tool fan-out in agents -> B4.M5; voice platform design and canvas -> A6 #17 | - | - | - | - |

## 3. Per-module detail blocks

Question-bank targets per module (template minimums, IDs `Q-B6-Mn-nn`): 12 MCQ, 6 code-output/predict-the-result (here: predict the pipeline/log output, queue state or WER result from a short Python snippet or trace), 4 harness or calculator tasks with hidden tests in the coding bucket (G-10), 8 verbal questions with follow-up trees (3 seeded below), 3 scenario/design questions. Placement diagnostics (5-8 per module) are drawn from the MCQ/predict buckets (section 8). Every lab states SIMULATED or REAL. Mastery check: 6–10 items, 80% pass, revision loop on misses. Module badge: mastery check passed plus the named practice artefact submitted.

**M1 Voice agent anatomy**
- Lessons: (1) The streaming dataflow view of a voice agent; (2) Cascaded pipeline stage by stage; (3) Speech-to-speech models and what they change; (4) Choosing and hybrid designs; (5) Frameworks vs hand-rolled orchestration. Simpler: one reference pipeline and a checklist. Deeper: S2S limitations (tool calling control, transcript fidelity, evaluation, vendor lock-in), hybrid patterns, cancellation propagation.
- Mastery focus: stage responsibilities, S2S vs cascaded trade-offs, what a transcript is needed for. Badge: architecture decision memo for three scenarios.

**M2 Audio fundamentals**
- Lessons: (1) PCM, sample rate, bit depth, bitrate arithmetic; (2) Frames, chunking and buffering; (3) Codecs: Opus, G.711, compressed formats; (4) Browser capture with AudioWorklet vs MediaRecorder; (5) Echo cancellation, noise suppression, gain. Simpler: rules of thumb and a conversion cheat sheet. Deeper: resampling artefacts, packet-loss concealment, effect of narrowband on STT.
- Mastery focus: arithmetic, format mismatches, capture API choice. Badge: working converter plus bandwidth calculation sheet.

**M3 Transport and streaming**
- Lessons: (1) WebSocket duplex audio; (2) WebRTC essentials: ICE, STUN/TURN, SRTP, jitter buffer; (3) Choosing transport and the media-server option; (4) audio-session design applying A2 mechanics: queues, cancellation, backpressure; (5) Auth and session security for streams; (6) **Challenge: WebRTC internals** (ICE candidates and pairs, STUN vs TURN, SDP offer/answer, SFU vs MCU; at most 1.5 h; `B:N A:N H:N`). Simpler: decision table. Deeper: head-of-line blocking analysis, TURN cost, QUIC/WebTransport (Baseline since March 2026).
- Mastery focus: reasoned transport choice, backpressure, security. Badge: relay server with bounded queues and clean cancellation.

**M4 VAD and turn-taking**
- Lessons: (1) VAD types and thresholds; (2) Endpointing strategies and semantic turn detection; (3) Barge-in mechanics and context truncation (including provider-held history in speech-to-speech APIs); (4) Echo and false interruptions; (5) Designing turn policy per use case. Simpler: state machine walk-through. Deeper: tuning with labelled fixtures, full-duplex models, backchannel handling.
- Mastery focus: correct barge-in sequence, history truncation, endpointing trade-offs. Badge: state machine passing the interruption fixtures within error limits.

**M5 STT and TTS**
- Lessons: (1) Streaming STT behaviour: interim vs final, timestamps; (2) Custom vocabulary and post-correction; (3) Indian-English and code-switching considerations; (4) Streaming TTS: chunking, SSML, pronunciation, normalisation; (5) Provider selection criteria and abstraction. Simpler: vocabulary list technique. Deeper: bias/prompt conditioning, Indic-language models, per-slice evaluation design, voice-cloning consent.
- Mastery focus: hotword vs post-correction, interim handling, chunking. Badge: measured WER/term-recall improvement report.

**M6 Latency budgets**
- Lessons: (1) Defining and measuring end-to-end voice latency; (2) The budget table and where each hop spends time; (3) Hidden latency: endpointing, buffering, cold starts, tail; (4) Optimisation techniques with trade-offs; (5) Perceived latency and UX. Simpler: stacked budget template. Deeper: speculative start economics, percentile maths, regional placement, queueing delay under load.
- Mastery focus: reading a waterfall, choosing the right fix. Badge: latency-budget workbook meeting the p50/p95 target with evidence.

**M7 Telephony**
- Lessons: (1) PSTN, SIP trunks and gateways; (2) SIP signalling vs RTP media, SDP; (3) G.711 and 8 kHz consequences; (4) DTMF, transfer, voicemail detection; (5) = B6.M7.5 voice compliance awareness (at most 20 min; recording disclosure, TRAI/DLT scope, AI-call disclosure, DPDP consent; pointer to X1; gated for human read); (6) Phone vs web architecture. Simpler: call flow narrative. Deeper: SBCs, NAT traversal, carrier quality, REFER vs bridged transfer.
- Mastery focus: SIP vs RTP roles, codec effects, handoff. Badge: phone-extension design with media path, handoff and cost.

**M8 Reliability and scaling**
- Lessons: (1) Failure catalogue for voice sessions; (2) Degradation ladder and text fallback; (3) Session state, reconnect and resume; (4) Scaling stateful sessions, draining deploys; (5) Capacity planning and load testing. Simpler: ladder template. Deeper: connection storm mitigation, session migration, multi-region, queueing and admission control.
- Mastery focus: resume correctness, scale signals, drain. Badge: reconnect/resume implementation plus capacity plan.

**M9 Cost, privacy, security**
- Lessons: (1) Cost per minute model; (2) Budget, quota and kill-switch design; (3) Consent, retention and deletion; (4) Threat model for voice endpoints; (5) Pointer only: regulatory awareness lives in M7.5 and X1 (no duplicate lesson). Simpler: cost workbook with fixed rates. Deeper: sensitivity analysis, provider zero-retention options, audio PII redaction, deepfake and spoofing.
- Mastery focus: complete cost stack, consent defaults, token handling. Badge: cost workbook plus consent/retention flow plus threat model.

**M10 Evaluation and testing**
- Lessons: (1) WER, CER and normalisation done right; (2) Voice metrics: per-stage latency percentiles, barge-in and false-barge-in rate, endpointing errors, production WER sampling (LLM observability: pointer to B5.M7; generic OTel: A7.M9); (3) Task success and conversation quality; (4) Simulated callers and regression suites; (5) Provider bake-off and production monitoring. Simpler: metric cheat sheet. Deeper: confidence intervals on small sets, slice analysis, judge calibration, canary comparisons.
- Mastery focus: metric choice and pitfalls, test design. Badge: scored per-slice report plus a failing-on-regression test.

**M11 Latency-budget workbook and generic interview-voice-agent case study**
- Lessons: (1) Requirements to architecture for a generic interview voice agent; (2) Latency, failure and cost overlays (workbook, harness values labelled SIMULATED); (3) Evaluation and bake-off plan; (4) Consent and retention defaults (pointer to M7.5, X1); (5) Defend-your-numbers: five-level defence of your own decision. Simpler: guided template. Deeper: adversarial reviewer, what-if (10x load, provider outage, new language). Pointer: A6 library #17 for platform-scale design (no duplicate canvas task or question bank).
- Mastery focus: integrated reasoning, honest trade-offs, number discipline. Badge: graded workbook plus case-study submission and the five-level drill.

## 4. Seed interview questions (3 per module, 33 total)

Seed IDs: `M<n>-Q<k>` below is `Q-B6-M<n>-<kk>` in the question bank (G-10; for example `M4-Q1` = `Q-B6-M4-01`). Each item also carries a goal tag (`B` Senior Python Backend, `A` AI Engineer, `H` hybrid; `M` MUST, `S` SHOULD, `N` NICE) and a one-line senior-bar note (mid vs senior vs staff).

Style-of-question items for practice only; none are claimed to be real questions from any company. Tags: difficulty (Medium/Hard), competency, company-style cluster.

### M1

**M1-Q1. Walk me through the architecture of a voice agent and tell me where it differs from a text chatbot.** Difficulty: Medium. Competency: `voice.architecture`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- Ideal answer: capture and VAD at the edge; streaming STT with interim and final results; dialogue layer or LLM with tools and state; streaming TTS; playback; all stages overlap as a dataflow rather than sequential calls.
- Cancellation paths: barge-in must cancel LLM, TTS and playback.
- Turn-taking and endpointing are first-class concerns that text does not have.
- Latency is perceived end to end, so time-to-first-audio matters more than total time.
- Audio specifics: codecs, sample rates, jitter, echo.
- Transcript and state still needed for audit, scoring and memory.
- Failure modes: ASR errors propagate into the LLM; fallbacks to text.
- Weak answers: "STT then LLM then TTS" with no streaming, no interruption, no latency view; naming vendors instead of design.
- Follow-ups: What flows over which transport? What if STT is wrong on a key term? How do you test it?
- Senior-bar note: mid names the stages; senior adds cancellation and overlap; staff adds transcript, audit and scoring needs.

**M1-Q2. When would you choose a speech-to-speech model over a cascaded pipeline, and when not?** Difficulty: Hard. Competency: `voice.architecture`. Cluster: global-product. Goal: `B:N A:S H:S`.
- S2S strengths: lower and more natural latency, paralinguistic cues, fewer hops, natural interruption handling.
- Cascaded strengths: component swap per provider, custom vocabulary and STT tuning, exact transcripts, deterministic control, easier tool use and guardrails, cheaper components, auditability.
- Requirements-driven: compliance/audit, scoring needs evidence quotes, language/accents, cost per minute, voice customisation.
- Hybrid: S2S for front-end conversation with side-channel transcription, or cascaded with fast first-response model.
- Evaluate on your own audio via bake-off, not demos.
- Vendor lock-in and model-version volatility.
- Weak answers: "S2S is the future so always" or "cascaded is more accurate" without data.
- Follow-ups: How would you get a reliable transcript from S2S? How do tools and guardrails behave? How would you compare cost?
- Senior-bar note: mid picks a side; senior is requirements-driven with a bake-off; staff weighs lock-in, audit and cost curves.

**M1-Q3. Compare Pipecat or LiveKit Agents-style frameworks with writing the orchestration yourself.** Difficulty: Medium. Competency: `voice.architecture`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- Frameworks give transport adapters, VAD/turn handling, interruption plumbing, provider plugins, observability hooks.
- Risks: abstraction leaks, version churn, opaque latency, debugging, lock-in to their frame model.
- Own orchestration: full control, fewer dependencies, but you rebuild cancellation, back-pressure and provider adapters.
- Decision on team size, time to market, differentiators, need for custom turn logic.
- Keep a thin provider interface regardless (AI Gateway pattern).
- State that you would prototype with a framework and keep the domain engine (state, scoring) independent.
- Weak answers: pure preference; not mentioning cancellation or testing.
- Follow-ups: What would make you drop the framework? How do you test provider swaps? (as of October 2026, verified on PyPI: Pipecat 1.0.0 was published 2026-04-14 and the latest is 1.12.0; LiveKit Agents 1.0.0 was published 2025-04-10 and the latest is 1.8.4; lessons name no versions)
- Senior-bar note: mid states preference; senior names cancellation, testing and leaks; staff keeps a thin interface and an exit plan.

### M2

**M2-Q1. Why does phone audio hurt speech recognition accuracy, and what can you do about it?** Difficulty: Medium. Competency: `voice.stt-quality`. Cluster: IT-services. Goal: `B:N A:S H:S`.
- Telephony is 8 kHz narrowband with G.711 companding; high-frequency consonant information is lost (about 4 kHz ceiling).
- Upsampling to 16 kHz does not restore information.
- Compression artefacts, packet loss, background noise on cellular legs.
- Use models trained or selected for telephony audio, correct sample rate settings, avoid double transcoding.
- Vocabulary boosting, confirmation of critical entities (read back numbers), DTMF for sensitive digits.
- Measure WER on telephony fixtures separately.
- Weak answers: "use a better model" only; thinking upsampling fixes it.
- Follow-ups: How do you confirm an account number? Where would a transcoding step hide?
- Senior-bar note: mid says use a better model; senior explains the 4 kHz ceiling and transcoding; staff adds read-back, DTMF and per-channel measurement.

**M2-Q2. In the browser, how do you capture microphone audio for streaming STT, and what are the pitfalls?** Difficulty: Medium. Competency: `voice.transport`. Cluster: global-product. Goal: `B:N A:S H:S`.
- `getUserMedia` with constraints (echoCancellation, noiseSuppression, autoGainControl); permission flow and recovery.
- `AudioWorklet` to get raw PCM frames with low latency; resample to the model rate (often 16 kHz) and convert float32 to int16.
- `MediaRecorder` yields container-encoded chunks (e.g. webm/opus), not frame-aligned, extra latency; fine if the server decodes.
- Chunk size trade-off (20–100 ms): overhead vs latency.
- Device switching, sample rate differences between devices, Safari/iOS quirks (**[verify: device test needed]**; WebKit notes show Safari 26.1 fixed spurious iOS devicechange events and 26.4 added multi-microphone capture on macOS, but AudioWorklet and iOS audio-routing quirks were not confirmed).
- Main-thread blocking; use worklet and transferable buffers.
- Weak answers: only MediaRecorder with no awareness of format; no permission recovery.
- Follow-ups: What happens when the user changes headset mid-call? How big are 100 ms chunks of 16-bit 16 kHz mono?
- Senior-bar note: mid uses MediaRecorder; senior picks AudioWorklet and handles devices; staff covers chunk-size and main-thread trade-offs.

**M2-Q3. Calculate bandwidth for 500 concurrent voice sessions, in each direction, for raw PCM, Opus and G.711.** Difficulty: Medium. Competency: `voice.transport`. Cluster: Big4. Goal: `B:N A:S H:S`.
- 16 kHz, 16-bit mono PCM = 256 kbps (32 KB/s); 24 kHz is 384 kbps.
- G.711 at 8 kHz, 8-bit = 64 kbps.
- Opus typically 16–32 kbps for speech wideband (configurable).
- Add protocol overhead (RTP/UDP/IP about 40 bytes per 20 ms packet, WebSocket framing, base64 +33% if JSON).
- 500 sessions times rate times two directions gives totals; mention that provider streams count too.
- Egress cost and NIC limits; compress on browser leg if needed.
- Weak answers: no unit discipline; forgetting overhead or direction.
- Follow-ups: Where would you transcode? What does TURN relaying do to cost?
- Senior-bar note: mid gives one number; senior adds overhead and direction; staff adds egress cost, TURN relay and transcoding placement.

### M3

**M3-Q1. WebSocket or WebRTC for a browser voice agent? Justify.** Difficulty: Hard. Competency: `voice.transport`. Cluster: global-product. Goal: `B:N A:S H:S`.
- WebSocket: simple, easy server-side control and auth, works through proxies, TCP ordering and retransmission cause head-of-line blocking under loss, no built-in AEC/jitter buffer/Opus handling (you build or rely on browser capture AEC).
- WebRTC: UDP-based SRTP, ICE/STUN/TURN for NAT traversal, adaptive jitter buffer, packet-loss concealment, Opus, congestion control; more complex, needs a media server or SFU, signalling, TURN infrastructure.
- Real networks with loss (mobile in India) favour WebRTC for quality; controlled networks and prototypes favour WebSocket.
- Server-side agent needs a WebRTC-capable endpoint (media server, or LiveKit/Pipecat transports).
- Decide with measurements: loss, jitter, p95 latency, operational cost.
- Weak answers: absolute statements; no mention of NAT, jitter, or operational complexity.
- Follow-ups: When do you need TURN? How would you start with WebSocket and migrate? What would you measure?
- Senior-bar note: mid is absolute; senior ties choice to loss, NAT and ops; staff plans migration and measurement.

**M3-Q2. Your async audio relay uses a queue between the browser socket and the STT socket and memory keeps growing. Diagnose.** Difficulty: Hard. Competency: `voice.transport`. Cluster: global-product. Goal: `B:N A:S H:S`.
- Producer faster than consumer: STT slow, network stall, or event loop blocked.
- Unbounded queue hides backpressure; bound it and define a policy (drop oldest audio vs block vs close session) based on use case.
- Check blocking calls in the loop (sync resampling, CPU-heavy VAD) and move to threads/executor or processes.
- Metrics: queue depth, loop lag, per-session bytes in/out.
- Cancel tasks on disconnect, use `TaskGroup` for structured concurrency, ensure sockets closed.
- Per-session memory caps and session-level circuit breaker.
- Weak answers: "add more memory"; ignoring backpressure policy.
- Follow-ups: Is dropping audio acceptable? How would you reproduce this in test? What alert would catch it earlier?
- Senior-bar note: mid adds memory; senior bounds the queue and names a drop policy; staff adds loop-lag metrics and session circuit breakers.

**M3-Q3. How do you authenticate and secure a browser-to-server audio stream, and a server-to-STT-provider stream?** Difficulty: Medium. Competency: `voice.transport`. Cluster: Big4. Goal: `B:N A:S H:S`.
- Browser: short-lived signed session token minted by the backend after user auth; send in first message or as a query token, not long-lived API keys; validate Origin; bind token to session and user; expiry and rotation.
- Server to provider: keys in secrets manager, per-environment, never to the client; egress allow-lists.
- TLS (wss) always; WebRTC uses DTLS-SRTP; TURN credentials time-limited.
- Rate limits and concurrent session limits per user; abuse protection and cost caps.
- Logging without leaking tokens or audio content.
- Weak answers: "put the API key in the frontend with a restricted domain"; no token expiry.
- Follow-ups: What if a token is stolen mid-session? How do you revoke?
- Senior-bar note: mid says wss; senior mints short-lived tokens and checks Origin; staff covers revocation, logging hygiene and cost caps.

### M4

**M4-Q1. Describe exactly what happens, step by step, when a user barges in while the agent is speaking.** Difficulty: Hard. Competency: `voice.turn-taking`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- Detect user speech during playback (VAD on the mic stream, with echo cancellation to avoid self-trigger) and apply a short confirmation window to avoid noise or backchannel false positives.
- Immediately stop playback and flush client and server audio buffers; cancel in-flight TTS synthesis and the LLM stream; cancel pending tool calls if safe.
- Determine what the user actually heard (playback position or word timestamps) and truncate the assistant message in conversation history to that point (optionally mark as interrupted).
- Switch state to user-speaking; start STT turn handling.
- Handle race conditions: late TTS chunks arriving after cancel must be dropped (generation or turn IDs).
- With a speech-to-speech API the provider holds the conversation items, so the client must report how much audio was actually played so the server-side item is truncated; otherwise the model "remembers" unheard speech (the same bug, moved across the API boundary). Check the provider's truncation or playback-position event in its current docs.
- Log metrics: time from speech onset to audio silence.
- Weak answers: only "stop the audio"; no history truncation, no race handling.
- Follow-ups: How do you distinguish "uh-huh" from a real interruption? What if the tool call has side effects? How do you test it?
- Senior-bar note: mid stops audio; senior truncates history and drops late chunks; staff adds side-effect tools, metrics and tests.

**M4-Q2. How would you tune end-of-turn detection for a mock interview vs a customer-support bot?** Difficulty: Hard. Competency: `voice.turn-taking`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- Endpointing is a trade-off: short silence timeout cuts off thinkers; long adds dead air to latency.
- Interview answers contain long thinking pauses and structured enumeration; use longer patience (e.g. higher silence threshold), semantic/turn-detection model or a "still thinking" cue, and allow "I'm not done" signals.
- Support bots need faster turn-taking; short utterances and confirmations.
- Adaptive: per-turn-type thresholds (after question vs after "let me think"), user-configurable pace.
- Use filler words and trailing conjunctions as cues; avoid purely acoustic decisions.
- Evaluate with labelled fixtures: cut-off rate and dead-air time; tune on both.
- Weak answers: a single fixed threshold; ignoring evaluation.
- Follow-ups: How do you show the system is still listening? How do you measure cut-offs? What does this do to the latency budget?
- Senior-bar note: mid uses one threshold; senior adapts per use case and measures cut-offs; staff budgets the latency cost.

**M4-Q3. The agent keeps interrupting itself. What are the likely causes?** Difficulty: Medium. Competency: `voice.turn-taking`. Cluster: IT-services. Goal: `B:N A:S H:S`.
- Echo: speaker output picked up by the mic and detected as user speech; check AEC enabled, headphone use, device gain, playback path.
- VAD threshold too sensitive or noisy environment; no hangover or minimum speech duration.
- Barge-in confirmation window absent.
- Server-side loop: TTS audio looped into STT input in a telephony bridge.
- Duplicate or mis-ordered events and missing turn IDs causing state confusion.
- Debug with recorded audio, VAD event logs, and spectrogram of mic input during playback.
- Mitigations: AEC, minimum duration, double-talk detection, half-duplex fallback for bad environments.
- Weak answers: "raise volume" or "lower sensitivity" without diagnosis.
- Follow-ups: How would you detect this automatically in pre-flight? What trade-off does half-duplex bring?
- Senior-bar note: mid blames volume; senior diagnoses echo, VAD and loops from logs; staff automates detection in pre-flight.

### M5

**M5-Q1. Your STT keeps mishearing technical terms like "FastAPI" and "pgvector" from Indian-English speakers. What do you do?** Difficulty: Hard. Competency: `voice.stt-quality`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- First measure: build a labelled fixture set across accents and terms; compute WER and term-recall per slice.
- Provider-side boosting: custom vocabulary, hotwords, keyterms, phrase hints; check limits and boost weights.
- Model choice: compare providers on this set (bake-off); consider models with Indic-English strengths.
- Post-STT correction: fuzzy/phonetic lexicon matching, LLM-assisted correction constrained to a glossary and context (the current question), with care to avoid hallucinated rewrites.
- Context priming: pass the expected topic/terms per question.
- Show live transcript and allow user edits; do not score on uncorrected transcripts blindly.
- Re-measure after each change; watch for regressions on other words.
- Weak answers: "use a bigger model"; no measurement; trusting one vendor's WER.
- Follow-ups: How do you avoid over-correcting real words? What is your acceptance threshold? How do you handle code-switching?
- Senior-bar note: mid says bigger model; senior measures per-slice and boosts vocabulary; staff controls over-correction with acceptance thresholds.

**M5-Q2. Explain interim vs final transcripts and how a downstream dialogue system should use them.** Difficulty: Medium. Competency: `voice.stt-quality`. Cluster: global-product. Goal: `B:N A:S H:S`.
- Streaming STT emits revisable interim hypotheses and stable final segments; interim text may change.
- UI can show interim live; the LLM should usually receive final text at end-of-turn.
- Optional speculative LLM start on stable interim prefixes, with cancellation if the final differs materially.
- Endpointing may use STT's own signals plus VAD; reconcile conflicts.
- Word timestamps support replay, delivery coaching and barge-in truncation.
- Confidence scores can trigger clarification ("did you say ...?").
- Weak answers: treating interim as final; no cancel strategy.
- Follow-ups: What are the cost and risk of speculative starts? What if finalisation is slow?
- Senior-bar note: mid conflates interim and final; senior uses final at end-of-turn; staff prices speculative starts.

**M5-Q3. How do you make TTS feel fast and natural when the LLM streams tokens?** Difficulty: Medium. Competency: `voice.tts`. Cluster: global-product. Goal: `B:N A:S H:S`.
- Chunk the LLM stream at sentence or clause boundaries and send to streaming TTS immediately; first chunk small for time-to-first-audio, later chunks larger for prosody.
- Text normalisation for numbers, currency (rupees), dates, acronyms; pronunciation lexicon/SSML for terms.
- Keep consistent voice and speed; avoid mid-word cut points.
- Buffer a small amount of audio to avoid underrun; handle cancellation on barge-in.
- Prompt the LLM for speakable output (short sentences, no markdown, no lists read aloud).
- Measure TTS time to first byte and underruns.
- Weak answers: waiting for full response; ignoring normalisation or markdown leakage.
- Follow-ups: What if a chunk boundary splits a number? How do you handle code snippets in an answer?
- Senior-bar note: mid waits for the full answer; senior chunks by sentence with normalisation; staff handles boundary splits and underruns.

### M6

**M6-Q1. Build me a latency budget for a voice turn and tell me where latency hides.** Difficulty: Hard. Competency: `voice.latency`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- Define the metric: end of user speech (actual) to first agent audio audible; list hops.
- Budget (harness-generated, labelled SIMULATED assumptions; the workbook regenerates them, never quote them as vendor figures): endpointing wait 300–700 ms; STT finalisation 100–300 ms; network RTTs 50–150 ms; LLM TTFT 300–800 ms; first-sentence accumulation 100–300 ms; TTS TTFB 100–300 ms; jitter/playback buffer 40–120 ms.
- Hidden latency: endpointing silence, finalisation delay, tool calls on the hot path, cold starts, region mismatch, connection setup, context assembly/DB calls, large prompts, buffering at the client.
- Use percentiles; tail comes from LLM and cold paths.
- Fixes: streaming and overlap, speculative start, prompt caching, smaller first-reply model, filler audio, warm pools, co-location, prefetch.
- Instrument each hop with timestamps and tracing.
- Weak answers: only LLM and TTS; no number discipline; averages.
- Follow-ups: Which item would you cut first and why? What does speculative start cost? How do you show progress when over budget?
- Senior-bar note: mid lists LLM and TTS; senior includes endpointing and playback with percentiles; staff ranks cuts by measured gain.

**M6-Q2. p50 is fine but p95 is three times worse. How do you investigate?** Difficulty: Hard. Competency: `voice.latency`. Cluster: global-product. Goal: `B:N A:S H:S`.
- Slice the percentile by hop using per-turn traces; find which stage's tail dominates.
- Suspects: LLM provider tail and rate-limit queuing, cold starts, GC or event loop blocking, noisy neighbours, long contexts, tool calls, retries, network path variance, specific regions/ISPs.
- Correlate with load, prompt size, time of day, provider status.
- Mitigations: timeouts with fallback model, hedged requests (with cost awareness), warm capacity, context trimming, regional routing, admission control.
- Tail-aware SLOs and alerts on p95/p99 per hop.
- Weak answers: "scale up" without data.
- Follow-ups: When are hedged requests appropriate? What is your degraded-mode behaviour at the timeout?
- Senior-bar note: mid scales up; senior slices the tail per hop; staff adds hedging economics and tail-aware SLOs.

**M6-Q3. Is speculative LLM execution on interim transcripts worth it?** Difficulty: Hard. Competency: `voice.latency`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- Idea: start generation before the endpoint is confirmed using the stable prefix; if the final transcript or end-of-turn differs, cancel and restart.
- Benefit: hides endpointing and finalisation latency (hundreds of ms) on turns where prediction holds.
- Costs: wasted tokens, higher load, complexity, risk of side-effect tools firing early (only allow read-only/no tools speculatively).
- Measure hit rate and net latency gain; enable by turn type.
- Interaction with prompt caching and rate limits.
- Weak answers: unconditional yes or no; no discussion of cancellation and side effects.
- Follow-ups: How do you measure hit rate? Which turns should be excluded?
- Senior-bar note: mid says yes or no; senior limits to read-only turns with cancellation; staff measures hit rate and net gain.

### M7

**M7-Q1. How does a phone call reach your voice agent? Explain SIP and RTP's roles.** Difficulty: Medium. Competency: `voice.telephony`. Cluster: IT-services. Goal: `B:N A:S H:S`.
- Caller on PSTN or mobile network; carrier routes to a SIP trunk provider (or CPaaS) via a gateway/SBC.
- SIP is signalling (INVITE, ringing, 200 OK, ACK, BYE); SDP negotiates codecs and media addresses; RTP carries the audio.
- Agent side either a SIP endpoint or a CPaaS that bridges media to your server (often via WebSocket with mu-law or PCM frames).
- Codecs: G.711 (PCMU/PCMA) narrowband; consequences for STT and TTS output format.
- Extras: DTMF events, call transfer/REFER, hold, voicemail detection, call recording disclosure.
- Operational: concurrency limits, per-minute carrier cost, failover trunks.
- Weak answers: "SIP sends the audio"; no codec or bridging detail.
- Follow-ups: What changes in your pipeline for 8 kHz? How do you transfer to a human?
- Senior-bar note: mid says SIP sends audio; senior separates SIP, SDP and RTP; staff adds trunk concurrency, failover and cost.

**M7-Q2. You must add inbound phone support to an existing web voice agent. What do you change?** Difficulty: Hard. Competency: `voice.telephony`. Cluster: global-IT-consulting. Goal: `B:N A:S H:S`.
- Media ingress: CPaaS media stream or SIP media server; decode G.711 to PCM, resample for STT; encode TTS back to the telephony codec/rate.
- Re-tune VAD/endpointing for narrowband noisy audio; no pre-flight device check, so greet and confirm audibility.
- Add DTMF handling, human handoff, voicemail/answering-machine detection, call timeouts, caller ID and authentication flow.
- Consent and recording announcements; regional rules (awareness, M7.5; **[verify: vendor blogs conflict on TRAI AI-call disclosure and recording-retention rules; primary TRAI text not read, human reviewer needed]**).
- Capacity planning on trunk channels and cost per minute; fraud and abuse protections.
- Separate eval set for phone audio; monitor per-channel metrics.
- Weak answers: "just connect Twilio" without audio/format or compliance details.
- Follow-ups: What if the caller is on speakerphone? How do you test at scale?
- Senior-bar note: mid says connect a CPaaS; senior handles codecs, DTMF and handoff; staff adds per-channel eval and fraud controls.

**M7-Q3. What is DTMF and why does it still matter in a voice AI product?** Difficulty: Medium. Competency: `voice.telephony`. Cluster: Big4. Goal: `B:N A:S H:S`.
- Dual-tone multi-frequency tones for keypad digits; transmitted in-band or as RTP telephone-event (RFC 4733).
- Reliable entry of sensitive or structured data (PINs, account numbers) where STT is error-prone and privacy matters.
- In-band tones can be distorted by codecs; out-of-band events are more reliable.
- Voice agent must detect, mask in transcripts, and combine with speech flows ("press 1 or say yes").
- Also a fallback when STT fails or the environment is noisy.
- Weak answers: unaware of out-of-band events; ignoring transcript redaction.
- Follow-ups: How do you avoid logging card digits? How do you test DTMF paths?
- Senior-bar note: mid defines DTMF; senior prefers RFC 4733 events and masking; staff designs the speech-plus-keypad flow and test plan.

### M8

**M8-Q1. A user's network drops for 10 seconds in the middle of a voice interview. Design the recovery.** Difficulty: Hard. Competency: `voice.reliability`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- Client detects loss (heartbeat/close), shows reconnecting state, buffers minimal audio locally if useful, retries with backoff and jitter.
- Session state lives server-side (external store) keyed by a session ID; reconnect with sequence number so the server can resume or replay missed events/transcript.
- Server keeps session alive for a grace window; pauses timers and cost accrual appropriately.
- Resume at the last question; the interviewer re-states or summarises; partial answer handling (keep transcript so far).
- If reconnect fails or latency too high, degrade to text mode with autosaved progress.
- Idempotent operations so replays do not double-score.
- Metrics: reconnect success rate, time to resume.
- Weak answers: "reload the page"; state kept in process memory only.
- Follow-ups: What if the server crashed instead? What about duplicate transcripts? How do you test it?
- Senior-bar note: mid reloads; senior externalises state with sequence-number resume; staff adds idempotent scoring and recovery metrics.

**M8-Q2. How do you scale a stateful, long-lived real-time service and deploy without dropping calls?** Difficulty: Hard. Competency: `voice.reliability`. Cluster: global-product. Goal: `B:N A:S H:S`.
- Sessions are long-lived connections pinned to a worker; scale on concurrent sessions and loop lag/CPU per session, not RPS.
- Keep durable state externalised (Redis/DB) so a worker loss leads to resume rather than loss; consider sticky routing at the load balancer with session-aware placement.
- Deploy with connection draining: stop new sessions to old workers, let sessions finish (with a max time) or migrate via resume.
- Capacity planning from load tests with synthetic callers; headroom for reconnect storms; admission control and queueing when full.
- Autoscaling lag: scale proactively ahead of known peaks; warm pools.
- Isolate heavy work (VAD, resampling) from the event loop; multi-process workers.
- Per-tenant quotas; regional placement near users and providers.
- Weak answers: "Kubernetes autoscaling on CPU"; killing pods on deploy.
- Follow-ups: What does the load balancer need to support? How do you handle a thundering herd after an outage? What is your pod termination grace period and why?
- Senior-bar note: mid autoscales on CPU; senior scales on sessions with draining; staff plans storms, admission control and grace periods.

**M8-Q3. Which failure would you fear most in production voice and how do you degrade gracefully?** Difficulty: Medium. Competency: `voice.reliability`. Cluster: IT-services. Goal: `B:N A:S H:S`.
- Pick and reason: silent failures (agent stops responding with no signal) are worse than loud failures.
- Per-stage timeouts and watchdog; user-visible states (listening/thinking/error).
- Degradation ladder: retry; fallback provider (STT/LLM/TTS); shorter model or cached responses; switch to text; offer callback or support contact.
- Circuit breakers per provider, health-based routing through the AI Gateway.
- Observability: alert on fallback rate, drop rate, p95 latency, empty-response rate.
- Runbooks and chaos drills.
- Weak answers: listing retries only; no user communication.
- Follow-ups: How do you avoid flapping between providers? What do you tell the user in each state?
- Senior-bar note: mid lists retries; senior fears silent failure and adds watchdogs; staff adds flap control, user messaging and drills.

### M9

**M9-Q1. Estimate the cost per minute of a voice agent and explain how you would control it.** Difficulty: Medium. Competency: `voice.cost-privacy`. Cluster: Indian-product. Goal: `B:N A:S H:S`.
- Components: STT (per audio second/minute), LLM (input and output tokens, growing with conversation history), TTS (per character or second), transport/media server and TURN, telephony carrier minutes, compute, storage/recordings, observability.
- Cascaded vs S2S (S2S billed by audio tokens, often higher per minute; pricing is volatile, so no prices are stated here; the workbook reads rates from a dated data file).
- Do not quote vendor prices as fixed; build a parameterised workbook, state assumptions, and measure real usage.
- Levers: stream only speech (VAD gating), cap session length, trim context/summarise, prompt caching, route to cheaper models for simple turns, shorter spoken answers, avoid idle billing, batch non-real-time work.
- Controls: per-session and monthly budgets, quotas, kill switches at the gateway, cost per minute dashboard, alerts.
- Weak answers: only LLM cost; fixed vendor prices; no controls.
- Follow-ups: What grows quadratically over a long call? Which line item surprises people? How do you set the cap?
- Senior-bar note: mid quotes LLM cost; senior builds the full parameterised stack; staff adds sensitivity, caps and kill switches.

**M9-Q2. Design consent, retention and deletion for recorded interview audio.** Difficulty: Medium. Competency: `voice.cost-privacy`. Cluster: Big4. Goal: `B:N A:S H:S`.
- Explicit, separate consent for microphone use and for recording/retention; plain-language purpose; withdraw anytime.
- Default minimal: process in real time; retain audio only if the learner opts in, short default retention then transcript only.
- No voice biometrics, no emotion or personality inference.
- Deletion on request across storage, backups (documented lag), derived data; export option.
- Encrypt in transit and at rest; access control and audit logs for any recordings; redact PII in transcripts where feasible.
- Provider terms: data processing agreements, no training on user audio, zero-retention modes (**[verify: per provider, current terms]**).
- Regulatory awareness only, pointer to X1: DPDP Rules were notified 14 November 2025 with phased commencement (core data-fiduciary obligations such as consent notices, retention and erasure apply from about May 2027; as of October 2026 source: PIB), plus GDPR for global users; not legal advice.
- Weak answers: "we store everything in case we need it"; consent buried in terms.
- Follow-ups: What if a user withdraws consent mid-session? How do you handle minors? Third-party processors?
- Senior-bar note: mid stores everything; senior separates consents with short retention; staff adds deletion across backups and processor terms.

**M9-Q3. Threat-model a public voice-agent endpoint.** Difficulty: Hard. Competency: `voice.cost-privacy`. Cluster: Big4. Goal: `B:N A:S H:S`.
- Assets: user audio and transcripts, provider keys, budget, service availability.
- Threats: cost-abuse (long calls, bots), token theft, toll fraud on telephony, prompt injection through speech or audio played into the mic, data exfiltration through tool calls, spoofing/deepfake impersonation, DoS via many sessions, recording leakage.
- Controls: authenticated short-lived session tokens, per-user concurrency and duration caps, rate limits, CAPTCHA/step-up for anonymous use, input guardrails and least-privilege tools with confirmation for sensitive actions, output filtering, secrets management, audit logs, anomaly alerts on usage.
- Do not use voice as sole authentication.
- Weak answers: only "use HTTPS"; ignoring cost and prompt-injection vectors.
- Follow-ups: How would you detect a bot? Why not voice biometrics for auth?
- Senior-bar note: mid says HTTPS; senior covers cost abuse, injection and token theft; staff prioritises by risk and step-up controls.

### M10

**M10-Q1. How do you evaluate STT quality properly? What is wrong with "WER is 8%"?** Difficulty: Hard. Competency: `voice.eval`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- WER = (substitutions + deletions + insertions) / reference words; CER for languages or short tokens.
- Normalise before scoring (case, punctuation, numerals, contractions); report the normaliser.
- Evaluate on representative audio: accents (Indian English), noise, telephony vs web, domain terms; per-slice WER and confidence intervals.
- Add term-recall/entity accuracy for jargon since WER can hide key-term errors; semantic or task-level impact.
- Reference quality: human-labelled gold set, double annotation.
- Compare providers on the same set; track drift over time and model versions.
- Weak answers: single vendor number; clean read speech benchmark; no normalisation.
- Follow-ups: How big must the test set be? How do you handle code-switched speech? How do you link WER to task success?
- Senior-bar note: mid quotes WER; senior normalises and slices; staff adds intervals, term-recall and link to task success.

**M10-Q2. How would you test a voice agent end to end in CI?** Difficulty: Hard. Competency: `voice.eval`. Cluster: global-product. Goal: `B:N A:S H:S`.
- Pyramid: unit tests for state machines (turn-taking, barge-in) and chunkers; component tests with fake streaming providers with injected latency, jitter and errors; contract tests for provider adapters; end-to-end tests with recorded audio fixtures and golden transcripts; simulated callers (scripted TTS audio plus LLM personas) with noise, interruptions and silence.
- Assertions: task success, latency percentiles under budget, interruption handling, no stuck sessions, cost per session within cap.
- Nondeterminism: seed, tolerance bands, rubric/LLM-judge with calibration (B5), multiple runs.
- Regression gating on provider/model/prompt changes; canary and shadow traffic in production.
- Soak/load tests for scaling and leak detection.
- Weak answers: manual testing only; mocking everything so nothing real is tested.
- Follow-ups: How do you test barge-in deterministically? How do you keep CI cost bounded?
- Senior-bar note: mid tests manually; senior builds the pyramid with fakes and fixtures; staff gates regressions and bounds CI cost.

**M10-Q3. You must choose between two STT/TTS/LLM provider stacks. Describe your bake-off.** Difficulty: Medium. Competency: `voice.eval`. Cluster: Indian-product. Goal: `B:N A:S H:S`.
- Define requirements and weights: Indian-English and technical-term accuracy, latency p50/p95 (time to first interim, finalisation, TTFB), barge-in behaviour, reliability/limits, cost per minute, privacy terms, language coverage.
- Build the evaluation set from your own audio (consented/synthetic), cover conditions.
- Run identical audio through candidates, same normalisation; measure with confidence intervals; test under load and from target regions.
- Include operational factors: SDK quality, streaming stability, support, rate limits.
- Choose a primary and fallback behind the gateway; re-run on a schedule.
- Weak answers: demos and vendor claims; accuracy only.
- Follow-ups: How do you weigh cost versus accuracy? How do you avoid overfitting to your test set?
- Senior-bar note: mid trusts demos; senior runs identical audio with weights; staff avoids test-set overfit and schedules re-runs.

### M11

**M11-Q1. Take a generic interview voice agent (live transcript, question engine, barge-in, text fallback) and tell me what could go wrong and how the design addresses it.** Difficulty: Hard. Competency: `voice.architecture`. Cluster: AI-startup. Goal: `B:N A:S H:S`.
- Walk the pipeline: pre-flight, capture/VAD, streaming STT with live transcript, question-state engine, provider abstraction, streaming TTS, barge-in cancel.
- Failure modes and designs: misheard terms (custom vocabulary, editable transcript), cut-offs on long pauses (interview-tuned endpointing), latency breaches (budget monitor and text fallback), disconnects (resume at last question), cost (per-session caps), consent (explicit mic/recording, short retention), scoring unfairness from ASR errors (score on corrected transcript, evidence quotes).
- Interview-specific: strict vs coaching mode, hybrid voice/code/diagram turns, delivery coaching without emotion inference.
- Metrics and launch gates: latency targets, accuracy on an Indian-English set, barge-in success, cost per minute.
- Honest gaps: what stays unverified until a bake-off.
- Weak answers: features without failure analysis or numbers.
- Follow-ups: What would you cut if the budget halves? How do you validate scorer fairness across accents?
- Senior-bar note: mid lists features; senior ties each failure to a metric and a fallback; staff sets launch gates and fairness checks.

**M11-Q2. Tell me about a voice agent you built. Defend one design decision through five levels of "why".** Difficulty: Hard. Competency: `voice.architecture` (secondary `project.tradeoffs`, owner C2). Cluster: IT-services. Goal: `B:N A:S H:S`.
- Context in one minute: problem, users, constraints, scale, your role.
- Pick one decision (endpointing policy, provider choice, text fallback, barge-in handling) with alternatives considered.
- Evidence: measurements (latency, WER, task success, cost), not adjectives.
- Trade-offs accepted and what failed or surprised you.
- What you would change now, and how you would test it.
- Ownership: what you did personally vs team.
- Weak answers: feature tour; no numbers; blaming vendors; claiming perfection.
- Follow-ups: How did you measure it? What broke in production? What would you do differently with a speech-to-speech model today?
- Senior-bar note: mid narrates the build; senior defends a trade-off with numbers; staff shows how the decision changed team practice.

**M11-Q3. `ref` Q-A6-M8-03 (A6 M8-Q3, library #17): design a multi-tenant voice agent platform for thousands of concurrent calls.** Not re-authored here (R-B6-5). B6 contributes only the inputs the answer must cite: the latency-budget workbook, per-stage metrics (M10), capacity numbers from M8 and bandwidth arithmetic from M2. Difficulty: Hard. Competency: `design.ai-systems` (owner A6). Cluster: global-product. Goal: `B:N A:S H:S`.
- Senior-bar note: staff answers start from the workbook numbers and say what breaks first at 10x.

## 5. Final assessment

- **Mastery check:** 25 items across M1–M11 (MCQ, predict-the-trace, short numeric: bandwidth, latency waterfall, cost per minute, WER), 80% pass, revision loop on missed modules.
- **Timed task (about 60 min, harness-based, SIMULATED):** fix a provided voice relay that has an unbounded queue, no barge-in truncation and no reconnect resume; hidden tests (>= 70%) check cancellation, history truncation, bounded memory and resume correctness; plus compute WER with a specified normaliser.
- **Latency and cost workbook (take-home within platform):** given harness component measurements, meet a stated p50 and p95 budget and per-minute cost cap; justify each optimisation; Defend-your-numbers prompts on three cells.
- **Design task:** voice platform design is the A6 library #17 canvas task and is not repeated here; B6 requires the generic interview-voice-agent case study (M11) scored on the rubric (requirements, numbers, failure ladder, consent, evaluation): overall >= 3.5, no dimension < 3, no critical rule findings (G-7); cites registry rule IDs only.
- **Voice mock round (about 25 min):** Interview Master voice session on "design and productionise a voice agent" including the five-level defence; scored with evidence quotes (no emotion inference). Badge: "Meets"; readiness: Senior bar (section 8).
- **Pass:** all components at or above threshold; weak modules routed back to Deeper lessons.

## 6. Course-level risks and freshness notes (as of October 2026)

Resolved this review (web search, October 2026; secondary sources noted):
- WebTransport is Baseline in all major browsers since Safari 26.4 (March 2026; WebKit release notes). Still teach WebSocket and WebRTC as the interview defaults; WebTransport is a trade-off mention.
- DPDP Rules were notified 14 November 2025 with phased commencement: consent-manager rules from about November 2026, core data-fiduciary obligations (consent notices, security, breach reporting, retention, erasure) from about May 2027 (PIB). Awareness only.
- Frameworks: Pipecat 1.0.0 was published 2026-04-14 and LiveKit Agents 1.0.0 on 2025-04-10 (PyPI, verified 2026-10). Lessons name no versions; the dated Provider Note carries them.
- G.711 variants (PCMU in North America and Japan, PCMA in Europe and India) are stable standards facts: not volatile.

Remaining **[verify]** (6):
Each marker names its exact check and owner:
1. Pipecat and LiveKit Agents plugin APIs: at authoring, read each project's current release notes and run the lab code against the pinned release (versions verified 2026-10); owner SME author.
2. Safari/iOS AudioWorklet and device-switch quirks: run the M2 capture lab on current iOS Safari and macOS Safari with a headset hot-swap and compare with WebKit release notes; owner voice engineer (device test, not a web search).
3. TRAI call-recording retention, DLT scope and AI-call disclosure (lesson 7.5, M7-Q2): a named compliance reviewer reads the primary TRAI regulations and current directions on trai.gov.in (vendor blogs conflict); gated publish; owner Content Lead.
4. Indian carrier/CPaaS specifics and trunk concurrency terms: read the chosen CPaaS and carrier documentation for media-stream format, codec and concurrency limits; owner voice team, during the bake-off.
5. Provider zero-retention and data-processing terms (M9, M9-Q2) and speech-to-speech pricing: read each shortlisted provider's current DPA and pricing page and store in the dated data file; no values in lessons.
6. Indic-language STT/TTS provider leaders (M5): no claim made; settled only by the bake-off on the S-4 fixture set.

Other notes:
- Fast-changing: speech-to-speech APIs, STT/TTS providers and prices, turn-detection models, Indic quality. Teach criteria and measurement, not vendor rankings; vendor statements are dated.
- Latency numbers are harness-generated assumptions (SIMULATED); never present them as provider performance.
- Lab realism: no real WebRTC or telephony in the sandbox (harness only; R-B6-1). Mitigation: fixtures, design exercises and the Challenge lesson.
- Overlap discipline: see Ownership notes; B6 deepens only voice-specific aspects.
- Case study uses generic requirements only (R-B6-4); product specifics need sign-off.
- Review cadence: 3 months for vendor-dependent content; 12 months for audio and telephony fundamentals.

## 7. Open items for boss (3)

1. B6.M7 lesson 7.5 depends on a human legal read of TRAI rules before publish (R-C4-3 style gate): confirm owner.
2. Confirm that the voice harness (P-08) will expose per-hop timestamps and a seeded jitter model so the workbook values are reproducible.
3. M11-Q3 now cites `Q-A6-M8-03` (A6 M8-Q3, library #17; A6 maps `M<n>-Q<k>` to `Q-A6-M<n>-<nn>`): confirm the ID is stable after the final A6 numbering freeze.

## 8. Learner-Fit Appendix (G-11, template section 6)

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 new | Simpler, 15.5 h, all lessons, link A2.M2-M5 and B2.M8 first | 60% guided harness labs, 30% calculators, 10% verbal | nothing | none; Challenge lesson 3.6 optional later |
| L2 junior-mid | Simpler, about 13 h, skim lessons already known | 50% labs, 30% workbook, 20% verbal | M2 arithmetic if diagnostic >= 85 | M6 speculative-start economics |
| L3 experienced | Deeper, about 10.5 h | 40% labs, 30% verbal drill, 30% workbook | foundations per bands | Challenge 3.6, M8 storm mitigation |
| L4 senior with a built voice agent | Deeper, "skim and defend", about 7-8 h `[judgement]` | 25% labs on gaps, 45% Defend-your-numbers and voice drills, 30% workbook | all lessons with diagnostic >= 85 | Defend-your-numbers mode (five-level why on every workbook number), Challenge 3.6, what-if (10x, outage, new language) |

Defend-your-numbers (L4): the engine asks for the value, source (SIMULATED or REAL), what changes it, and the next-cheapest fix; a cell passes only with all four.

### LF-2 Module matrix

Diagnostic items are ordinary `Q-B6-Mn-nn` MCQ or predict items carrying the `placement` tag (G-10; counts in the table), not separate IDs. Bands (G-5): >= 85 skip lessons and take the mastery check, 70-84 skim (Deeper-only lessons plus check), 50-69 study, < 50 deep-study with remediation links (LF-5). Tags `B A H`: all modules `B:N A:S H:S` (B6 is NICE for backend, SHOULD for AI); Backend learners take only the NICE primer slice B6.M1 lessons 1-3 (streaming dataflow view, cascaded pipeline, speech-to-speech), as the study-route doc assumes. Weight columns are interview-time weights in percent for Backend / AI / Hybrid (each sums to 100, rationale below, `[judgement]`).

| M | Diagnostic titles (count) | Weight B/A/H | Rehearsal modes | Module readiness criterion |
|---|---|---|---|---|
| M1 | cascaded vs S2S choice, cancellation path, transcript need, framework trade-off, hybrid (5) | 8/12/10 | voice drill, project deep-dive | mastery >= 85 on retake, strict voice drill `voice.architecture` Senior bar twice |
| M2 | bitrate arithmetic, 8 kHz effect, capture API, base64 overhead, resampling (5) | 5/4/4 | voice drill, calculator | timed bandwidth calculation correct in 3 minutes |
| M3 | WebSocket vs WebRTC, bounded queue, token auth, TURN need, HOL blocking, backpressure (6) | 12/8/10 | voice drill, harness coding | timed relay task >= 70% hidden tests; transport answer at Senior bar |
| M4 | barge-in sequence, history truncation, endpointing trade-off, echo, late-chunk race (6) | 8/10/9 | voice drill, harness coding | state machine passes interruption fixtures; Q4-Q1 at Senior bar |
| M5 | interim vs final, hotwords vs post-correction, per-slice WER, TTS chunking, normalisation (6) | 7/10/8 | voice drill | measured improvement report; Q5-Q1 Senior bar |
| M6 | waterfall reading, endpointing delay, percentile vs mean, speculative start, tail cause (7) | 14/12/13 | voice drill, canvas numbers | workbook meets p50/p95 target; Defend-your-numbers pass on 3 cells |
| M7 | SIP vs RTP, G.711, DTMF, handoff, phone vs web (5) | 6/4/5 | voice drill | M7-Q1 Senior bar; design memo >= 3.5 |
| M8 | reconnect resume, drain, scale signal, degradation ladder, storm (7) | 16/10/13 | voice drill, canvas, harness coding | resume tests pass; Q8-Q2 Senior bar |
| M9 | cost stack, consent defaults, token minting, injection, kill switch (6) | 8/8/8 | voice drill, design | cost workbook correct; consent flow rubric >= 3.5 |
| M10 | WER normalisation, slices, regression test, bake-off, term-recall (6) | 8/12/10 | voice drill, harness coding | failing-on-regression test passes; Q10-Q1 Senior bar |
| M11 | requirement traceability, failure story, number defence (5) | 8/10/10 | voice drill, project deep-dive, behavioural | case-study rubric >= 3.5; voice mock Senior bar |

Rationale (`[judgement]`): backend interviews touch voice only through reliability, transport and latency (M3, M6, M8 heavy); AI Engineer interviews weight architecture, turn-taking, STT quality and evaluation; hybrid blends. Overall B6 time is cut by the route multiplier (NICE 0.25, SHOULD 0.6) and tilted by these weights; voice raises its weight in AI-startup and product company packs.

### LF-3 Experience bridge ("you already know" to "explain at senior depth")

| M | You already know | Gap between using and explaining |
|---|---|---|
| M1 | You wired STT, LLM and TTS in a voice agent and used LLM APIs | Naming where the cascade stops being right, cancel paths, audit transcript, S2S trade-offs with evidence |
| M2 | Handled audio chunks, base64 over HTTP/MQTT payloads | Sample-rate vs bitrate arithmetic, codec choice reasons, resampling cost |
| M3 | FastAPI WebSockets, Redis queues, Docker networking | Backpressure policy, HOL blocking, ICE/TURN and when WebRTC wins, key hygiene |
| M4 | Used VAD and barge-in in your agent | The exact cancellation order, truncating history to what was heard, late-chunk IDs, tuning per use case |
| M5 | Tuned STT settings, saw misheard terms | Per-slice WER, term-recall, correction without hallucination, TTS chunking mechanics |
| M6 | Felt latency, perhaps logged timings | Percentile discipline, hidden hops (endpointing, playback), speculative-start economics |
| M7 | Likely used a CPaaS WebSocket stream | SIP vs RTP vs SDP, 8 kHz effects, DTMF, REFER, per-minute carrier cost |
| M8 | Redis state, Lambda/ECS deploys, retries | Session-aware scaling, drain, storm control, silent-failure watchdogs |
| M9 | Provider bills, secrets, IAM | Full per-minute cost stack, consent separation, threat model for speech input |
| M10 | Unit/integration testing, CI | Voice test pyramid, normalisation, simulated callers, bake-off statistics |
| M11 | One real project with real trade-offs | Turning it into numbers, failure stories and a five-level defence |

### LF-4 Senior-bar framing

| M | Mid answer | Senior answer | Staff answer |
|---|---|---|---|
| M1 | Lists stages | Overlap, cancellation, S2S vs cascaded by requirements | Lock-in, audit and cost curves, exit plan |
| M2 | Knows 16 kHz | Arithmetic with overhead per direction | Egress and transcoding placement cost |
| M3 | Picks a transport | Reasoned by loss, NAT, ops; bounded queues | Migration path, measured switch criteria |
| M4 | Stops audio | Truncates history, drops late chunks | Side effects, tests, per-segment tuning metrics |
| M5 | Bigger model | Measured per slice, vocabulary boost | Over-correction guards, acceptance thresholds |
| M6 | LLM plus TTS | Full waterfall with percentiles | Ranks cuts by gain, hedging and SLO design |
| M7 | Connect a CPaaS | Codec, DTMF, handoff plan | Capacity, fraud, per-channel evaluation |
| M8 | Reconnect | State externalised, drain, scale on sessions | Storm and admission control, grace periods, drills |
| M9 | LLM cost | Full stack, caps, short retention | Sensitivity analysis, processor terms, kill switches |
| M10 | One WER | Slices, regression gates | Intervals, judge calibration, bake-off governance |
| M11 | Feature tour | Numbers, failure stories | Launch gates, fairness checks, team practice change |

### LF-5 Adaptive hooks

- Remediation: M3 and M8 -> A2.M2-M5, A2.M7; M3 protocol choice -> A5.M9; M5-M6 LLM streaming and caching -> B2.M8-M9; M10 -> B5 (judge) and B5.M7; M9 -> X1 (A8.M9.4).
- Thresholds: module check 80; readiness 85 on a retake after >= 7 days; Defend-your-numbers pass on 3 of 3 cells; two voice drills >= 48 h apart.
- Challenge extensions: lesson 3.6 (WebRTC internals, `B:N A:N H:N`), M6 speculative economics, M8 storm mitigation, M11 what-ifs.
- Reorder: the engine may defer M7 (telephony) and Challenge 3.6 for backend routes; M2 and M7 may be compressed to skim for L3-L4; M6 before M5 is allowed; M11 stays last. Route placement: after B2 and A2; deferrable for backend routes (B6 `NICE`); the reference learner will likely band at skim.

### LF-6 Applied labs (production-framed, harness-only, SIMULATED unless stated)

| M | Lab |
|---|---|
| M1 | Choose architectures for a support bot, an interview coach and an IVR replacement; write a decision memo |
| M2 | Convert mu-law 8 kHz fixtures to PCM and measure WER impact; bandwidth sheet for a contact centre (REAL arithmetic) |
| M3 | FastAPI duplex relay with bounded queues; jitter injection; diagnose a leaking queue from metrics |
| M4 | Fix an agent that interrupts itself using VAD event logs and interruption fixtures |
| M5 | Technical-term lexicon plus post-correction on Indian-English fixtures (consent-cleared or synthetic) |
| M6 | Hit a p95 budget on the harness waterfall and defend each cell |
| M7 | Add phone ingress to a web agent: decode mu-law stream, DTMF masking, handoff design |
| M8 | Reconnect and resume with sequence numbers under scripted drops; capacity plan for 2000 sessions |
| M9 | Cost per 30-minute session for two architectures with caps; consent and retention flow; voice threat model |
| M10 | Per-slice WER report and a simulated-caller regression test that fails above budget |
| M11 | Interview-voice-agent case study plus five-level defence of a real decision |

### LF-7 Currency and quality bar (as of October 2026)

| M | Visual or analogy | Top 3 misconceptions handled |
|---|---|---|
| M1 | Plumbing with a shutoff valve at every joint: voice is a stream that must be cancellable | S2S always better; voice is chat plus TTS; transcripts are optional |
| M2 | Photograph resolution vs file size (sample rate vs bitrate) | Upsampling restores quality; MediaRecorder gives raw PCM; Opus equals G.711 |
| M3 | Motorway (WebRTC, tolerates loss) vs registered post (WebSocket, retransmits and blocks) | WebRTC is always faster; unbounded queue is fine; browser key is fine with domain limits |
| M4 | A polite conversation: wait, overlap, yield; the agent must forget what was cut off | Stop audio is enough; one threshold fits all; echo is a volume problem |
| M5 | Draft vs final manuscript (interim vs final) | One WER is accuracy; headline WER holds on your audio; send full answer to TTS |
| M6 | Relay-race waterfall: every handover loses time | Only LLM matters; averages suffice; speculate freely |
| M7 | Postal letter vs voice courier: SIP is the address label, RTP the courier | SIP carries audio; 8 kHz can be fixed by upsampling; DTMF is obsolete |
| M8 | Airport: gates drain before closing, passengers rebook | Voice is stateless; scale on RPS; reload is recovery |
| M9 | Taxi meter plus recording consent form | Cost equals LLM cost; record by default; voice biometrics are harmless |
| M10 | Exam with different question sets per student (slices) | WER alone; clean speech benchmark; vibe-based success |
| M11 | Courtroom cross-examination of your own numbers | Features equal depth; vendor blame; unmeasured claims |

Each lesson carries a "last reviewed" stamp and the "as of" date; examples are original.

## 9. Boss compliance log

- G-1: Ownership notes row added; asyncio, SSE choice, LLM streaming, observability, compliance, judge, platform design reduced to pointers.
- G-2/section 5: header 15.5 S / 10.5 D with per-module hours, assessment allowance about 2.0 h, S/D semantics stated.
- G-3/G-4/G-5/G-6/G-7: goal tags, level paths, bands, readiness, thresholds applied.
- G-8: tier language, no model names, dated Provider Note for frameworks.
- G-9: tags `voice.*` owned; secondary tags listed without credit. G-10: goal tag and senior-bar note on 33 seeds; placement diagnostics; harness tasks in coding bucket; `Q-B6-Mn-nn` IDs.
- G-11: Appendix LF-1 to LF-7. G-12: dated facts; no invented statistics; awareness-only compliance.
- R-B6-1: harness-only, no BYO keys. R-B6-2: fixture owner stated. R-B6-3: Challenge lesson 3.6, no new module. R-B6-4/5: M11 rewritten, A6 #17 pointer. R-A7-3: voice metrics owned, pointers to B5.M7 and A7.M9. R-B5-3: compliance <= 20 min, pointer to X1. R-A5-2/R-A2-5: pointers.
- Boss section 8 items 1-8 (B6): applied (Learner-Fit, M11, Challenge, harness labelling, metrics, compliance awareness, SIMULATED numbers, goal tags).
- Track lead B review (2026-10-06): see `_reviews/TrackLead_B_Report.md`. Applied: swapped sections so Learner-Fit Appendix is section 8 (G-11) and open items section 7; diagnostic IDs aligned to Q-B6-Mn-nn with placement tag; M7 lesson order so B6.M7.5 is the compliance lesson; M2-Q1 retagged voice.stt-quality; added S2S provider-held history truncation to M4 and M4-Q1; design rubric threshold per G-7; Challenge lesson hour allocation; company-pack goal-tag wording; named reviewer roles; each remaining [verify] given an exact check and owner, one inline marker removed.
