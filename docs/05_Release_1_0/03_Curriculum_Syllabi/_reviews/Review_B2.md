# Review log: B2 LLM Application Engineering (REV-B2, 2026-10-05)

## Issues found

| # | Severity | Issue |
|---|---|---|
| 1 | High | No seed verbal questions (required: 3 per module with ideal outline, weak answers, probes, difficulty, competency and cluster tags) |
| 2 | High | No reasoning/thinking-model coverage (effort/budget, reasoning tokens, state across tool turns); outdated for 2026 |
| 3 | High | No per-module lesson lists, question-bank targets, mastery focus or module badge criteria |
| 4 | High | Header incomplete: no placement-skip rule, outcomes not tied to competencies, no badge/certificate evidence, no owner/reviewer |
| 5 | Medium | Missing multimodal inputs, batch APIs, long-context vs RAG decision, token accounting (cached/reasoning tokens), streaming details (tool deltas, cancellation, usage events, proxy buffering), provider fallbacks, model lifecycle/deprecation |
| 6 | Medium | Evals, MCP, security only vaguely pointed to B5/B4; no scope-boundary table vs B3/B4/B5/B6/B7 |
| 7 | Medium | No final-assessment detail, no risks/freshness section, no [verify] flags, no open questions |
| 8 | Low | Sequencing: model/reasoning selection came too late; multimodal absent; cost module precedes security (kept, but eval basics follow security) |
| 9 | Low | Effort estimate too low for scope |

## Changes made (file rewritten in place, v1.1)

- Expanded to 12 modules (added M2 Model landscape and reasoning models, M6 Multimodal inputs; split streaming from context; M7 now includes long-context vs RAG decision).
- Added full header, scope-boundary table, module table with outcomes and pitfalls, per-module lesson lists (Simpler/Deeper), mastery focus, badge criteria, question-bank targets (12/6/4/8/3).
- Added 36 seed verbal questions (3 per module) with ideal outline, weak answers, probes, difficulty, competency and cluster tags.
- Added final assessment, risks/freshness table with [verify] flags, open questions.
- Effort raised to ~18 h Simpler / ~13 h Deeper. Model names removed; tier language used.

## Verification (web search, 5 Oct 2026)
Confirmed at doc level: Anthropic structured outputs (JSON outputs and strict tool use), 5-min and 1-hour prompt cache, extended thinking (minimum budget 1,024), Message Batches at 50% cost; OpenAI Responses API with `text.format` structured outputs, reasoning effort levels, prompt caching with extended retention defaults, Batch API. Exact parameter names, schema limits, cache minimums, prices and context sizes left as [verify]; I did not verify current context-window numbers and deliberately excluded them.

## Unresolved questions for the boss agent
1. Provider-neutral code samples: primary SDK plus adapter, or abstraction library?
2. Confirm ownership: B3 for RAG depth, B4 for MCP depth, B5 for OWASP LLM Top 10 depth, B6 for voice pipeline depth.
3. Sandbox testing policy: mocked providers only vs live-provider budget.
4. Is 12 modules (~13-18 h) acceptable against the track effort budget?
