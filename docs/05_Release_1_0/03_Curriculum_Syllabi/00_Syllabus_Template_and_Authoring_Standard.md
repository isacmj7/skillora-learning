---
name: Skillora Learning 1.0 — Syllabus Template & Authoring Standard
version: 1.1
date: 2026-10-06
purpose: One template every 1.0 course syllabus follows, so authoring, review, question banks, visuals, badges and interview coverage are consistent
related: ../01_Scope/Skillora_Learning_Release_1_0_Web_Scope_and_Founder_Curriculum.md, ../../02_Low_Level_Design/02_Learning_Experience/Skillora_Learning_Visual_Learning_System.md
---

# Syllabus Template and Authoring Standard

## 1. COURSE HEADER (REQUIRED)

| Field | Content |
|---|---|
| Course ID / title / track | e.g. A1 — Python Internals for Interviews / Track A |
| Audience and prerequisites | Level L1–L4 assumptions; what placement test result lets a learner skip |
| Outcomes | 5–8 measurable "can do" statements, each tied to an interview competency |
| Estimated effort | Hours at Simpler and Deeper levels |
| Interview coverage | Which Interview Master competencies/question tags this course feeds |
| Badge and certificate | Course badge + module badges; evidence needed |
| Authoring owner / SME reviewer | Named roles |

## 2. MODULE TEMPLATE (REPEAT PER MODULE)

| Element | Requirement |
|---|---|
| Module outcome | One sentence, testable |
| Lesson list | 3–6 lessons, each with Simpler and Deeper variants |
| Visuals | At least one diagram/step-through per lesson (Mermaid → SVG at publish, or interactive); alt text |
| Worked example | Real code (Python/SQL) or design, annotated |
| Practice | 3–8 tasks in the IDE (visible + hidden tests) or design exercises |
| Pitfalls and misconceptions | Common wrong answers interviewers listen for |
| Interview questions | Tagged by competency and difficulty (target counts per module in §3) |
| Mastery check | 6–10 items, pass threshold 80% (configurable), revision loop on misses |
| Module badge | Criteria and evidence |

## 3. QUESTION-BANK TARGETS (PER MODULE, MINIMUM FOR 1.0)

| Type | Count |
|---|---|
| MCQ / concept check | 12 |
| Code-output / predict-the-result | 6 |
| Coding / SQL tasks (with hidden tests) | 4 |
| Verbal interview questions with follow-up trees (voice and text) | 8 |
| Scenario / design questions | 3 |

Every verbal question has: ideal answer outline, rubric (competency, level anchors), common mistakes, 2–3 follow-up probes, tags (track, module, difficulty, company-style cluster).

## 4. QUALITY GATES (BEFORE PUBLISH)

1. SME technical review (accuracy, current versions).
2. Pedagogy review (sequencing, level variants, cognitive load).
3. Visual review (diagram correctness, accessibility).
4. Question-bank review (answer keys, rubric clarity, no duplicates, difficulty calibration from pilot).
5. Pilot with at least 3 learners/reviewers; fix top confusion points.
6. Version stamped; "last reviewed" date; review cadence (fast-moving AI content: every 3 months; stable content: every 12 months).

## 5. CONTENT RULES

- Original content only; no copied course text or leaked interview questions; cite official docs for facts.
- Code runs on pinned versions in the sandbox; each example is tested in CI.
- Indian context where useful (examples, HR norms), globally valid technically.
- AI/LLM content carries a "last reviewed" date and a note about model/version volatility.

## 6. LEARNER-FIT LAYER (REQUIRED IN EVERY SYLLABUS — WORLD-CLASS, PERSONALISED STANDARD)

Every syllabus must be oriented to **the learner's level of experience and goal**, not generic. The reference learner (user zero) is a **senior (11+ years) engineer, ~4 years Python, strong in FastAPI/Django/Flask, SQL, Redis, Docker, AWS Lambda/S3, LLM APIs and voice agents, targeting Senior Python Backend and AI Engineer roles**. The platform must serve other learners too, so the layer is expressed as rules, not as one person's biography.

| Element | Requirement |
|---|---|
| **Entry diagnostics** | 5–10 placement items per module (feeds the placement test) with explicit **skip / skim / study / deep-study** decisions by score band |
| **Level paths (L1–L4)** | For each module: what L1 (new), L2 (junior-mid), L3 (experienced), L4 (senior/expert) does differently — content depth, time, practice mix; the Simpler/Deeper switch maps to these |
| **Goal tags** | Every module and question tagged **MUST / SHOULD / NICE** for each target role: Senior Python Backend, AI Engineer, Python+AI hybrid; and for company-style clusters |
| **Experience bridge ("From what you already know")** | For L3/L4 learners: map the concept to commonly held experience (e.g. FastAPI/Django/Postgres/Redis/AWS/LLM-API work) and state the gap between *using* it and *explaining it at senior depth* |
| **Senior-bar framing** | For each module: what separates a mid-level answer from a senior and a staff answer (signals interviewers look for: trade-offs, failure modes, scale, operations, mentoring, cost) |
| **Interview-weighted time** | Study-time allocation weighted by how often/heavily the topic appears in target-role interviews (state the rationale, mark [verify] if based on judgement) |
| **Adaptive hooks** | Mastery thresholds, remediation links to prerequisite modules, "challenge" extensions when the learner is strong, and which modules the adaptive engine may reorder |
| **Applied labs from real-world work** | At least one practice task per module framed on realistic production scenarios (backend, IoT, SaaS, AI assistants, voice, enterprise) rather than toy examples |
| **Interview rehearsal link** | Which Interview Master modes exercise this module (voice drill, coding, design canvas, behavioural, project deep-dive) and the target readiness criterion |
| **Readiness criterion** | An explicit, observable "ready for interviews on this topic" definition (score thresholds, timed performance, voice mock result) |
| **World-class quality bar** | Currency (dated, verified or [verify]); original examples; accuracy; clear mental models with at least one memorable visual or analogy per module; common-misconception handling; no filler |

## 7. FRONT MATTER, STATUS STAMPS AND FILE CONVENTIONS (added by the boss, 2026-10-06; binding for all 19 final syllabi and any later edit)

Every syllabus starts with YAML front matter: `name`, `course_id`, `track`, `version` (content version, `'1.0'` for Release 1.0), `status`, `date`, `last_reviewed`, `integration_pass`, `draft_lineage`, `boss_approved`, `conditions`.

`status` takes exactly one of two stamps:
- `FINAL v1.0 — boss approved (AI-authored; human SME review pending)` when only routine SME review, currency re-reads and engineering CI checks remain.
- `APPROVED WITH CONDITIONS — see Human Action Register` when a legal, professional, engineering-spike or fixture dependency remains. The `conditions` key lists the register IDs (HAR-nn) in `_reviews/Boss_Final_Approval.md`.

Conventions the importer relies on (all rulings are in `_Boss_Decisions_and_Ownership_Matrix.md` section 11):
1. Module and lesson IDs `A5.M4` and `A5.M4.2`; question IDs `Q-<course>-M<n>-<nn>`; placement items carry a `placement` tag. Legacy ID forms and difficulty words are normalised by the importer alias table (FR-10).
2. A seed's goal tag and senior-bar note may be inherited from the module row where the syllabus says so (FR-05).
3. Learner levels `L1` to `L4` are learner levels only; rubric anchors use scores (1-5), except C1's documented answer-level anchors.
4. Any change to hours, ownership, tags or module numbering after final approval needs a boss ruling and a matching edit in `05_Integration/` (the syllabus wins over integration documents unless it breaks the ownership matrix).
5. Status stamps are changed only by the boss or a named human reviewer; an edit to a FINAL syllabus that changes content resets it to review.
6. Mermaid blocks use balanced fences and plain flowchart, stateDiagram, erDiagram or gantt syntax only.

