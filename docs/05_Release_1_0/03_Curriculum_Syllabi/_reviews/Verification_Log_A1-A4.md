# Verification Log A1-A4 (VER-1, 2026-10-05; stamp "verified 2026-10")

Legend: C = confirmed, X = corrected, O = left open with short marker.

## A1 Python Internals
| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| A1-1 | Default to 3.15 only when 3.15.1+ ships **[verify at that time]** | O->gate | n/a (future decision) | Reworded as a future decision gate; marker removed |
| A1-2 | Secondary source says free-threading "no longer experimental" in 3.15 **[verify]** | X | https://docs.python.org/3.15/howto/free-threading-python.html (3.15.0rc3), https://docs.python.org/3.15/whatsnew/3.15.html | Wording is not in either page; text now says do not quote it, status is PEP 779 supported-but-optional |
| A1-3 | 3.14t overhead 1-8% (secondary) **[verify]** | C | https://docs.python.org/3.14/howto/free-threading-python.html (about 1% macOS aarch64 to 8% x86-64 Linux) | Marker replaced with official figure (3 places) |
| A1-4 | 3.10 EOL scheduled Oct 2026 **[verify exact date]** | X | https://peps.python.org/pep-0619/ (EOL 2026-10-01, 3.10.22 final) | Changed to "reached EOL on 2026-10-01" |
| A1-5 | Lambda startup options (SnapStart, provisioned concurrency) **[verify AWS]** | C | https://docs.aws.amazon.com/lambda/latest/dg/snapstart.html (Python 3.12+ supported; not combinable with provisioned concurrency) | Marker replaced |
| A1-6 | Tachyon CLI usage **[verify]** | C (rc3) | https://docs.python.org/3.15/whatsnew/3.15.html (attach/run/dump, --mode, --flamegraph, --async-aware) | Resolved against rc3; re-check at final noted |
| A1-7 | 3.15 feature list (PEP 810, 814, 661, 798, 686, 799, 803, 831), GC revert in 3.14.5 | C | 3.15 What's New (same URL) | No change |
| A1-8 | 3.15.0 final 2026-10-09 | C | https://peps.python.org/pep-0790/ (expected 2026-10-09; rc3 on 2026-10-02) | No change; open item: re-read at final |
| A1-open | Remaining after 3.15.0 final ships | O | n/a | `[verify: 3.15.0 final due 2026-10-09 ...]` left in section 7.1 table |

Not re-fetched (unchanged): JIT percentages, PEP 751 tool support (pip lock 25.1 / 26.1, PDM), NumPy/pandas/Polars preference (`[judgement]`).

## A2 Concurrency and Async
| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| A2-1 | 3.15 `abi3t` free-threaded stable ABI **[verify final notes]** | C (rc3) | https://docs.python.org/3.15/whatsnew/3.15.html (PEP 803; separate `cp315t` build); https://peps.python.org/pep-0790/ | Stamped; open marker kept for final re-read |
| A2-2 | AnyIO default thread limiter 40 | C | https://anyio.readthedocs.io/en/stable/threads.html (4.15.1) | Stamped |
| A2-3 | Arq maintenance-only | C | https://github.com/python-arq/arq (README banner) | Stamped; Taskiq/Dramatiq "active" stays secondary |
| A2-4 | pytest-asyncio 1.4.0 alpha April 2026; "default will become function" **[verify pin]** | X | https://github.com/pytest-dev/pytest-asyncio/releases (1.4.0 stable 2026-05-26) | Updated to stable; removed unsupported "default will become function" claim; added 1.4.0 changes. Pin itself stays engineering `[verify]` |
| A2-5 | Celery 5.x / FastAPI / Starlette / SQLAlchemy pins **[verify]** | O | Celery docs (5.6.3, 2026-03-26) via search: https://docs.celeryq.dev/en/stable/history/index.html | Celery latest noted; FastAPI/SQLAlchemy pins left as `[verify: engineering image-build]` |
| A2-6 | 3.14 forkserver default, `python -m asyncio ps/pstree`, child watchers removed | C | https://docs.python.org/3.14/whatsnew/3.14.html (forkserver, ps/pstree confirmed; removal items not re-read) | No change |

## A3 DSA
| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| A3-1 | `heapq` max-heap helpers added in 3.14 | C | https://docs.python.org/3.14/library/heapq.html ("Added in version 3.14") | Stamped |
| A3-2 | Other feature versions (bisect key 3.10, pairwise 3.10, bit_count 3.10, batched 3.12, graphlib/cache/lcm 3.9) | not re-fetched | n/a | Unchanged; covered by planned CI import check |
| A3-3 | CI import check on pinned image | O | n/a engineering | Marker kept with reason |
| A3-4 | AI-tool policy in coding rounds **[verify]** (2 places) | O | Only secondary blogs found (e.g. finalroundai.com, interviewcoder.co); no primary company page retrieved | Marker kept with reason; added note that policies vary by company and round |
| A3-5 | Hidden-test calibration (spike S-5) | O | engineering measurement | Marker kept with reason |

## A4 Databases
| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| A4-1 | PG 18.6 current, 17 supported, PG19 Beta 4 on 2026-09-24 | C | https://www.postgresql.org/support/versioning/ ; https://www.postgresql.org/about/news/postgresql-19-beta-4-released-3386/ | Unchanged |
| A4-2 | PG19 GA "expected by end of October" | X | Beta 4 announcement (RC early Oct, GA "may" be October; several features reverted) | Reworded in 4 places |
| A4-3 | MySQL 9.7 released 2026-04-21, LTS; calendar versions from 26.7 | C | https://dev.mysql.com/doc/relnotes/mysql/9.7/en/ ; https://dev.mysql.com/doc/refman/9.7/en/mysql-releases.html (9.7 final sequential line; "28.4.x LTS" used in upgrade example) | Stamped. "no 9.8" kept implied; 28.4 April 2028 timing marked as inferred |
| A4-4 | MySQL 8.0 EOL 2026-04-30 | C (vendor blog + secondary) | https://blogs.oracle.com/mysql/extending-mysql-8-0-support-in-mysql-heatwave ; https://www.percona.com/blog/mysql-8-0-end-of-life-date/ | Stamped; "final 8.0.46" removed (unverifiable) |
| A4-5 | MySQL 9.7 driver/ORM support **[verify]** | O | Oracle docs only say Connector/Python 9.7.0 works with 8.0/8.4/9.x; third-party (mysqlclient, PyMySQL, SQLAlchemy, Django) not documented (Django databases page fetch timed out) | Marker kept with reason |
| A4-6 | MySQL row-value inequality range use **[verify]** | partial | https://dev.mysql.com/doc/refman/8.4/en/row-constructor-optimization.html | Added documented guidance (prefer expanded OR form); full-prefix case still `[verify]` |
| A4-7 | PgBouncer 1.21 (Oct 2023) prepared statements; default on from 1.24 | C | https://www.pgbouncer.org/2023/10/pgbouncer-1-21-0 (2023-10-16) ; http://www.pgbouncer.org/2025/01/pgbouncer-1.24.0 (default 200, via search) | Stamped |
| A4-8 | Transaction mode breaks `LISTEN/NOTIFY`, temp tables | X | https://www.pgbouncer.org/features.html (NOTIFY works; LISTEN never; only PRESERVE/DELETE ROWS temp tables break; PREPARE/DEALLOCATE never) | Corrected in M7 row and M7 question (2 places) |
| A4-9 | PG 18: skip scan, temporal constraints, uuidv7, async I/O, checksums default, pg_upgrade keeps stats | C | https://www.postgresql.org/docs/18/release-18.html | Stamped |
| A4-10 | PG 17 failover of logical slots | C | https://www.postgresql.org/docs/17/release-17.html | No change |
| A4-11 | MySQL skip scan since 8.0.13; `skip_scan` switch | C | https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-13.html ; https://dev.mysql.com/doc/refman/8.4/en/range-optimization.html | Stamped |
| A4-12 | MySQL Community VECTOR lacks DISTANCE(); Percona 9.7 adds it | C | https://dev.mysql.com/doc/refman/9.7/en/vector-functions.html ; https://docs.percona.com/new/2026/09/28/percona-server-for-mysql-972-2-has-been-released | Stamped |

Not re-fetched (unchanged): MySQL 8.0.16 CHECK, 8.0.1 SKIP LOCKED, PostgreSQL 11 fast ADD COLUMN DEFAULT, PostgreSQL 15 NULLS NOT DISTINCT / 17 RETURNING (merge), `DETACH PARTITION CONCURRENTLY`; these are long-standing and were not marked time-sensitive.

## Open items left
1. A1/A2: re-read 3.15.0 final What's New and Tachyon flags after 2026-10-09.
2. A2: FastAPI/Starlette/SQLAlchemy/pytest-asyncio pins (engineering).
3. A3: CI import check, calibration (engineering); AI-tool policy (no primary source).
4. A4: MySQL 9.7 third-party driver/ORM support; full-prefix row-comparison optimisation.
