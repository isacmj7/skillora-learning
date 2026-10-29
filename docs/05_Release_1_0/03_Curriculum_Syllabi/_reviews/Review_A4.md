# Review Log — A4 Databases: PostgreSQL and MySQL for Senior Engineers

Reviewer: REV-A4 | Date: 2026-10-05 | Result: file rewritten in place (v1.1) to meet the brief's required-output structure.

Verification done: PostgreSQL versioning page (18 current at 18.6; 17, 16, 15, 14 supported; PostgreSQL 19 Beta 4 on 2026-09-24); MySQL 9.7 release notes (9.7.0 on 2026-04-21, 9.7.3 on 2026-08-18; documentation also lists "26.7" and "26.10" version labels). Other feature facts come from reviewer knowledge and are marked [verify] where version-sensitive.

## Issues found

| # | Severity | Issue |
|---|---|---|
| 1 | High | Missing required structure: no per-module lesson lists, no question-bank targets, no seed verbal questions (rubric, weak answers, probes), no badge criteria, no risks/freshness notes, no open questions, no owner/reviewer/cadence |
| 2 | High | No engine versions stated; nothing on PostgreSQL 17/18 or MySQL 8.4 LTS/9.x currency (8.0 EOL, 9.7 release) |
| 3 | High | Missing topics: vacuum/bloat and XID wraparound, PgBouncer/pooling modes, observability (pg_stat_statements, slow log, performance_schema), backups/PITR depth, JSONB indexing, full-text search, partitioning constraints, online DDL differences per engine, replication types (physical/logical/semi-sync/Group Replication), deadlock analysis, pgvector pointer to B3, major-version upgrades |
| 4 | Medium | Isolation/MVCC coverage was only a label; risk of the common wrong claims (RR prevents phantoms in all engines; PostgreSQL RR serializable). Now spelled out per engine, including READ UNCOMMITTED = READ COMMITTED in PostgreSQL, SSI retry, InnoDB latest-read behaviour and lost updates |
| 5 | Medium | Effort estimate (18 h Simpler / 12 h Deeper) unrealistic for 10 modules plus labs; raised to 30/22 h |
| 6 | Medium | Overlap risk: Redis caching and sharding (A6), pagination and idempotency (A5), managed cloud DBs (A7), vector search (B3) had no boundaries |
| 7 | Medium | "Join Venn" visual is misleading for SQL joins; N+1 listed in both M4 and M9 |
| 8 | Low | Sequencing: observability, vacuum and backups had no home; M7 lumped replication, partitioning, pooling without detail; hash/full-text indexes lumped together |
| 9 | Low | Missing dialect gaps (no FULL JOIN or FILTER in MySQL, no MERGE, no partial/INCLUDE indexes, CHECK enforced from 8.0.16) and 2-session IDE requirement for anomaly labs |

## Changes made

- Rebuilt header (outcomes tied to competency tags, placement skip meaning, badge/certificate evidence, owner/reviewer/cadence, scope boundaries).
- Added version baseline: PostgreSQL 18 (19 beta), MySQL 8.4 LTS pin with 9.x notes, 8.0 EOL flagged.
- Module table now has outcome, specific topics, visuals, practice and pitfalls for 11 modules (old M8 split into M8 Migrations/online DDL and new M9 Operations: vacuum, observability, backups; ORMs became M10; decision became M11).
- Added per-module detail (3–5 lessons with Simpler/Deeper notes, mastery focus, badge criteria, template question-bank targets 12/6/4/8/3).
- Wrote 33 seed verbal questions (3 per module) with ideal-answer outlines, weak answers, probes, difficulty, competency and company-style tags.
- Added final assessment (adds two-session concurrency practical and design task), risks and freshness notes, and open questions.
- Replaced Venn visual; moved N+1 solely to M10 (pointer from M4).

## Unresolved questions for the boss agent

1. MySQL pin: 8.4 LTS only, or add 9.7? Is 9.7 an LTS, and what do the "26.x" version labels mean for naming? [verify]
2. PostgreSQL 19 GA status and feature list at publish time [verify]; sandbox should pin 18 until confirmed.
3. IDE capability: two-session connections, replication and PITR labs in 1.0, or fallback to guided step-throughs?
4. Ownership boundaries with A5/A6/A7/B3 (sharding, caching, pagination, managed DB selection, pgvector).
5. Effort revision (30/22 h) versus the route-map budget; also whether the placement skip may cover the whole course.

Items still marked [verify] in the syllabus: PgBouncer prepared-statement version, PostgreSQL 18 temporal-constraint syntax, MySQL skip-scan and row-value index behaviour, MySQL VECTOR maturity, 8.0 EOL date.
