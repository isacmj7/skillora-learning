# 03 — Prompts

Execution inputs for AI/dev agents. They reference the High-Level and Low-Level documents (by file name) and must not contradict them; the Master Specification v2 wins on conflict.

| Subfolder | Contents |
|---|---|
| `01_Product_Master_Prompt` | Product-wide build prompt: curriculum seed content (admin knowledge-base seed, not code), data model, tutor behaviour, phases |
| `02_Client_Build_Prompts` | Web (learner app + Admin Portal surface) and Mobile (iOS/Android) build prompts, v2 |

Tip: when giving a prompt to a build agent, also supply the Master Specification, the MVP Roadmap and the relevant Low-Level specs, and tell it which roadmap phase to implement.
