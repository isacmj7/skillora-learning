# Verification Log B4-B7 (VER-4, 2026-10-05)

Method: web search plus direct fetch of primary pages where reachable (MCP spec, OWASP GenAI, Dynatrace, Anthropic engineering blog, Willison blog, Google Open Source Blog) and PyPI JSON for versions and dates. Status key: CONFIRMED, CORRECTED, OPEN (marker kept with reason).

## B4 Agents and Workflows

| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| 1 | MCP remote servers need authorisation [verify current spec revision] | CONFIRMED: current revision 2026-07-28, stateless (no initialize, no Mcp-Session-Id), OAuth issuer (RFC 9207) validation; Roots, Sampling, Logging, HTTP+SSE and Dynamic Client Registration deprecated | https://modelcontextprotocol.io/specification/2026-07-28/changelog | Marker replaced; table row extended |
| 2 | A2A "emerging layer" [verify]; table: v1.0 January 2026; under Linux Foundation Agentic AI Foundation alongside MCP | CORRECTED: v1.0 released March 2026 (12 March per secondary), Linux Foundation hosted since 23 June 2025; no source found for the AAIF claim | https://opensource.googleblog.com/2026/04/a-year-of-open-collaboration-celebrating-the-anniversary-of-a2a.html | Date corrected, AAIF claim removed |
| 3 | Multi-agent token cost multiples [verify figures] and table (4x, 15x, 80% variance) | CONFIRMED: agents about 4x, multi-agent about 15x chat tokens; token usage explains 80% of BrowseComp variance | https://www.anthropic.com/engineering/multi-agent-research-system | Marker replaced |
| 4 | Lethal trifecta coined by Willison, June 2025 | CONFIRMED: post dated 16 June 2025 | https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/ | Date made exact |
| 5 | Temporal-style engines: deterministic workflow code plus activities [verify specifics] | CONFIRMED: replay of workflow code against event history, determinism required, activity results recorded | https://docs.temporal.io/encyclopedia/event-history | Marker replaced |
| 6 | Prompt caching on stable prefixes [verify pricing and semantics] | CONFIRMED semantics (order tools, system, messages; 5 min and 1 h TTL; reads cheaper, writes dearer). No prices stated in lessons | https://platform.claude.com/docs/en/build-with-claude/prompt-caching | Marker replaced; pricing left to B2 provider notes |
| 7 | LangGraph 1.0 October 2025 | CONFIRMED: PyPI 1.0.0 2025-10-17; GA announced 22 Oct 2025; latest 1.2.12 | PyPI langgraph; secondary reports | Dates added |
| 8 | LangGraph 2026 additions (per-node timeouts, durable streaming) | OPEN: not found in release notes | n/a | Narrow [verify] kept |
| 9 | OpenAI Agents SDK, minor version [verify] | CONFIRMED: pre-1.0, latest 0.23.1 (2026-10-02) | PyPI openai-agents | Marker replaced |
| 10 | Microsoft Agent Framework GA April 2026; AutoGen maintenance-only | CONFIRMED: 1.0.0 on 2026-04-02; AutoGen maintenance mode 2 Oct 2025 | PyPI agent-framework; InfoQ and secondary reports | Marker replaced |
| 11 | Google ADK 2.0 shipped | CONFIRMED: Python 2.0.0 2026-05-19; latest 2.11.0; graph runtime, breaking changes | PyPI google-adk; https://google.github.io/adk-docs/2.0/ | Marker replaced |
| 12 | CrewAI, Pydantic AI, LlamaIndex, Claude Agent SDK active | CONFIRMED active on PyPI (versions listed in the file) | PyPI | Marker replaced |
| 13 | Provider tool search / deferred loading at two providers [verify] | CONFIRMED by vendor docs: Anthropic tool search tool and OpenAI Responses tool_search, both with defer_loading | https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool ; https://developers.openai.com/api/docs/guides/tools | Marker replaced; no limits quoted |
| 14 | OTel GenAI conventions Development status mid-July 2026 | CONFIRMED (secondary): moved to a dedicated repo 12 June 2026, still Development in August | https://john-hodge.com/blog/opentelemetry-genai-semantic-conventions/ | Wording refined |

## B5 Evaluation, Safety and LLMOps

| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| 15 | OWASP Top 10 for LLM Apps = 2025 edition, check for newer | CORRECTED: a 2026 edition was published 3 Aug 2026. Edition confirmed on the primary page; the entry list was read only from secondary summaries that disagree on order | https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/ ; https://cybersecuritynews.com/owasp-genai-llm-top-10-2026/ | Content updated, 2025 list kept as reference, narrow [verify] kept for exact 2026 numbering |
| 16 | OWASP agentic guidance title and version | CONFIRMED: Top 10 for Agentic Applications for 2026 (ASI01-ASI10), 9 Dec 2025; Secure MCP Server Development guide, 16 Feb 2026 | https://genai.owasp.org/resources/ | Marker replaced |
| 17 | EU Digital Omnibus on AI dates | CONFIRMED by law-firm sources: Regulation (EU) 2026/1744, OJ 24 July 2026, in force 27 July 2026; Annex III to 2 Dec 2027; Annex I to 2 Aug 2028; Art. 50 on 2 Aug 2026 with 2 Dec 2026 marking grace for systems already on the market | https://www.freshfields.com/en/our-thinking/blogs/technology-quotient/eu-ai-act-unpacked-34-the-final-digital-omnibus-on-ai-key-amendments-to-the-a-102nber ; https://www.nicfab.eu/en/posts/digital-omnibus-ai-official-journal/ | OJ reference added; [verify] narrowed to EUR-Lex article text, which was not read |
| 18 | DPDP Rules notified 14 Nov 2025; 12 and 18 month phasing | CONFIRMED (PIB for notification and 18-month period; secondary for 13 Nov 2026 and 13 May 2027 dates) | https://www.pib.gov.in/PressReleasePage.aspx?PRID=2190014&reg=3&lang=2 | Text confirmed |
| 19 | RBI, SEBI, IRDAI AI guidance [verify] | OPEN: RBI FREE-AI is a committee report (13 Aug 2025); SEBI consulted June 2025, final guidelines still pending per 2026 remarks; IRDAI not checked | https://kpmg.com/in/en/insights/2025/08/rbi-free-ai-committee-report-on-framework-for-responsible-and-ethical-enablement-of-artificial-intelligence.html | Context added; [verify] kept for binding status |
| 20 | Aadhaar handling rules | OPEN: storage restrictions exist (Aadhaar Act, UIDAI data-vault rules) but legal review is required | n/a | Marker kept |
| 21 | NIST AI RMF revision status | OPEN (secondary only): 1.0 remains; revision directed July 2025; no draft by late Sept 2026 per tracker. NIST page timed out | https://www.normgrid.com/nist-ai-rmf/timeline/ | Narrow [verify] kept |
| 22 | PSI/KL on raw embeddings unreliable [verify] | CONFIRMED technically (high dimensionality); practitioner sources recommend centroid cosine, MMD, domain classifier | https://machinelearningmastery.com/monitoring-embedding-drift-in-production-scikit-llm-pipelines/ | Marker replaced; MMD added |
| 23 | Arize acquisition not confirmed closed | CORRECTED: completed 1 Oct 2026 | https://www.dynatrace.com/news/blog/dynatrace-completes-acquisition-of-arize/ | Corrected in M7 and section 6 |
| 24 | Langfuse acquired by ClickHouse Jan 2026 | CONFIRMED (16 Jan 2026) | https://www.orrick.com/en/News/2026/01/Open-source-LLM-Observability-Langfuse-Acquired-by-ClickHouse-Inc | No change |
| 25 | NeMo Guardrails latest v0.20.0 | CORRECTED: latest 0.24.1 (2026-09-16); beta label not rechecked | PyPI nemoguardrails | Updated |
| 26 | Presidio actively maintained | CONFIRMED: presidio-analyzer 2.2.364, 2026-07-22 | PyPI | Source added |
| 27 | OTel gen_ai.* Development, dedicated repo June 2026 | CONFIRMED (secondary) | as #14 | No change |

## B6 Realtime and Voice

| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| 28 | Pipecat stable 1.x April 2026; LiveKit Agents 1.x [verify versions] | CONFIRMED: Pipecat 1.0.0 2026-04-14 (latest 1.12.0); LiveKit Agents 1.0.0 2025-04-10 (latest 1.8.4) | PyPI | Marker replaced (two places) |
| 29 | WebTransport Baseline since Safari 26.4 (March 2026) | CONFIRMED (secondary) | https://webrtc.ventures/2026/04/webtransport-is-now-baseline-what-it-means-for-real-time-media/ | No change |
| 30 | Safari/iOS AudioWorklet and device-switch quirks | OPEN: only partial WebKit facts found (26.1 devicechange fix, 26.4 multi-mic on macOS) | https://webkit.org/blog/17862/webkit-features-for-safari-26-4/ | [verify: device test needed] kept |
| 31 | TRAI recording, DLT, AI-call disclosure | OPEN: vendor blogs conflict (30-second spoken disclosure vs declaration on DLT only); no TRAI primary text read. Do not state specifics | e.g. https://chambers.com/articles/trai-s-crackdown-on-spam-calls-and-ai-driven-telemarketing | Reasoned [verify] kept (two places) |
| 32 | Indic STT/TTS leaders; Indian CPaaS and carriers | OPEN by design: settled by bake-off, vendor claims volatile | n/a | Reasoned [verify] kept |
| 33 | S2S pricing; provider zero-retention and DPA terms | OPEN by design: no prices or terms stated; dated data file | n/a | Reasoned [verify] kept |
| 34 | DPDP phasing in B6 section 6 | CONFIRMED (see #18) | PIB | No change |
| 35 | G.711 PCMU/PCMA regional note | Stable standards fact (not volatile); not re-searched | n/a | No change |

## B7 Fine-tuning, Model Selection, MLOps

| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| 36 | TRL v1.0 April 2026; 1.14.x late Sept | CORRECTED: 1.0.0 published 2026-03-30; 1.14.1 on 2026-09-29 | PyPI trl | Date corrected |
| 37 | vLLM about 0.29-0.30 | CORRECTED: 0.31.0 released 2026-10-05 (0.29.0 on 09-09, 0.30.0 on 09-22) | PyPI vllm | Updated |
| 38 | vLLM V1 prefix caching default, chunked prefill | CONFIRMED by vLLM docs via search | https://docs.vllm.ai/en/stable/configuration/optimization/ | Kept |
| 39 | Multiple speculative-decoding drafters; SGLang chunked prefill | OPEN: not rechecked | n/a | Flagged in text as not rechecked |
| 40 | PEFT/LoRA | PEFT latest 0.21.2 (2026-10-01) | PyPI | Added |
| 41 | DPDP and sector rules via X1 and counsel [verify] | OPEN by design (legal) | n/a | Reasoned [verify] kept |
| 42 | GPU price and availability in Indian cloud regions | OPEN: volatile, never hard-code | n/a | Marker kept |
| 43 | Provider terms on training from outputs and fine-tune retention | OPEN: per provider, dated | n/a | Marker kept |

## Summary
Resolved or corrected: 31 items. Corrections of substance: A2A date and AAIF claim, OWASP 2026 edition, Arize closing, NeMo version, TRL date, vLLM version. Open markers remaining: B4 1, B5 5 (OWASP 2026 numbering, EU article text, NIST, regulators, Aadhaar), B6 8 (reasoned), B7 4.
