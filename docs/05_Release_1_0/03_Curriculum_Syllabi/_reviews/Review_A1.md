# Review log — A1 Python Internals for Interviews

Reviewer: REV-A1 | Date: 2026-10-05 | File reviewed and edited in place: A1_Python_Internals_for_Interviews.md

## Verification performed
Web searches confirmed (secondary sources, python.org docs snippets): free-threading officially supported but optional in 3.14 (PEP 779); experimental JIT in official macOS/Windows 3.14 binaries, not for production; Python 3.15 scheduled final release 1 Oct 2026 (rc2 on 1 Sep); 3.15 adds lazy imports (PEP 810), built-in frozendict and a sampling profiler; JIT gains roughly 8–13% in 3.15 and still experimental. Claims of "free-threading no longer experimental in 3.15" and default status were not confirmed.

## Issues found

| # | Severity | Issue |
|---|---|---|
| 1 | High | Template non-compliance: no per-module lesson lists, question-bank targets, seed verbal questions (rubric, weak answers, probes, tags), module/course badge criteria, risks, freshness notes, open questions, review cadence, owner/reviewer fields |
| 2 | High | Missing topics senior interviews probe: import system, exceptions (incl. exception groups), logging, dataclasses/enums, pattern matching, memoryview/buffer protocol, pickling and serialisation, hash/eq contract and container internals, string/int internals |
| 3 | Medium | Free-threading described only as "status as of review date"; no PEP 703/779 phases, no JIT status, no version baseline |
| 4 | Medium | Outcomes were a topic list, not measurable "can do" statements tied to competencies |
| 5 | Medium | No overlap boundaries with A2/A3/A5/A7/A8; async/coroutines not pointed to A2 |
| 6 | Medium | Placement-skip semantics undefined; effort figures inconsistent (Deeper listed as shorter without explanation) |
| 7 | Low | "Coroutines origins" in M4 and "C-extensions overview" in M8 vague; Pydantic and packaging mixed into one thin typing module; interview-coverage tags too few |
| 8 | Low | Final assessment lacked item counts, timed coding task, rubric and pass criteria |

## Changes made
- Rewrote the file to the brief's 7-part structure: header table, version baseline, sequencing and overlap table, module table (11 modules, with outcome, topics, visuals, practice, pitfalls), per-module detail blocks (5 lessons each with Simpler/Deeper notes, mastery focus, badge criteria, question-bank targets 12/6/4/8/3), 33 seed verbal questions (ideal outline, weak answers, probes, difficulty, competency, cluster tag, shared rubric anchors), final assessment, risks and freshness table, open questions.
- Restructured modules: M1 object model (+hashing, containers, strings); M2 memory/GC; M3 execution, GIL, free-threading, JIT (async pointer to A2); M4 iterators/generators; M5 functions/decorators/context managers; M6 data model/descriptors/metaclasses; M7 new modern-Python/typing module (dataclasses, enums, match, typing); M8 new exceptions and logging; M9 new import system/packaging/environments; M10 new bytes/buffers/serialisation; M11 performance (profiling tools modernised).
- Effort changed to about 22 h Simpler / 15 h Deeper; 6-month cadence set for fast-moving modules.
- Marked **[verify]** on: 3.15 final release, free-threading 3.15 status, JIT numbers, incremental GC in 3.14.x/3.15, PEP 758, PEP 751 adoption, frozendict, sampling profiler name, tool landscape, EOL dates, deferred annotations details.

## Unresolved questions for the boss agent
1. Sandbox Python version (3.14 vs 3.15) and availability of a free-threaded build for M3 practice.
2. Ownership of the concurrency decision guide (A1 pointer vs A2 full) to avoid duplicate items.
3. Whether container internals stay in M1 or move wholly to A3.
4. Whether logging fundamentals stay in A1 with A7 covering only platform observability.
5. Whether 11 modules per course is acceptable relative to the other Track A courses (A4 and B2 module counts not compared here).

## Notes
Authoring bank remaining: 5 additional verbal questions per module (55 total), plus MCQ/code-output/coding/scenario items per template targets, still to be authored. Human SME review should confirm all [verify] items against official What's New pages before publication.
