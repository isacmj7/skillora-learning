---
name: A1 — Python Internals for Interviews
course_id: A1
track: A (Backend)
version: '1.0'
status: 'FINAL v1.0 — boss approved (AI-authored; human SME review pending)'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-03 (see _reviews/Boss_Final_Approval.md)
---

# A1 — Python Internals for Interviews (Track A)

Version stamp: **2.0 (wave-2 upgrade by SME-A1, 2026-10-05; builds on REV-A1 v1.1)**. All version facts are dated October 2026 and were checked on 2026-10-05 against python.org release notes and docs (primary) or named secondary sources; anything still unconfirmed is marked **[verify]** with the exact check needed. Boss rulings (G-n, R-A1-n) override earlier text; see the compliance log in section 10.

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | A1 — Python Internals for Interviews / Track A (Senior Python Backend Mastery) |
| Audience | Learners at L1-L4 (G-4). Reference learner: senior engineer, 11+ years, about 4 years Python, daily FastAPI/Django/Flask, who can use these features but cannot always explain them at interview depth. L1-L2 follow the Simpler path; L3-L4 the Deeper path with placement skips. |
| Prerequisites | Working Python (classes, comprehensions, virtualenvs). No internals knowledge assumed. |
| Placement skip | Per module, 5 diagnostic items (section 8, LF-2) and the G-5 bands: **>= 85 skip lessons and take the mastery check (plus optional Challenge); 70-84 skim (Deeper-only lessons plus check); 50-69 study; < 50 deep-study (Simpler path plus remediation links)**. A module is "mastered by placement" only if its mastery check also passes at 80 percent. No band waives the final assessment. Whole-course test-out only via the final mastery check at 85 percent plus a Senior-bar voice mock (G-5). |
| Outcomes (measurable, each tied to a competency) | O1 Trace names, objects, mutability, hashing and copying and predict output of unseen code (`python.core`). O2 Explain refcounting, the cyclic GC (including the 3.14 incremental-GC episode), weak references and diagnose a leak with `tracemalloc` snapshot diffs (`python.memory`). O3 State what the GIL guarantees and does not, the free-threading and JIT status as of the review date, and interpret a 3.14 vs 3.14t benchmark (`python.concurrency-basics`). O4 Build lazy iterator/generator pipelines, explain generator state and cleanup, and write closures, decorators (plain and parameterised) and context managers correctly (`python.core`). O5 Explain attribute lookup, descriptors, MRO, metaclasses and `__init_subclass__` at interview depth (`python.oop-datamodel`). O6 Use dataclasses, enums, `match`, type hints and exception groups idiomatically and set up stdlib logging, imports and packaging (`python.modern`, `python.exceptions-logging`, `python.import-packaging`). O7 Handle bytes/buffers and choose serialisation safely (`python.serialization`). O8 Profile a slow function, separate algorithmic from micro-optimisation and justify the fix with measurements (`python.performance`). |
| Estimated effort | **About 22 h Simpler / about 15 h Deeper, plus about 2 h assessment allowance** (boss table section 5; Deeper capped at 14 h when M10 is skimmed by band, R-A1-5). Module hours in section 3 sum exactly to these figures. Simpler = L1-L2 route with full scaffolding. Deeper = L3-L4 route, foundations skimmed per placement, senior-bar and Challenge content included, hours net of typical skip-ahead, so it is shorter in wall-clock time but harder per hour. |
| Interview coverage tags (owned) | `python.core`, `python.memory`, `python.concurrency-basics`, `python.oop-datamodel`, `python.modern`, `python.exceptions-logging`, `python.import-packaging`, `python.serialization`, `python.performance`. Secondary (no credit): `api.fastapi-async` (A2), `sec.supply-chain` (A8). |
| Badge and certificate | Module badge: mastery check >= 80 percent (G-7) plus all module practice tasks passing hidden tests. Course badge "Python Internals": all 11 module badges, final timed mastery check >= 80 percent, timed coding task >= 70 percent hidden tests with stated complexity, and voice mock at "Meets" (average >= 3.0, none < 2). Interview readiness is a stricter, separate criterion (section 7). Certificate evidence: scores, practice pass records, mock transcript and rubric report. |
| Authoring owner / reviewer | SME agent A1 (wave 2, v2.0); REV-A1 (v1.1); human senior Python SME: to be assigned |
| Review cadence | 6 months for M2, M3, M7, M9, M11 (GC history, free-threading, JIT, typing, packaging, profiling tools); 12 months for the rest. Re-review within 4 weeks of each CPython feature release (October each year). Next review: April 2027, or within 4 weeks of 3.15.1. Last reviewed 2026-10-05. |

### Version baseline (as of 5 October 2026, G-8)

- **Sandbox**: Python **3.14 default**; 3.13 and 3.15 selectable for version-difference lessons; **free-threaded 3.14t selectable for A1.M3 (and A2.M1) only**. 3.12 is the minimum idiom floor. Switch the default to 3.15 only when 3.15.1 or later ships and key wheels exist (G-8); this is a future decision gate, re-check wheel availability at that time.
- **3.15 status (python.org Python Insider, 2026-10-02)**: 3.15.0rc3 is the final planned release candidate; a surprise rc3 was added because of last-minute lazy-import blockers, and 3.15.0 final is postponed to **2026-10-09**. So 3.15 is a release candidate on the review date. Confirmed in the 3.15 What's New: PEP 810 explicit lazy imports (`lazy import`), PEP 814 built-in `frozendict`, PEP 661 built-in `sentinel`, PEP 798 unpacking in comprehensions, PEP 686 UTF-8 default encoding, PEP 799 Tachyon sampling profiler (`profiling.sampling`) in a new `profiling` package, PEP 803 stable ABI for free-threaded builds (`abi3t`), PEP 831 frame pointers on by default, upgraded experimental JIT (reported 7-8 percent geometric mean over the standard interpreter on x86-64 Linux and 11-12 percent over the tail-calling interpreter on AArch64 macOS), tail-calling interpreter in official Windows 64-bit binaries, official macOS binaries install free-threading support by default.
- **Free-threaded CPython**: experimental in 3.13 (PEP 703); **officially supported but optional in 3.14 (PEP 779, phase II)**; the default-build decision (phase III) is left to a future PEP with no date, so never claim it is the default. A secondary source describes free-threading as "no longer experimental" in 3.15, but that wording does not appear in the 3.15.0rc3 What's New or free-threading HOWTO, so do not quote it; the status is PEP 779 "supported but optional" and not a default change (verified 2026-10). Single-thread overhead: the official HOWTO says the average pyperformance overhead ranges from about 1 percent on macOS aarch64 to 8 percent on x86-64 Linux, depending on workload and hardware (python.org 3.14 and 3.15 free-threading HOWTO, verified 2026-10).
- **JIT**: experimental since 3.13 (PEP 744); shipped in official macOS/Windows 3.14 binaries; still experimental and not for production in 3.15. Teach the idea and the measuring method, not the percentages.
- **Garbage collector**: the incremental cycle GC shipped in 3.14.0-3.14.4 and was **reverted to the 3.13 generational GC in 3.14.5 and later and in 3.15** after memory-pressure reports (3.15 What's New, gc section; confirmed 2026-10-05). Teach the generational GC as current and the incremental GC as a dated case study.
- **Other confirmed 3.14 facts**: deferred annotation evaluation (PEP 649/749); `except`/`except*` without parentheses (PEP 758); `concurrent.interpreters` (PEP 734).
- **Packaging**: PEP 751 `pylock.toml` is supported as an export/import format by uv, with experimental `pip lock` (25.1) and experimental `pip install -r pylock.toml` (26.1), PDM as a producer (secondary sources, May 2026); tools keep native lockfiles as primary. Teach the standard plus a replaceable "current tools" box.
- **End of life**: 3.9 is EOL; 3.10 reached end of life on 2026-10-01 (PEP 619; 3.10.22 was the final security release; verified 2026-10).

## 2. Sequencing and overlap rules

Default sequence: M1 → M2 → M3 → M4 → M5 → M6 → M7 → M8 → M9 → M10 → M11 (the adaptive engine may reorder per section 8, LF-5). M1-M3 are the foundation (objects, memory, execution); M4-M6 language mechanics; M7-M10 the modern and practical layer; M11 depends on everything.

| Neighbouring course | A1 owns (depth, labs, items) | Pointer only (G-1: one lesson section of 20 minutes or less, no lab, at most one `ref` item) |
|---|---|---|
| A2 Concurrency and Async (O-4) | GIL semantics, free-threading phases, JIT status, generators as the origin of coroutines (M4) | The "which concurrency tool" decision guide is **owned by A2.M1** (R-A1-3); A1.M3 keeps one paragraph and one `ref` item |
| A3 DSA (O-1) | Mechanism of dict/set hashing, list over-allocation, str and int representation (R-A1-1); one hash-table probe animation | A3.M1 owns the cost model and complexity exercises; A1 holds no complexity-table exercise |
| A5 API and Backend Architecture | Dataclass, enum, typing and Pydantic-boundary mechanics (M7) | FastAPI/Django design, request validation architecture -> A5.M2 |
| A7 Cloud, DevOps and Observability | stdlib `logging` mechanics (R-A1-4): hierarchy, handlers, `dictConfig`, `QueueHandler`, contextvars filter (M8) | Structured logging at platform level (JSON, correlation IDs in pipelines, cost, PII) -> A7.M9; Lambda cold-start platform tuning -> A7 |
| A8 Testing, Quality and Security | How pickle executes code and why it is unsafe (M10); dependency mechanics (M9) | Security policy, supply-chain risk, pytest strategy -> A8.M7-M8 |
| A4 Databases | Nothing beyond context managers and exceptions used by DB code | ORM and DB behaviour -> A4.M10 |

## 3. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice tasks | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Object model and identity (S 2.25 h / D 1.5 h) | Predict the behaviour of references, mutation, hashing and copying in unseen code. | Names vs objects; `id`, `is` vs `==`; mutability; small-int cache (-5 to 256) and string interning as implementation details; hash/eq contract and why mutable objects are unhashable; dict (insertion-ordered, open-addressing hash table) and set internals; list over-allocation; `str` as Unicode code points (PEP 393) vs `bytes`; arbitrary-precision `int`; float pitfalls and `Decimal`; shallow vs deep copy, `copy` protocol; mutable default arguments; `__slots__` (what it saves, what it breaks); immortal objects (3.12) | Name-to-object diagram; mutation aliasing step-through; hash-table probe animation | Predict-the-output set (aliasing, defaults, `is`); fix 5 aliasing bugs; implement a class with correct `__eq__`/`__hash__` | Treating `is` as a faster `==`; "Python is pass-by-value/reference" (correct: pass by assignment of object references); relying on interning; mutable default args; `a = b = []`; claiming `__slots__` is always a win |
| M2 | Memory management and garbage collection (S 2.25 h / D 1.75 h) | Explain how CPython reclaims memory and find a leak in a running service. | Reference counting and `sys.getrefcount`; deterministic destruction; `__del__` caveats; cyclic GC and generations (`gc` module, thresholds, `gc.freeze`, `gc.get_objects`); incremental GC as a dated case study (shipped in 3.14.0-3.14.4, reverted to the generational GC in 3.14.5+ and 3.15, confirmed in 3.15 What's New 2026-10-05; Django `migrate` memory growth mitigated with `gc.collect()`); weak references (`weakref`, `WeakValueDictionary`, finalizers); pymalloc arenas and why RSS rarely shrinks; leaks in services (unbounded caches, closures, global registries, exceptions holding tracebacks, ORM identity maps); `tracemalloc` snapshot diffs, `objgraph`, `memray` (current and maintained as of 2026-10) | Refcount timeline; cycle collection animation; generation diagram; allocator arena sketch | Find and fix a leak in a sample service using `tracemalloc` snapshot diff; implement a cache with `weakref`; demonstrate a cycle with `__del__` | "Python has no memory leaks because of GC"; saying GC is the main reclamation mechanism (refcount is); believing `del x` frees memory; confusing memory returned to the allocator with returned to the OS; ignoring exception tracebacks holding frames |
| M3 | Execution model, the GIL and free-threading (S 2.25 h / D 2.0 h) | Explain what runs when you execute Python and choose a concurrency model with justification. | Compile to bytecode, `dis`, `.pyc`, eval loop; specialising adaptive interpreter (3.11, PEP 659); experimental JIT (PEP 744; still experimental in 3.15, reported 7-8 percent geometric mean gain on x86-64 Linux) and tail-calling interpreter (build-dependent, official Windows 3.15 binaries); what the GIL protects (interpreter state, refcounts) and what it does not (compound operations such as `x += 1`, check-then-act); switch interval; I/O releases the GIL; C extensions and NumPy releasing it; free-threaded build (`python3.14t`, PEP 703/779): biased/deferred refcounting, per-object locks, single-thread overhead, extension compatibility and `sys._is_gil_enabled()`, `PYTHON_GIL`; per-interpreter GIL and sub-interpreters (3.12 PEP 684, `concurrent.interpreters` in 3.14 PEP 734); one-paragraph pointer to the concurrency decision guide owned by A2.M1 (R-A1-3); async as a pointer only; `abi3t` stable ABI for free-threaded builds (3.15) | Bytecode walk-through; GIL hand-off timeline; GIL vs free-threaded side-by-side timeline | Disassemble functions and explain; 3.14 vs 3.14t benchmark lab on CPU-bound code (the only multi-thread lab in A1); demonstrate a race that survives the GIL | "The GIL makes my code thread-safe"; "Python can't do parallelism"; "threads are useless in Python"; outdated claim that free-threading is unavailable or that it is the default; ignoring I/O-bound vs CPU-bound distinction |
| M4 | Iterators and generators (S 1.75 h / D 1.0 h) | Build and reason about lazy pipelines and explain generator state and cleanup. | Iterable vs iterator protocols (`__iter__`, `__next__`, `StopIteration`); `for` desugaring; generator functions, `send`, `throw`, `close`, `GeneratorExit`; `yield from` and return values; generator expressions; `itertools`; one-shot exhaustion; generators as the ancestor of coroutines (pointer to A2); lazy evaluation and memory behaviour; streaming file/DB-cursor processing; `contextlib.contextmanager` generator link | Generator state machine (created, running, suspended, closed); pipeline dataflow | Build a streaming ETL pipeline over a large file; implement a custom iterator class vs generator; chunking/batching helper | Iterating twice over a generator; confusing iterable and iterator; generator cleanup not running; building lists "to be safe"; swallowing `StopIteration` inside generators (PEP 479) |
| M5 | Functions, closures, decorators and context managers (S 2.0 h / D 1.0 h) | Write correct decorators and context managers and explain closure behaviour. | First-class functions; arguments (positional-only, keyword-only, `*args/**kwargs`); scopes (LEGB), `nonlocal`, `global`; closures and cell objects; late binding in loops; `functools` (`wraps`, `partial`, `lru_cache`, `cache`, `singledispatch`, `cached_property`); decorators with arguments, class-based decorators, decorating methods/async functions (pointer); `contextlib`, `__enter__`/`__exit__`, `ExitStack`; `contextvars` introduction; (3.14) deferred annotation evaluation (PEP 649/749, `annotationlib`) and its effect on decorators that inspect annotations | Closure cell diagram; decorator stacking onion; context manager lifecycle | Write retry, timing and cache decorators; async-safe timing decorator; context manager for a transaction/timer | Forgetting `functools.wraps`; late-binding lambda in a loop; `lru_cache` on methods leaking `self`; caching mutable results; swallowing exceptions in `__exit__`; decorator order confusion |
| M6 | Classes and the data model (S 2.25 h / D 1.5 h) | Explain attribute lookup, MRO, descriptors and metaclasses and apply them. | Attribute lookup order (data descriptors, instance dict, non-data descriptors, `__getattr__`); `property`; descriptors (`__get__`, `__set__`, `__set_name__`); bound methods; MRO and C3 linearisation, cooperative `super()`; `__new__` vs `__init__`; `__call__`; operator overloading and rich comparison; `__repr__`/`__str__`; class vs static methods; `type` as a class, metaclasses, `__init_subclass__`, `__class_getitem__`; ABCs vs `typing.Protocol` (nominal vs structural); composition vs inheritance; singleton and registry patterns done properly | MRO linearisation diagram; attribute lookup flow chart; metaclass instantiation timeline | Implement a validating descriptor; build a plugin registry via `__init_subclass__`; resolve a diamond-inheritance bug | Metaclass overuse when `__init_subclass__` or a decorator suffices; misunderstanding `super()` (it follows the MRO, not "the parent"); class attribute vs instance attribute shadowing; `__eq__` without `__hash__` |
| M7 | Modern Python: dataclasses, enums, pattern matching and typing (S 2.25 h / D 1.25 h) | Use modern language features and type hints idiomatically and explain their runtime reality. | `dataclasses` (`field`, `frozen`, `slots`, `kw_only`, `__post_init__`, `InitVar`, mutable-default rule); `NamedTuple`, `TypedDict`, `enum` (`Enum`, `IntEnum`, `StrEnum`, `Flag`); structural pattern matching (3.10: literal, sequence, mapping, class, guard patterns, pitfalls); walrus operator; type hints are not enforced at runtime; generics, `TypeVar`, `ParamSpec`, PEP 695 syntax (3.12) and defaults (3.13), `Protocol`, `Literal`, `Self`, `@override`, `Annotated`; deferred annotation evaluation in 3.14 (PEP 649/749, confirmed); template strings (PEP 750, 3.14) awareness; `mypy`/`pyright` (and newer checkers) vs runtime validation; boundary with Pydantic (runtime validation) and dataclasses; built-in `frozendict` (PEP 814) and `sentinel` (PEP 661) in 3.15, `TypedDict` extra items (PEP 728) awareness | Typing flow chart (static vs runtime); match-statement decision tree | Add types to an untyped module and pass strict mypy/pyright; refactor if/elif chains to `match`; convert dict-based code to dataclasses | "Type hints make Python statically typed"; mutable default in dataclass; using `match` as a switch only; `Optional` confusion; `isinstance` against generics; `Any` as an escape hatch everywhere |
| M8 | Exceptions and logging (S 1.75 h / D 1.25 h) | Design error handling and logging that survive production. | Exception hierarchy; `try/except/else/finally`; raising and chaining (`raise from`, `__cause__`, `__context__`); custom exception design; exception groups and `except*` (3.11); `add_note`; context managers and cleanup; EAFP vs LBYL; `assert` pitfalls (`-O`); `traceback` and frames-in-memory link to M2; `warnings`; stdlib `logging` architecture (loggers, handlers, formatters, filters, propagation, levels), `getLogger(__name__)`, `NullHandler` for libraries, `dictConfig`, `QueueHandler` for non-blocking logging, lazy `%`-style args, logging with `contextvars` for request IDs (platform-level logging belongs to A7) | Exception propagation and chaining diagram; logger hierarchy and propagation diagram | Design an exception hierarchy for a service client; configure logging with `dictConfig` and request-ID filter; handle an `ExceptionGroup` | Bare `except:`; catching `Exception` and passing; losing the cause; logging and re-raising (double logging); configuring logging at import time in a library; f-strings in log calls on hot paths; using `assert` for validation |
| M9 | Import system, packaging and environments (S 1.75 h / D 1.0 h) | Explain how imports resolve and set up reproducible, correctly packaged projects. | `import` mechanics: `sys.modules`, finders/loaders (`importlib`), `sys.path`, packages, namespace packages, `__init__.py`, `__main__`, relative vs absolute imports; module execution at first import; circular imports and fixes; import side effects and startup time; explicit lazy imports (PEP 810, `lazy import` statement, 3.15; last blockers fixed in rc3 on 2026-10-02); `__pycache__`; `pyproject.toml` (PEP 517/518/621), wheels vs sdists, build backends; virtualenvs and externally-managed environments (PEP 668); dependency resolution, lockfiles (`pylock.toml`, PEP 751: uv export/install, experimental `pip lock` and `pip install -r pylock.toml`, PDM producer; native lockfiles remain primary); tool landscape (pip, uv, Poetry, pip-tools) in a replaceable "current tools" box; linting/formatting (`ruff`), `mypy`/`pyright`; Python version support policy | Import resolution flowchart; circular-import dependency graph; wheel build pipeline | Break and fix a circular import; package a small library with `pyproject.toml` and build a wheel; trace imports with `-X importtime` | Believing modules are re-run on each import; `from x import *`; mutating `sys.path`; circular imports "fixed" with function-level imports without understanding why; pinning confusion (libraries vs applications) |
| M10 | Bytes, buffers and serialisation (S 1.25 h / D 0.75 h) | Choose a serialisation format safely and handle binary data without needless copies. | `bytes`, `bytearray`, `memoryview` and the buffer protocol; zero-copy slicing; `struct`; `readinto`; `pickle` (protocols, `__reduce__`, `__getstate__`, what cannot be pickled, protocol 5 out-of-band buffers) and why unpickling untrusted data is arbitrary code execution; `json` limits (types, precision, `default=`), `copyreg`; why `multiprocessing` needs picklability; alternatives (MessagePack, Protocol Buffers, Parquet) at one-paragraph depth; encoding/decoding errors and `utf-8` defaults | Buffer sharing diagram; pickle opcode/reduce flow | Parse a binary header with `struct` and `memoryview` without copies; implement custom `__reduce__`; demonstrate (in sandbox) why untrusted pickle is unsafe | Using pickle for external input or cross-version storage; "memoryview copies data"; confusing `str` and `bytes`; assuming `json` round-trips tuples, datetimes or large ints |
| M11 | Performance and profiling (S 2.25 h / D 2.0 h) | Find a bottleneck with measurement and fix it at the correct level. | Measure first: `timeit`, `pyperf`, `cProfile`, sampling profilers (`py-spy` for 3.14 and earlier; Tachyon, `profiling.sampling`, new in 3.15 per PEP 799), `memray`, `sys.monitoring` (3.12), flame graphs; algorithmic vs micro-optimisation; data-structure choice (pointer to the A3.M1 cost model; no complexity-table exercise here); caching (`lru_cache`, memoisation, invalidation); avoiding repeated attribute lookups and global access (when it matters); string building; comprehension vs loop; `__slots__` memory effect; C extensions, Cython, Rust bindings overview; NumPy vectorisation; multiprocessing/free-threading as scale-out (pointer to A2); startup-time and memory profiling; "when not to optimise" | Flame graph walk-through; cost-of-operation ladder | Profile and speed up a slow function with before/after evidence; vectorise a loop with NumPy; fix an accidental O(n²) | Optimising without profiling; micro-optimising hot paths that are not hot; claiming "Python is slow so use X" without evidence; confusing latency with throughput; cache without invalidation story |
| Ownership notes (G-1) | per module | **OWNS**: M1 container internals, hashing, str/int (O-1); M2 memory and GC (O-2); M3 GIL, free-threading, JIT (O-3); M4-M5 generators, iterators, decorators, context managers (O-5); M6-M7 data model, typing mechanics, dataclasses, enums, match (O-6); M8 exceptions and stdlib logging mechanics (O-7); M9 import system, packaging (O-8); M10 serialisation, pickle, buffers (O-9); M11 Python profiling (O-10). **POINTS**: concurrency decision guide -> A2.M1; complexity cost model -> A3.M1; Pydantic at schema level -> A5.M2; structured logging platform -> A7.M9; pickle security policy and dependency risk -> A8.M7-M8; async coroutines -> A2 | none | none | none | none |

## 4. Per-module detail blocks

Lesson IDs are `A1.Mn.k`. Question-bank targets per module (IDs `Q-A1-Mn-nn`, template minimums): 12 MCQ/concept, 6 code-output, 4 coding tasks with hidden tests, 8 verbal with follow-up trees (the 3 seeds in section 5 are the first 3; 5 to be authored), 3 scenario/design. Mastery check per module: 8 items (MCQ, code-output, one verbal), threshold 80 percent (G-7), revision loop on misses (re-served after a cooldown, then a fresh item). Placement diagnostics are 5 `placement`-tagged items per module drawn from the MCQ and code-output buckets (section 8, LF-2). Every lesson has at least one visual with alt text and one annotated worked example.

| Module | Hours S / D | Lessons (Simpler / Deeper notes) | Mastery-check focus | Module badge criteria |
|---|---|---|---|---|
| M1 | 2.25 / 1.5 | A1.M1.1 Names, objects and references (S: guided step-throughs; D: skim). A1.M1.2 Mutability, aliasing and copying (S: one concept per screen; D: `copy` protocol, `__copy__`). A1.M1.3 Equality, hashing and container internals (S: contract only; D: open addressing, resize, insertion order, the single probe animation). A1.M1.4 Strings, numbers and interning (S: Unicode vs bytes, `Decimal`; D: PEP 393 layout, immortal objects). A1.M1.5 `__slots__` and object layout (S: what it saves; D: key-sharing dicts, inheritance traps) | aliasing predictions, `is` vs `==`, hash/eq contract, mutable defaults | mastery >= 80%; 3 practice tasks pass hidden tests; zero aliasing misses on retake |
| M2 | 2.25 / 1.75 | A1.M2.1 Reference counting (S: timeline; D: immortal objects, free-threaded refcounting). A1.M2.2 Cyclic GC and generations (S: cycles and `gc`; D: thresholds, `gc.freeze`, the incremental-GC episode as a case study). A1.M2.3 Weak references and finalisation (S: `WeakValueDictionary`; D: `finalize`, `WeakMethod`). A1.M2.4 Allocators and memory growth (S: why RSS stays high; D: pymalloc arenas, fragmentation). A1.M2.5 Finding leaks in services (S: `tracemalloc` snapshot diff; D: `objgraph`/`memray`, identity maps and caches) | refcount vs GC roles, leak sources, weakref use, RSS vs Python heap | mastery >= 80%; leak-hunt task solved with a snapshot diff as evidence |
| M3 | 2.25 / 2.0 | A1.M3.1 Source to bytecode (S: `dis` walk-through; D: specialising interpreter, JIT and tail-call status). A1.M3.2 What the GIL does and does not protect (S: `x += 1` race; D: switch interval, what releases the GIL). A1.M3.3 Concurrency tool pointer (S and D: one paragraph and one `ref` item to A2.M1; 20 minutes or less). A1.M3.4 Free-threaded build and sub-interpreters (S: what and why; D: phases, deferred refcounting, per-object locks, `sys._is_gil_enabled()`, extension compatibility, `abi3t`). A1.M3.5 3.14 vs 3.14t benchmark lab (S: guided; D: unguided with written conclusion) | GIL guarantees, races under the GIL, correct reading of benchmark results, dated free-threading and JIT status | mastery >= 80%; benchmark submitted with a written conclusion matching measured results |
| M4 | 1.75 / 1.0 | A1.M4.1 Iterable vs iterator (S: desugared `for`; D: skim). A1.M4.2 Writing generators (S: pipelines; D: laziness and memory). A1.M4.3 `send`, `throw`, `close`, `yield from` (S: skim; D: lifecycle, PEP 479). A1.M4.4 `itertools` and streaming pipelines (S: recipes; D: batching, backpressure by design). A1.M4.5 Generators to coroutines (pointer to A2, 20 minutes or less) | protocol desugaring, exhaustion, laziness, cleanup | mastery >= 80%; streaming pipeline runs under a memory cap |
| M5 | 2.0 / 1.0 | A1.M5.1 Arguments and scope (S: LEGB; D: skim). A1.M5.2 Closures and late binding (S: cells; D: `__closure__`, `nonlocal`). A1.M5.3 Decorators (S: template; D: parameterised, class-based, methods, `ParamSpec`, annotation-inspecting decorators under PEP 649). A1.M5.4 `functools` toolkit (S: `wraps`, `partial`; D: `lru_cache` on methods, `cached_property`, `singledispatch`). A1.M5.5 Context managers (S: `with`; D: `ExitStack`, `__exit__` semantics, `contextvars`) | closure cells, `wraps`, argument-taking decorators, `__exit__` | mastery >= 80%; retry, timing and cache decorators pass hidden tests incl. exception and metadata cases |
| M6 | 2.25 / 1.5 | A1.M6.1 Attribute lookup and bound methods (S: order diagram; D: `__getattribute__`). A1.M6.2 Properties and descriptors (S: `property`; D: `__set_name__`, per-instance storage). A1.M6.3 Inheritance, MRO and `super` (S: diamond; D: cooperative `__init__`, mixins). A1.M6.4 Metaclasses and `__init_subclass__` (S: `__init_subclass__` only; D: `__prepare__`, conflicts). A1.M6.5 ABCs and Protocols (S: when to use which; D: nominal vs structural, `__class_getitem__`) | lookup order, MRO prediction, descriptor behaviour, metaclass justification | mastery >= 80%; descriptor and registry tasks pass hidden tests |
| M7 | 2.25 / 1.25 | A1.M7.1 Dataclasses and friends (S: basics; D: `slots`, `kw_only`, hashing rules). A1.M7.2 Enums (S: `Enum`/`StrEnum`; D: `Flag`, serialisation). A1.M7.3 Structural pattern matching (S: patterns; D: capture-vs-compare traps, `__match_args__`). A1.M7.4 Type hints, generics, Protocols (S: basics; D: PEP 695, `ParamSpec`, deferred annotations and `annotationlib`). A1.M7.5 Static vs runtime: Pydantic boundary (S: concept; D: when each, cost) | dataclass semantics, match pitfalls, what hints do and do not do | mastery >= 80%; untyped module passes a strict type-checker run; refactor keeps hidden tests green |
| M8 | 1.75 / 1.25 | A1.M8.1 Exception model and chaining (S: `try` forms; D: `__cause__`, `__context__`, frames in memory). A1.M8.2 Designing exception hierarchies (S: template; D: retryable vs permanent). A1.M8.3 Exception groups and `except*` (S: concept; D: `split`, `subgroup`, PEP 758). A1.M8.4 `logging` architecture (S: hierarchy and propagation; D: `lastResort`, filters). A1.M8.5 Production logging patterns (S: `dictConfig` recipe; D: `QueueHandler`, contextvars request ID; platform logging is A7.M9) | chaining, swallowing errors, propagation, logger hierarchy | mastery >= 80%; logging config task passes tests on routing and request-ID presence |
| M9 | 1.75 / 1.0 | A1.M9.1 How `import` works (S: `sys.modules`; D: finders and loaders, hooks). A1.M9.2 Packages, namespace packages, circular imports (S: fixes; D: partial initialisation). A1.M9.3 Startup cost and lazy imports (S: `-X importtime`; D: PEP 810 semantics and risks, Lambda cold-start link). A1.M9.4 Environments, dependencies and lockfiles (S: standard template; D: libraries vs applications, PEP 751, tools box). A1.M9.5 Building and distributing a package (S: wheel; D: backends, tags) | `sys.modules`, resolution order, circular fixes, import-time measurement | mastery >= 80%; package builds a wheel that installs in a clean environment; import-time budget task passes |
| M10 | 1.25 / 0.75 | A1.M10.1 `bytes`, `bytearray`, `memoryview` (S: copies vs views; D: buffer protocol, `cast`, resize locking). A1.M10.2 Binary parsing with `struct` (S: header parse; D: zero-copy). A1.M10.3 `pickle` internals and danger (S: why unsafe; D: `__reduce__`, protocol 5, Celery/queue exposure). A1.M10.4 JSON and format trade-offs (S: round-trip traps; D: `default=`, schema evolution, alternatives in one paragraph) | zero-copy, pickle security, round-trip pitfalls | mastery >= 80%; zero-copy parser passes buffer-identity tests |
| M11 | 2.25 / 2.0 | A1.M11.1 Measuring correctly (S: `timeit`; D: `pyperf`, noise). A1.M11.2 Profilers and flame graphs (S: `cProfile`; D: `py-spy`, Tachyon on 3.15, `sys.monitoring`, `memray`). A1.M11.3 Algorithmic and data-structure fixes (S: checklist; D: pointer to A3.M1 cost model). A1.M11.4 Caching and vectorisation (S: `lru_cache`; D: invalidation, NumPy dtype and copy costs). A1.M11.5 Native extensions and scale-out (S: overview; D: free-threading trade-offs, pointer to A2) | measure-first discipline, picking the level of fix | mastery >= 80%; optimisation task shows the required speed-up with uploaded profile evidence |

Hours check: Simpler 2.25+2.25+2.25+1.75+2.0+2.25+2.25+1.75+1.75+1.25+2.25 = 22.0; Deeper 1.5+1.75+2.0+1.0+1.0+1.5+1.25+1.25+1.0+0.75+2.0 = 15.0 (M3 and M11 are the heaviest because they are `B:M` and the most probed, section 8 LF-2).

## 5. Seed interview questions (3 per module, 33 total)

Seed IDs: `Mn-Qk` is the k-th seed of module n and maps to bank ID `Q-A1-Mn-0k` (opaque numbering, legacy ID kept by the importer). Format: tags are difficulty, competency, company-style cluster. These are style-of-question items, not actual questions from any company. Rubric anchors apply to all: Level 1 = incorrect or buzzwords only; Level 3 = correct mechanism with one example; Level 5 = correct mechanism, trade-offs, failure modes and current-version awareness unprompted. Difficulty words map to the data-model integer scale 1-5 at import: easy 1, easy-medium 2, medium 3, medium-hard 4, hard 5 (same mapping as Track C); calibrated after the pilot.

### M1 Object model and identity

**M1-Q1 — "What happens with `a = b = []`, and how does Python pass arguments to functions?"** (Medium; `python.core`; IT-services)
- Ideal answer:
  - Names are bindings to objects; `a` and `b` refer to one list.
  - Mutation through either name is visible through both; rebinding is not.
  - Arguments are passed by assignment of object references ("pass by object reference").
  - Mutating a parameter changes the caller's object; rebinding does not.
  - Immutable objects cannot be mutated, so they appear to behave "by value".
  - Fix: separate literals or `copy`/`list()`.
- Weak answers: "Python is pass by reference" or "pass by value" as a flat statement; cannot explain rebind vs mutate.
- Probes: What does `x += [1]` do for a list vs a tuple? How do mutable default arguments relate? How would you detect aliasing bugs in code review?

**M1-Q2 — "Explain the hash/equality contract and why a list cannot be a dict key."** (Medium; `python.core`; global-product)
- Ideal answer:
  - Equal objects must have equal hashes; hash must be stable for an object's lifetime.
  - Dicts and sets use hash to locate a slot, then `==` to confirm (collisions handled by probing).
  - Mutable containers can change equality, which would orphan them in the table; hence unhashable.
  - Defining `__eq__` without `__hash__` sets `__hash__` to `None`.
  - Tuples are hashable only if all elements are.
  - Hash randomisation for `str`/`bytes` means do not rely on hash values across runs.
- Weak answers: "because lists are mutable" with no mechanism; confuses `id` with hash.
- Probes: How does insertion order get preserved in dicts? What is the average and worst-case lookup cost? How would you make a custom class a safe key?

**M1-Q3 — "When would you use `__slots__`, and what are the drawbacks?"** (Medium; `python.core`; Indian-product)
- Ideal answer:
  - Removes per-instance `__dict__`, saving memory (significant for millions of small objects) and slightly faster attribute access.
  - Prevents arbitrary attributes; no `__weakref__` unless declared.
  - Inheritance subtleties: subclass without `__slots__` regains `__dict__`; multiple inheritance with non-empty slots conflicts.
  - Interacts with `dataclass(slots=True)` (3.10+).
  - Measure with `tracemalloc` or `sys.getsizeof` including referents before adopting.
  - Not a substitute for fixing a data-model problem (e.g. use arrays/NumPy for bulk data).
- Weak answers: "makes it faster" only; claims it is always better.
- Probes: How would pickling or copying behave? Does it still help given key-sharing instance dicts (since 3.3) and inline object values (3.11+), and how would you measure that? When did you last see it help in production?

### M2 Memory management and garbage collection

**M2-Q1 — "How does CPython manage memory? Why does it also need a garbage collector?"** (Medium; `python.memory`; global-IT-consulting)
- Ideal answer:
  - Primary mechanism is reference counting; objects freed immediately at zero.
  - Cycles keep counts above zero, so a cyclic GC periodically detects unreachable cycles of container objects.
  - Generational design (young objects collected more often); thresholds tunable via `gc`.
  - The incremental GC shipped in 3.14.0-3.14.4 and was reverted to the 3.13 generational design in 3.14.5+ and 3.15 after memory-pressure reports; a senior answer knows this.
  - `__del__` and finalisation order history; weakrefs avoid strong cycles.
  - Immortal objects (3.12) skip refcount changes.
- Weak answers: "garbage collector frees everything"; no mention of refcounting.
- Probes: What triggers a GC run? What does `gc.freeze` do and when is it useful (pre-fork servers)? How does free-threading change refcounting?

**M2-Q2 — "Your FastAPI service's memory grows steadily. How do you investigate?"** (Hard; `python.memory`; AI-startup)
- Ideal answer:
  - Confirm growth with RSS and container metrics; distinguish Python heap growth from native/fragmentation growth.
  - Reproduce under load; take `tracemalloc` snapshots and diff by line/traceback.
  - Inspect object counts by type (`objgraph`, `gc.get_objects`) and referrers.
  - Typical culprits: unbounded caches/dicts, global lists, closures capturing large objects, tracebacks stored on exceptions, per-request objects in module state, ORM sessions not closed, thread-local growth.
  - Fix with bounded caches (`lru_cache(maxsize)`, TTL), weakrefs, proper cleanup.
  - Guard: memory limits with worker recycling as mitigation, not a cure; add a regression test or dashboard alert.
  - Validate the fix with the same load test.
- Weak answers: "restart the pod"; "call `gc.collect()`"; no measurement.
- Probes: RSS grows but `tracemalloc` is flat; what next? How do you profile a production process safely? What about memory fragmentation with many workers?

**M2-Q3 — "What are weak references and where have you used them?"** (Medium; `python.memory`; Indian-product)
- Ideal answer:
  - Reference that does not increase refcount; target can be collected, weakref then returns `None`.
  - Use for caches, observer/listener registries, parent back-references to avoid cycles.
  - `WeakValueDictionary`, `WeakSet`, `weakref.finalize` for cleanup.
  - Not all objects are weak-referenceable (e.g. `int`, `list` instances directly; need subclass or `__weakref__` slot).
  - Callbacks run at collection time and must be safe.
  - Pitfall: bound methods are recreated, use `WeakMethod`.
- Weak answers: "like a pointer"; cannot give a use case.
- Probes: Why does `lru_cache` on methods leak and how would a weakref fix it? What happens to weakrefs in a cycle?

### M3 Execution model, the GIL and free-threading

**M3-Q1 — "What is the GIL, and how does it affect a web service?"** (Medium; `python.concurrency-basics`; IT-services)
- Ideal answer:
  - A mutex ensuring one thread executes Python bytecode at a time in a process, protecting interpreter internals such as refcounts.
  - It does not make compound operations atomic; locks are still needed for shared state.
  - I/O-bound work scales with threads (GIL released while blocking); CPU-bound pure Python does not.
  - C extensions (NumPy, hashlib, compression) may release it.
  - Web services typically scale with multiple worker processes plus threads or async per worker.
  - Mention free-threaded build as an option, with caveats.
- Weak answers: "Python is single-threaded"; "GIL makes everything thread-safe"; "async bypasses the GIL".
- Probes: Show a race condition that occurs despite the GIL. What is the switch interval? How does gunicorn/uvicorn worker model relate?

**M3-Q2 — "What is the state of free-threaded Python today, and would you adopt it?"** (Hard; `python.concurrency-basics`; global-product)
- Ideal answer:
  - PEP 703 landed experimentally in 3.13; PEP 779 made it officially supported but optional in 3.14 (phase II); the default-build decision is left to a future PEP with no date; 3.15 adds the `abi3t` stable ABI and official macOS binaries ship free-threading support, still optional (as of 2026-10).
  - Separate build (`python3.14t`); can check `sys._is_gil_enabled()`; importing an extension not marked GIL-safe may re-enable the GIL.
  - Mechanisms: biased and deferred reference counting, immortalisation, per-object locks, thread-safe allocator.
  - Costs: single-thread slowdown (roughly 1 percent on macOS aarch64 to 8 percent on x86-64 Linux on pyperformance per the official HOWTO, verified 2026-10), ecosystem wheel availability, new races now visible.
  - Adoption approach: benchmark representative CPU-bound multi-thread workloads, test dependencies, canary, keep a process-based fallback.
  - Existing pure-Python code relying on GIL-accidental atomicity needs audit.
- Weak answers: "GIL is removed in Python 3"; "it's default now"; no trade-offs.
- Probes: What breaks for C extensions? How do `dict` and `list` behave under concurrent mutation? Compare with sub-interpreters and multiprocessing.

**M3-Q3 — "You benchmark a CPU-bound function on 1, 2 and 4 threads on 3.14 and on 3.14t. How do you read the numbers and decide?"** (Hard; `python.concurrency-basics`; AI-startup)
- Ideal answer:
  - Expect no speed-up on 3.14 (GIL) and near-linear scaling on 3.14t only if the work is pure Python and contention is low.
  - Compare 1-thread 3.14 vs 1-thread 3.14t to quantify the single-thread overhead before judging scaling.
  - Report repeats, variance, core count and CPU pinning; warm up; use `pyperf` rather than a single `time` run.
  - Check `sys._is_gil_enabled()` and that no imported extension re-enabled the GIL.
  - Shared mutable state still needs locks; contention can erase the gain.
  - Decide with a stated threshold (for example required speed-up and acceptable overhead), a dependency audit and a process-pool fallback.
  - Date the conclusion: free-threading is optional in 3.14; re-run on later versions.
  - For the choice of tool across workloads, defer to the A2.M1 guide (`ref` item: A2 seed Q1.2, bank ID `Q-A2-M1-02`; resolved in the Track A lead review).
- Weak answers: "threads are faster now"; one timing run; ignores single-thread overhead.
- Probes: What does it mean if 4 threads on 3.14t are slower than 1? How would you pin cores and control noise? Which workloads gain nothing (I/O-bound)?

### M4 Iterators and generators

**M4-Q1 — "Iterable vs iterator vs generator: explain and show how a `for` loop works."** (Easy-Medium; `python.core`; IT-services)
- Ideal answer:
  - Iterable has `__iter__` returning an iterator; iterator has `__next__` and `__iter__` returning itself.
  - `for` calls `iter()`, then `next()` until `StopIteration`.
  - Generators are iterators created by `yield` functions or expressions.
  - Iterators are single-pass; iterables like lists can be iterated repeatedly.
  - Generators keep local state across suspensions.
  - Laziness reduces memory.
- Weak answers: treats them as synonyms; cannot describe `StopIteration`.
- Probes: Write a class-based iterator and its generator equivalent. What happens if you modify a list while iterating? What does `yield` return?

**M4-Q2 — "Process a 50 GB log file with limited memory. Design it with generators."** (Medium; `python.core`; Indian-product)
- Ideal answer:
  - Stream lines via file iterator; never `readlines()`.
  - Compose generator stages (parse → filter → transform → batch).
  - Use `itertools` (`islice`, `batched` in 3.12+) for chunking.
  - Handle resource cleanup with context managers / `try-finally` in generators.
  - Consider backpressure and failure handling (bad lines, checkpointing offsets).
  - When parallelism is needed, shard by file ranges using processes (pointer to A2).
  - Measure memory and throughput.
- Weak answers: loads into list; uses recursion; no error handling.
- Probes: How do you resume after a crash? What if one stage is slow? Is a generator pipeline thread-safe?

**M4-Q3 — "What do `send()`, `throw()`, `close()` and `yield from` do? How are generators related to coroutines?"** (Hard; `python.core`; global-product)
- Ideal answer:
  - `send(v)` resumes and makes the paused `yield` expression evaluate to `v`; first call must send `None`.
  - `throw()` raises an exception at the suspension point; `close()` raises `GeneratorExit`.
  - `yield from` delegates to a sub-generator, forwarding values, exceptions and capturing its return value.
  - Return value rides in `StopIteration.value`.
  - Native coroutines (`async def`) evolved from this machinery; `await` is conceptually delegation to an awaitable (event loop details in A2).
  - PEP 479: `StopIteration` inside generators becomes `RuntimeError`.
- Weak answers: only knows `yield`; no lifecycle knowledge.
- Probes: What happens if a generator is garbage collected unfinished? Can you `send` into a fresh generator? Why was `yield from` introduced?

### M5 Functions, closures, decorators and context managers

**M5-Q1 — "What is a closure? Why does `[lambda: i for i in range(3)]` return all 2s?"** (Medium; `python.core`; IT-services)
- Ideal answer:
  - Closure = function plus captured variables from enclosing scope (cell objects).
  - Variables are captured by reference to the cell, not value (late binding).
  - By call time loop variable holds final value.
  - Fixes: default argument `lambda i=i: i`, `functools.partial`, factory function.
  - `nonlocal` needed to rebind captured variables.
  - Inspect via `__closure__`.
- Weak answers: "lambdas are buggy"; cannot name fix mechanisms.
- Probes: What is LEGB? What does `nonlocal` do? How does this relate to class bodies and comprehensions scoping?

**M5-Q2 — "Write a retry decorator with parameters. What mistakes do people make?"** (Medium; `python.core`; AI-startup)
- Ideal answer:
  - Three-level structure: decorator factory → decorator → wrapper.
  - Use `functools.wraps` to preserve metadata.
  - Parameters: attempts, exceptions to catch (specific), backoff with jitter, optional logging.
  - Do not retry non-idempotent operations blindly; re-raise last exception preserving cause.
  - Handle async functions separately (`inspect.iscoroutinefunction`) to avoid blocking sleep.
  - Make usable with and without arguments if desired; typing with `ParamSpec` (3.10+).
  - Test with fake clock.
- Weak answers: catches `Exception` broadly; `time.sleep` in async; no `wraps`.
- Probes: Where does retry belong versus a library like tenacity or the HTTP client? How do you avoid retry storms? How do you decorate a method vs a function?

**M5-Q3 — "Explain context managers. How would you build one for a database transaction?"** (Medium; `python.core`; Big4)
- Ideal answer:
  - Protocol: `__enter__` returns resource; `__exit__(exc_type, exc, tb)` runs on exit and may suppress by returning truthy.
  - `contextlib.contextmanager` turns a generator into one (try/finally around `yield`).
  - Transaction: begin on enter, commit on clean exit, rollback on exception, always release connection; re-raise.
  - Nested/dynamic stacks with `ExitStack`; async variants (`async with`) pointer to A2.
  - Do not swallow exceptions accidentally.
  - Test the failure path.
- Weak answers: only `with open()`; returns True from `__exit__` by habit.
- Probes: What happens if `__exit__` itself raises? Reentrancy and thread safety? How does `contextvars` differ from thread-locals?

### M6 Classes and the data model

**M6-Q1 — "Explain the MRO and how `super()` works in diamond inheritance."** (Medium; `python.oop-datamodel`; global-product)
- Ideal answer:
  - MRO from C3 linearisation: preserves local precedence and monotonicity; view with `Class.__mro__`.
  - `super()` returns a proxy that continues lookup after the current class in the MRO of `type(self)`, not "the parent".
  - Cooperative multiple inheritance requires consistent signatures and every class calling `super().__init__(**kwargs)`.
  - Failure case: inconsistent hierarchies raise `TypeError` at class creation.
  - Mixins: small, no state, placed left.
  - Prefer composition when hierarchy gets complex.
- Weak answers: "depth-first left-to-right"; thinks `super()` = base class.
- Probes: Compute the MRO for a given diamond. What breaks if one class does not call `super()`? How do mixins interact with `__init__` arguments?

**M6-Q2 — "What is a descriptor? How do `property`, methods and `classmethod` use the protocol?"** (Hard; `python.oop-datamodel`; global-product)
- Ideal answer:
  - Object with `__get__` (and optionally `__set__`/`__delete__`/`__set_name__`) defined on a class.
  - Data descriptors (with `__set__`) take priority over instance `__dict__`; non-data descriptors do not.
  - Functions are non-data descriptors that produce bound methods.
  - `property`, `classmethod`, `staticmethod`, `__slots__` members implemented as descriptors.
  - Use case: reusable validation/lazy attributes (`cached_property`).
  - Attribute lookup order summary including `__getattribute__` and `__getattr__` fallback.
- Weak answers: only knows `@property`; cannot explain lookup order.
- Probes: Write a validating descriptor storing per-instance values (where to store?). Why is `__set_name__` useful? Why can per-instance data not live on the descriptor itself?

**M6-Q3 — "When would you use a metaclass? What are alternatives?"** (Hard; `python.oop-datamodel`; AI-startup)
- Ideal answer:
  - Classes are instances of `type`; metaclass customises class creation (`__new__`/`__prepare__`/`__call__`).
  - Legit uses: frameworks (ORMs, enums, ABCs), registration, enforcing class-level contracts.
  - Alternatives usually better: class decorators, `__init_subclass__`, descriptors, `Protocol`/ABC.
  - Costs: complexity, metaclass conflicts in multiple inheritance, harder debugging.
  - Show awareness of how `Enum` or Django models use them.
  - Default recommendation: do not, unless alternatives cannot express the need.
- Weak answers: "to add methods to classes"; "singleton via metaclass" as the only example.
- Probes: Implement a plugin registry two ways. What is a metaclass conflict? What is `__prepare__` for?

### M7 Modern Python: dataclasses, enums, pattern matching and typing

**M7-Q1 — "Dataclass vs NamedTuple vs plain class vs Pydantic model: how do you choose?"** (Medium; `python.modern`; AI-startup)
- Ideal answer:
  - `dataclass`: mutable (or `frozen`) record with generated `__init__`/`__repr__`/`__eq__`; supports `slots`, `kw_only`, `__post_init__`; no runtime validation.
  - `NamedTuple`: immutable, tuple-like, unpackable, light.
  - Plain class when behaviour dominates or custom init logic is heavy.
  - Pydantic: runtime validation/parsing/serialisation at system boundaries (APIs, config); some cost.
  - Mutable default needs `field(default_factory=...)`; `dataclass` rejects unhashable defaults.
  - `eq=True` with `frozen=False` sets `__hash__` to `None`.
- Weak answers: "dataclass is just less code"; unaware of default_factory.
- Probes: Why does `field(default=[])` raise? How does `frozen=True` interact with hashing? Where does A5-style validation belong in layered architecture?

**M7-Q2 — "Are Python type hints enforced? What do they buy you, and what recent changes should I know?"** (Medium; `python.modern`; global-IT-consulting)
- Ideal answer:
  - Not enforced at runtime; tools (mypy, pyright) check statically; libraries may use them at runtime (Pydantic, FastAPI).
  - Benefits: documentation, refactoring safety, IDE support, catching None/contract errors.
  - Generics, `Protocol` (structural), `TypedDict`, `Literal`, `Self`, `Annotated`, `@override`.
  - Recent: PEP 695 `type`/generic syntax (3.12), type parameter defaults (3.13), deferred annotation evaluation (3.14, PEP 649/749: annotations evaluated lazily, `annotationlib`, `from __future__ import annotations` no longer needed for forward references).
  - Gradual typing: `Any` leaks, `# type: ignore` hygiene, strictness settings.
  - Adopt incrementally with CI gate.
- Weak answers: "Python is statically typed with hints"; unaware of Protocol.
- Probes: Nominal vs structural typing example? What does `from __future__ import annotations` do and what replaced the need? How would you type a decorator preserving signatures?

**M7-Q3 — "What does structural pattern matching offer beyond a switch statement? Show pitfalls."** (Medium; `python.modern`; Indian-product)
- Ideal answer:
  - `match`/`case` (3.10) destructures sequences, mappings, class instances, with guards and OR/as patterns.
  - Bare names capture (not compare); use dotted names or literals for constants.
  - Class patterns use `__match_args__` (auto for dataclasses).
  - Mapping patterns ignore extra keys; sequence patterns do not match `str`/`bytes` as sequences.
  - Good for parsing ASTs, commands, event payloads; avoid when `if` is clearer.
  - Exhaustiveness checking with `assert_never` and type checkers.
- Weak answers: calls it a switch; falls into capture-vs-compare trap.
- Probes: Why does `case RED:` always match? How would you match a JSON event shape? Performance expectations?

### M8 Exceptions and logging

**M8-Q1 — "How should you design exception handling in a service client library?"** (Medium; `python.exceptions-logging`; Big4)
- Ideal answer:
  - Define a small hierarchy rooted in a library base exception; distinguish retryable vs permanent errors.
  - Catch narrow exceptions; wrap low-level errors with `raise NewError(...) from exc` to preserve cause.
  - Never bare `except`; do not swallow; use `finally`/context managers for cleanup.
  - Do not log and re-raise at every layer; log once at the boundary.
  - Include context (ids, operation) without leaking secrets.
  - Map to API error responses at the edge.
- Weak answers: catches `Exception` everywhere; returns error codes/None.
- Probes: `__cause__` vs `__context__`? When is `raise ... from None` right? How do you test error paths?

**M8-Q2 — "What are exception groups and `except*`? When would you encounter them?"** (Hard; `python.exceptions-logging`; global-product)
- Ideal answer:
  - `ExceptionGroup`/`BaseExceptionGroup` (3.11, PEP 654) bundle multiple unrelated exceptions.
  - `except*` handles subsets by type, can run multiple handlers; unhandled parts re-raised as a group.
  - Motivation: concurrent tasks failing simultaneously (e.g. asyncio `TaskGroup`).
  - Cannot mix `except` and `except*` in one `try`; `return/break/continue` not allowed inside `except*`.
  - Use `.split()`/`.subgroup()`, `add_note()` for context.
  - 3.14 relaxes the parenthesis requirement for multiple exception types without `as` (PEP 758, confirmed).
- Weak answers: never heard of it; treats it as a list of errors only.
- Probes: What does `TaskGroup` raise? How do you unwrap a single expected exception? How does this change FastAPI error handling?

**M8-Q3 — "Explain Python's logging architecture. Why is configuring logging inside a library a bad idea?"** (Medium; `python.exceptions-logging`; IT-services)
- Ideal answer:
  - Hierarchy of named loggers; records propagate to ancestor handlers; handlers decide destinations; formatters and filters shape output.
  - Use `getLogger(__name__)` per module.
  - Libraries add only `NullHandler`; applications configure once at entry (`dictConfig`).
  - Levels at logger and handler; `propagate` flag; duplicate output cause.
  - Use lazy `logger.info("x %s", v)`; `logger.exception` for tracebacks.
  - Non-blocking: `QueueHandler`/`QueueListener`; request context via `contextvars` + filter.
  - Structured (JSON) output for aggregation (platform side in A7).
- Weak answers: `print` debugging; `basicConfig` in libraries; duplicate handlers.
- Probes: Why do I see every log line twice? What does the root logger's `lastResort` handler do? Impact of logging on latency under load?

### M9 Import system, packaging and environments

**M9-Q1 — "What happens when you `import foo`? How do you resolve a circular import?"** (Medium; `python.import-packaging`; Indian-product)
- Ideal answer:
  - Check `sys.modules`; else finders on `sys.meta_path` locate spec; loader creates module and executes it; module object cached before execution completes.
  - Search order via `sys.path`; packages and namespace packages.
  - Circular import: partially initialised module in `sys.modules`; `from a import x` fails if `x` not yet defined.
  - Fixes: restructure to remove cycle (extract shared module, dependency inversion), import module not names, local imports as last resort, `TYPE_CHECKING` for typing-only cycles.
  - Explicit lazy imports (PEP 810, `lazy import`) in 3.15 as a new tool; semantics and risks taught in A1.M9.3.
  - Side effects at import time are a design smell.
- Weak answers: "it copies the file"; fixes cycles by random reordering.
- Probes: Why is a module executed only once? What does `python -X importtime` show? What does `if __name__ == "__main__"` do?

**M9-Q2 — "How do you manage dependencies and packaging for an application vs a library?"** (Medium; `python.import-packaging`; global-IT-consulting)
- Ideal answer:
  - `pyproject.toml` as standard metadata (PEP 621) with build backend (PEP 517/518).
  - Libraries: broad version ranges, minimal pins; applications: locked, reproducible, hash-pinned dependencies.
  - Tools: pip/uv/Poetry/pip-tools (replaceable "current tools" box); lockfile standard PEP 751 `pylock.toml` supported by uv and PDM and experimentally by pip, native lockfiles still primary.
  - Wheels vs sdists; platform tags; compiled deps.
  - One virtual environment per project; PEP 668 prevents system-site breakage.
  - CI: cached installs, security audit (`pip-audit`), dependency update bot, Python version matrix.
  - Docker: multi-stage, no dev deps, pinned base image.
- Weak answers: `pip freeze > requirements.txt` only; global installs.
- Probes: Diamond dependency conflict, what do you do? How do you publish internally? How do you upgrade Python versions safely?

**M9-Q3 — "How would you reduce the startup time of a CLI or Lambda function written in Python?"** (Hard; `python.import-packaging`; AI-startup)
- Ideal answer:
  - Measure with `-X importtime`; identify heavy imports.
  - Defer heavy imports into functions or use explicit lazy imports (PEP 810, 3.15).
  - Trim dependencies, avoid import-time work (client construction, config reads).
  - Cold-start specifics for Lambda: package size, layers, init outside handler for reuse, provisioned concurrency/SnapStart-style options (platform detail owned by A7; AWS docs confirm SnapStart supports Python 3.12 and later on published versions/aliases and does not support provisioned concurrency, so the two are alternatives, verified 2026-10).
  - Bytecode caching (`.pyc` pre-compiled in image).
  - Trade-offs: first-call latency moves to request path; failures surface later.
- Weak answers: "use a faster language"; no measurement.
- Probes: What breaks with lazy imports? How do you keep import cost from regressing in CI?

### M10 Bytes, buffers and serialisation

**M10-Q1 — "What is `memoryview` and when would you use it?"** (Medium; `python.serialization`; global-product)
- Ideal answer:
  - View onto an object's buffer (buffer protocol) without copying: `bytes`, `bytearray`, `array`, NumPy arrays.
  - Slicing a memoryview is zero-copy; slicing `bytes` copies.
  - Use: network/file parsing, `readinto`, large binary payloads, passing data to C code.
  - Writable only if the underlying object is mutable; keeps the exporter locked against resize while views exist.
  - `cast()` for typed views; `release()` / context manager.
  - Combine with `struct.unpack_from`.
- Weak answers: "like bytes"; not aware of copy semantics.
- Probes: What happens if you resize a `bytearray` with an active view? How does this relate to NumPy? Show a zero-copy parse of a header.

**M10-Q2 — "Why is unpickling untrusted data dangerous, and what would you use instead?"** (Medium; `python.serialization`; Big4)
- Ideal answer:
  - Pickle is a stack-machine bytecode able to import and call arbitrary callables via `__reduce__`; loading executes code.
  - Never unpickle data from untrusted or unauthenticated sources (including cache servers, queues, user uploads).
  - Restricting unpicklers is brittle; signing (HMAC) helps integrity but is not a license to accept untrusted input.
  - Alternatives: JSON, MessagePack, Protocol Buffers, Avro/Parquet with schemas.
  - Pickle remains fine for trusted, same-version, short-lived use (e.g. multiprocessing).
  - Pickles are Python-version-fragile; avoid long-term storage.
  - Policy and scanning belong to A8.
- Weak answers: "pickle is slow"; no mention of code execution.
- Probes: Demonstrate a malicious `__reduce__` (conceptually). How do Celery/Redis caches expose this risk? What is protocol 5 for?

**M10-Q3 — "What will not survive a JSON round trip, and how do you handle it?"** (Easy-Medium; `python.serialization`; IT-services)
- Ideal answer:
  - Tuples become lists; dict keys become strings; `datetime`, `Decimal`, `set`, `bytes`, `UUID` unsupported by default.
  - Large ints lose precision in JavaScript consumers; `NaN`/`Infinity` non-standard.
  - Use `default=` hooks, custom encoders, or a validated model layer (Pydantic) for serialisation.
  - Encode datetimes as ISO 8601 with timezone; money as strings or integers of minor units.
  - Schema/versioning for evolving payloads.
  - Streaming/NDJSON for large outputs.
- Weak answers: "JSON handles any Python object".
- Probes: How would you round-trip a `Decimal` safely? What are `ensure_ascii` implications? How to evolve payloads without breaking consumers?

### M11 Performance and profiling

**M11-Q1 — "A function is slow in production. Walk me through your approach."** (Medium; `python.performance`; global-product)
- Ideal answer:
  - Define the metric (latency, throughput, memory) and reproduce with representative data.
  - Measure before changing: `cProfile`/sampling profiler (`py-spy`), flame graph; microbenchmarks with `timeit`/`pyperf`.
  - Identify whether time is CPU, I/O, lock contention or GC.
  - Fix at the highest level first: algorithm and data structure, reduce I/O round trips, batching, caching.
  - Then micro-optimisations or native code (NumPy, Cython, Rust) if justified.
  - Verify improvement with before/after numbers and guard with a benchmark/regression test.
  - Consider scale-out vs optimise cost trade-off.
- Weak answers: "rewrite in C", "use PyPy" without data; list of tricks.
- Probes: Profiler overhead and sampling vs deterministic trade-off? How do you profile in production safely? What if the profile is flat?

**M11-Q2 — "When does `lru_cache` help, and when does it hurt?"** (Medium; `python.performance`; Indian-product)
- Ideal answer:
  - Memoises pure functions keyed by hashable args; bounded by `maxsize` (LRU eviction); `cache` is unbounded.
  - Helps for expensive deterministic calls with repeated inputs.
  - Hurts: impure functions, unhashable args, mutable return values shared between callers, memory growth, stale data, per-process (not shared across workers).
  - On methods keeps `self` alive (leak); use per-instance cache or `cached_property`.
  - Thread-safe for the cache structure, but the function may run twice concurrently for the same key.
  - Distributed cache (Redis) for cross-process, with TTL and invalidation (A5).
  - Inspect with `cache_info()`.
- Weak answers: "always add it"; no invalidation story.
- Probes: How do you handle TTL? What about async functions? What is the hit-rate threshold that justifies it?

**M11-Q3 — "How would you speed up numeric processing over millions of records in Python?"** (Medium; `python.performance`; AI-startup)
- Ideal answer:
  - Profile first; eliminate Python-level loops over large data.
  - Vectorise with NumPy/pandas/Polars (contiguous arrays, C loops, SIMD) `[judgement: tool preference]`.
  - Choose appropriate dtypes; avoid object arrays; reduce copies and temporary arrays.
  - Chunk for memory; use generators/iterators for streaming.
  - For non-vectorisable logic: Numba/Cython/Rust extension, or multiprocessing/free-threaded threads (A2).
  - Validate correctness (floating-point tolerance) and benchmark with realistic data sizes.
  - Consider data layout and I/O format (Parquet vs CSV).
- Weak answers: "use multithreading" in standard CPython for CPU-bound loops; no profiling.
- Probes: Why is a NumPy loop element-wise still slow? What is broadcasting memory cost? When does the JIT in CPython help, and why not much here?

### 5.1 Goal tags and senior-bar notes for every seed question (G-10)

Goal tag `B:M A:S H:M` = Senior Python Backend / AI Engineer / hybrid (MUST, SHOULD, NICE as `M`, `S`, `N`). Note = what mid, senior and staff add. Rows marked **E** are the module's **evidence question**: the strong answer must include a measurement or a production failure story (boss item 7); the required evidence is in section 5.2.

| ID | Goal | Senior-bar note (mid / senior / staff) |
|---|---|---|
| M1-Q1 | B:M A:S H:M | mid states reference semantics / senior separates rebind from mutate and cites `+=` on list vs tuple / staff describes how aliasing bugs are caught in review and tests |
| M1-Q2 | B:M A:S H:M | mid says "mutable" / senior gives the table mechanism and the `__eq__` without `__hash__` rule / staff designs safe key types and notes hash randomisation in sharding or caching keys |
| M1-Q3 **E** | B:S A:S H:S | mid says "saves memory" / senior lists the costs and inheritance traps / staff shows a measured saving and chooses arrays or NumPy instead when data is bulk |
| M2-Q1 | B:M A:S H:M | mid says "GC" / senior leads with refcounting and cycles / staff knows the 3.14 incremental GC was reverted in 3.14.5 and what that taught about GC changes in production |
| M2-Q2 **E** | B:M A:M H:M | mid restarts or `gc.collect()` / senior diffs `tracemalloc` snapshots under load / staff adds RSS vs heap split, worker recycling as mitigation only, and a regression alert |
| M2-Q3 | B:S A:S H:S | mid defines weakref / senior gives use cases and the not-weakref-able types / staff ties it to `lru_cache` on methods and cache design |
| M3-Q1 | B:M A:S H:M | mid says "one thread at a time" / senior shows a race that survives the GIL / staff relates worker model, GIL release points and capacity planning |
| M3-Q2 | B:M A:S H:M | mid says "no-GIL is here" / senior gives PEP 703/779 phases and costs / staff gives an adoption plan with canary, dependency audit and process fallback, dated |
| M3-Q3 **E** | B:M A:S H:M | mid predicts speed-up / senior reads timings, variance and single-thread overhead / staff decides adoption from the measurements and states what would change the decision |
| M4-Q1 | B:M A:M H:M | mid defines terms / senior desugars `for` and explains exhaustion / staff designs APIs that return iterables, not iterators, to avoid one-shot bugs |
| M4-Q2 **E** | B:M A:M H:M | mid streams lines / senior adds batching, bad-line handling, memory measured under a cap / staff adds checkpoint and resume, sharding and observability of the pipeline |
| M4-Q3 | B:S A:S H:S | mid knows `yield` / senior knows `send`, `close`, PEP 479 and delegation / staff links the machinery to coroutine behaviour and cleanup guarantees |
| M5-Q1 | B:S A:S H:S | mid describes the loop trap / senior explains cells and fixes / staff relates class-body and comprehension scoping rules |
| M5-Q2 **E** | B:S A:M H:M | mid writes three levels / senior handles `wraps`, idempotency, async and jitter / staff designs against retry storms and decides library vs decorator vs client policy |
| M5-Q3 | B:S A:S H:S | mid shows `with open` / senior builds a transaction manager with the failure path tested / staff covers reentrancy, async variants and `contextvars` vs thread-locals |
| M6-Q1 | B:S A:S H:S | mid says "depth first" / senior computes C3 and explains cooperative `super` / staff advises composition over deep mixins and signature discipline |
| M6-Q2 | B:S A:N H:S | mid knows `property` / senior explains data vs non-data and per-instance storage / staff links the protocol to ORM fields, `cached_property` and lookup cost |
| M6-Q3 **E** | B:S A:N H:S | mid names Django models / senior prefers `__init_subclass__` and decorators / staff tells a metaclass-conflict or framework-upgrade story and sets a team rule |
| M7-Q1 **E** | B:M A:M H:M | mid compares features / senior places validation at boundaries / staff measures Pydantic validation cost on a hot path and keeps internal models as dataclasses where justified |
| M7-Q2 | B:M A:M H:M | mid says "not enforced" / senior covers Protocol, generics and 3.12-3.14 changes / staff sets a gradual-typing CI gate and `Any` budget |
| M7-Q3 | B:S A:S H:S | mid calls it a switch / senior avoids capture-vs-compare traps / staff knows when `if` is clearer and uses `assert_never` exhaustiveness |
| M8-Q1 | B:S A:S H:S | mid catches broadly / senior wraps with `from` and logs once at the boundary / staff maps retryable vs permanent to API and SLO behaviour |
| M8-Q2 | B:S A:S H:S | mid has not met it / senior explains `except*`, `TaskGroup` and restrictions / staff explains unwrapping strategy at framework boundaries |
| M8-Q3 **E** | B:S A:S H:S | mid configures handlers / senior diagnoses duplicate lines and library `NullHandler` / staff shows measured logging latency and `QueueHandler` as the fix, and hands platform concerns to A7 |
| M9-Q1 | B:S A:S H:S | mid says "copies the file" / senior explains `sys.modules` and partial initialisation / staff designs module boundaries to prevent cycles |
| M9-Q2 | B:S A:S H:S | mid says `pip freeze` / senior separates libraries from applications and uses lockfiles with hashes / staff adds upgrade policy, audit in CI and Python-version matrix |
| M9-Q3 **E** | B:S A:S H:S | mid says "use another language" / senior measures with `-X importtime` and defers heavy imports / staff budgets import time in CI and weighs lazy-import failure modes |
| M10-Q1 | B:S A:S H:S | mid says "like bytes" / senior shows zero-copy slicing and the resize lock / staff ties it to NumPy and network parsing throughput |
| M10-Q2 **E** | B:S A:S H:S | mid says "pickle is slow" / senior explains `__reduce__` code execution / staff recounts a queue or cache exposure and moves the format to a schema'd one |
| M10-Q3 | B:S A:S H:S | mid lists types / senior handles money, datetimes and big ints / staff versions payloads and plans schema evolution |
| M11-Q1 **E** | B:M A:M H:M | mid lists tricks / senior profiles first and fixes at the highest level / staff sets a benchmark guard and weighs scale-out cost |
| M11-Q2 | B:M A:S H:M | mid says "add a cache" / senior knows the method-leak and mutable-return traps / staff designs TTL, invalidation and per-process vs shared caching |
| M11-Q3 | B:S A:M H:M | mid says "use threads" / senior vectorises and fixes dtypes and copies / staff chooses the data layout and I/O format and validates numerically |

### 5.2 Evidence requirements and senior-bar upgrade probes (two staff-level probes per module)

| Module | Evidence the strong answer to the E question must contain | Staff probe 1 | Staff probe 2 |
|---|---|---|---|
| M1 | M1-Q3: a measured per-instance saving (`tracemalloc` or `sys.getsizeof` with referents) for a stated object count | A cached config object is shared across requests and one tenant's mutation leaks to another: how do you prove and prevent it? | How would you enforce hashable, immutable value types across a team codebase? |
| M2 | M2-Q2: a `tracemalloc` diff naming the allocating line, before/after RSS under the same load test | After upgrading Python the memory profile of a batch job changes with no code change: what do you check, and what does the 3.14 incremental-GC episode teach? | How do you decide between worker recycling and fixing a leak, and how long is recycling acceptable? |
| M3 | M3-Q3: timings from 3.14 and 3.14t with thread count, variance and single-thread overhead stated | Which of your dependencies would block 3.14t and how do you find out? | Design a one-week canary for free-threading on a real service, with abort criteria |
| M4 | M4-Q2: peak memory measured under the cap and a resume-after-crash design with offset checkpoints | A generator holds a DB cursor open and the consumer stops early: what leaks and how do you guarantee cleanup? | How do you make a pipeline observable (lag, rejects) without breaking laziness? |
| M5 | M5-Q2: a retry-storm incident story (count of retries, backoff, jitter fix) | How do you ship a decorator used by 40 services without breaking signatures and typing? | Where does `contextvars` propagation break around decorators and thread pools? |
| M6 | M6-Q3: a concrete metaclass conflict or `__init_subclass__` refactor with what broke and how it was found | How would you migrate a metaclass-heavy internal framework to `__init_subclass__` incrementally? | What lookup-cost effects do descriptors and `__getattr__` have on a hot path, and how would you measure them? |
| M7 | M7-Q1: measured validation cost per call for a Pydantic model vs a dataclass on a hot path | How do you roll out strict type checking to a 300k-line codebase without stalling delivery? | When does deferred annotation evaluation (3.14) change behaviour in your framework code? |
| M8 | M8-Q3: measured request latency with synchronous vs `QueueHandler` logging, or a duplicate-log incident with the cause | How do you prevent sensitive data reaching logs through exception messages and tracebacks? | What does a `TaskGroup` failure look like end to end in a FastAPI endpoint, and what should the client see? |
| M9 | M9-Q3: `-X importtime` output naming the top import and the measured cold-start before/after | What can go wrong with lazy imports in production and how would you test for it? | How would you stop import-time regressions in CI across many services? |
| M10 | M10-Q2: a queue or cache pickle exposure story and the format migration with a rollback path | How do you evolve a binary or JSON message schema across producers and consumers deployed at different times? | When is `memoryview` slower or riskier than copying, and how do you tell? |
| M11 | M11-Q1: profile output identifying the hot path, before/after numbers and a benchmark guard | How do you profile a latency regression in production with minimal overhead? | Free-threaded threads vs processes vs vectorising: how do you decide for a CPU-bound service? |

## 6. Final assessment

- **Timed mastery check:** 25 items, 40 minutes, sampled across all modules (weighted to the learner's weakest modules after placement): 10 MCQ/concept, 8 predict-the-output, 4 short code tasks with hidden tests, 3 scenario questions. Pass at 80%; failures route to a targeted revision plan by module.
- **Timed coding task (45 minutes, IDE):** one of a set of internals-flavoured tasks (e.g. a descriptor-based validator, a weakref-backed cache using the data model and a streaming pipeline, with tests). Scored on correctness, hidden tests, complexity discussion in comments.
- **Voice mock round (15 minutes):** adaptive Python-internals interviewer, 5–6 questions drawn from the seed bank with follow-up probes; scored on the rubric (1–5 per competency: correctness, depth, trade-offs, currency, clarity) with transcript evidence; "Meets" (average >= 3.0, none below 2); readiness uses the Senior bar in section 7.2. Final timed mastery check passes at 80 percent (G-7); allowance about 2 h.
- **Design/canvas task:** not required for A1; a lightweight "diagnose this service" scenario (memory growth plus GIL contention) is included as a scenario question instead.

## 7. Course-level risks, freshness notes and interview rehearsal link

### 7.1 Risks and freshness (as of 2026-10-05)

| Risk or fast-moving item | Handling |
|---|---|
| 3.15.0 final slipped to 2026-10-09 (rc3 on 2026-10-02) | Teach 3.15 features as "release candidate, final scheduled 2026-10-09" until final ships; then restamp M3, M7, M9, M11; keep 3.14 as the pinned default (G-8) |
| Free-threading: phase III default decision undated; 3.15 wording | State PEP 703/779 phases and the date; never claim default; 3.15.0rc3 free-threading HOWTO and What's New checked: no "no longer experimental" wording; overhead 1-8 percent confirmed (verified 2026-10); re-read at 3.15.0 final |
| Incremental GC shipped then reverted (3.14.0-3.14.4 vs 3.14.5+ and 3.15) | Teach as a dated case study; confirm again if a future release re-introduces it |
| JIT gains experimental and shifting (7-8 percent x86-64 Linux, 11-12 percent AArch64 macOS in 3.15 What's New) | Teach the method and the status, not the numbers; never imply production readiness |
| PEP 810 lazy imports had last-minute blockers fixed in rc3 | Re-read the final What's New before publishing M9.3; teach semantics and risks, keep examples minimal |
| Tool landscape (uv, pip lock/`pylock.toml`, ruff, type checkers, Tachyon CLI flags, memray) | Principle-first lessons; tool names in a replaceable "current tools" box; Tachyon usage (`attach`, `run`, `run -m`, `dump`, `--mode wall/cpu/gil/exception`, `--flamegraph`, `--async-aware`) matches the 3.15.0rc3 What's New (verified 2026-10); re-check flags at 3.15.0 final |
| Remaining open items | (1) 3.10 EOL: resolved, 2026-10-01 (verified 2026-10); (2) 3.14t overhead: resolved against the official HOWTO (verified 2026-10); (3) AWS Lambda startup options: resolved against AWS docs (verified 2026-10); (4) NumPy/pandas/Polars tool preference is `[judgement]`; (5) [verify: 3.15.0 final due 2026-10-09 per PEP 790, so re-read What's New and the Tachyon CLI once it ships] |
| Code examples must run on pinned versions | CI runs every example on 3.14 (3.13/3.15 where a version difference is taught; 3.14t for M3.5 only) |
| Security demos (pickle) | Hardened sandbox, no network |
| Overlap drift with A2/A3/A5/A7/A8 | Ownership notes row per module (section 3) and one-way pointers (G-1) |
| Review cadence | 6 months for M2, M3, M7, M9, M11 (next April 2027 or 3.15.1); 12 months for the rest |

### 7.2 Interview rehearsal link

| Interview Master mode | Modules exercised | Use |
|---|---|---|
| Voice drill (strict mode) | all; heaviest M3, M1, M2, M11, M7 | Spoken GIL and free-threading answer, hash contract, leak investigation, slow-function walkthrough |
| Coding (timed) | M1, M4, M5, M6, M8, M10, M11 | Decorators, descriptors, generators, logging config, zero-copy parse; feeds C3 |
| Design canvas | none required (R-A1 section 6) | A "diagnose this service" scenario (memory growth plus GIL contention) is a scenario question |
| Project deep-dive | M2, M3, M9, M11 | Defend a real memory, startup or performance incident with measurements |
| Behavioural | M2, M11 | Production-incident stories (link to C1) |

**A1 readiness (G-6 specialised)**: for each tag (`python.core`, `python.memory`, `python.concurrency-basics`, `python.oop-datamodel`, `python.modern`, `python.exceptions-logging`, `python.import-packaging`, `python.serialization`, `python.performance`) the learner has a module mastery check >= 85 percent on a retake at least 7 days after the first pass; the timed coding task passed (>= 70 percent hidden tests plus correct stated complexity); strict-mode voice drill at Senior bar (average >= 3.5, none < 3) in two sessions at least 48 hours apart, including one GIL/free-threading answer with the correct dated status and one measurement-backed performance or leak answer; no open remediation flag on prerequisite modules (section 8, LF-5).

## 8. Learner-Fit Appendix

Reference learner: senior engineer, 11+ years, about 4 years Python, daily FastAPI/Django/Flask, SQL, Redis, Docker, AWS Lambda/S3, MQTT, LLM APIs and voice agents, targeting Senior Python Backend and AI Engineer roles. Rules apply to any learner. Band scores come from each module's 5 placement items (G-5): **>= 85 skip lessons and take the mastery check, 70-84 skim, 50-69 study, < 50 deep-study**. Diagnostic items are `Q-A1-Mn-D1..D5`: predict-the-output or MCQ items built around a subtle trap, so they separate "uses it" from "can explain it" (boss item 8).

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 (new to Python internals) | Simpler, 22 h, M1 to M11 in order, prerequisites linked | 70 percent guided, 30 percent unguided; M3 benchmark guided | none | none until M1-M3 pass |
| L2 (junior-mid) | Simpler, about 19 h; skim lessons already known (M5, M7, M9 basics) | 50/50 guided and unguided | known basics | one Challenge per passed module |
| L3 (experienced, cannot always explain) | Deeper, about 14-15 h; skim M1, M4, M5; study "explain at depth" lessons in M2, M3, M6, M8, M11 | 70 percent unguided, timed tasks, M3 and M11 labs unguided | lessons banded 85+ | GC and free-threading dated-status brief; descriptor-based validator |
| L4 (senior, owns it in production; reference learner) | Deeper, about 9-12 h expected after skip (M5 skipped per boss item 1; M1, M4, M9 skimmed; M10 skimmed so Deeper stays <= 14 h) `[judgement]`; goes to mastery checks, then evidence questions and voice drills | 80 percent timed and voice; labs only where a check misses | M5 lessons and others by band | staff probes (section 5.2), leak or cold-start incident write-up |

### LF-2 Module matrix

Weights are percent of Python-internals interview time per target: B Senior Python Backend, A AI Engineer, H hybrid (each column sums to 100). Rationale: backend interviews probe GIL, memory, performance and the data model most; AI-engineer interviews lean on generators and streaming, typing and Pydantic, and performance of data paths; the hybrid sits between `[judgement]`. Deeper hours in section 4 follow these weights.

| Module | Diagnostics (5: traps) | Tags | Weight B / A / H | Rehearsal modes | Module readiness criterion |
|---|---|---|---|---|---|
| M1 | `a = b = []` and `+=` on tuple holding list; mutable default across calls; `__eq__` without `__hash__`; `is` on large ints and strings; `copy` vs `deepcopy` with shared inner list | B:M A:S H:M | 12 / 11 / 12 | voice, coding | check 85, 3 tasks pass, voice Senior bar |
| M2 | refcount after `del` with a cycle; `__del__` in a cycle; weakref to list vs subclass; RSS vs `tracemalloc` figure; what 3.14.5+ GC is | B:M A:S H:M | 10 / 9 / 10 | voice, project deep-dive | check 85, leak hunt evidence, one measured leak story |
| M3 | race that survives the GIL; what releases the GIL; `sys._is_gil_enabled()` after importing an old extension; which 3.14 builds ship JIT; free-threading default status | B:M A:S H:M | 14 / 9 / 12 | voice, project deep-dive | check 85, benchmark conclusion matches data, dated-status answer correct |
| M4 | iterate a generator twice; `StopIteration` inside a generator; value of `yield` expression; `send` to fresh generator; generator cleanup on early break | B:M A:M H:M | 9 / 11 / 10 | coding, voice | check 85, pipeline under memory cap |
| M5 | late-binding lambdas; decorator order output; `lru_cache` on a method; `nonlocal` vs `global`; `__exit__` return value | B:S A:S H:S (L4 skip likely) | 8 / 9 / 9 | coding, voice | check 85, 3 decorator tasks with hidden tests |
| M6 | MRO of a diamond; data vs non-data descriptor priority; `super()` target; class vs instance attribute shadowing; `__init_subclass__` timing | B:S A:S H:S | 9 / 7 / 8 | voice, coding | check 85, descriptor and registry tasks pass |
| M7 | `field(default=[])`; `case RED:` capture trap; `frozen` plus `eq` hashing; hints not enforced; annotations evaluated lazily in 3.14 | B:M A:M H:M | 8 / 14 / 11 | coding, voice | check 85, strict-typing refactor passes |
| M8 | `raise from` vs implicit context; `except*` with two groups; duplicate log lines; `lastResort`; `assert` under `-O` | B:S A:S H:S | 7 / 7 / 7 | coding, voice, behavioural | check 85, logging task passes routing and request-ID tests |
| M9 | circular `from a import x`; module executed once; `-X importtime` reading; library vs app pinning; wheel vs sdist | B:S A:S H:S | 5 / 5 / 5 | voice, project deep-dive | check 85, wheel and import-budget tasks pass |
| M10 | slice of `bytes` vs `memoryview`; resize with active view; unpickle of `__reduce__` payload; JSON round-trip of tuple, int key and `Decimal`; protocol 5 purpose | B:S A:S H:S | 6 / 8 / 7 | voice, coding | check 85, zero-copy parser passes buffer-identity tests |
| M11 | profiler reading of a flat profile; `lru_cache` hit-rate trap; string concat in loop; NumPy temp-array copies; sampling vs deterministic overhead | B:M A:S H:M | 12 / 10 / 9 | voice, coding, project deep-dive | check 85, speed-up task with profile evidence, voice Senior bar |

### LF-3 Experience bridge ("you already know" -> "gap to senior depth")

| Module | You already know | Gap between using it and explaining it at senior depth |
|---|---|---|
| M1 | Pydantic models, Django model instances, dict-heavy code | hash/eq contract and probing, aliasing in shared config and cached objects, why Django's ORM identity map and request-scoped objects can leak state |
| M2 | Gunicorn/uvicorn workers recycled on memory, Docker memory limits | explaining refcount vs GC, finding the leak with snapshot diffs (identity maps, caches, tracebacks holding frames), RSS vs heap, the 3.14 GC episode |
| M3 | multi-worker FastAPI/Django deployments, `run_in_executor`, Lambda concurrency | what the GIL guarantees, races that survive it, dated free-threading and JIT status, reading a benchmark honestly |
| M4 | streaming responses, DB cursors, LLM token streams | the iterator protocol, generator lifecycle and cleanup, backpressure by design |
| M5 | FastAPI dependency injection and route decorators, Celery task decorators, `@lru_cache` | closures and cells behind decorators, `wraps`, `__exit__` semantics; FastAPI `Depends` yield-dependencies are context managers |
| M6 | Django models and fields, SQLAlchemy columns, Pydantic validators | descriptors behind ORM fields and `property`, MRO and cooperative `super`, when a metaclass is justified |
| M7 | Pydantic v2 models, dataclasses, FastAPI type hints, enums | what hints do and do not do at runtime, dataclass hashing rules, `match` traps, Protocol vs ABC, Pydantic vs dataclass choice |
| M8 | `logging.getLogger`, try/except in views, Sentry | exception chaining and groups, `dictConfig`, `QueueHandler`, why libraries add only `NullHandler`, duplicate-log diagnosis |
| M9 | `pip`/`uv`, Dockerfiles, Lambda deployment packages | import resolution and `sys.modules`, circular imports, import-time cost behind Lambda cold start, libraries vs applications, lockfiles |
| M10 | JSON APIs, Celery with pickle or JSON serialisers, Redis values, MQTT payloads, voice-agent audio buffers | pickle as code execution (Celery and cache exposure), `memoryview` and zero-copy for audio and binary frames, JSON round-trip losses |
| M11 | slow-endpoint triage, `cProfile` once, adding Redis cache | measure-first method, flame graphs, `py-spy` and Tachyon, cache invalidation, picking the level of fix and proving it |

### LF-4 Senior-bar framing

Per-question mid / senior / staff notes are in section 5.1; measurement and failure-story requirements in section 5.2. Course-wide: mid answers describe features and recall rules; senior answers explain the mechanism, state version-specific behaviour with a date, give failure modes and back claims with a measurement; staff answers add operations (canaries, alerts, rollback), cost and team-wide prevention (lint rules, CI budgets, review checklists, mentoring).

### LF-5 Adaptive hooks

| Item | Rule |
|---|---|
| Mastery thresholds | module check 80 percent (badge); 85 percent on a retake after at least 7 days (readiness) |
| Remediation links | M2 -> M1; M3 -> M1, M2; M4 -> M1; M5 -> M1, M4; M6 -> M1, M5; M7 -> M5, M6; M8 -> M5; M9 -> M1; M10 -> M1, M7; M11 -> M2, M3, M4 |
| Challenge extensions | M1 immutable-value-types lint rule; M2 memory-regression alert design; M3 free-threading canary plan; M4 resumable pipeline; M5 library-grade decorator with typing; M6 incremental metaclass-to-`__init_subclass__` migration; M7 strict-typing rollout plan; M8 sensitive-data scrubbing filter; M9 import-time CI budget; M10 schema-evolution plan; M11 production profiling plan |
| Engine may reorder or defer | M7 and M4 earlier for AI-engineer routes (typing, streaming); M8 and M10 deferrable to the end for the Backend route if time is short (`B:S`); M9 any time after M1; fixed order: M1 before everything, M2 before M3 and M11, M5 before M6 |
| Route multipliers (G-3) | Backend route: M3, M11 at 1.0 and `B:M` modules at 1.0, `S` modules at 0.6; AI route: M1-M4, M7, M11 as tagged, M6 and M5 at 0.6 (tag S; core lessons chosen by diagnostic, descriptors and MRO first), M8-M10 at 0.6; hybrid as tagged |

### LF-6 Applied labs (production-framed, one per module)

| Module | Lab (mode) |
|---|---|
| M1 | Find and fix the shared-state bug in a cached settings object served across API requests |
| M2 | Leak hunt in a FastAPI chat-history service with `tracemalloc` snapshot diffs and a regression test |
| M3 | 3.14 vs 3.14t benchmark of a CPU-bound feature-extraction function on 1, 2, 4 threads with a written adoption memo (the only multi-thread lab in A1) |
| M4 | Streaming ETL for a 50 GB device-telemetry (IoT/MQTT) log under a memory cap with resume offsets |
| M5 | Retry, timing and TTL-cache decorators for an LLM-API client, including exception and async cases |
| M6 | Validating descriptor and plugin registry for a pluggable voice-agent tool layer |
| M7 | Add strict types and dataclass/enum/`match` refactor to an untyped webhook-event handler |
| M8 | `dictConfig` with request-ID filter and `QueueHandler`, plus an exception hierarchy for a payment-provider client |
| M9 | Cut Lambda-style cold start of a Python function with `-X importtime`, plus package a wheel for an internal library |
| M10 | Zero-copy parse of a binary audio-frame header and a pickle-risk demonstration in the hardened sandbox |
| M11 | Profile and speed up a slow report endpoint with before/after flame graphs and a benchmark guard |

### LF-7 Currency and quality bar

| Module | Stamp | Memorable visual or analogy | Top 3 misconceptions handled |
|---|---|---|---|
| M1 | 2026-10 | Name tags on boxes: names stick to boxes, never the reverse | `is` as fast `==`; pass by value or reference; `__slots__` always wins |
| M2 | 2026-10 | Coat check with a ticket count, plus a janitor for tangled tickets (cycles) | GC is the main reclaimer; `del` frees memory; RSS shows the Python heap |
| M3 | 2026-10 | One microphone passed around a meeting, vs several microphones (free-threading) | GIL makes code thread-safe; Python cannot do parallelism; free-threading is the default |
| M4 | 2026-10 | Tap water, not a bucket: pull a cup at a time, one-shot | iterating a generator twice; building lists "to be safe"; `StopIteration` inside generators |
| M5 | 2026-10 | Backpack of cells carried by the inner function; onion layers for decorators | late binding; missing `wraps`; `lru_cache` on methods |
| M6 | 2026-10 | Mail room sorting order: data descriptors first, then the instance shelf | `super()` means parent; metaclass for everything; `@property` is the whole story |
| M7 | 2026-10 | Road signs vs traffic police: hints advise, validators enforce | hints enforced at runtime; mutable dataclass defaults; `match` as a switch only |
| M8 | 2026-10 | Fire alarm chain: every log call travels up the logger tree | bare `except`; log-and-re-raise; configuring logging inside a library |
| M9 | 2026-10 | Library card catalogue: `sys.modules` is the already-borrowed shelf | modules re-run per import; fixing cycles by reordering; `pip freeze` is enough |
| M10 | 2026-10 | A window onto the same room vs a photocopy (`memoryview` vs `bytes`) | `memoryview` copies; pickle for external input; JSON round-trips everything |
| M11 | 2026-10 | Doctor's chart: diagnose with a flame graph before prescribing | optimise without profiling; "Python is slow so use X"; cache without invalidation |

## 9. Open items for boss

1. Confirm the sandbox may expose a 3.15 image as selectable from 3.15.0 final (scheduled 2026-10-09) while the default stays 3.14, and that 3.15.1 remains the switch gate (G-8).
2. Confirm that Tachyon (`profiling.sampling`) lab content may be added to M11.2 as a 3.15-only optional lab once 3.15.0 final ships and the CLI is re-verified.
3. Resolved (Track A lead review): the single `ref` item for the concurrency decision guide used in A1.M3.3 is `Q-A2-M1-02` (A2 seed Q1.2).

## 10. Boss compliance log

| Decision | Applied |
|---|---|
| G-1, O-1 to O-10 | Ownership notes row per module (section 3); pointers one-way and within 20 minutes to A2.M1, A3.M1, A5.M2, A7.M9, A8; one `ref` item for the A2 guide |
| G-2 | Header 22 S / 15 D, module hours sum exactly, S/D semantics sentence, 2 h assessment allowance; Deeper cap 14 h with M10 skim |
| G-3, G-11 | Learner-Fit Appendix LF-1 to LF-7; B/A/H tags per module and per seed question (section 5.1); M3, M11 `B:M`; M8, M10 `B:S A:S H:S`; M5 `B:S` with L4 skip |
| G-4, G-5, G-6, G-7 | Level paths, bands, readiness (section 7.2), thresholds 80 module and course, 85 readiness |
| G-8 | 3.14 default, 3.13/3.15 selectable, 3.14t for A1.M3 only; 3.15 gate at 3.15.1; no model names |
| G-9, G-10 | Tags hyphen-style and owned; IDs `Q-A1-Mn-nn`; goal tag and senior-bar note per seed; 2 staff probes per module; 5 diagnostics per module |
| G-12 | Dated facts (2026-10-05), `[judgement]` marked, no invented statistics |
| R-A1-1 | A1.M1 owns mechanism; no complexity-table exercise; one probe animation |
| R-A1-2 | 3.14 default, 3.14t selectable; 3.15 features confirmed against What's New or kept `[verify]` |
| R-A1-3 | Concurrency guide reduced to one paragraph and one `ref` in A1.M3.3; M3-Q3 replaced by a benchmark-reading question |
| R-A1-4 | A1.M8 owns logging mechanics; platform structured logging removed to A7.M9 |
| R-A1-5, R-A1-6 | 11 modules kept; effort per G-2; free-threading phases and "as of" date stated, default never claimed |
| Section 8 items 1-9 | Appendix; experience bridges (DI and decorators -> descriptors and closures, ORM identity map and Pydantic -> object model and leaks, Lambda cold start -> imports, Celery pickling -> serialisation); M3 lab only multi-thread lab; M1 container internals kept; M8 logging confirmed; all [verify] items attempted (GC revert, PEP 810, PEP 814, Tachyon, PEP 758, PEP 649/749, PEP 751, JIT figures) with remaining checks listed in 7.1; evidence question per module; hours 15 |

- Track A lead review (2026-10-05, TRACK-LEAD-A): ref item Q-A2-M1-02 resolved; AI-route multiplier for M5-M6 and M8-M10 corrected to 0.6 (tag S); difficulty-to-integer mapping stated. Verdict: Approve. See _reviews/TrackLead_A_Report.md.
