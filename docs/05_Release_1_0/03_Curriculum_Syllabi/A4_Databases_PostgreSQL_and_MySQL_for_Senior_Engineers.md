---
name: A4 — Databases - PostgreSQL and MySQL for Senior Engineers
course_id: A4
track: A (Backend)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-05, HAR-07 (see _reviews/Boss_Final_Approval.md)
---

# A4 — Databases: PostgreSQL and MySQL for Senior Engineers (Track A)

Version stamp: **2.0 (wave-2 upgrade by SME-A4, 2026-10-05; builds on REV-A4 v1.1)**. Facts dated October 2026. Engine baseline for content and sandbox: **PostgreSQL 18.x (18.6 current at review date; 17 also supported; PostgreSQL 19 is in beta, Beta 4 on 2026-09-24, GA not yet announced, so never taught as fact and not pinned)**; **MySQL 8.4 LTS pinned** (the content says "MySQL 8.4 LTS and later LTS"). MySQL 9.7 was released 2026-04-21 and is an LTS series (MySQL 9.7 release notes and Reference Manual section 1.3; latest 9.7.3 on 2026-08-18; verified 2026-10); it is taught only as "innovation and LTS notes" and **no 9.7 image is pinned** until third-party driver and ORM support is confirmed **[verify: mysqlclient, PyMySQL, SQLAlchemy and Django MySQL backend support for 9.7 is not stated in primary vendor docs; Oracle's own Connector/Python 9.7.0 is documented as compatible with MySQL Server 8.0, 8.4 and 9.x]**. After 9.7 the MySQL version label changed to calendar style (MySQL 9.7 is documented as the final sequential-version line; MySQL 26.7 is the July 2026 release; the manual's upgrade-path example names 28.4 as the next LTS, so a 28.4 LTS in 2028 is expected but its date is not stated in primary docs; verified 2026-10). MySQL 8.0 reached end of life on 2026-04-30 (Oracle Lifetime Support schedule, Oracle MySQL blog and Percona coverage; verified 2026-10); learners still meet it in legacy systems, so lessons flag 8.0 differences.

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | A4 — Databases: PostgreSQL and MySQL for Senior Engineers / Track A |
| Audience | Backend engineers (L3–L4) with working SQL; goal is senior-level depth in both engines for Python backend and AI-engineer interviews |
| Prerequisites and placement skip | Basic SQL joins and aggregates. **Placement may skip M1 and M2 lessons only** (mastery check still offered); M3-M10 test out via the module mastery check (band actions in section 8, LF-2); whole-course test-out only via the course final mastery check at 85 percent plus a Senior-bar voice mock (G-5). No band waives the course final assessment |
| Outcomes (each tied to a competency) | O1 Write correct, dialect-aware analytical SQL (windows, CTEs, NULL logic) — `db.sql`. O2 Design schemas, keys, constraints and multi-tenant models and defend trade-offs — `db.schema`. O3 Choose, build and explain indexes (B-tree, composite, partial, covering, GIN, InnoDB clustered) — `db.indexing`. O4 Read EXPLAIN/EXPLAIN ANALYZE output in both engines and fix the bottleneck — `db.tuning`. O5 Explain isolation levels and MVCC per engine and reproduce/fix anomalies, deadlocks and queue patterns — `db.transactions`, `db.locking`. O6 Plan replication, partitioning, pooling and read scaling, state when sharding is justified, and make an honest PostgreSQL vs MySQL recommendation — `db.scaling`, `db.sql`. O7 Plan zero-downtime migrations and run day-2 operations (vacuum/bloat, observability, backups/PITR) — `db.operations`. O8 Diagnose and fix ORM pitfalls in SQLAlchemy and Django ORM — `db.orm` |
| Estimated effort | **~30 h Simpler / ~22 h Deeper, plus ~3 h assessment allowance** (approved; module hours sum to the header, see section 3). Simpler = L1-L2 route with full scaffolding. Deeper = L3-L4 route, foundations skimmed per placement, senior-bar and Challenge content included, hours net of typical skip-ahead, so it is shorter in wall-clock time but harder per hour |
| Interview coverage tags (owned) | `db.sql`, `db.schema`, `db.indexing`, `db.tuning`, `db.transactions`, `db.locking`, `db.scaling`, `db.operations`, `db.orm`. Secondary (no credit): `api.design` (A5), `design.scaling` (A6) |
| IDE (platform capabilities) | Per-attempt disposable PostgreSQL 18 (pg_stat_statements enabled, extension allowlist) and MySQL 8.4 LTS (P-04); dialect-aware validation and per-dialect hidden tests; plan viewer; limited-privilege role allowing `VACUUM`, `ANALYZE`, `pg_locks`, `pg_stat_*`, `performance_schema` reads (P-06). **Two-session mode (P-05) is mandatory** for M5-M6 with scripted interleavings and lock-wait and deadlock visualisation. **No live replication or multi-node labs in 1.0**: replication (M7) and PITR (M9) run as guided simulators with recorded outputs. Every task carries a mode label: `SANDBOX`, `TWO-SESSION` or `SIMULATED` |
| Badge and certificate | Module badge: mastery check at or above 80% plus all IDE tasks passed (visible and hidden tests) plus one voice or text verbal question rated at or above "Meets" on the rubric. Course badge ("Database Depth — PostgreSQL and MySQL"): all 11 module badges, final 25-item mastery check at or above 80% (G-7), timed SQL set passed (at least 70 percent hidden tests plus stated complexity), and voice round at "Meets" (average 3.0 or above, none below 2). Interview readiness is a separate, stricter criterion (section 7). Certificate issued on course badge with verification link |
| Authoring owner / reviewer | SME agent A4 (wave 2, v2.0) / REV-A4 (v1.1 review); human SME reviewer: to be assigned |
| Review cadence | Engine versions and feature notes every 6 months (PostgreSQL 19 GA may occur in October 2026 (Beta 4 announcement, 2026-09-24: release candidate expected early October; GA date not yet announced): add the "What changes in 19" box only after GA and review; MySQL calendar-versioned releases); core theory every 12 months. Last reviewed 2026-10-05 |
| Scope boundaries (ownership, G-1 and R-A4-4) | A4 owns single-database internals, SQL, schema and RLS data model, indexing and plans, keyset SQL, isolation and locking including the `SKIP LOCKED` primitive, DB-level replication, partitioning and pooling, migrations, vacuum, backup and observability, ORM data-access pitfalls. **Pointers only**: REST pagination contract and cursors (A5.M3), idempotency keys and rate limiting (A5.M4), outbox implementation (A5.M8), HTTP and Redis caching (A5.M5, A6.M2), sharding strategy and consistency models (A6.M3), DB-queue-vs-broker decision (A2.M9), async pools (A2.M8), managed DB selection and ops (A7.M5), DR implementation (A7.M10), vector indexing and pgvector tuning (B3.M4). Lesson IDs use `.n` (for example `A4.M5.3`); `L1`-`L4` mean learner levels only |

## 2. Module table

| # | Module | Module outcome (testable) | Key topics | Visuals | Practice (IDE / design) | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | SQL fluency refresh | Learner writes top-N-per-group, running totals, gaps-and-islands and recursive-hierarchy queries correctly in both dialects | Joins and row-flow semantics, aggregates, window functions (frames: ROWS vs RANGE, ranking, LAG/LEAD), CTEs and recursive CTEs, set ops, NULL three-valued logic, `NOT IN` vs `NOT EXISTS`, upsert (`ON CONFLICT` vs `ON DUPLICATE KEY UPDATE`), `MERGE` (PostgreSQL 15+, `RETURNING` in 17+), `LATERAL`, `DISTINCT ON`; MySQL gaps: no `FULL OUTER JOIN`, no aggregate `FILTER`, `INTERSECT/EXCEPT` only from 8.0.31 | Row-flow diagram of joins (replaces Venn), window frame sliding animation | 12 query tasks, each run in both dialects with dialect-specific hidden tests | `NOT IN` with NULLs, `COUNT(col)` vs `COUNT(*)`, `RANGE` default frame surprise, `GROUP BY` non-aggregated columns (MySQL `ONLY_FULL_GROUP_BY`), CTE materialisation assumptions (PostgreSQL 12+ inlines non-recursive CTEs unless `MATERIALIZED`) |
| M2 | Schema design | Learner produces and critiques a schema (keys, constraints, types, tenancy) and justifies each denormalisation | Normal forms in practice, surrogate vs natural keys, identity vs sequence, UUIDv4 vs UUIDv7 (`uuidv7()` built in PostgreSQL 18; random UUID PKs cause page splits in InnoDB clustered PK), constraints (CHECK enforced from MySQL 8.0.16, FK, exclusion constraints, deferrable constraints, PostgreSQL 18 temporal constraints (`WITHOUT OVERLAPS` on PRIMARY KEY/UNIQUE, `PERIOD` on FOREIGN KEY; confirmed against release notes 2026-10-05)), data types (`numeric` for money, `timestamptz`, utf8mb4 and collations), JSONB vs relational, generated columns (virtual generated columns default in PostgreSQL 18), soft deletes with partial unique indexes, multi-tenant models (shared schema with `tenant_id`, row-level security, schema-per-tenant), audit/history tables, outbox table | ER diagrams, trade-off matrix, tenant isolation spectrum (analogy: apartment block with shared lobby vs separate houses) | Design and critique 3 schemas (order system, multi-tenant SaaS, IoT time series); fix a flawed schema. **Owner lab: multi-tenant RLS data model** (`tenant_id` composite keys, RLS policy, per-transaction tenant setting, missing-`WHERE` leak test); A5.M11 and B3.M7 reference it. `SANDBOX` | Storing money as float, missing FK indexes, "JSON for everything", `timestamp` without zone, soft delete breaking uniqueness, `ENUM` migration pain in MySQL |
| M3 | Indexing | Learner picks, orders and justifies indexes for given query workloads and predicts index-only behaviour in both engines | B-tree structure and lookup, composite order (equality then range; leftmost prefix), selectivity, covering indexes (`INCLUDE` in PostgreSQL vs wider composite in MySQL), partial and expression indexes (PostgreSQL; MySQL 8.0.13+ functional indexes), PostgreSQL 18 B-tree skip scan (reduces need for leading-column indexes when the skipped column has few distinct values, not a replacement), InnoDB clustered PK and secondary indexes carrying PK, visibility map and index-only scans, GIN/GiST/BRIN/SP-GiST/hash, JSONB indexing (GIN `jsonb_ops` vs `jsonb_path_ops`, expression index on `->>`; MySQL multi-valued indexes and generated-column indexes), full-text search (PostgreSQL `tsvector` + GIN, `websearch_to_tsquery`, `pg_trgm`; MySQL InnoDB FULLTEXT natural/boolean mode), write amplification and index bloat, invisible indexes (MySQL) for safe removal; **one pointer lesson (20 min, no lab) to pgvector, owned by B3.M4**; MySQL skip-scan range access exists since 8.0.13 for narrow cases | B-tree walk-through animation (analogy: a phone book sorted by surname then first name), clustered vs heap comparison, GIN posting-list diagram | Fix 8 slow queries by adding or reshaping indexes; JSONB containment query tuning; build a search feature with FTS in both engines. All `SANDBOX` | "Index every column", wrong composite order, function on indexed column defeating index, leading wildcard `LIKE`, implicit type casts, low-selectivity boolean index, ignoring write cost |
| M4 | Query plans and tuning | Learner reads a plan, identifies the dominant cost and proposes a verified fix | `EXPLAIN (ANALYZE, BUFFERS)` (BUFFERS reported by default in `EXPLAIN ANALYZE` from PostgreSQL 18), MySQL `EXPLAIN ANALYZE` and `EXPLAIN FORMAT=TREE`, scan types, join algorithms (PostgreSQL nested loop/hash/merge; MySQL nested loop and hash join from 8.0.18, no merge join), estimates vs actuals, statistics (`ANALYZE`, `default_statistics_target`, `CREATE STATISTICS`; MySQL histograms), planner settings, parameter sniffing and generic vs custom plans for prepared statements, pagination (OFFSET vs keyset: **A4 owns the SQL, index and plan lab; A5.M3 reuses it and owns the API cursor contract**), top-N, anti-patterns (`SELECT *`, functions on columns, N+1 pointer to M10), `pg_stat_statements` and slow query log as the entry point to tuning | Plan tree reading guide (analogy: a recipe tree read from the leaves up), estimate-vs-actual divergence highlighter | Read 10 plans and predict bottleneck; fix 6 queries with before/after plans; keyset pagination lab on a 5M-row feed. `SANDBOX` | Trusting cost numbers over actual time, ignoring row-estimate errors, "force index" first, tuning without realistic data volume, optimising a query that runs once |
| M5 | Transactions, isolation and MVCC | Learner explains and reproduces each anomaly under each engine's default and stricter levels and chooses a fix | ACID and durability settings (`fsync`, `synchronous_commit`, `innodb_flush_log_at_trx_commit`), anomalies (dirty read, non-repeatable, phantom, lost update, write skew), **PostgreSQL**: tuple versions with `xmin/xmax`, snapshots, READ COMMITTED (new snapshot per statement), REPEATABLE READ (snapshot isolation; first-updater-wins gives serialization failure SQLSTATE 40001; write skew still possible), SERIALIZABLE (SSI; can fail with 40001, app must retry), READ UNCOMMITTED behaves as READ COMMITTED. **InnoDB**: undo-log MVCC, default REPEATABLE READ with consistent snapshot created at first consistent read, locking reads and writes act on latest committed data, gap and next-key locks prevent phantoms for locking reads, plain read-modify-write loses updates silently, READ COMMITTED reduces gap locking, SERIALIZABLE turns plain SELECT into shared locking reads (when autocommit is off); retry loops; savepoints; deadlocks (detection and error 40P01 in PostgreSQL after `deadlock_timeout`; InnoDB error 1213 with immediate detection) and their analysis | Timeline diagrams per anomaly per engine (analogy: two clerks editing one paper ledger, with snapshot vs live-copy rules), tuple-visibility step-through, next-key lock range diagram | `TWO-SESSION` (P-05, scripted interleavings): reproduce lost update, write skew, phantom in both engines; fix with `FOR UPDATE`, atomic `UPDATE`, constraint, or SERIALIZABLE plus retry | Saying "REPEATABLE READ prevents phantoms" for all engines, "PostgreSQL has no phantoms at RR so it is serializable" (write skew), assuming MySQL RR throws errors on conflicts, forgetting retry logic, long transactions |
| M6 | Locking and concurrency patterns | Learner builds a safe job queue and diagnoses a blocking chain and a deadlock from logs | Row-level locks (`FOR UPDATE/SHARE/NO KEY UPDATE/KEY SHARE`), `NOWAIT`, `SKIP LOCKED` (PostgreSQL 9.5+, MySQL 8.0.1+), table lock modes and lock queueing (why `ALTER TABLE` blocks everything behind it), metadata locks in MySQL, advisory locks (session vs transaction scope; pooling interaction), optimistic concurrency with version columns, idempotent consumers via unique constraints, lock ordering, `lock_timeout`/`statement_timeout`/`idle_in_transaction_session_timeout`, diagnosing with `pg_stat_activity`, `pg_locks`, `pg_blocking_pids()`, MySQL `performance_schema.data_locks`, `sys.innodb_lock_waits`, `SHOW ENGINE INNODB STATUS`, `log_lock_waits`, `innodb_print_all_deadlocks` | Lock wait graph, lock-compatibility matrix, queue state machine (analogy: a supermarket queue stuck behind one slow shopper) | `TWO-SESSION`: build job queue (`SKIP LOCKED`) with visibility timeout and test with concurrent workers (A2.M9 reuses this worker); analyse 3 deadlock logs; fix; diagnose a DDL blocking chain | Using `SELECT` then `UPDATE` without lock, `FOR UPDATE` on large scans, queue without retry/dead-letter, holding transactions open across network calls, inconsistent lock order |
| M7 | Scaling: replication, partitioning, pooling | Learner designs a read-scaling and growth plan for a given workload with explicit consistency and lag handling | **Replication**: PostgreSQL physical streaming (async/sync, `synchronous_commit` levels, slots, `hot_standby_feedback` and recovery conflicts), logical replication (publications/subscriptions, per-table, DDL not replicated, used for zero-downtime major upgrades; failover slots in 17), HA tooling (Patroni, managed failover); MySQL async binlog (row format, GTID), semi-sync, parallel replica applier, Group Replication, InnoDB Cluster/ReplicaSet, replica terminology in 8.4 (`SHOW REPLICA STATUS`); lag causes and read-your-writes strategies. **Partitioning**: PostgreSQL declarative range/list/hash, pruning, no global indexes so PK must include partition key, `DETACH PARTITION CONCURRENTLY`, `pg_partman`, default partition; MySQL range/list/hash/key, PK/unique keys must include all partition columns, no foreign keys on partitioned InnoDB tables; when partitioning helps (retention, pruning) and when it does not. **Connection pooling**: why PostgreSQL connections are expensive (process per connection), PgBouncer session vs transaction vs statement mode, what breaks in transaction mode (session `SET`, advisory session locks, `LISTEN` (but not `NOTIFY`, which works), session-level temp tables (`PRESERVE`/`DELETE ROWS`; `ON COMMIT DROP` is fine), `PREPARE`/`DEALLOCATE` statements; protocol-level prepared statements in transaction mode since PgBouncer 1.21 (2023-10-16) via `max_prepared_statements`, enabled by default (200) from 1.24 (PgBouncer features page, 1.21.0 announcement, 1.24.0 release notes; verified 2026-10)), pool sizing, app-side pools (SQLAlchemy `pool_size`/`max_overflow`/`pool_pre_ping`, Django `CONN_MAX_AGE`, `CONN_HEALTH_CHECKS`), ProxySQL for MySQL, managed proxies. **Sharding**: only "when is sharding justified at database level" and the cost of cross-shard queries; **strategy, shard-key design and consistent hashing point to A6.M3** (Citus/Vitess named only). Caching: DB-side only (buffer cache, materialised views); mechanics point to A5.M5, topology to A6.M2 | Topology diagrams, replication lag timeline, pooler mode comparison (analogy: a hotel front desk serving many guests with few room keys), partition pruning diagram | Design read-scaling plan (`SIMULATED` replication step-through with recorded lag outputs); **IoT time-series lab**: partition an events table, verify pruning in the plan, drop an old partition for retention (`SANDBOX`, single node); simulate pool exhaustion and fix (`SANDBOX`) | "Add replicas" as universal answer, ignoring lag and read-your-writes, thinking partitioning always speeds queries, pool size equal to max connections times app instances, sharding too early |
| M8 | Migrations and online DDL | Learner plans a risky migration as an ordered, reversible, lock-aware sequence | Expand/contract pattern, backfills in batches, PostgreSQL: `CREATE INDEX CONCURRENTLY`, `REINDEX CONCURRENTLY`, `ADD COLUMN` with constant default (instant since 11), `NOT VALID` constraint then `VALIDATE`, `SET NOT NULL` via validated CHECK, type changes that rewrite tables, `lock_timeout` plus retry, FK index requirement; MySQL: `ALGORITHM=INSTANT/INPLACE/COPY`, `LOCK=NONE`, instant add/drop column limits, metadata-lock queueing behind long transactions, gh-ost/pt-online-schema-change/replication-lag-aware throttling; Alembic and Django migrations (non-atomic migrations for concurrent index, `RunPython` backfills, `SeparateDatabaseAndState`), major version upgrades (`pg_upgrade`, logical replication cut-over; MySQL in-place upgrade) | Migration state flow diagram, lock queue timeline for DDL (analogy: one car stalled at a toll gate blocks the whole highway) | **SaaS multi-tenant migration lab** (`SANDBOX`): add NOT NULL column with batched backfill and split a table in both engines under simulated concurrent traffic; write safe Alembic and Django migrations; major-upgrade cut-over as `SIMULATED` step-through | `ALTER TABLE` without `lock_timeout`, single giant backfill transaction, dropping column before code deploy, assuming InnoDB `ALTER` never locks, migration and deploy ordering errors |
| M9 | Operations: maintenance, observability, backup | Learner diagnoses bloat/wraparound risk, finds top queries, and defines a tested backup/PITR plan with RPO and RTO | PostgreSQL vacuum and bloat (dead tuples, autovacuum thresholds and per-table tuning, HOT updates and `fillfactor`, visibility map, long transactions/replication slots/prepared transactions holding back the xmin horizon, transaction-ID wraparound and freezing, `VACUUM FULL` vs `pg_repack`); InnoDB purge, history list length, undo growth from long transactions, buffer pool tuning basics, redo log sizing; observability (`pg_stat_statements`, `pg_stat_activity`, `pg_stat_io`, `auto_explain`, `log_min_duration_statement`; MySQL slow query log, `performance_schema`, `sys` schema, `pt-query-digest`); key metrics (connections, cache hit, replication lag, bloat, checkpoint pressure); backups: logical (`pg_dump`, `mysqldump`/MySQL Shell dump) vs physical (`pg_basebackup`, incremental backup in PostgreSQL 17, pgBackRest/Barman, Percona XtraBackup), WAL archiving and PITR (`recovery_target_time`), binlog-based PITR for MySQL, managed snapshots, restore drills; PostgreSQL 18 notes (async I/O, data checksums on by default in `initdb`, `pg_upgrade` retains planner statistics) | Dead-tuple lifecycle animation (analogy: a library where old book copies cannot be shelved away while one reader still holds the old edition), xmin-horizon diagram, PITR timeline | Create bloat via long transaction and recover, plus wraparound-age demo (`SANDBOX`, limited-privilege role); find top-3 queries via `pg_stat_statements` and slow log (`SANDBOX`); PITR drill as a `SIMULATED` guided step-through with recorded outputs (an optional single-node base-backup-and-restore lab is `[judgement]`) | "Autovacuum is slow so turn it off", `VACUUM FULL` on production, backups never restored, no `idle_in_transaction_session_timeout`, confusing RPO with RTO |
| M10 | ORMs and data access | Learner finds and fixes N+1, over-fetching and transaction-scope bugs in SQLAlchemy and Django ORM code | Lazy vs eager loading (`select_related`, `prefetch_related`, `selectinload`, `joinedload`), bulk operations (`bulk_create`, `executemany`, `COPY`/batch insert), raw SQL and when to use it, session/unit-of-work lifetime, transaction boundaries (`atomic`, `select_for_update`, `on_commit`), async drivers (asyncpg, psycopg 3) and pool behaviour, repositories vs active record, query logging, `only/defer`, keyset pagination in ORM, SQL injection and parameter binding | N+1 query count visualiser (analogy: one trip to the shop per item vs one shopping list) | Fix N+1 and over-fetching in a FastAPI plus SQLAlchemy app and a Django app; add query-count assertion tests. `SANDBOX` | "ORM is slow" without evidence, sessions shared across requests/threads, calling `.all()` into memory, transactions spanning HTTP calls, string-formatted SQL |
| M11 | PostgreSQL vs MySQL decision | Learner gives a structured, balanced recommendation for a given workload and migration scenario | Feature and behaviour differences (planner and join algorithms, MVCC and storage model, isolation defaults, extensions, JSONB vs JSON, replication options, partial/INCLUDE/expression indexes, DDL behaviour, tooling), managed offerings (selection and operations point to A7.M5), ecosystem and licensing/ownership notes (Oracle-owned MySQL, forks such as MariaDB named only), pgvector vs MySQL vector support (**pointer to B3.M4**; MySQL 9.x `VECTOR` type: storage and parsing in Community, but distance functions and ANN indexes are HeatWave-only as of 2026-10-05; Percona Server 9.7 adds a `DISTANCE()` function), migration between engines (type/collation/semantics gaps), decision criteria | Decision tree; side-by-side behaviour matrix | Interview-style comparison drill; write a one-page recommendation incl. a MySQL-to-PostgreSQL migration risk memo. Managed selection points to A7.M5 | Tribal "X is faster" claims, ignoring team skills and managed-service constraints, comparing outdated versions |

### Ownership notes (G-1)

| Module | OWNS | POINTS |
|---|---|---|
| M1 | SQL fluency, windows, CTEs, dialect gaps (O-28) | practical SQL round execution -> C3.M7 |
| M2 | Schema design, constraints, UUIDv7, multi-tenant data model and RLS (O-29, O-38) | tenant-aware API design -> A5.M11; multi-tenant RAG isolation -> B3.M7; tenant architecture -> A6.M9 |
| M3 | Indexing, FTS, JSONB indexing (O-30) | pgvector and ANN indexing -> B3.M4 (one 20-minute pointer lesson) |
| M4 | Plans, tuning, keyset SQL (O-30) | API pagination contract and cursors -> A5.M3; N+1 -> A4.M10 (this course, later module) |
| M5 | Isolation, MVCC (O-31) | delivery semantics and distributed consistency -> A6.M3 |
| M6 | Locking, deadlocks, `SKIP LOCKED` primitive (O-16, O-31) | DB-queue-vs-broker decision -> A2.M9; idempotency keys -> A5.M4; outbox -> A5.M8 |
| M7 | DB-level replication, partitioning, pooling (O-32) | sharding strategy and consistent hashing -> A6.M3; async pools -> A2.M8; managed DB and proxies -> A7.M5; caching -> A5.M5 and A6.M2 |
| M8 | Migrations, online DDL (O-34) | deploy pipelines and blue-green -> A7.M8 |
| M9 | Vacuum, backup and PITR concepts, DB observability (O-34) | DR implementation and restore testing at infra level -> A7.M10; dashboards and tracing -> A7.M9 |
| M10 | ORM pitfalls, N+1, sessions (O-35) | async handler concurrency -> A2.M8; API layering -> A5.M10 |
| M11 | Engine comparison and migration risks | managed selection -> A7.M5; vector depth -> B3.M4 |

## 3. Per-module detail

Hours: Simpler 30.0 h and Deeper 22.0 h sum exactly to the header (assessment allowance of about 3 h is separate). Lesson IDs are `A4.Mn.k`. Question-bank targets apply to every module (template minimum, IDs `Q-A4-Mn-nn`): 12 MCQ/concept, 6 predict-the-result, 4 SQL tasks with hidden tests (run on both engines unless the topic is engine-specific), 8 verbal questions with follow-up trees (the 3 seeds below are the first 3 of the 8), 3 scenario/design questions. Mastery check per module: 6–10 items, 80% pass threshold, revision loop on misses. Each lesson has at least one diagram or step-through with alt text and one annotated worked example.

| Module | Hours S / D | Lessons (Simpler / Deeper notes) | Mastery-check focus | Module badge criteria |
|---|---|---|---|---|
| M1 | 1.5 / 1.0 | A4.M1.1 Join and NULL semantics (S: row-flow walkthroughs; D: anti-joins, `NOT IN` traps). A4.M1.2 Window functions (S: ranking and running totals; D: frames, gaps-and-islands). A4.M1.3 CTEs and recursion (S: readability CTEs; D: recursive hierarchies, materialisation control). A4.M1.4 Dialect map and upserts (S: cheat-sheet; D: `MERGE`, `ON CONFLICT` vs `ON DUPLICATE KEY`) | Window frames, NULL logic, dialect gaps | Mastery 80%+, 12 SQL tasks pass in both dialects |
| M2 | 2.5 / 2.0 | A4.M2.1 Keys and identity (S: PK basics; D: UUIDv7, clustered PK impact). A4.M2.2 Constraints and types (S: core constraints; D: exclusion, deferrable, collations). A4.M2.3 JSONB vs relational (S: rules of thumb; D: indexing implications). A4.M2.4 Tenancy and soft delete (S: patterns; D: RLS, partial unique indexes) | Constraint choice, type choice, trade-off justification | Mastery 80%+, 3 schema critiques scored at or above rubric "Meets" |
| M3 | 3.5 / 2.5 | A4.M3.1 B-tree and clustered vs heap (S: lookup walkthrough; D: page splits, fillfactor). A4.M3.2 Composite, covering, partial (S: ordering rules; D: skip scan, index-only and visibility map). A4.M3.3 Specialised indexes (S: when GIN/BRIN; D: JSONB opclasses, multi-valued indexes). A4.M3.4 Full-text and search (S: basic FTS; D: ranking, trigram). A4.M3.5 Index cost and pgvector pointer (S: write cost; D: bloat, invisible indexes; pgvector part is a 20-minute pointer to B3.M4, no lab) | Composite order, selectivity, covering logic | Mastery 80%+, 8 index-fix tasks pass |
| M4 | 3.5 / 2.5 | A4.M4.1 Reading plans (S: node-by-node; D: estimates vs actuals, buffers). A4.M4.2 Join algorithms and stats (S: choosing algorithm; D: `CREATE STATISTICS`, histograms). A4.M4.3 Anti-patterns and rewrites (S: sargability; D: pagination, generic plans). A4.M4.4 Finding what to tune (S: slow log; D: `pg_stat_statements` workflow) | Plan interpretation, bottleneck prediction | Mastery 80%+, 6 tuning tasks with verified plan improvement |
| M5 | 4.0 / 3.0 | A4.M5.1 ACID and durability knobs (S: definitions; D: flush settings). A4.M5.2 PostgreSQL MVCC and isolation (S: snapshots; D: SSI, xmin). A4.M5.3 InnoDB MVCC and locking reads (S: consistent vs locking reads; D: next-key locks). A4.M5.4 Anomaly lab (S: guided; D: unguided with fixes and retries). A4.M5.5 Deadlocks (S: causes; D: reading logs) | Per-engine isolation behaviour, anomaly-to-fix mapping | Mastery 80%+, all lab anomalies reproduced and fixed in both engines |
| M6 | 3.0 / 2.5 | A4.M6.1 Row locks and modes (S: `FOR UPDATE`; D: lock strength matrix). A4.M6.2 Queue with `SKIP LOCKED` (S: basic; D: visibility timeout, poison messages). A4.M6.3 Advisory and optimistic locks (S: use cases; D: pooler interaction). A4.M6.4 Diagnosing blocking (S: views; D: blocking chains, DDL queue) | Choosing a locking strategy, blocking diagnosis | Mastery 80%+, queue passes concurrency hidden tests |
| M7 | 3.5 / 2.5 | A4.M7.1 Replication types (S: concepts; D: slots, GTID, logical). A4.M7.2 Lag and consistency (S: read-your-writes; D: causal reads, `synchronous_commit`). A4.M7.3 Partitioning (S: range partitioning; D: pruning, maintenance, constraints). A4.M7.4 Connection pooling (S: why and what; D: PgBouncer modes and sizing). A4.M7.5 When sharding is justified (S: when not to; D: cost of cross-shard queries; strategy is a pointer to A6.M3) | Topology choice, lag handling, pooler pitfalls | Mastery 80%+, scaling plan rubric at or above "Meets", pruning task passes |
| M8 | 2.5 / 2.0 | A4.M8.1 Expand/contract (S: sequence; D: multi-deploy coordination). A4.M8.2 PostgreSQL safe DDL (S: concurrent index; D: `NOT VALID`, rewrites). A4.M8.3 MySQL online DDL (S: algorithms; D: MDL queueing, gh-ost). A4.M8.4 Backfills and tool-specific migrations (S: batches; D: Alembic/Django non-atomic, upgrades) | DDL lock reasoning, migration ordering | Mastery 80%+, migration plan rubric "Meets", both-engine lab passes |
| M9 | 2.5 / 2.0 | A4.M9.1 Vacuum and bloat (S: dead tuples; D: xmin horizon, wraparound). A4.M9.2 InnoDB purge and buffer pool basics (S: history list; D: redo sizing). A4.M9.3 Observability toolkit (S: where to look; D: statement stats workflows). A4.M9.4 Backup and PITR (S: strategies; D: WAL/binlog PITR, incremental, drills) | Diagnosis from symptoms, RPO/RTO reasoning | Mastery 80%+, PITR drill and bloat lab completed |
| M10 | 2.0 / 1.0 | A4.M10.1 N+1 and loading strategies (S: detect and fix; D: strategy trade-offs). A4.M10.2 Bulk and raw SQL (S: bulk APIs; D: `COPY`, batching). A4.M10.3 Sessions and transactions (S: lifetimes; D: async, `on_commit`). A4.M10.4 Injection and safe SQL (S: binding; D: dynamic SQL patterns) | Query-count reasoning, transaction scope | Mastery 80%+, two apps fixed with query-count tests |
| M11 | 1.5 / 1.0 | A4.M11.1 Behaviour matrix (S: summary; D: planner/storage internals). A4.M11.2 Decision criteria (S: checklist; D: weighted trade-off). A4.M11.3 Migration between engines (S: gaps list; D: semantics and collation traps). A4.M11.4 Comparison drill (S: guided; D: unprompted voice round) | Balanced recommendation, factual accuracy | Mastery 80%+, recommendation rubric "Meets" |

## 4. Seed interview questions (3 per module; style-of-question items, not questions from any named company)

Format: Q — difficulty — competency tag — company-style cluster. Ideal-answer outline; weak answers; probes. Difficulty words map to the data-model integer scale 1-5 at import: easy 1, easy-medium 2, medium 3, medium-hard 4, hard 5 (same mapping as Track C); calibrated after the pilot.

### M1
**M1-Q1** Find the top 3 earners per department and handle ties — Medium — `db.sql` — IT-services / Big4
- Ideal: window function `DENSE_RANK()` or `ROW_NUMBER()` partitioned by department; state tie semantics difference (`RANK` gaps vs `DENSE_RANK` vs arbitrary `ROW_NUMBER`); filter in outer query or CTE because windows cannot be in `WHERE`; mention `LATERAL` alternative and index on (dept, salary desc); MySQL 8.0+ supports windows, so portable; NULL salary ordering (`NULLS LAST` PostgreSQL only).
- Weak: correlated subquery with `LIMIT` per department; using `GROUP BY` only; no tie discussion.
- Probes: What changes if only 2 rows per department are needed at scale? How do you make it index-friendly? What if salary can be NULL?

**M1-Q2** Why does `WHERE id NOT IN (SELECT ref_id FROM t)` return nothing sometimes? — Medium — `db.sql` — global-product / IT-services
- Ideal: three-valued logic; `x NOT IN (…, NULL)` evaluates to UNKNOWN so no rows; use `NOT EXISTS` or `LEFT JOIN … IS NULL`; planner can turn `NOT EXISTS` into an anti-join; NOT NULL constraint prevents it; same in both engines; related NULL pitfalls (`= NULL`, `COUNT(col)`, unique constraints with NULLs and PostgreSQL 15 `NULLS NOT DISTINCT`).
- Weak: "NOT IN is slower" without NULL explanation; blaming data type.
- Probes: Does `IN` have the same issue? How do you verify via the plan? What does a unique index do with multiple NULLs in each engine?

**M1-Q3** Compute daily active users with 7-day rolling retention using SQL — Hard — `db.sql` — global-product / Indian-product
- Ideal: distinct user-day CTE; self-join or window with `RANGE BETWEEN INTERVAL` (PostgreSQL) vs `ROWS` over a calendar spine (portable); explain `ROWS` vs `RANGE` frame; generate calendar with `generate_series` or recursive CTE (MySQL); handle missing days; consider pre-aggregation or materialised view for scale; verify with small data.
- Weak: counting rows not distinct users; ignoring days with no events; using `ROWS` over sparse data.
- Probes: How does this scale to billions of events? Materialised view refresh strategy? Differences in MySQL for date intervals?

### M2
**M2-Q1** Would you use UUID or bigint primary keys? — Medium — `db.schema` — global-product / AI-startup
- Ideal: bigint identity compact and index-local; UUID helps client-side generation, merging systems, non-guessable ids; random UUIDv4 fragments B-trees and in InnoDB the clustered index and every secondary index (PK is stored in secondaries); time-ordered UUIDv7 (`uuidv7()` in PostgreSQL 18, app-generated otherwise) restores locality; store as `uuid` or `BINARY(16)` (MySQL `UUID_TO_BIN`) not char(36); expose public ids separately if needed.
- Weak: "UUID is always better for security/scale"; storing as varchar without cost awareness.
- Probes: Impact on index size and cache hit rate? Sharded environment consideration? Sorting by creation time?

**M2-Q2** Design a multi-tenant SaaS schema — Hard — `db.schema` — global-IT-consulting / global-product
- Ideal: options (shared schema with `tenant_id`, schema-per-tenant, database-per-tenant) and trade-offs (isolation, ops cost, noisy neighbours, migrations across many schemas); composite keys/unique constraints including `tenant_id`; indexes leading with `tenant_id`; PostgreSQL row-level security as defence in depth, with pooling caveat (set tenant per transaction); partition by tenant for large tenants; backup/restore of one tenant; plan for moving a big tenant out.
- Weak: only "add tenant_id column"; ignoring cross-tenant leaks and migration cost.
- Probes: How do you prevent a missing `WHERE tenant_id` bug? How do you migrate 5,000 schemas? Noisy neighbour mitigation?

**M2-Q3** When would you store data as JSONB instead of columns? — Medium — `db.schema` — AI-startup / IT-services
- Ideal: JSONB for sparse, evolving, user-defined attributes and raw payload capture; relational for fields used in joins, constraints, filters, aggregates; JSONB indexing (GIN, expression indexes) and its limits; no per-key statistics so plans suffer (extract hot keys to columns or generated columns); validation via CHECK or app schema; update rewrites whole value (TOAST); MySQL JSON with generated-column/multi-valued indexes; hybrid approach.
- Weak: "JSON avoids migrations so use it everywhere"; unaware of statistics and update cost.
- Probes: How to index "tags contains X"? What happens on updating one key of a large document? How to migrate a hot key to a column online?

### M3
**M3-Q1** Why does my composite index on (a, b, c) not help `WHERE b = 5`? — Medium — `db.indexing` — IT-services / Indian-product
- Ideal: B-tree sorted by a then b then c, so leftmost-prefix rule; without a equality, b is not contiguous; options: reorder, add index on (b, …), or in PostgreSQL 18 skip scan may help when `a` has few distinct values (verify via plan); equality columns before range columns; selectivity; cost of extra indexes; check with EXPLAIN not guesswork; MySQL has had a skip-scan range access method since 8.0.13 (optimizer switch `skip_scan`, narrow conditions: verify with EXPLAIN rather than assume).
- Weak: "index not used because of statistics" with no structural explanation; "create index on every column".
- Probes: What about `WHERE a = 1 ORDER BY c`? How does column order change with a range on `a`? What does an index-only scan need in PostgreSQL?

**M3-Q2** Explain InnoDB's clustered index and its consequences — Hard — `db.indexing` — global-product / Big4
- Ideal: table stored as B+tree keyed on PK; secondary indexes store PK values as row pointer, so lookup costs two B-tree traversals (covering avoids it); wide PK bloats every secondary index; random PK causes page splits; auto-increment gives append-only inserts; no PK means hidden row id; contrast PostgreSQL heap with ctid-pointing indexes and HOT updates; effect on range scans by PK and on `UPDATE` of PK.
- Weak: "primary key is just a unique index"; no secondary index behaviour.
- Probes: Impact of UUIDv4 PK? Why can PostgreSQL do index-only scans only with a visibility map? Where does PostgreSQL CLUSTER differ?

**M3-Q3** Design indexing for a search page: filter by status, JSONB attributes and free-text — Hard — `db.indexing` — global-product / AI-startup
- Ideal: partial index for hot status; GIN on JSONB (`jsonb_path_ops` for containment only, smaller) or expression indexes for frequently filtered keys; full-text via `tsvector` generated column plus GIN, `websearch_to_tsquery`, ranking; `pg_trgm` for fuzzy/`ILIKE`; MySQL FULLTEXT and multi-valued index equivalents; measure write cost; pointer: semantic/vector search belongs to pgvector (course B3), not forced into GIN/FTS; consider dedicated search engine beyond a size/feature threshold.
- Weak: `LIKE '%term%'` with B-tree; one giant index on all columns.
- Probes: GIN fast-update pending list and write cost? When do you leave the database for search? How do you check an index is used?

### M4
**M4-Q1** Walk me through reading an EXPLAIN ANALYZE output — Medium — `db.tuning` — all clusters
- Ideal: read the tree bottom-up, inner to outer; compare estimated vs actual rows (large gaps signal stale stats or correlation); actual time × loops; scan type and filter vs index condition; rows removed by filter; join algorithm fit; buffers (shared hit/read; default in PG 18 `EXPLAIN ANALYZE`) and sorts spilling to disk; note MySQL `EXPLAIN ANALYZE` tree format and `rows`/`loops`; run on realistic data; caution that `ANALYZE` executes the query (wrap writes in a rolled-back transaction).
- Weak: reading only the top cost number; ignoring actual vs estimate; running on tiny dev data.
- Probes: What does "Rows Removed by Filter: 2M" suggest? What if estimates are 1 and actuals 1M? How do you safely analyse an `UPDATE`?

**M4-Q2** A query got slow after data growth; nothing changed in code. What do you do? — Hard — `db.tuning` — IT-services / Indian-product
- Ideal: capture the query and parameters (`pg_stat_statements`, slow log); compare old/new plans; refresh stats (`ANALYZE`), check bloat and autovacuum; plan flip due to row-count change (nested loop to hash, or index to seq scan); missing or non-selective index; generic plan for prepared statements; extended statistics for correlated columns; lock waits vs execution time; fix by index/rewrite, not hints first; add regression guard (plan test or alert).
- Weak: "add more RAM", "add index" without evidence; restart database.
- Probes: How do you prove it is a plan regression and not contention? When do you use `pg_hint_plan` or MySQL optimizer hints? How would you stop recurrence?

**M4-Q3** Why is `OFFSET 100000 LIMIT 20` slow and what is the alternative? — Medium — `db.tuning` — global-product / IT-services
- Ideal: engine must produce and discard offset rows; cost grows linearly; keyset (seek) pagination `WHERE (created_at, id) < (:c, :id) ORDER BY created_at DESC, id DESC LIMIT 20` with matching composite index; deterministic tie-breaker; row-value comparison support (PostgreSQL yes; MySQL accepts row constructors, but the 8.4 manual warns the optimizer is less likely to use an index when the row constructor does not cover an index prefix and recommends the expanded form `a < x OR (a = x AND b < y)`, verified 2026-10; whether a full-prefix inequality `(a, b) < (x, y)` is range-optimized is not documented, so confirm in EXPLAIN **[verify: not stated in MySQL 8.4 or 9.7 docs; run EXPLAIN in the sandbox image]**); trade-offs (no random page jump, API cursor design lives in A5); total counts are separate cost.
- Weak: "add an index on the order column" only; cache the pages.
- Probes: How do you handle backward paging? Sorting by non-unique column? Cost of `COUNT(*)` on large tables?

### M5
**M5-Q1** What does REPEATABLE READ guarantee in PostgreSQL vs MySQL InnoDB? — Hard — `db.transactions` — global-product / Big4
- Ideal: PostgreSQL RR = snapshot isolation; snapshot taken at first statement; no phantoms or non-repeatable reads; concurrent update to a row you changed raises serialization failure (40001), so retry; write skew still possible (only SERIALIZABLE/SSI prevents). InnoDB RR: consistent snapshot at first consistent read; plain SELECT reads snapshot, but UPDATE/DELETE/locking reads act on latest committed rows, so you can "see" changes you did not read and lose updates in read-modify-write; next-key/gap locks block phantoms for locking reads; no automatic serialization error. SQL standard allows phantoms at RR, engines differ.
- Weak: reciting the textbook table; claiming both behave the same; "RR prevents all anomalies".
- Probes: Show how a lost update occurs in MySQL RR and how to fix it. Give an example of write skew. Which level would you choose for money transfer in each engine?

**M5-Q2** Explain MVCC in PostgreSQL and why vacuum exists — Medium — `db.transactions` — global-product / IT-services
- Ideal: updates create new tuple versions; `xmin`/`xmax` and snapshots determine visibility; readers do not block writers; old versions become dead once no snapshot needs them; vacuum reclaims space, updates visibility map, freezes old XIDs to avoid wraparound; long transactions, abandoned replication slots and prepared transactions hold the horizon back; HOT updates avoid index churn; contrast with InnoDB undo logs (old versions reconstructed from undo; purge thread; history list length).
- Weak: "PostgreSQL locks rows on read"; "vacuum just defragments".
- Probes: What causes table bloat despite autovacuum running? What is transaction ID wraparound and what protects against it? Why can `VACUUM FULL` be dangerous?

**M5-Q3** Two concurrent requests both check inventory then decrement; stock goes negative. Fix it. — Medium — `db.transactions` — all clusters
- Ideal: identify check-then-act race; options ranked: atomic conditional `UPDATE stock SET qty = qty - :n WHERE id = :id AND qty >= :n` and check affected rows; CHECK constraint `qty >= 0`; `SELECT … FOR UPDATE` in a transaction; optimistic version column with retry; SERIALIZABLE with retry (PostgreSQL SSI; MySQL locks); keep transaction short; idempotency via unique request id; test with two sessions.
- Weak: "use Python lock"; "use higher isolation" without retry handling; application-level check only.
- Probes: Behaviour under MySQL RR if you use plain select then update? How does `FOR UPDATE` behave with multiple rows and lock ordering? What retry policy?

### M6
**M6-Q1** How do you build a reliable job queue in PostgreSQL or MySQL? — Medium — `db.locking` — global-product / AI-startup
- Ideal: table with status, run_at, attempts, locked_until; claim with `SELECT … FOR UPDATE SKIP LOCKED LIMIT n` inside `UPDATE … RETURNING` CTE (PostgreSQL) or select-then-update in one transaction (MySQL 8.0.1+); partial index on pending rows; visibility timeout and heartbeat; retry/backoff and dead-letter; idempotent handlers; keep claim transaction short; vacuum/bloat considerations from high churn; limits versus a broker (throughput, fan-out), `LISTEN/NOTIFY` for wake-ups (not durable); outbox pattern.
- Weak: polling with `SELECT` then `UPDATE` without locks; deleting rows immediately without failure handling.
- Probes: What if a worker crashes mid-job? Why do queue tables bloat and how to mitigate? When do you move to Redis/SQS/Kafka (see A6)?

**M6-Q2** Given this deadlock log, explain the cause and prevention — Hard — `db.locking` — IT-services / Big4
- Ideal: identify two transactions locking rows in opposite order (or gap/next-key lock conflicts on InnoDB, or FK-related locks); engine behaviour: PostgreSQL waits `deadlock_timeout` then aborts one (40P01); InnoDB detects immediately and rolls back the victim (1213); retry the victim; prevent via consistent lock ordering, smaller transactions, locking up front, lower isolation on InnoDB (RC reduces gap locks), index to narrow scanned range; enable logging (`log_lock_waits`, `innodb_print_all_deadlocks`); `lock_timeout` for DDL.
- Weak: "deadlocks mean the DB is broken"; increase timeout only.
- Probes: Why can a deadlock occur with a single-row insert pattern in InnoDB? How do you reproduce it in a test? Should you retry automatically?

**M6-Q3** Why can a harmless `ALTER TABLE` take your service down? — Hard — `db.locking` — global-product / IT-services
- Ideal: `ACCESS EXCLUSIVE` lock (PostgreSQL) or metadata lock (MySQL) waits behind a long-running transaction, and all later queries queue behind the waiting DDL; outage is caused by the queue; mitigate with `lock_timeout` plus retries, killing/avoiding long transactions, `idle_in_transaction_session_timeout`, doing DDL at low traffic, online DDL options; diagnosing via `pg_locks`/`pg_blocking_pids`, `performance_schema.metadata_locks`.
- Weak: "ALTER was slow because the table is big" only.
- Probes: Which `ALTER` operations are instant? How do you detect the long transaction? What about foreign key additions?

### M7
**M7-Q1** Design read scaling for a read-heavy API and explain the consistency impact — Medium — `db.scaling` — global-product / IT-services
- Ideal: add async replicas, route reads via app/proxy; lag source (single-threaded apply, long queries on replica, large transactions); read-your-writes strategies (read from primary after write for a window, session stickiness, LSN/GTID wait, semi/synchronous for critical flows); replica query conflicts in PostgreSQL (`max_standby_streaming_delay`, `hot_standby_feedback` bloat trade-off); caching alongside; monitor lag; failover implications (possible data loss with async); know when to stop at vertical scale and indexing first.
- Weak: "just add replicas" with no lag discussion; routing all reads to replicas including post-write reads.
- Probes: How do you detect lag in each engine? What happens to replicas on failover? What if the replica is serving long analytics queries?

**M7-Q2** Explain PgBouncer modes and what breaks in transaction pooling — Hard — `db.scaling` — global-product / AI-startup
- Ideal: PostgreSQL process-per-connection cost, so pool many clients onto few server connections; session mode (full compatibility, low multiplexing), transaction mode (best multiplexing, server connection held per transaction), statement mode (rare); breakage in transaction mode: session-level `SET`, session advisory locks, `LISTEN` (`NOTIFY` itself works), session-level temp tables, SQL-level `PREPARE`/`DEALLOCATE` (PgBouncer 1.21 and later track protocol-level prepared statements when `max_prepared_statements` is non-zero; default 200 from 1.24; verified 2026-10); pool sizing (cores-driven, not client-count-driven); app pools plus PgBouncer layering; Django `CONN_MAX_AGE` and SQLAlchemy `pool_pre_ping`; serverless/Lambda connection storms and RDS Proxy.
- Weak: "pooler makes queries faster"; set pool size equal to `max_connections`.
- Probes: How do you size it? What do you do for advisory locks? How do async apps (FastAPI) interact with pools?

**M7-Q3** When would you partition a table and what are the constraints? — Medium — `db.scaling` — Big4 / global-product
- Ideal: use cases: time-based retention (drop/detach partition vs mass delete), pruning on partition key, maintenance (vacuum per partition), data lifecycle; not a general speed-up; PostgreSQL: declarative range/list/hash, PK/unique must include partition key, no global indexes, plans need partition key in `WHERE` for pruning, `DETACH CONCURRENTLY`, automation with `pg_partman`; MySQL: unique keys must include partitioning columns, no FKs, partition count limits; many partitions raise planning cost; compare to sharding.
- Weak: "partitioning always improves performance"; ignoring unique-constraint limitation.
- Probes: How do you verify pruning? How do you migrate an existing large table into partitions online? Hash vs range?

### M8
**M8-Q1** How do you add an index on a huge production table safely? — Medium — `db.operations` — all clusters
- Ideal: PostgreSQL `CREATE INDEX CONCURRENTLY` (two scans, no write block, cannot run in a transaction block, can leave an INVALID index to drop/retry, takes longer, watch disk and replication); `REINDEX CONCURRENTLY`; MySQL InnoDB online `ALGORITHM=INPLACE, LOCK=NONE` (brief metadata lock at start/end; wait behind long transactions), or gh-ost/pt-osc when needed; check replica lag and I/O; `lock_timeout`; verify with `EXPLAIN` and `pg_stat_progress_create_index`; Alembic/Django non-atomic migration setting.
- Weak: plain `CREATE INDEX` at off-peak; "just add it".
- Probes: What if it fails halfway? How do you monitor progress? Unique index concurrently, any risk?

**M8-Q2** Add a NOT NULL column with a default to a 500M-row table — Hard — `db.operations` — global-product / IT-services
- Ideal: PostgreSQL 11+: `ADD COLUMN … DEFAULT const` is metadata-only (volatile defaults rewrite); NOT NULL via nullable add, batched backfill, add `CHECK (col IS NOT NULL) NOT VALID`, `VALIDATE CONSTRAINT`, then `SET NOT NULL` (skips scan when valid check exists) ; MySQL `ALGORITHM=INSTANT` add column (8.0.12+, limits on row versions) then backfill in batches by PK ranges with throttling; expand/contract with deploy ordering; backfill transaction size, replica lag and vacuum; rollback plan.
- Weak: single `ALTER TABLE … NOT NULL DEFAULT` in a deploy without lock analysis; one giant `UPDATE`.
- Probes: How big should batches be? What order for code deploy vs migration? How do you keep replicas healthy?

**M8-Q3** Plan a zero-downtime PostgreSQL major version upgrade — Hard — `db.operations` — global-product / Big4
- Ideal: options: `pg_upgrade` (in-place, downtime short with `--link`, retains planner statistics from PG 18), logical replication to a new cluster with cut-over (sequences and DDL handled manually), managed blue/green; rehearsal on a restored copy; extension compatibility; post-upgrade `ANALYZE`; rollback plan; replication slots and failover slots (PG 17); application connection draining; test on realistic traffic.
- Weak: `pg_dump`/restore of a multi-TB database during business hours; no rollback.
- Probes: How do you handle sequences? What is your rollback point? How do you validate data equality?

### M9
**M9-Q1** Autovacuum is running but a table keeps bloating. Diagnose. — Hard — `db.operations` — global-product / IT-services
- Ideal: check `pg_stat_user_tables` (n_dead_tup, last_autovacuum); find xmin holders: long transactions, idle-in-transaction sessions, stale replication slots, prepared transactions, `hot_standby_feedback` replicas; per-table autovacuum scale factor and cost limits for large/high-churn tables; fillfactor/HOT; remediate by killing/limiting long transactions and timeouts, tuning thresholds, `pg_repack` or partitioning for reclaim; `VACUUM FULL` locks exclusively; monitor bloat and wraparound age.
- Weak: "run VACUUM FULL nightly"; increase `autovacuum_naptime` only.
- Probes: How do you detect wraparound risk? What does a stale replication slot do? How does bloat hurt index-only scans?

**M9-Q2** How do you find and prioritise the slowest queries in production? — Medium — `db.operations` — all clusters
- Ideal: PostgreSQL `pg_stat_statements` (needs `shared_preload_libraries`; rank by total time, mean time, calls, rows, shared blocks), `log_min_duration_statement`, `auto_explain`; MySQL slow query log, `performance_schema` digest tables and `sys.statement_analysis`, `pt-query-digest`; prioritise by total impact (calls × time) not worst single; correlate to endpoints via query tags/comments; reset stats around changes; sample vs overhead; then plan analysis.
- Weak: "look at CPU graphs"; fix the single slowest query seen once.
- Probes: What is queryid normalisation? What overhead? How do you tie a query to a service endpoint?

**M9-Q3** Design backup and recovery for a 2 TB PostgreSQL database with RPO 5 minutes — Hard — `db.operations` — Big4 / global-IT-consulting
- Ideal: physical base backups plus continuous WAL archiving (pgBackRest/Barman), PITR via `recovery_target_time`; incremental backups (PostgreSQL 17 native or tool-based) for time; RPO driven by WAL archive frequency/sync replication, RTO by restore speed and replay; separate logical dumps for portability/partial restores; offsite and immutable copies, encryption, retention; regularly test restores and measure RTO; MySQL equivalent (XtraBackup/Shell dump plus binlog PITR); managed-service snapshot and PITR limits; runbook and alerting on archive failures.
- Weak: nightly `pg_dump` only; backups never restored; confusing replication with backup.
- Probes: How do you PITR to just before a bad `DELETE`? How do you validate backups? What if a replica replicated the bad delete?

### M10
**M10-Q1** Explain and fix the N+1 problem in Django and SQLAlchemy — Easy — `db.orm` — all clusters
- Ideal: one query for parents, one per child access; detect with query logging, `assertNumQueries`, SQLAlchemy events, tools; fix: Django `select_related` (joins, FK/one-to-one) vs `prefetch_related` (separate query, many); SQLAlchemy `joinedload` vs `selectinload` and `lazy` settings; trade-offs (join row explosion vs extra query); limit columns (`only`, `load_only`); add test guard.
- Weak: "cache it"; "use raw SQL everywhere".
- Probes: When is `joinedload` worse than `selectinload`? How to avoid N+1 in serializers (DRF)? Async SQLAlchemy lazy-load error?

**M10-Q2** How should transactions and sessions be scoped in a FastAPI or Django app? — Medium — `db.orm` — global-product / AI-startup
- Ideal: one session/transaction per request (or unit of work) via dependency/middleware; never share sessions across threads/tasks; keep transactions short, no network calls inside; Django `ATOMIC_REQUESTS` trade-offs, `transaction.on_commit` for side effects (tasks, emails); SQLAlchemy `expire_on_commit`, explicit commit/rollback; async engines and pool sizing; outbox for reliable events; retry on serialization/deadlock errors.
- Weak: global session; auto-commit everywhere; enqueue task before commit.
- Probes: Why does a Celery task sometimes not see the row? How does pool size relate to worker count? How handle partial failures?

**M10-Q3** When do you drop to raw SQL, and how do you keep it safe and testable? — Medium — `db.orm` — IT-services / Indian-product
- Ideal: complex analytics, window functions, bulk upserts, `COPY`, performance-critical paths, engine-specific features; always parameter binding (never f-strings), use SQLAlchemy Core/`text()` with bind params; repository layer to contain it; tests against real engine (the sandbox mirrors this); migrations ownership; mapping rows to typed models; trade-off of portability.
- Weak: "ORMs are bad"; concatenating user input.
- Probes: How do you prevent injection in dynamic `ORDER BY`? How do you test raw queries across both engines? Bulk insert best approach in each engine?

### M11
**M11-Q1** PostgreSQL or MySQL for a new product? — Medium — `db.sql` — global-IT-consulting / IT-services
- Ideal: ask workload, team skills, managed-service constraints, consistency needs first; PostgreSQL strengths: richer SQL, extensions (PostGIS, pgvector), JSONB and GIN, partial/INCLUDE/expression indexes, SSI, strict constraints; MySQL strengths: operational familiarity, mature replication tooling (Group Replication/InnoDB Cluster, ProxySQL/Vitess ecosystem), simple read-heavy OLTP; honest caveats (version-specific, both viable); avoid dogma; plan for exit and hosting; conclude with a recommendation and conditions that would flip it.
- Weak: "PostgreSQL is better, period"; benchmark folklore.
- Probes: What would change your decision for heavy JSON workloads? For multi-region writes? For an existing MySQL team?

**M11-Q2** What breaks when migrating an app from MySQL to PostgreSQL? — Hard — `db.sql` — Big4 / global-IT-consulting
- Ideal: case sensitivity/collation defaults (MySQL case-insensitive), `GROUP BY` strictness, upsert syntax, `AUTO_INCREMENT` vs identity/sequences, zero dates and silent truncation (strict mode), `ENUM`/unsigned types, boolean semantics, isolation behaviour (RR differences, serialization errors needing retry), locking behaviour, `LIMIT` in updates, quoting, full-text and JSON function differences, ORM dialect gaps; tooling (pgloader, AWS DMS) and parallel-run/dual-write validation; performance retuning of indexes and plans.
- Weak: "just dump and load"; only syntax differences.
- Probes: How would you validate data parity? Which isolation-level effect surprises teams most? How do you cut over with minimal downtime?

**M11-Q3** How do pgvector and MySQL's vector support fit into this decision? — Medium — `db.scaling` — AI-startup / global-product
- Ideal: pgvector gives embeddings storage and ANN indexes (HNSW/IVFFlat) inside PostgreSQL, convenient for moderate scale with transactional co-location and metadata filters; detailed index tuning, recall, hybrid search and RAG design are in B3; MySQL has a `VECTOR` type from 9.0, but as of 2026-10-05 Community Edition has storage and parsing only: distance functions and ANN indexes are HeatWave-only (Percona Server 9.7 adds a `DISTANCE()` function); considerations: scale thresholds, filtered search, operational load on OLTP database, when to use a dedicated vector store; keep A4 answer short and route to RAG depth (**pointer item; ref to B3.M4 bank, no re-authoring**).
- Weak: "just use a vector DB always" or claims MySQL and PostgreSQL are equal for ANN without checking.
- Probes: How does filtered vector search behave with HNSW? What load impact on the primary? When would you separate the vector workload?

### 4.1 Goal tags, senior-bar notes and staff-level probes (G-10)

Goal tag format `B:x A:x H:x` for Senior Python Backend, AI Engineer and hybrid (M/S/N = MUST/SHOULD/NICE). Each module adds two "senior-bar upgrade" probes (staff-level follow-ups) after its three seeds. Seed IDs are `Q-A4-Mn-01..03`.

| Module and seeds | Goal tag | Senior-bar note (mid / senior / staff) | Two staff-level probes |
|---|---|---|---|
| M1 Q1-Q3 | B:M A:S H:M | Mid writes a working query; senior states tie semantics, NULL and dialect traps and index support; staff reasons about pre-aggregation, materialised views and cost at billions of rows | (a) Make the retention query incremental so a daily job processes only new events. (b) How do you test SQL correctness on skewed data with hidden duplicates? |
| M2 Q1-Q3 | B:M A:S H:M | Mid adds `tenant_id`; senior compares isolation models with migration and noisy-neighbour cost; staff plans tenant eviction, per-tenant restore and RLS failure modes with pooling | (a) A top tenant outgrows shared tables: describe the zero-downtime move. (b) RLS policy causes a plan regression: how do you detect and fix it? |
| M3 Q1-Q3 | B:M A:S H:M | Mid knows leftmost prefix; senior links clustered index, covering and visibility map to measured behaviour; staff budgets index write cost and drops unused indexes safely | (a) Show with `pg_stat_user_indexes` how you prove an index is unused before dropping it. (b) How does a hot-key GIN index hurt write latency and what do you do? |
| M4 Q1-Q3 | B:M A:S H:M | Mid reads cost; senior compares estimates to actuals and finds root cause; staff builds a regression guard and handles plan flips and generic plans | (a) Design an automated plan-regression alert from `pg_stat_statements`. (b) A plan flips only at month end: hypotheses and evidence you collect. |
| M5 Q1-Q3 | B:M A:S H:M | Mid recites levels; senior explains per-engine behaviour, write skew and retry; staff chooses isolation per business flow and designs retry budgets and observability of 40001 and 1213 | (a) Pick isolation and retry policy for a payments ledger on each engine and justify. (b) How do you prove in CI that a flow is safe under concurrency? |
| M6 Q1-Q3 | B:M A:S H:M | Mid uses `FOR UPDATE`; senior builds a crash-safe `SKIP LOCKED` queue and reads blocking chains; staff quantifies queue limits versus a broker and the bloat and vacuum cost of high churn | (a) At what rate and fan-out do you retire the table queue (point to A2.M9 matrix)? (b) Write the runbook for a blocking chain caused by DDL in production. |
| M7 Q1-Q3 | B:M A:N H:M | Mid adds replicas; senior handles lag, read-your-writes and pooler limits; staff plans failover data loss, replica conflicts, pool sizing and when to stop at vertical scale | (a) Derive pool size from cores, workers and `max_connections` for 3 services behind PgBouncer. (b) Async failover just lost 2 seconds of writes: what is your remediation and policy? |
| M8 Q1-Q3 | B:M A:S H:M | Mid knows concurrent index; senior sequences expand and contract with lock and replica awareness; staff designs migration policy, tooling guardrails and rollback for fleets | (a) Add a lint that blocks unsafe migrations in CI: which rules? (b) Plan an upgrade for 5,000 tenant schemas. |
| M9 Q1-Q3 | B:M A:N H:S | Mid restarts or runs `VACUUM FULL`; senior finds the xmin holder and tunes per table; staff defines RPO and RTO, restore drills, wraparound alerting and capacity | (a) Which three alerts prevent a wraparound emergency, and thresholds? (b) Prove RTO: how do you measure a restore of 2 TB and what dominates? |
| M10 Q1-Q3 | B:M A:S H:M | Mid knows `select_related`; senior reasons about session scope, `on_commit`, async and query-count tests; staff sets team conventions and guardrails (lint, query budgets) | (a) Define a per-endpoint query budget and enforce it in CI. (b) A Celery task cannot see a row: trace the cause across commit and enqueue order. |
| M11 Q1-Q3 | B:S A:N H:S | Mid declares a favourite; senior gives conditions and trade-offs; staff weighs team skill, exit cost, managed constraints and a migration plan with parity checks | (a) Build the cut-over plan from MySQL to PostgreSQL with data parity and rollback. (b) What would make you choose MySQL 8.4 LTS or 9.7 LTS today? |

## 5. Final assessment

- Assessment allowance about 3 h (G-2). 25-item mastery check (mixed across M1–M11, at least 2 items from each module, 80% pass), with revision loop.
- Timed SQL set (60 minutes): 6 tasks in both dialects covering windows, index choice, plan reading, an anomaly fix, and one migration plan; hidden tests check dialect differences.
- Two-session concurrency practical (20 minutes): reproduce and fix a lost update or write skew in the engine of the learner's choice, then explain the behaviour in the other.
- Voice mock round (15 minutes, adaptive follow-ups): "optimise this slow endpoint's queries" followed by an isolation and scaling drill; rubric scores accuracy, structure, trade-off reasoning, and clarity.
- Design/canvas task (30 minutes): read-scaling and migration plan for a growing multi-tenant service (database-level; sharding strategy points to A6.M3), rubric overall 3.5 or above, no dimension below 3.

## 6. Course-level risks and freshness notes (as of 2026-10-05)

- **Verified this pass** (vendor release pages, release-note coverage; checked 2026-10-05): MySQL 8.0 EOL 2026-04-30 (the earlier "final 8.0.46" detail could not be confirmed and was removed); MySQL 9.7 released 2026-04-21 as LTS (first LTS since 8.4; LTS series follow 5 years Premier plus 3 Extended per the manual); calendar versioning YY.M from 26.7; PostgreSQL 18 temporal constraints (`WITHOUT OVERLAPS`, `PERIOD`), skip scan, `uuidv7()`, async I/O, data checksums on by default in `initdb`, `pg_upgrade` retaining planner statistics (PostgreSQL 18 release notes); PostgreSQL 17 failover of logical slots (release notes); PgBouncer 1.21 prepared statements in transaction mode (default on from 1.24); MySQL Community `VECTOR` has no native `DISTANCE()` (documented as HeatWave on OCI and MySQL AI only, not Community or Commercial; Percona Server 9.7.2-2 adds `DISTANCE()`/`VECTOR_DISTANCE()`); MySQL skip scan since 8.0.13 (8.0.13 release notes; `skip_scan` switch on by default in 8.4). All verified 2026-10.
- **Fast-changing**: PostgreSQL 19 is in beta (Beta 4 on 2026-09-24; the announcement says GA may occur in October 2026 after a release candidate in early October, not confirmed; Beta 4 reverted several planned features, e.g. SQL/PGQ and `FOR PORTION OF`, so none of the 19 candidate list is safe to teach yet): add the "What changes in 19" box only after GA and review (candidates seen in coverage: parallel autovacuum workers, `WAIT` command for standbys, plan-advice extensions; unreviewed, do not teach). MySQL innovation releases are quarterly (26.7 first); the next LTS is expected as 28.4 (April 2028 timing is inferred, not stated in primary docs).
- **Still `[verify]` (2 items)**: (1) [verify: MySQL docs do not state whether a full-prefix inequality row comparison `(a, b) > (x, y)` is range-optimized in 8.4 LTS and 9.7 LTS; they only advise the expanded OR form] run EXPLAIN in the sandbox image and record; (2) [verify: Python driver and ORM support for MySQL 9.7 LTS (mysqlclient, PyMySQL, SQLAlchemy, Django backend) is not documented by the vendor] confirm before any 9.7 image is added.
- Managed services (RDS, Aurora, Cloud SQL) change behaviour and limits; teach engine fundamentals and flag managed differences; A7.M5 owns selection and operations.
- Sandbox risk: two-session mode (S-2) is the critical dependency for M5-M6; fallback is recorded step-throughs. Replication and PITR are simulators in 1.0 (R-A4-3), so claims about their behaviour must come from recorded real outputs, not invented ones.
- Authoring load: 11 modules; first wave M3, M4, M5, M6, M8 (R-A4-5).
- Overlap control: pointers stay one-way (A4 is upstream of A5, A6; A5.M3 reuses A4.M4 lab; A2.M9 reuses A4.M6 worker).
- Review cadence: 6 months for engine content (next review April 2027, or on PostgreSQL 19 GA), 12 months for theory; stamp "last reviewed" on every lesson.

## 7. Interview rehearsal link and course readiness criterion

| Interview Master mode | Modules exercised | Use |
|---|---|---|
| Voice drill (strict mode) | all; heaviest M5, M3, M4, M7, M8 | Spoken explanations of isolation per engine, composite index order, plan reading, replication lag, safe DDL |
| Coding / SQL timed | M1, M3, M4, M6 | 60-minute SQL set, hidden tests per dialect; feeds C3.M7 |
| Design canvas | M2, M7, M8, M9 | Schema, read-scaling, migration and backup plans at database level; A6 owns the system canvas |
| Project deep-dive | M4, M8, M9 | Learner defends a real incident (slow query, migration, outage) using the A4 vocabulary |
| Behavioural | M8, M9 | Incident and postmortem stories (link to C1) |

**A4 readiness (G-6 specialised)**: for each tag (`db.sql`, `db.indexing`, `db.tuning`, `db.transactions`, `db.locking`, `db.scaling`, `db.operations`) the learner has module mastery check at or above 85 percent on a retake at least 7 days after first pass; the timed SQL set passed; the two-session practical passed; strict-mode voice drill at Senior bar (average 3.5 or above, none below 3) in two sessions at least 48 hours apart, including one isolation-per-engine answer and one plan-reading answer; no open remediation flag on M1-M2 for L1-L2 learners.

## 8. Learner-Fit Appendix

Reference learner: senior engineer, 11+ years, about 4 years Python, daily PostgreSQL/MySQL, Redis, Docker, AWS, FastAPI/Django/Flask, LLM APIs and voice agents, targeting Senior Python Backend and AI Engineer roles. Rules apply to any learner. Band scores come from each module's 5-8 placement items (G-5): **>= 85 skip, 70-84 skim, 50-69 study, < 50 deep-study**. Per R-A4-5 only M1 and M2 lessons may be skipped; for M3-M10 "skip" means "take the mastery check directly".

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 (new to SQL or databases) | Simpler, 30 h; start with M1 (all lessons), prerequisite SQL basics linked; M7-M9 core concepts only on first pass | 70 percent guided tasks, 30 percent unguided; two-session labs guided | none | none until M1-M4 pass |
| L2 (junior-mid) | Simpler, about 27 h; M1 skim known lessons | 50/50 guided and unguided | known M1 lessons | one Challenge per passed module |
| L3 (experienced, cannot always explain) | Deeper, about 19-22 h; skim M1, M2; study "explain at depth" lessons in M3-M6, M8 | 70 percent unguided, timed SQL, two-session labs unguided | M1 and M2 lessons if banded | per-engine isolation matrix, plan-regression guard |
| L4 (senior, owns it in production; reference learner) | Deeper, about 15-18 h expected after skip (M1-M2 lessons skipped, M10 skimmed, M3-M4 skimmed on score) `[judgement]`; goes to mastery checks, then senior-bar probes and voice drills | 80 percent timed and voice; labs only where the check misses | M1-M2 lessons; others by band | staff-level probes (section 4.1), incident postmortem write-up |

### LF-2 Module matrix

Seed `Mn-Qk` items are bank IDs `Q-A4-Mn-0k` (section 4.1). Diagnostic IDs are `Q-A4-Mn-D1..Dk` (placement-tagged items drawn from the MCQ and predict buckets). Weights are percent of database-interview time per target: B Senior Python Backend, A AI Engineer, H hybrid (each column sums to 100); rationale: databases and transactions are probed in nearly every backend and services/Big 4 solution round, and less deeply in AI-engineer rounds except SQL fluency, schema and ORM `[judgement]`.

| Module | Diagnostics (count: topics) | Tags | Weight B / A / H | Rehearsal modes | Module readiness criterion |
|---|---|---|---|---|---|
| M1 | 6: NOT IN with NULL, COUNT(col), RANGE vs ROWS default frame, top-N per group, gaps-and-islands, MySQL FILTER and FULL JOIN gap | B:M A:S H:M | 10 / 14 / 12 | coding, voice | check 85, timed SQL set in both dialects |
| M2 | 6: money type, timestamptz, UUIDv4 vs v7 in InnoDB, soft delete uniqueness, JSONB vs columns, tenant_id keys | B:M A:S H:M | 8 / 12 / 10 | design canvas, voice | check 85, RLS lab leak test passes, 3 schema critiques at 3.5 |
| M3 | 8: composite order, covering and INCLUDE, function on column, partial index, clustered secondary lookup, GIN opclass, skip scan, write cost | B:M A:S H:M | 12 / 10 / 11 | voice, coding | check 85, 8 index fixes, voice Senior bar |
| M4 | 8: estimate vs actual, rows removed by filter, hash vs nested loop, stale stats, OFFSET cost, keyset tie-breaker, generic plan, EXPLAIN ANALYZE side effect | B:M A:S H:M | 12 / 10 / 11 | voice, coding, project deep-dive | check 85, 6 tuning tasks verified by plan, keyset lab |
| M5 | 8: RR per engine, write skew, lost update in InnoDB, RC snapshot per statement, 40001 retry, MVCC xmin, deadlock error codes, READ UNCOMMITTED in PostgreSQL | B:M A:S H:M | 14 / 12 / 13 | voice, coding (two-session) | check 85, two-session practical, voice Senior bar |
| M6 | 7: FOR UPDATE modes, SKIP LOCKED claim, advisory lock with pooler, lock queue behind ALTER, optimistic version, lock ordering, blocking diagnosis views | B:M A:S H:M | 9 / 8 / 9 | voice, coding, design canvas | check 85, queue passes hidden concurrency tests |
| M7 | 7: replication lag causes, read-your-writes, partition PK rule, pruning, PgBouncer modes, pool sizing, when not to shard | B:M A:N H:M | 10 / 6 / 8 | design canvas, voice | check 85, scaling plan rubric 3.5 |
| M8 | 7: CONCURRENTLY failure state, NOT VALID and VALIDATE, instant add column, MDL queue, backfill batching, expand and contract order, Alembic non-atomic | B:M A:S H:M | 9 / 6 / 8 | design canvas, voice, behavioural | check 85, migration plan rubric 3.5 |
| M9 | 6: dead tuples and xmin holders, wraparound, slow-query workflow, RPO vs RTO, PITR steps, backup vs replication | B:M A:N H:S | 6 / 4 / 5 | voice, design canvas, project deep-dive | check 85, bloat lab, PITR simulator completed |
| M10 | 6: N+1 fix choice, session scope, on_commit, bulk insert, injection in ORDER BY, async lazy load | B:M A:S H:M | 7 / 12 / 10 | coding, voice | check 85, query-count tests pass |
| M11 | 5: engine differences, collation and case, GROUP BY strictness, vector support facts, decision conditions | B:S A:N H:S | 3 / 6 / 3 | voice | check 85, recommendation rubric 3.5 |

### LF-3 Experience bridge ("you already know" -> "gap to senior depth")

| Module | You already know | Gap between using it and explaining it at senior depth |
|---|---|---|
| M1 | ORM filters and raw SELECTs in Django/FastAPI | windows, frames, NULL logic and dialect gaps without an ORM; porting a query across engines |
| M2 | Django models, Alembic tables, Postgres in Docker | defending keys, UUIDv7 vs bigint, tenant models and RLS with pooling |
| M3 | adding an index when a query is slow | composite order, covering, clustered vs heap, proving use and write cost |
| M4 | seeing a slow query in logs | reading EXPLAIN ANALYZE, estimate errors, plan flips, regression guards |
| M5 | `atomic()` and default isolation | what each engine does at RR and SERIALIZABLE, write skew, retry design |
| M6 | Celery workers and `select_for_update` | `SKIP LOCKED` queue correctness, blocking chains, lock queueing on `ALTER TABLE` |
| M7 | RDS replicas and Redis cache | lag and read-your-writes, partition constraints, PgBouncer interplay with A2.M8 async pools, when sharding is justified |
| M8 | running `alembic upgrade` or `migrate` | lock-aware, reversible sequences, MySQL online DDL, big-table backfills |
| M9 | RDS dashboards and backups on | vacuum and xmin horizon, wraparound, `pg_stat_statements` workflow, restore proof and RPO/RTO |
| M10 | SQLAlchemy sessions and Django ORM daily | N+1 evidence, session scope, `on_commit`, bulk and raw SQL safely |
| M11 | one engine in production, one in side projects | balanced, version-correct comparison and migration risks |

### LF-4 Senior-bar framing

Per-question mid / senior / staff notes are in section 4.1. Course-wide: mid answers describe features; senior answers state engine-specific behaviour, failure modes and a measurement; staff answers add scale limits, operations (alerts, runbooks, rollback), cost and how to prevent recurrence across a team.

### LF-5 Adaptive hooks

| Item | Rule |
|---|---|
| Mastery thresholds | module check 80 percent (badge); 85 percent on a retake after at least 7 days (readiness) |
| Remediation links | M3 -> M2 (keys, types); M4 -> M3; M5 -> M1 (NULL, transactions basics); M6 -> M5; M7 -> M5 and M6; M8 -> M6 (DDL locks); M9 -> M5 (MVCC); M10 -> M4 and M5; M11 -> M3, M5, M7 |
| Challenge extensions | M3 pg_stat index-usage audit; M4 plan-regression alert; M5 per-engine isolation matrix with proofs; M6 queue throughput limits; M8 CI migration lint; M9 wraparound alert design |
| Engine may reorder or defer | M10 and M11 can run earlier for AI-engineer routes; M9 and M7 may be deferred for A-route (`A:N`); M1 and M2 placed first for L1-L2; M5 -> M6 order is fixed; M3 -> M4 order is fixed |
| Route multipliers | B route all modules at 1.0; A route (the `A:S` modules M1-M6, M8 and M10) at 0.6 and (the `A:N` modules M7, M9, M11) at 0.25, primer lessons only; hybrid as tagged (`H:M` modules M1-M8 and M10 at 1.0, `H:S` modules M9 and M11 at 0.6) |

### LF-6 Applied labs (production-framed, one per module)

| Module | Lab (mode) |
|---|---|
| M1 | SaaS cohort and retention report in both dialects (SANDBOX) |
| M2 | Multi-tenant SaaS RLS data model with leak test (SANDBOX; owner lab) |
| M3 | Product-search page: status, JSONB attributes and full-text (SANDBOX) |
| M4 | Feed endpoint 5M rows: plan reading and keyset pagination (SANDBOX; A5.M3 reuses) |
| M5 | Wallet and inventory races: lost update and write skew (TWO-SESSION) |
| M6 | Queue table under load with crash-recovery workers, plus DDL blocking chain (TWO-SESSION; A2.M9 reuses worker) |
| M7 | IoT sensor time-series partitioning and retention (SANDBOX); read-scaling lag step-through (SIMULATED) |
| M8 | SaaS multi-tenant migration under traffic (SANDBOX); major upgrade cut-over (SIMULATED) |
| M9 | Long-transaction bloat and wraparound age (SANDBOX); PITR recovery to before a bad DELETE (SIMULATED) |
| M10 | FastAPI plus Django query budgets for an AI-assistant conversation history API (SANDBOX) |
| M11 | MySQL-to-PostgreSQL migration risk memo for an enterprise app (design exercise) |

### LF-7 Currency and quality bar

| Module | Stamp | Memorable visual or analogy | Top 3 misconceptions handled |
|---|---|---|---|
| M1 | 2026-10 | Row-flow join diagram (no Venn) | NOT IN with NULL; RANGE default frame; MySQL lacks FULL JOIN and FILTER |
| M2 | 2026-10 | Apartment block vs houses (tenancy spectrum) | float money; JSON for everything; UUIDv4 as free choice |
| M3 | 2026-10 | Phone book sorted by surname then first name | index every column; wrong composite order; function on indexed column |
| M4 | 2026-10 | Recipe tree read leaves-up | trust cost number; ignore row estimates; force index first |
| M5 | 2026-10 | Two clerks and one ledger | RR prevents phantoms everywhere; PostgreSQL RR is serializable; MySQL RR errors on conflict |
| M6 | 2026-10 | Supermarket queue behind one slow shopper | SELECT then UPDATE is safe; FOR UPDATE on big scans; deadlocks are DB bugs |
| M7 | 2026-10 | Hotel front desk with few room keys | add replicas fixes everything; partitioning always speeds up; pool size equals max_connections |
| M8 | 2026-10 | Stalled car at a toll gate | ALTER only slow on big tables; one giant backfill; InnoDB ALTER never locks |
| M9 | 2026-10 | Library with a reader holding the old edition | turn autovacuum off; VACUUM FULL in production; backups never restored |
| M10 | 2026-10 | One trip per item vs shopping list | ORM is slow with no evidence; shared sessions; tasks enqueued before commit |
| M11 | 2026-10 | Decision tree with flip conditions | X is faster folklore; ignoring team and managed limits; stale-version comparisons |

## 9. Open items for boss

1. Confirm that a MySQL 9.7 LTS sandbox image may be added once the driver and ORM check (verify item 2) passes, keeping 8.4 LTS as the default, or hold to 8.4 until the next review.
2. When PostgreSQL 19 reaches GA (possible in October 2026, not yet announced), schedule a review pass to add the boxed "What changes in 19" within 30 days.

## 10. Boss compliance log

| Decision | Applied |
|---|---|
| G-1 | Ownership notes table per module; pointers one-way (A5, A6, A7, A2, B3) with 20-minute pointer limit |
| G-2 | Header hours 30 S / 22 D, module hours sum exactly, S/D semantics sentence, 3 h assessment allowance |
| G-3, G-11 | Learner-Fit Appendix LF-1 to LF-7; B/A/H tags per module and per seed question (section 4.1) |
| G-4, G-5, G-6, G-7 | Level paths, bands, readiness (section 7), thresholds at 80 module/course, 85 readiness |
| G-8 | PostgreSQL 18.x, MySQL 8.4 LTS pinned; 9.x as notes; no 9.7 image |
| G-9, G-10 | Tags kept hyphen-style and owned; question IDs `Q-A4-Mn-nn`; goal tag and senior-bar note plus 2 staff probes per module |
| G-12 | Dated facts (2026-10-05), no invented statistics, `[judgement]` marked |
| R-A4-1, R-A4-2 | MySQL 8.4 LTS primary, 9.7 LTS notes only; PostgreSQL 18.x, no PG 19 content as fact |
| R-A4-3, P-05, P-06 | Two-session mandatory for M5-M6; replication and PITR simulated; mode labels per task |
| R-A4-4 | Boundaries with A5, A6, A7, B3 as pointers |
| R-A4-5 | Placement skip limited to M1-M2 lessons; authoring wave M3, M4, M5, M6, M8 |
| Section 8 items 1-9 | Lesson IDs `.n`; version rulings; lab labels; M4 keyset owner; M7 sharding pointer; experience bridge; M3 pgvector pointer; M2 RLS owner lab; IoT, SaaS migration and queue labs; verify list closed except 2 items |

- Track A lead review (2026-10-05, TRACK-LEAD-A): hours, version pins and ownership checked; difficulty mapping stated. Verdict: Approve with conditions (MySQL 9.7 driver check, PostgreSQL 19 GA review). See _reviews/TrackLead_A_Report.md.
