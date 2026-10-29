---
name: B3 — RAG and Vector Search
course_id: B3
track: B (AI Engineer)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-14 (see _reviews/Boss_Final_Approval.md)
---

# B3 — RAG and Vector Search (Track B)

Version 2.0 (wave-2 upgrade). Last reviewed: October 2026 (2026-10-05). AI/LLM content is volatile: version claims below are stamped "as of Oct 2026"; items still marked **[verify]** name the exact check needed (section 6). Lessons use tier language and dated Provider Notes sidebars, not model names (G-8).

Verified October 2026 (web search of official or primary sources; re-check at each 3-month review): pgvector 0.8.0 added iterative index scans (`hnsw.iterative_scan` with `off`, `strict_order`, `relaxed_order`, capped by `hnsw.max_scan_tuples`); `halfvec` indexable to 4,000 dimensions, `vector` HNSW/IVFFlat indexable to 2,000 dimensions, `sparsevec` to 1,000 non-zero elements, `bit` to 64,000; 0.8.2 (2026-02-26) fixed a parallel HNSW build buffer-overflow CVE-2026-3172 (affects 0.6.0 to 0.8.1), so teach "use 0.8.2 or later"; managed hosts are mixed: RDS PostgreSQL 18.4 and Aurora PostgreSQL 18.4 (Aug 2026) ship 0.8.2 while some other minor versions still ship 0.8.1 (verified 2026-10 in AWS release notes); Aurora PostgreSQL 16.8+ ships 0.8.x; Postgres built-in `ts_rank` is not BM25; true BM25 in Postgres comes from extensions (ParadeDB `pg_search`, Tiger Data `pg_textsearch` v1.0 of March 2026 for Postgres 17/18; Google Cloud SQL and AlloyDB offer a native BM25 index built on `pg_textsearch`, in preview as announced), which are not documented on RDS/Aurora extension lists in sources found, so the managed-AWS default is `tsvector` ranking or an OpenSearch side-car (design-only) `[verify: absence from current RDS/Aurora extension lists, check per region/engine version]`; Bedrock Knowledge Bases offers hybrid search (including Aurora PostgreSQL stores), reranking, metadata filtering and query decomposition; Azure AI Search offers hybrid, semantic ranking and agentic retrieval (verified 2026-10: the 2026-04-01 REST API is GA for generally available knowledge source types with minimal, extractive retrieval; newer features such as answer synthesis remain in preview); Google's offering is split into RAG Engine (documented as "RAG Engine on Gemini Enterprise Agent Platform", the platform formerly Vertex AI) and Agent Search (formerly Vertex AI Search, renamed 2026-04-22). Product names and GA/preview status move quickly: dated examples only.

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | B3 — RAG and Vector Search / Track B (AI Engineer) |
| Audience | Senior Python backend engineers (FastAPI/Django, PostgreSQL, Redis, Docker, AWS) moving into AI Engineer roles; have called Claude/OpenAI APIs and built chatbots/voice agents but may not have built or evaluated a production retrieval system |
| Prerequisites | B2 (LLM Application Engineering) is required (solid edge B2 to B3, waivable by placement); B3 labs lean most on B2.M1, M4, M5 and M7. A4.M3 (indexing) recommended. **Placement skip:** per-module bands of G-5 (>= 85 skip lessons and take the mastery check; 70-84 skim; 50-69 study; < 50 deep-study with linked remediation). M4 to M8 carry the senior-depth content, so even a skip there still requires the mastery check plus the lab evidence. Prerequisite pointers: B1.M8 (embeddings concept, similarity, tokenisation), A4.M2 (RLS data model) and A4.M3 (indexing basics) |
| Outcomes (each tied to an interview competency) | 1. Draw and justify an end-to-end RAG architecture (ingest, index, retrieve, rerank, generate, cite) and decide RAG vs long context vs fine-tuning (`rag.architecture`) 2. Choose chunking and parsing strategies per document type and defend them with retrieval measurements (`rag.ingestion`) 3. Select an embedding model and dimension using cost, latency, language and benchmark-plus-own-data evidence (`rag.embeddings`) 4. Configure pgvector and compare HNSW vs IVFFlat and alternative vector stores, including filtered-search behaviour (`rag.vector-db`) 5. Build hybrid (BM25 plus vector, fused) retrieval with reranking, query rewriting and metadata filters, and show the lift on an eval set (`rag.retrieval`) 6. Design multi-tenant RAG with enforced access control, freshness and deletion guarantees (`rag.security`) 7. Produce grounded, cited answers and detect/handle unsupported claims and abstention (`rag.grounding`) 8. Evaluate retrieval and generation separately, run failure analysis, debug a failing RAG system methodically (`rag.evaluation`, `rag.debugging`), and estimate and cut cost and latency with caching and tuning (`rag.production`); agentic RAG trade-offs (`rag.agentic`) are assessed in the capstone |
| Estimated effort | **15.5 h Simpler / 10.5 h Deeper**, plus **2.5 h assessment allowance** (final mastery check, timed build, debugging task, canvas capstone, voice mock, revision loops) `[judgement]`. Simpler = L1-L2 route with full scaffolding; Deeper = L3-L4 route, hours net of typical skip-ahead, so shorter in wall-clock but harder per hour (G-2). Module hours sum to the header (section 3) |
| Interview coverage | Owned tags (G-9): `rag.architecture`, `rag.ingestion`, `rag.embeddings`, `rag.vector-db`, `rag.retrieval`, `rag.grounding`, `rag.security`, `rag.evaluation`, `rag.production`, `rag.debugging`, `rag.agentic`. Secondary tags (no credit): `llm.security`, `llm.eval-design`, `sec.compliance`. Feeds Interview Master "LLM/RAG depth" rounds, "design a RAG service" design-library entry, and 10-minute "RAG" quick drills |
| IDE | Per-attempt disposable PostgreSQL 18 with pgvector (P-04, R-B3-3) on the G-8 pinned Python. **Frozen embedding-fixture mode (precomputed vectors, pinned model id recorded) is the default for every graded task** so results are offline and deterministic. A small pinned CPU embedding model is available for ungraded exploration and for the labs flagged "live-CPU" in section 3 (M3 model comparison, M5 reranker, M9 timing). BM25 via a pure-Python library or Postgres full-text; OpenSearch is design-only. OCR and layout labs use pre-parsed fixtures. A provided mini corpus and labelled eval set |
| Badge and certificate | Course badge "RAG Engineer (Interview-Ready)": all 10 module badges, final mastery check at 80% (G-7), timed build task passed (>= 70% hidden tests plus stated complexity), canvas capstone rubric >= 3.5 with no dimension < 3 and no critical rule findings, voice mock "Meets" (average >= 3.0, none < 2) on `rag.architecture` and `rag.debugging`; the Senior bar (average >= 3.5, none < 3, two sessions at least 48 h apart) is the G-6 readiness criterion, not a badge condition. Evidence: passing hidden tests, own eval report (recall@k, MRR, faithfulness table) from the capstone, transcript scores. Per-module badges described in section 3 |
| Authoring owner | SME agent SME-B3 |
| Reviewer | To be assigned |
| Review cadence | Every 3 months (fast-moving); model, library and pgvector version notes dated; stable concepts (chunking, fusion, metrics) reviewed every 12 months |

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice tasks | Pitfalls interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | RAG architecture and when to use it | Draw a production RAG pipeline and choose RAG vs long context vs fine-tuning for a scenario | Parametric vs non-parametric knowledge; offline (ingest) vs online (query) paths; naive vs advanced vs modular RAG; RAG vs long-context stuffing vs fine-tuning vs tools/SQL; cache-augmented generation (small static corpus preloaded into a cached prompt prefix) as the long-context variant, with its freshness and ACL limits; where each stage fails; build-vs-buy lesson: own pgvector stack vs managed services, naming Amazon Bedrock Knowledge Bases, Azure AI Search and Vertex AI Search (Agent Search / RAG Engine) as dated Oct 2026 examples, no labs; owns the RAG-vs-long-context decision tree (fine-tune decision framework: pointer to B7.M1) | End-to-end RAG pipeline diagram (offline and online lanes); decision tree RAG vs long context vs fine-tune; failure-point map | Given 4 business scenarios, pick an approach and justify; sketch the pipeline for a policy-Q&A bot | "RAG removes hallucination" (it reduces, not removes); "fine-tune to add knowledge"; "long context makes RAG obsolete" without cost/latency/freshness/ACL reasoning; no mention of evaluation |
| M2 | Ingestion, parsing and chunking | Build an ingestion pipeline and choose a chunking strategy backed by measured retrieval | Connectors; parsing PDFs, HTML, DOCX, tables, scanned docs (OCR), layout-aware parsers; cleaning, dedup, near-dup; chunk size/overlap trade-off; fixed, recursive, sentence/semantic, structure-aware (heading-based), parent-child (small-to-big), sliding window; late chunking and contextual chunk enrichment (prepending document/section context, as in contextual retrieval); metadata capture (source, section, page, ACL, timestamp, version); idempotent re-ingestion and content hashing | Chunking comparison step-through (same text, 4 strategies); parent-child retrieval diagram; ingestion pipeline with idempotency keys | Ingest a mixed corpus (PDF with table, HTML, markdown); implement recursive plus heading-aware chunker; measure recall@5 across chunk sizes 256/512/1024 | One-size-fits-all chunk of 1000 chars; splitting tables and code mid-row; losing headings so chunk is context-free; no metadata; OCR ignored (document-AI/multimodal ingestion is overview only, <= 45 min across M2 and M10, fixtures only; R-B3-4); re-embedding the whole corpus on each run |
| M3 | Embeddings | Choose, evaluate and operate an embedding model including dimension and multilingual choices | Pointer recap (<= 20 min, preview): what an embedding is, similarity metrics and normalisation -> B1.M8; this module uses them, it does not re-teach them; dimension vs quality vs storage/latency; Matryoshka truncation; quantisation (float16, int8, binary) and its recall cost; asymmetric query/document prompts and instruction prefixes; domain and multilingual models (including Indic languages, code-mixed Hinglish); API vs self-hosted (cost, privacy, latency, batching); MTEB-style benchmarks and why to test on own data; model versioning and re-embedding migrations; embedding drift; limits (negation, numbers, rare entities) | Embedding space 2D projection explorer; storage and cost calculator; dual-index migration diagram | Compute storage for 5M chunks at 768/1536/3072 dims and float32 vs float16; compare two embedding models on the provided labelled set; test truncation to 256 dims | "Higher dimension is always better"; mixing models between index and query; not normalising with dot-product index; trusting leaderboard rank; ignoring language mix; no plan for model upgrade |
| M4 | Vector databases and indexes | Configure pgvector indexes, explain HNSW vs IVF, and compare stores for a given scale | Exact (flat) vs approximate search; recall vs latency vs memory; HNSW (graph layers, `m`, `ef_construction`, `ef_search`); IVF (clusters, `lists`, `probes`); product quantisation concept; pgvector: `vector`, `halfvec`, `sparsevec`, `bit` types, operator classes (`vector_cosine_ops` etc.), index build time and memory (`maintenance_work_mem`), index dimension limits as of Oct 2026 (`vector` 2,000, `halfvec` 4,000, `sparsevec` 1,000 non-zeros, `bit` 64,000), iterative index scans for filtered queries (pgvector 0.8+, `hnsw.iterative_scan`, `ivfflat.iterative_scan`, `hnsw.max_scan_tuples`), security patch level (0.8.2 or later); filtered-search pitfalls (post-filter returns fewer than k rows); partial indexes and partitioning per tenant; DiskANN-style options (pgvectorscale; self-managed or specific hosts only, reported not available on Amazon RDS as of mid-2026 per secondary sources `[verify: AWS extension list]`); alternatives: Qdrant, Weaviate, Milvus, Pinecone, OpenSearch/Elasticsearch kNN, FAISS (library), Chroma (prototyping), Redis vector search; when pgvector is enough (single DB, transactional consistency, up to roughly low tens of millions of vectors when RAM-sized, a `[judgement]` rule of thumb to be replaced by the learner's own capacity maths) vs dedicated store; capacity planning; backup and consistency | HNSW search walk-through animation (greedy descent across layers); IVF cluster-probe diagram; recall-vs-ef_search curve; store decision matrix | In disposable Postgres: create HNSW and IVFFlat indexes on 200k fixture vectors, measure recall vs exact scan at several `ef_search`/`probes`; reproduce the "filter returns too few rows" problem and fix it; EXPLAIN a vector query | "Just use cosine on everything" without matching operator class; index created but query uses a different operator so index is ignored; forgetting recall is tunable and must be measured against exact search; building HNSW on tiny tables; ignoring RAM; picking a dedicated DB for 100k vectors; treating ANN as exact |
| M5 | Retrieval quality: hybrid, reranking, query rewriting, metadata filters | Raise retrieval quality with layered techniques and prove each layer's contribution | BM25/lexical (Postgres `tsvector` ranking is not true BM25; BM25 extensions such as pg_search or pg_textsearch where the host allows; pure-Python BM25 in the sandbox; OpenSearch design-only) vs dense vs learned sparse (SPLADE-style); why hybrid helps (exact IDs, codes, rare terms); fusion: Reciprocal Rank Fusion vs weighted score normalisation; two-stage retrieve-then-rerank with cross-encoders and hosted rerankers; candidate depth (retrieve 50 to 100, rerank to 5 to 10); query rewriting: HyDE, multi-query, step-back, decomposition for multi-hop, conversational condensation; query routing; metadata filtering and self-querying filters; MMR and diversity; late-interaction (ColBERT-style) overview; dealing with tables/numeric queries | Hybrid retrieval flow with RRF worked example; rerank funnel diagram; query rewriting step-through | Implement hybrid retrieval in Postgres (tsvector plus pgvector) with RRF; add a cross-encoder reranker; report recall@k/nDCG deltas for each layer; implement conversational query condensation | Adding a reranker before fixing chunking; fusing raw scores of different scales; always rewriting queries (latency and drift); retrieving too few candidates for the reranker; filter applied after top-k; no ablation to show which step helped |
| M6 | Generation, grounding and citations | Build prompts and output contracts that yield grounded, cited, abstaining answers | Context assembly and ordering (lost-in-the-middle), token budgeting; prompt contract ("answer only from sources, else say you do not know"); citation strategies (chunk IDs in context, structured output with quote spans, post-hoc verification); provider citation features (tier language; Provider Notes sidebar, dated); attribution vs faithfulness; abstention and low-confidence thresholds; handling conflicting sources and stale docs; summarisation vs extractive answers; streaming with citations; RAG-specific application of indirect injection (retrieved text is untrusted data; trust boundary and output hygiene), kept to a <= 20 min pointer to B5.M4-M6 with no new red-team lab; structured output with Pydantic | Context assembly diagram with budget bar; citation verification flow; indirect injection attack diagram | Implement a cited-answer endpoint (FastAPI) returning answer plus chunk IDs plus quotes; add a verifier that checks each claim has a supporting span; run the B5-supplied poisoned-document fixture against the endpoint as a hidden test case (no new attack-design task) | Citations that are decorative (model invents IDs); stuffing top-20 chunks blindly; no abstention path; trusting retrieved text as instructions; evaluating only the answer, not the evidence |
| M7 | Multi-tenant RAG, access control and freshness | Design tenant isolation, per-document ACLs, and update/delete flows that hold under audit | Isolation models: index-per-tenant, partition/partial index, shared index with tenant filter; Postgres row-level security with pgvector; ACL propagation (document, group, user), permission-aware retrieval (pre-filter, not post-filter on generated text); ACL changes and revocation lag; RLS data model -> pointer A4.M2, tenancy architecture patterns (pool, bridge, silo, cells) -> pointer A6.M9, only the RAG-specific application is taught here; PII and data residency -> pointer X1 (A8.M9.4: DPDP, GDPR basics, retention, residency; awareness only, not legal advice); deletion guarantees ("right to erasure": vectors, caches, logs, backups); freshness: incremental sync, change detection via hashing, tombstones, versioned chunks, TTL, re-embedding on model change, blue/green index swap; embedding caches; noisy-neighbour and per-tenant quotas | Tenant isolation options comparison; ACL-aware retrieval sequence; blue/green re-index timeline | Implement a tenant-and-ACL-filtered retrieval with RLS in Postgres; write a test that proves tenant A can never see tenant B chunks (including via cache); implement incremental re-ingest with deletes | Filtering after the LLM call; cache keys without tenant or ACL context; deleting source doc but leaving chunks/vectors/cache; trusting the model to "not reveal" restricted text; one global index with no tenant filter test |
| M8 | Evaluation and failure analysis | Build a retrieval and generation eval harness and perform structured root-cause analysis | Retrieval metrics: recall@k, precision@k, MRR, nDCG, hit rate, context precision/recall; generation metrics: faithfulness/groundedness, answer relevance, correctness, citation accuracy; LLM-as-judge: 20-minute pointer lesson only (calibration, bias, human-label validation -> B5.M3; general eval methodology, slicing, statistics and CI gating -> B5.M1-M2); building a retrieval golden set (from real queries, synthetic generation plus human review, hard negatives); offline vs online eval (thumbs, escalations, reformulation rate); regression gating in CI; tools and frameworks as a dated code-reading overview (Ragas, DeepEval, TruLens, Phoenix, LangSmith, Langfuse; APIs change often, so metrics are hand-implemented in the lab) `[verify at each review: each tool's current API names against its docs and PyPI release page; none are used in graded tasks]`; failure taxonomy: not-ingested, bad parse, bad chunk, embedding miss, filter bug, rank miss, context overflow, generation ignores context, wrong abstention; per-stage tracing; debugging playbook | Eval pipeline diagram; failure funnel (where did the right chunk get lost); metric worked examples (MRR, nDCG) | Build a harness computing recall@k and MRR on a labelled set; score faithfulness on recorded outputs with the prebuilt B5.M3 judge fixture and compare to 20 provided human labels (judge use only; judge design and calibration are B5.M3, no judge-building lab here); given 6 failing traces, classify root cause and fix | Only eyeballing answers ("looks good"); single end-to-end score hiding stage failures; synthetic eval set leaked from the same chunks; unvalidated LLM judge; changing many things at once; no regression suite |
| M9 | Production: cost, latency, caching and operations | Estimate and reduce cost and latency; operate RAG reliably | Latency budget per stage (rewrite, embed, ANN, rerank, LLM first token); parallelisation; streaming; embedding and rerank batching; caching layers: query-embedding cache, retrieval cache, semantic cache (risks: stale/wrong hits, ACL leakage), prompt/prefix caching of provider; cost model (embedding at ingest, storage, rerank calls, LLM tokens dominate); smaller model routing; context compression; index memory costs; observability: trace per request with retrieved IDs and scores, drift and empty-retrieval alerts; fallbacks and timeouts; async ingestion with queues (Celery/SQS), backpressure, rate limits of embedding APIs | Latency waterfall chart; cost breakdown stacked bar; cache layer diagram | Instrument a RAG endpoint with per-stage timings; reduce p95 by 40% with measurable changes; build a cost calculator for 1M docs and 50k queries/day | Optimising the ANN search when LLM time dominates; semantic cache without tenant scoping; no timeouts around reranker; ignoring ingestion cost and rate limits; no per-stage traces |
| M10 | Agentic RAG and capstone architecture | Explain agentic RAG trade-offs and design a complete RAG system under interview conditions | Agentic RAG: retrieval as a tool, iterative retrieve-reflect loops, routing across indexes/SQL/web, corrective RAG and self-RAG ideas (overview), multi-hop; costs: latency, nondeterminism, loop limits; GraphRAG and knowledge-graph retrieval (when it helps: global/relationship questions) overview; multimodal RAG overview (images, tables; with M2 total <= 45 min, fixtures only); long-context plus RAG hybrids; text-to-SQL vs RAG for structured data; capstone: design review checklist citing only registry rule IDs R-AI-3, R-AI-4, R-TENANT-2 (no local rules); common-failure-modes review (links to B4 and B5) | Agentic RAG loop sequence diagram; reference architecture for enterprise RAG service | Capstone: build a small RAG service end to end (ingest, hybrid retrieve, rerank, cited answer, eval report) and write a one-page design note; design-canvas task | "Agents fix bad retrieval" (they amplify it and add cost); unbounded loops; GraphRAG as default; no baseline comparison against plain hybrid RAG |
| | **Ownership notes** | **OWNS:** RAG architecture and RAG-vs-long-context decision tree, build-vs-buy of managed RAG (M1); ingestion, parsing, chunking (M2); embedding-model selection, dimension, Matryoshka, quantisation, query/document prompts, multilingual and re-embedding migrations (M3); ANN indexing (HNSW, IVF) and pgvector in practice (M4); hybrid retrieval, rerank, query rewriting (M5); grounding and citations (M6); RAG-specific tenant/ACL pre-filtering and freshness (M7); RAG metric definitions, retrieval golden sets, failure analysis (M8); RAG cost/latency/caching (M9); agentic RAG and RAG reference architecture (M10). **POINTS:** what an embedding is, similarity, tokenisation -> B1.M8; prompt vs RAG vs fine-tune -> B7.M1; RLS data model -> A4.M2; indexing basics -> A4.M3 (A4 holds one pointer lesson to M4); tenancy architecture -> A6.M9; compliance (DPDP, GDPR) -> X1 (A8.M9.4); LLM-as-judge design, bias, calibration -> B5.M3; eval methodology, slicing, CI gating -> B5.M1-M2; prompt injection depth, red-team, guardrails -> B5.M4-M6; agent trajectory evaluation -> B4.M8; context-window management -> B2.M7; token cost template -> B2.M9 (X3); MCP and tool design -> B4.M2; RAG system design case -> A6.M8 | | | | |

## 3. Per-module detail blocks

Question-bank targets for every module (template numbers): 12 MCQ/concept, 6 code-output/predict, 4 coding tasks with hidden tests, 8 verbal with follow-up trees, 3 scenario/design. Mastery check: 6 to 10 items per module, pass at 80% (G-7), revision loop on misses. Item IDs `Q-B3-Mn-k`; placement diagnostics are `placement`-tagged items drawn from the MCQ/predict buckets. Lessons are numbered `B3.Mn.k` in the order listed; every lesson ships at least one diagram or step-through with alt text, a "last reviewed" stamp and a dated Provider Notes sidebar where a vendor is named. Each module block below adds the template's worked example (code runs on pinned versions and is tested in CI).

| Module | Hours S / D | Live-CPU lab allowed (all graded runs use frozen fixtures) |
|---|---|---|
| M1 | 1.25 / 0.75 | no |
| M2 | 1.5 / 1.0 | no (parsing uses pre-parsed fixtures) |
| M3 | 1.5 / 1.0 | yes: model comparison, truncation test |
| M4 | 2.25 / 1.25 | no (indexes built on frozen vectors in disposable Postgres) |
| M5 | 2.0 / 1.5 | yes: cross-encoder reranker |
| M6 | 1.5 / 1.0 | no (LLM calls use recorded fixtures, R-B2-4) |
| M7 | 1.5 / 1.0 | no |
| M8 | 1.75 / 1.25 | no (judge uses recorded outputs) |
| M9 | 1.5 / 1.0 | yes: per-stage timing (relative numbers only) |
| M10 | 0.75 / 0.75 | no |
| **Total** | **15.5 / 10.5** (+2.5 assessment) | |

### M1 — RAG architecture and when to use it
- Lessons: (1) Why RAG exists: what the model does not know; (2) The two paths: offline ingest and online query; (3) RAG vs long context vs fine-tuning vs tools (owns the long-context decision tree; fine-tune framework pointer B7.M1); (4) Naive to advanced RAG and the failure-point map; (5) Build vs buy: own pgvector stack vs Bedrock Knowledge Bases, Azure AI Search, Vertex AI Search (dated comparison table, no labs).
- Worked example: Annotated decision walk-through for the 2,000-PDF policy bot: clarifying questions, break-even of long-context cost vs retrieval cost (placeholder numbers from the dated data file), and the chosen pipeline sketch.
- Simpler/Deeper: Simpler adds an analogy (open-book exam) and one guided pipeline sketch; Deeper adds cost/latency/freshness/ACL reasoning for the decision and a break-it-down of failure points.
- Mastery focus: decision-making among approaches; naming each stage and its failure mode; hallucination reduction vs elimination.
- Module badge: 80% mastery check plus a scenario-decision exercise graded against rubric.

### M2 — Ingestion, parsing and chunking
- Lessons: (1) Parsing real documents (PDF, HTML, tables, OCR); (2) Cleaning, dedup and metadata; (3) Chunking strategies compared; (4) Parent-child and contextual enrichment; (5) Idempotent, incremental ingestion.
- Worked example: Annotated Python ingestion run on the mixed fixture corpus: pre-parsed PDF table kept as one markdown unit, heading-aware recursive chunker, SHA-256 content hash per chunk, idempotent upsert (second run changes zero rows).
- Simpler/Deeper: Simpler uses fixed and recursive splitters only; Deeper adds semantic and structure-aware chunking, contextual enrichment, and measured sweeps.
- Mastery focus: choosing chunk size with evidence; handling tables/code; metadata design; re-ingestion without duplication.
- Module badge: 80% plus working ingestion pipeline passing hidden tests and a recall-by-chunk-size table.

### M3 — Embeddings
- Lessons: (1) What embeddings capture and what they miss (pointer recap of B1.M8, then limits: negation, numbers, rare entities); (2) Similarity metrics in practice: matching pgvector operator class and normalisation to the model (concept owned by B1.M8, no re-teaching); (3) Model selection: API vs open, multilingual, domain; (4) Dimensions, Matryoshka and quantisation; (5) Versioning and re-embedding migrations.
- Worked example: Annotated SQL and Python: storage arithmetic for 5M chunks at 768/1536/3072 dims, then recall@5 of full vs 256-dim truncated (renormalised) vectors on the labelled fixture set.
- Simpler/Deeper: Simpler uses intuition and 2D projections; Deeper adds storage maths, truncation/quantisation recall trade-offs and dual-index migration.
- Mastery focus: metric/operator matching; dimension and cost maths; evaluating on own data; migration plan.
- Module badge: 80% plus model-comparison report on the provided labelled set.

### M4 — Vector databases and indexes
- Lessons: (1) Exact vs approximate search and recall; (2) HNSW internals and parameters; (3) IVF and quantisation concepts; (4) pgvector in practice: types, operators, filters, iterative scans; (5) Choosing a store: pgvector vs dedicated vs search engines.
- Worked example: Annotated psql session on 200k fixture vectors: `CREATE INDEX ... USING hnsw (embedding vector_cosine_ops)`, `SET hnsw.ef_search`, EXPLAIN showing the index used, recall vs exact scan, then the filtered query returning fewer than k rows and the iterative-scan fix.
- Simpler/Deeper: Simpler focuses on pgvector with defaults and recall measurement; Deeper adds graph/cluster internals, build memory, partitioning, filtered-ANN behaviour, and store comparison at scale.
- Mastery focus: parameter effects (ef_search up gives recall up, latency up; probes likewise); operator class vs query operator; filtered search; capacity maths.
- Module badge: 80% plus measured recall/latency table from the IDE task and a correct fix to the filter task.

### M5 — Retrieval quality
- Lessons: (1) Lexical vs dense and why hybrid; (2) Fusion: RRF vs weighted; (3) Reranking funnels; (4) Query rewriting and routing; (5) Metadata filters and diversity.
- Worked example: Annotated hybrid query: `tsvector` rank and pgvector distance fused by RRF in SQL, with a hand-computed RRF table for two 5-item ranked lists, then the same pipeline with a cross-encoder rerank and an ablation table.
- Simpler/Deeper: Simpler builds hybrid plus rerank on the fixture; Deeper adds ablations, latency budgeting for each stage, multi-hop decomposition and learned sparse/late interaction overview.
- Mastery focus: when each layer helps; RRF arithmetic; candidate depth; ablation discipline.
- Module badge: 80% plus hybrid-plus-rerank pipeline showing measured lift on the eval set.

### M6 — Generation, grounding and citations
- Lessons: (1) Context assembly and budgeting; (2) Grounded prompt contracts and abstention; (3) Citation designs and verification; (4) Conflicting/stale sources; (5) Indirect injection through retrieved documents: RAG-specific trust boundary only (<= 20 min pointer to B5.M4-M6; attack and guardrail depth owned there).
- Worked example: Annotated FastAPI endpoint returning structured JSON (claim, chunk ID, quote span) plus the verifier function that rejects unknown IDs and non-substring quotes.
- Simpler/Deeper: Simpler implements chunk-ID citations; Deeper adds quote-span verification and claim-level checks, and runs the B5 poisoned-document fixture.
- Mastery focus: faithfulness vs correctness vs attribution; abstention; injection hygiene.
- Module badge: 80% plus cited-answer endpoint passing hidden tests including a poisoned-document case.

### M7 — Multi-tenant RAG, access control and freshness
- Lessons: (1) Tenant isolation patterns; (2) ACL-aware retrieval and RLS; (3) Deletion, erasure and revocation; (4) Incremental updates and versioning; (5) Re-indexing without downtime.
- Worked example: Annotated RLS policy plus retrieval query where tenant ID comes from the auth token (never the client), and the cross-tenant probe test including a cache-key case.
- Simpler/Deeper: Simpler covers tenant filter plus RLS; Deeper adds group ACL propagation, cache leakage, erasure across vectors/caches/logs/backups, blue/green swaps.
- Mastery focus: pre-retrieval enforcement; leak paths (cache, logs, rerank, citations); deletion completeness.
- Module badge: 80% plus isolation test suite passing (including cache scenario).

### M8 — Evaluation and failure analysis
- Lessons: (1) Retrieval metrics by hand; (2) Building a golden set; (3) Generation metrics (faithfulness, answer relevance, citation accuracy) plus a 20-minute LLM-as-judge pointer to B5.M3; (4) Failure taxonomy and tracing; (5) Retrieval regression gates (applying B5.M1-M2 methodology and CI gating; no re-teaching).
- Worked example: Annotated harness computing recall@k, MRR and nDCG by hand on 12 labelled queries, then a failure-funnel trace through one failing query (right chunk lost at rerank).
- Simpler/Deeper: Simpler computes recall/MRR and runs a prebuilt judge; Deeper adds hard negatives, retrieval-specific golden-set construction, online metrics and stage-wise failure analysis (judge calibration and CI-gating depth live in B5).
- Mastery focus: metric calculation; stage-wise diagnosis; judge limits; eval-set hygiene.
- Module badge: 80% plus eval harness and a six-trace failure diagnosis scored on rubric.

### M9 — Production: cost, latency, caching and operations
- Lessons: (1) Latency budget and waterfall; (2) Cost model; (3) Caching layers and their risks; (4) Async ingestion and rate limits; (5) Observability and fallbacks.
- Worked example: Annotated per-stage timing waterfall for one request (relative numbers) and a worked unit-economics table for 1M docs and 50k queries/day using placeholder price inputs.
- Simpler/Deeper: Simpler instruments and caches embeddings; Deeper adds semantic cache risk analysis, routing, compression and capacity planning.
- Mastery focus: identifying the dominant cost/latency; safe caching; tracing design.
- Module badge: 80% plus measured p95 reduction and a cost calculator within tolerance.

### M10 — Agentic RAG and capstone architecture
- Lessons: (1) Retrieval as a tool and agentic loops; (2) Routing, corrective retrieval, multi-hop; (3) GraphRAG, multimodal (overview, with M2 total <= 45 min) and structured-data alternatives; (4) Reference architecture walkthrough; (5) Capstone design review and failure-mode checklist using only registry rule IDs R-AI-3, R-AI-4, R-TENANT-2.
- Worked example: Annotated design review of a reference enterprise-RAG diagram against R-AI-3, R-AI-4 and R-TENANT-2, plus a loop-budget guard for a retrieval tool.
- Simpler/Deeper: Simpler is concept and decision-tree level; Deeper adds loop budgets, evaluation of agentic vs static RAG, and a full capstone with design note.
- Mastery focus: when agentic is worth it; baseline-first discipline; architecture coherence.
- Module badge: 80% plus capstone service and design note meeting rubric.

## 4. Seed interview questions

Format per question: tags, ideal outline, weak answers, follow-ups. Style-of-question items only; none are drawn from any company's actual interviews.

Addenda (G-10): every question below carries a goal tag and a senior-bar note in the table that follows; each module also lists two staff-level "senior-bar upgrade" probes. Question IDs map `Q1.1` -> `Q-B3-M1-01` and so on. Goal tag format `B:x A:x H:x` (M/S/N). Ownership: Q8.1 to Q8.3 are the owner items for RAG metrics; Q6.2 is secondary to B5.M4 (`ref`, no credit; at bank build it becomes a `ref` to the B5 item with only the retrieval-side angle kept here).

| Q | Goal tag | Senior-bar note (mid / senior / staff) |
|---|---|---|
| 1.1 | B:N A:M H:M | Mid lists boxes; senior names failure modes, eval and cost per stage; staff adds ownership, rollout and evolution |
| 1.2 | B:N A:M H:M | Mid picks one; senior asks clarifying questions and costs all three; staff sets a decision policy and a pilot gate |
| 1.3 | B:N A:M H:M | Mid says "reduces"; senior splits extrinsic/intrinsic and measures faithfulness; staff designs abstention policy and risk tiers |
| 2.1 | B:N A:M H:M | Mid quotes 512/50; senior ties size to query type and measures; staff builds a sweep harness others reuse |
| 2.2 | B:N A:S H:S | Mid says OCR; senior covers layout, tables, confidence, DLQ; staff plans throughput, cost and human review |
| 2.3 | B:N A:M H:M | Mid says nightly re-index; senior uses hashes, tombstones, reconciliation; staff defines staleness SLOs per source |
| 3.1 | B:N A:M H:M | Mid picks newest; senior shortlists then evaluates on own data; staff sets qualification and upgrade policy |
| 3.2 | B:N A:M H:M | Mid says more is better; senior computes storage and explains Matryoshka; staff chooses by eval curve plus cost ceiling |
| 3.3 | B:N A:M H:M | Mid blames model; senior checks version/prefix/normalisation/truncation; staff prevents with dual-index canary |
| 4.1 | B:N A:M H:M | Mid says "graph index"; senior explains layers, params and measured recall; staff covers build memory, vacuum and rebuild ops |
| 4.2 | B:S A:M H:M | Mid name-drops; senior gives scale/filter/ops criteria; staff adds migration seam and consistency plan |
| 4.3 | B:S A:M H:M | Mid removes filter; senior explains post-filter and iterative scan; staff partitions for 10k tenants and tests filtered recall |
| 5.1 | B:N A:M H:M | Mid averages scores; senior does RRF and explains scale mismatch; staff routes by query type and ablates |
| 5.2 | B:N A:M H:M | Mid says better embeddings; senior explains funnel and candidate ceiling; staff budgets latency and timeouts |
| 5.3 | B:N A:M H:M | Mid embeds raw follow-up; senior condenses and guards drift; staff evaluates the rewriter as a component |
| 6.1 | B:N A:M H:M | Mid asks model to cite; senior verifies IDs and quotes; staff measures citation precision/recall and sets failure policy |
| 6.2 | B:N A:M H:M | Mid adds a system-prompt line; senior layers privilege separation and output filtering; staff owns red-team CI and trust tiers |
| 6.3 | B:N A:M H:M | Mid trusts the LLM; senior models versions and surfaces conflict; staff defines document ownership and governance |
| 7.1 | B:N A:M H:S | Mid adds a filter in the prompt; senior enforces at data layer and covers leak paths; staff tiers tenants and proves isolation to auditors |
| 7.2 | B:N A:M H:S | Mid filters after generation; senior pre-filters on principals; staff sets revocation SLOs and fail-closed policy |
| 7.3 | B:N A:S H:S | Mid deletes the source; senior traces lineage across derived stores; staff covers backups, receipts and process |
| 8.1 | B:N A:M H:M | Mid eyeballs; senior separates retrieval from generation metrics; staff gates releases and segments by query type |
| 8.2 | B:N A:M H:M | Mid tweaks prompt; senior walks the four-question funnel; staff builds tracing and triage before incidents |
| 8.3 | B:N A:M H:M | Mid has a model generate questions; senior reviews, stratifies, adds unanswerables; staff manages leakage, refresh and agreement |
| 9.1 | B:N A:M H:S | Mid blames the vector DB; senior measures per stage; staff sets TTFT budgets per channel and verifies quality held |
| 9.2 | B:N A:M H:S | Mid says always cache; senior lists false-hit, staleness, ACL risks; staff sequences cheaper caches first and monitors hit precision |
| 9.3 | B:N A:M H:S | Mid gives one number; senior states assumptions and finds the dominant line; staff runs sensitivity and a cost owner process |
| 10.1 | B:N A:S H:S | Mid says agents are better; senior adds budgets and baseline-first; staff evaluates trajectory plus outcome |
| 10.2 | B:N A:S H:S | Mid hypes graphs; senior names global/multi-hop fit and extraction cost; staff designs incremental benefit eval |
| 10.3 | B:N A:M H:M | Mid lists AWS services; senior draws data flow with ACLs and eval; staff adds rollout, cost control, failure at 10x |

Staff-level "senior-bar upgrade" probes (two per module):
- M1: (a) "Your CFO asks why not stuff all 2,000 PDFs into a long-context call each time; produce the break-even in cost and latency." (b) "Which three signals tell you to stop RAG and move to SQL/tools?"
- M2: (a) "Design a chunker regression suite so a splitter change cannot silently drop recall." (b) "Contextual enrichment doubles ingest cost; how do you justify or reject it?"
- M3: (a) "Plan a zero-downtime embedding-model migration for 50M chunks, including rollback." (b) "Show that 256-dim truncation is safe for one language but not another."
- M4: (a) "Size RAM for 30M x 1024 float32 with HNSW and decide halfvec vs binary-with-rescore." (b) "Your filtered recall is 0.6 for small tenants and 0.95 for large; propose per-tier index strategy."
- M5: (a) "Prove with an ablation that the reranker, not the chunker fix, produced the lift." (b) "Design query routing that skips hybrid or rerank for cheap queries without recall loss."
- M6: (a) "Design a claim-level verifier budget: when is its latency worth it?" (b) "Define source trust tiers and how they change retrieval and prompting."
- M7: (a) "Prove to an auditor that a revoked user cannot retrieve cached or reranked content within 5 minutes." (b) "Plan tenant offboarding including vectors, caches, traces and backups."
- M8: (a) "Set a retrieval release gate with confidence intervals on a 300-question set (methodology from B5)." (b) "Triage 100 complaints into root-cause buckets and decide the first two fixes by expected lift."
- M9: (a) "Present a unit-economics model per answered query and the three cuts you would make first." (b) "Design cache invalidation tied to document versions across tenants."
- M10: (a) "Design the experiment that decides whether agentic RAG ships over the static baseline." (b) "Critique a reference architecture using R-AI-3, R-AI-4 and R-TENANT-2 and nothing else."

### M1

**Q1.1 "Walk me through the architecture of a production RAG system and explain what can go wrong at each stage."**
Difficulty: medium. Competency: `rag.architecture`. Cluster: global-product.
- Offline path: connectors, parse, clean, chunk, enrich with metadata, embed, index; incremental updates and deletes.
- Online path: query understanding or rewriting, retrieval (hybrid), rerank, context assembly, generation with citations, post-checks.
- Cross-cutting: ACLs, caching, observability, evaluation harness.
- Failure points per stage: parse loss, bad chunk, embedding miss, filter bug, rank miss, context overflow, ignoring context.
- Mentions evaluating retrieval separately from generation.
- Mentions latency/cost hot spots (LLM dominates, rerank adds tens to hundreds of ms `[judgement]` placeholder ranges, measured in the learner's own lab).
Weak: "embed docs, store in vector DB, send top 3 to the LLM"; no offline/online split; no failure modes.
Follow-ups: Where would you add caching? How do deletes propagate? Which stage do you instrument first?

**Q1.2 "A client wants a chatbot over 2,000 internal PDFs. Would you use RAG, long context, or fine-tuning?"**
Difficulty: medium. Competency: `rag.architecture`. Cluster: global-IT-consulting / IT-services.
- Clarify: corpus size and growth, update frequency, ACLs, query types, latency and budget, accuracy and audit needs.
- Long context: simple for small, static corpus but cost per query, latency, recall degradation in long contexts, no ACL granularity.
- Fine-tuning: good for style/format/behaviour, poor for factual updates, no citations, hard deletion.
- RAG: freshness, citations, ACL enforcement, cheaper per query at scale.
- Hybrid options: RAG plus long-context window for retrieved documents; SQL/tools for structured data.
- Proposes pilot with eval set before committing.
Weak: picks fine-tuning to "teach" documents; claims long context makes retrieval pointless; no clarifying questions.
Follow-ups: What if the corpus is 200 pages total? What if documents change daily? How do you show citations to an auditor?

**Q1.3 "Users report wrong answers from your RAG bot. How do you tell whether retrieval or generation failed, and what do you fix on the retrieval side?"** (hallucination measurement, faithfulness metrics and verifiers are owned by B5.M5 / Q5.2: `ref`)
Difficulty: easy to medium. Competency: `rag.grounding`. Cluster: AI-startup / global-product.
- RAG does not eliminate hallucinations: retrieval can miss, return wrong or stale content, and the model can ignore or over-extend context.
- Triage by stage: was the right chunk in the corpus, in the candidate set (recall@k), in the context after rerank? If yes, it is a generation failure (hand off to B5.M5); if no, it is a retrieval failure.
- Retrieval-side fixes: chunking and metadata, hybrid search, reranking, query rewriting, freshness and ingestion repair, abstain when no evidence is retrieved (threshold on retrieval score).
- Measurement used here: retrieval recall and context precision only; faithfulness and citation metrics are B5.
Weak: "yes, RAG fixes it"; only "use temperature 0".
Follow-ups: How do you build the labelled set that separates the two failure types? How do you calibrate the abstain threshold? Which stage would you fix first?

### M2

**Q2.1 "How do you choose a chunking strategy and chunk size?"**
Difficulty: medium. Competency: `rag.ingestion`. Cluster: global-product / AI-startup.
- Start from document structure and query type (lookup vs synthesis).
- Trade-off: small chunks give precise matching but lose context; large chunks dilute embeddings and waste tokens.
- Options: recursive character, sentence/semantic, heading/structure-aware, parent-child, sliding window with overlap.
- Enrich chunks with section titles or generated context summaries.
- Decide empirically: sweep sizes and overlap, measure recall@k and downstream answer quality on a golden set.
- Handle tables, code and lists as units.
- Store metadata for filters and citation.
Weak: "512 tokens with 50 overlap always"; no measurement.
Follow-ups: How do you handle a 10-page table? What does overlap cost? When does parent-child help?

**Q2.2 "Your PDFs are scanned contracts with tables. What does your ingestion look like?"**
Difficulty: medium to hard. Competency: `rag.ingestion`. Cluster: Big4 / IT-services.
- Detect scanned vs digital; OCR or layout-aware parsing (document-AI services or open-source layout models: tool choice is a dated example; lab uses fixtures).
- Preserve layout: headings, tables as structured text (markdown/HTML/JSON) with row/column context.
- Quality checks: OCR confidence, sample review, fallbacks to manual queue.
- Chunk by clause/section; keep page and bounding-box metadata for citations.
- Metadata: parties, dates, version, ACL.
- PII handling and encryption.
- Idempotent pipeline with queue, retries and dead-letter handling.
Weak: "PyPDF text extract then split"; ignores tables and OCR errors.
Follow-ups: How do you test parse quality? How would you cite page and region? What is the throughput plan for 500k pages?

**Q2.3 "How do you keep the index in sync when source documents change or are deleted?"**
Difficulty: medium. Competency: `rag.ingestion` / `rag.security`. Cluster: IT-services / Indian-product.
- Change detection via content hash, ETag or modified time; document and chunk IDs deterministic.
- Upsert changed chunks, delete orphans, tombstones for deletes.
- Version metadata; blue/green index when model or chunker changes.
- Event-driven (webhook/CDC) plus periodic reconciliation sweep.
- Deletes must propagate to vectors, lexical index, caches, and derived stores.
- Monitor staleness lag.
Weak: "re-index everything nightly"; no delete story.
Follow-ups: What if the chunker changes? How do you verify nothing is orphaned? What lag is acceptable and for whom?

### M3

**Q3.1 "How do you choose an embedding model?"**
Difficulty: medium. Competency: `rag.embeddings`. Cluster: global-product / AI-startup.
- Requirements: language(s), domain, max input length, latency, cost, privacy/residency.
- Candidates: hosted APIs vs open models self-hosted; check benchmark (MTEB-like) as a shortlist only.
- Evaluate on own labelled queries: recall@k, MRR, plus latency and cost.
- Dimension and storage implications; Matryoshka support; quantisation.
- Query/document prefixes or asymmetric modes.
- Plan for versioning and re-embedding.
Weak: "use the newest hosted model"; relies on leaderboard.
Follow-ups: What changes for Hindi or Hinglish queries? What if the vendor deprecates the model? Cost for 20M chunks?

**Q3.2 "Dimensions: 3072 vs 768. What are the trade-offs and what is Matryoshka?"**
Difficulty: medium to hard. Competency: `rag.embeddings`. Cluster: global-product.
- Storage = vectors x dims x bytes; memory for HNSW scales with it; latency scales roughly with dims.
- Higher dims can carry more information, with diminishing returns, task-dependent.
- Matryoshka representation learning: leading dimensions carry most information so vectors can be truncated (and renormalised) with graceful quality loss.
- Quantisation (fp16, int8, binary) plus re-scoring with full vectors from disk.
- Decide via eval curve of recall vs dims.
- Mentions index dimension limits (pgvector: 2,000 for `vector`, 4,000 for `halfvec`, so high-dimension models need Matryoshka truncation or `halfvec` casting; Oct 2026).
Weak: "more dimensions is always better"; cannot compute storage.
Follow-ups: Compute RAM for 10M x 1536 float32. How does binary quantisation plus rescoring work? Do you need to re-embed to truncate?

**Q3.3 "Our retrieval got worse after we switched embedding models. How do you investigate?"**
Difficulty: hard. Competency: `rag.debugging`. Cluster: AI-startup / global-product.
- Verify queries and documents use the same model/version and same prefix mode and normalisation.
- Check partial re-embedding (mixed vectors in one index).
- Operator class and metric mismatch; index rebuilt?
- Run the golden set on old vs new, break down by query type and language.
- Check chunk length vs the new model's max tokens (silent truncation).
- Roll back using dual-index strategy; canary.
Weak: "the new model must be worse"; no systematic comparison.
Follow-ups: How would you have prevented this? How do you roll out safely? What logs would show truncation?

### M4

**Q4.1 "Explain HNSW. What do m, ef_construction and ef_search do?"**
Difficulty: medium to hard. Competency: `rag.vector-db`. Cluster: global-product / AI-startup.
- Multi-layer proximity graph; sparse upper layers for long hops, dense bottom layer; greedy search descending layers.
- `m`: max connections per node; higher gives better recall and more memory/build time.
- `ef_construction`: candidate list size at build; higher gives better graph quality, slower build.
- `ef_search`: candidate list size at query; main recall/latency knob; must be at least k.
- Approximate: recall is measured against exact search.
- Memory heavy; deletes/updates leave dead entries until VACUUM and can degrade recall until REINDEX (`REINDEX INDEX CONCURRENTLY`); mention the build-memory (`maintenance_work_mem`) and parallel-build trade-off and the 0.8.2 security patch.
- pgvector documented defaults as of Oct 2026: `m` 16, `ef_construction` 64, `hnsw.ef_search` 40, `ivfflat.probes` 1; say "check the version docs" rather than assert from memory.
Weak: "it is a faster index"; cannot name parameters or trade-offs.
Follow-ups: HNSW vs IVFFlat: when would you choose each? What happens to recall with heavy filtering? Build time and memory at 50M vectors?

**Q4.2 "Is pgvector enough or do we need a dedicated vector DB?"**
Difficulty: medium. Competency: `rag.vector-db`. Cluster: global-IT-consulting / AI-startup / Indian-product.
- pgvector wins: existing Postgres, transactional consistency with metadata, RLS, ops familiarity, hybrid with full-text in one query, cost.
- Limits: very large scale (RAM-bound HNSW), advanced filtering performance, sharding story, specialised features (some quantisation, multi-vector, sparse).
- Dedicated stores (Qdrant, Weaviate, Milvus, Pinecone) offer horizontal scale, filtered-ANN optimisation, managed ops, with cost: extra system, consistency with source of truth, ACL duplication.
- Search engines (OpenSearch/Elasticsearch) when lexical and faceting already central.
- Decide by vector count, QPS, filter complexity, team skills, SLAs; benchmark with own data.
- Start simple, keep an abstraction boundary, plan migration.
Weak: tool-name dropping; "dedicated is always better"; no numbers.
Follow-ups: At what scale would you move? How do you keep two stores consistent? What do you lose by leaving Postgres?

**Q4.3 "Vector search with a metadata filter returns fewer than k results or misses relevant items. Why and how do you fix it?"**
Difficulty: hard. Competency: `rag.vector-db`. Cluster: global-product / AI-startup.
- ANN index returns top candidates first (e.g. `ef_search` candidates), then the filter removes rows (post-filtering), leaving fewer than k.
- Fixes: raise `ef_search`; iterative index scans (pgvector 0.8+: `hnsw.iterative_scan` = `strict_order` or `relaxed_order`, bounded by `hnsw.max_scan_tuples`; IVFFlat has the same idea); partial indexes or partitions for high-cardinality tenant filters; pre-filter in stores with filter-aware traversal; exact scan when the filter is very selective (planner choice).
- Check with `EXPLAIN`.
- Measure recall under filter, not only unfiltered.
- Combine with B-tree index on filter column for selective cases.
Weak: "filters are slow, remove them"; unaware of post-filter behaviour.
Follow-ups: How do you handle 10,000 tenants? What does the planner choose for very selective filters? How do you test recall under filters?

### M5

**Q5.1 "Why hybrid search, and how do you combine BM25 and vector results?"**
Difficulty: medium. Competency: `rag.retrieval`. Cluster: global-product / AI-startup / Indian-product.
- Dense captures semantics, misses exact tokens (IDs, error codes, names, rare acronyms); BM25 the opposite.
- Run both retrievers in parallel, take top-N each.
- Fusion: Reciprocal Rank Fusion (score = sum 1/(k + rank), commonly k near 60) is scale-free; weighted sum needs normalisation.
- Tune weights or use query-type routing; measure on eval set.
- Postgres can do both: `tsvector` ranking (not true BM25) or a BM25 extension where the host allows it (pg_search, pg_textsearch; not found on RDS/Aurora extension lists as of Oct 2026 `[verify: current AWS extension lists]`; Google Cloud SQL and AlloyDB have a preview native BM25 index based on pg_textsearch), plus pgvector; or OpenSearch plus a vector store.
- Follow with reranker.
Weak: "average the scores"; cannot explain why lexical still matters.
Follow-ups: Compute RRF for these two ranked lists. When might hybrid hurt? How do you tune weights without overfitting?

**Q5.2 "What does a reranker do and where does it sit? Cross-encoder vs bi-encoder?"**
Difficulty: medium. Competency: `rag.retrieval`. Cluster: global-product / AI-startup.
- Stage one: cheap, high-recall retrieval of 50 to 100 candidates; stage two: precise scoring of query-document pairs jointly.
- Bi-encoder: independent embeddings, precomputable, fast, lower precision; cross-encoder: joint attention, accurate, cost scales with candidates, cannot precompute.
- Hosted rerankers vs self-hosted cross-encoders; LLM-based reranking trade-offs.
- Latency and cost budget; candidate depth; truncation of long chunks.
- Gains measured through nDCG/recall of final top-k; ceiling = candidate recall.
- Fallback if reranker times out.
Weak: "a better embedding model"; no latency awareness.
Follow-ups: What if recall@100 is already poor? How many candidates do you rerank? Would you rerank in a voice agent with a tight latency budget?

**Q5.3 "Users ask follow-up questions like 'what about the second one?'. How do you retrieve properly?"**
Difficulty: medium. Competency: `rag.retrieval`. Cluster: global-product / AI-startup.
- Conversation-aware query condensation: LLM rewrites into standalone query using history.
- Resolve references and entities; keep last-turn retrieved context if relevant.
- Decide when to skip retrieval (chit-chat) via routing.
- Alternatives: multi-query, decomposition for complex ones.
- Latency cost of rewrite; use small fast model; cache.
- Evaluate rewrite quality; guard against drift by retrieving with both original and rewritten.
Weak: embed the raw follow-up; append whole history to the query.
Follow-ups: How do you evaluate the rewriter? What if rewriting drops a constraint? How does this differ in voice?

### M6

**Q6.1 "How do you make a RAG system produce reliable citations?"**
Difficulty: medium. Competency: `rag.grounding`. Cluster: Big4 / global-product.
- Give each chunk a stable ID in the context; require output as structured JSON with claim, chunk ID, supporting quote.
- Verify programmatically: IDs exist and belong to retrieved set; quote is a substring or high similarity.
- Optional claim-level entailment check by a judge model.
- Provider-native citation features where available (Provider Notes sidebar, dated).
- UI links to exact page/section; handle multiple sources.
- Metrics: citation precision and recall.
- Failing verification triggers regeneration or abstention.
Weak: "ask the model to cite sources" and trust it.
Follow-ups: What if the claim combines two chunks? What is the latency cost? How do you evaluate citation accuracy?

**Q6.2 "How would an attacker abuse your RAG system via documents, and how do you defend?"**
Difficulty: hard. Competency: `rag.security`. Cluster: global-product / Big4 / AI-startup.
- Indirect prompt injection: instructions hidden in ingested content (web pages, emails, PDFs).
- Also data poisoning of the corpus to bias answers; exfiltration via markdown image links or tool calls.
- Defences: treat retrieved text as data, delimit and instruct; privilege separation (no tools or limited tools when processing untrusted content); output filtering (strip links/images); ingestion-time scanning; source trust levels; least-privilege retrieval ACLs; human approval for actions.
- Monitoring and red-team test suite in CI.
- Acknowledge no complete fix.
Weak: "add a system prompt saying ignore instructions in documents".
Follow-ups: How would you test for it? What if the agent can send email? How do you handle user-uploaded docs vs curated corpus?

**Q6.3 "Retrieved context contains conflicting or outdated information. How should the system respond?"**
Difficulty: medium to hard. Competency: `rag.grounding`. Cluster: Big4 / IT-services.
- Metadata: effective date, version, authority/source rank used at retrieval/rerank.
- Prompt policy: prefer latest/authoritative; surface conflicts to user explicitly with citations.
- Ingestion: supersede old versions (tombstone or filter `is_current`).
- Abstain or escalate when unresolved.
- Evaluate with conflict test cases.
- Governance: doc owners, review cycle.
Weak: "the LLM will pick the right one".
Follow-ups: How do you model versions in the index? What about "as of date" queries? Who owns correctness?

### M7

**Q7.1 "Design multi-tenant RAG for a SaaS where tenants must never see each other's data."**
Difficulty: hard. Competency: `rag.security`. Cluster: global-product / Indian-product / global-IT-consulting.
- Isolation options: separate DB/index per tenant (strongest, costly), partition or partial index per tenant, shared index with mandatory tenant filter plus RLS.
- Choose by tenant count, size, compliance; tiering (big tenants isolated, small shared).
- Enforce at the data layer (RLS, server-side filter injected, never from client); tenant ID derived from auth token.
- Cover every leak path: caches (tenant in key), logs, traces, rerankers, embedding caches, citations/file URLs.
- Tests: automated cross-tenant probing in CI.
- Per-tenant quotas, encryption keys, deletion and export.
- Residency requirements (India/EU) shape regional deployment (X1 awareness; legal specifics confirmed with counsel).
Weak: "add tenant_id filter in the prompt"; shared semantic cache.
Follow-ups: How do you handle 50k small tenants vs 20 large ones? How do you prove isolation to an auditor? How do you delete a tenant?

**Q7.2 "Documents have per-user permissions that change often. How do you enforce them in retrieval?"**
Difficulty: hard. Competency: `rag.security`. Cluster: Big4 / global-product.
- Store ACL metadata (users, groups, roles) with each chunk; resolve the user's principals at query time.
- Pre-filter during retrieval (not after generation); ensure filter is part of ANN query.
- Sync ACL changes from source (events plus reconciliation); acceptable revocation lag and fail-closed policy.
- Alternative: check permission via source system API on top-k (just-in-time) with over-fetch; trade latency for freshness.
- Large groups: group IDs not expanded users; cap on filter size.
- Audit logging of what was retrieved for whom.
Weak: filter after the LLM generates; ignores revocation lag.
Follow-ups: What if top-k all fail the permission check? How do you test revocation? What about cached answers?

**Q7.3 "How do you honour a deletion (erasure) request in a RAG system?"**
Difficulty: medium to hard. Competency: `rag.security`. Cluster: Big4 / IT-services / global-product.
- Locate all derived data via document ID lineage: chunks, vectors, lexical index, caches, eval sets, logs/traces, backups, fine-tune data.
- Delete synchronously from primary index; asynchronous purge of caches and derived stores with verification query.
- Backups: expiry-based purge policy documented.
- Do not log raw content unnecessarily; log retention limits.
- Evidence: deletion receipt and test.
- Legal process to be confirmed with counsel (awareness only; X1 pointer).
Weak: "delete the source file".
Follow-ups: How do you prove deletion? What about embeddings already inside a fine-tuned model? Semantic cache entries?

### M8

**Q8.1 "How do you evaluate a RAG system?"**
Difficulty: medium. Competency: `rag.evaluation`. Cluster: AI-startup / global-product.
- Separate retrieval eval and generation eval.
- Golden set: real queries, labelled relevant chunks/docs, hard negatives; synthetic augmentation with human review.
- Retrieval metrics: recall@k, precision@k, MRR, nDCG; context recall/precision.
- Generation metrics: faithfulness, answer relevance/correctness, citation accuracy, abstention correctness.
- LLM-as-judge validated against human labels, with known biases (position, verbosity, self-preference).
- Online signals; regression gates in CI; segment by query type.
- Track cost and latency alongside quality.
Weak: "we test manually" or only a single "RAGAS score".
Follow-ups: How big should the golden set be? Compute MRR for this example. How do you know your judge is reliable?

**Q8.2 "Users say answers are wrong. Walk me through debugging."**
Difficulty: hard. Competency: `rag.debugging`. Cluster: global-product / AI-startup / IT-services.
- Reproduce with trace: query, rewritten query, retrieved IDs/scores, final context, answer.
- Question 1: was the right chunk in the corpus? (ingestion/parse) Question 2: in the top-k candidates? (chunking, embedding, hybrid) Question 3: surviving rerank and context? (rank, budget) Question 4: did the model use it? (prompt, order, model).
- Classify by failure taxonomy; fix the earliest failing stage.
- Add case to the golden set; run regression.
- Avoid changing multiple variables at once.
- Quantify fix impact; monitor after rollout.
Weak: "tweak the prompt" first; "use a bigger model".
Follow-ups: What if retrieval is perfect but answers are still wrong? How do you triage 100 complaints? What tooling do you want in place beforehand?

**Q8.3 "How would you build a golden dataset without leaking test data and with limited labelling budget?"**
Difficulty: medium to hard. Competency: `rag.evaluation`. Cluster: AI-startup / global-product.
- Source real user queries (anonymised) as priority; stratify by intent and difficulty.
- Synthetic question generation from chunks for coverage, with human review and filtering of trivially lexical-overlap questions.
- Use different model or prompt than the one under test; keep separate from tuning data.
- Label at document/chunk level with graded relevance; sample for double-annotation agreement.
- Hold-out split; freeze versions; refresh periodically.
- Add adversarial and unanswerable questions.
Weak: "have an LLM generate 1000 questions" and trust all.
Follow-ups: How do you avoid overfitting to the golden set? How do you treat unanswerable queries? How often do you refresh it?

### M9

**Q9.1 "Our RAG answer takes 6 seconds. How do you find and cut latency?"**
Difficulty: medium. Competency: `rag.production`. Cluster: IT-services / global-product / Indian-product.
- Instrument per-stage timings (rewrite, embed, retrieve, rerank, generate TTFT, total).
- Usually LLM generation dominates; then reranker and rewriter.
- Fixes: stream tokens, parallelise retrieval branches, smaller/faster model for rewrite, skip rewrite when unneeded, reduce context size, lower candidate depth, batch/pool connections, cache query embeddings and results, regional co-location, prompt caching.
- Tune ANN parameters only if retrieval is the bottleneck.
- Set timeouts and fallbacks.
- Verify quality unchanged against eval set.
Weak: "use a faster vector DB" with no measurement.
Follow-ups: What is acceptable TTFT for chat vs voice? What do you give up when you drop the reranker? How would you do this on AWS?

**Q9.2 "Would you add a semantic cache to a RAG system? What are the risks?"**
Difficulty: medium to hard. Competency: `rag.production`. Cluster: global-product / AI-startup.
- Benefits: cost and latency reduction for repeated or paraphrased queries.
- Risks: false-positive hits (similar but different question, such as different numbers/entities), stale answers after doc updates, ACL/tenant leakage, personalised answers served to others.
- Mitigations: high similarity threshold, scope key by tenant/ACL/doc version, TTL and invalidation on source change, cache only for public or FAQ-type, verify with lightweight check, measure hit precision.
- Cheaper alternatives first: embedding cache, retrieval cache, exact-match cache, provider prompt caching.
Weak: "yes, always, it saves money"; ignores tenancy.
Follow-ups: How would you pick the threshold? How do you invalidate on doc change? How do you monitor wrong hits?

**Q9.3 "Estimate the monthly cost of a RAG service for 1M documents and 50,000 queries per day."**
Difficulty: medium. Competency: `rag.production`. Cluster: Big4 / global-IT-consulting / IT-services.
- State assumptions: avg tokens per doc, chunk count, embedding price, dims, query tokens, context size, answer length, model prices (use configurable price inputs from the dated data file).
- Ingest: one-time embedding cost plus update churn.
- Storage: vectors x dims x bytes plus index overhead (HNSW multiplier) plus text.
- Query: embedding (small), rerank per query, LLM input tokens (context dominates) plus output tokens.
- Infra: DB instance sized for RAM, app servers, queues.
- Levers: caching, smaller model routing, fewer context chunks, quantisation, batch ingestion.
- Sensitivity analysis and monitoring.
Weak: single number with no assumptions; ignores LLM tokens.
Follow-ups: Which line item dominates and why? How does cost change at 10x queries? What would you cut first?

### M10

**Q10.1 "What is agentic RAG and when is it worth the extra complexity?"**
Difficulty: medium to hard. Competency: `rag.agentic`. Cluster: AI-startup / global-product.
- Retrieval exposed as one or more tools; the model decides whether, what and how many times to retrieve; can reformulate, route across sources, verify.
- Good for multi-hop, ambiguous, multi-source questions, comparison across docs.
- Costs: more LLM calls, latency, variance, harder evaluation, loop risks.
- Controls: step and token budgets, deterministic fallback, tracing, tool permission scoping.
- Baseline first: well-tuned hybrid-plus-rerank RAG; add agentic only for measured failure classes.
- Evaluate trajectory plus outcome.
Weak: "agents are better, always"; no loop limits.
Follow-ups: How do you stop runaway retrieval loops? How do you evaluate it versus static RAG? How does it relate to ReAct-style tool loops?

**Q10.2 "When would you use GraphRAG or a knowledge graph instead of plain vector RAG?"**
Difficulty: hard. Competency: `rag.agentic` / `rag.architecture`. Cluster: AI-startup / global-product.
- Plain RAG struggles with global questions ("themes across the corpus"), multi-hop relationship questions, entity-centric queries.
- GraphRAG: extract entities and relations, build graph (and community summaries), retrieve via graph traversal or summaries.
- Costs: expensive LLM-based extraction, schema/maintenance, update difficulty, quality of extraction.
- Use when relationships are central (compliance networks, org structure, supply chain); otherwise hybrid RAG plus metadata may suffice.
- Evaluate incremental benefit on targeted question set.
- Alternatives: metadata joins in SQL, hierarchical summarisation (RAPTOR-style recursive summary trees; naming per the original paper, Oct 2026).
Weak: "graph is the next generation of RAG" without trade-offs.
Follow-ups: How do you keep the graph updated? What would your eval set look like? Cost of building for 1M docs?

**Q10.3 "Review this proposed architecture for an enterprise knowledge assistant on AWS (20,000 employees, department-level permissions). What would a design review flag?"** (the end-to-end AWS system design is owned by A6 library #15 / M8-Q1: `ref`; here the review uses the registry rules)
Difficulty: hard. Competency: `rag.architecture`. Cluster: global-IT-consulting / Big4 / IT-services.
- Given a diagram: connectors to S3 and SQS workers, embedding service, pgvector store, FastAPI query service, LLM via gateway.
- Check against registry rules only: R-AI-3 (ingestion path and staleness/refresh plan present?), R-AI-4 (user or document content flowing to an LLM with tools: injection guard, least-privilege tool boundary?), R-TENANT-2 (vector index and caches carry tenant and department ACL filter on the retrieval path?). No other rule IDs are cited in B3 (G-10 and boss section 7); gateway and fallback rules belong to A6.
- RAG-specific points owned here: ACL pre-filtering in retrieval, ACL change propagation to the index, hybrid retrieval and rerank placement, golden set and eval pipeline before launch (Aurora PostgreSQL ships pgvector 0.8.x; Bedrock Knowledge Bases supports Aurora PostgreSQL stores with hybrid search, Oct 2026).
- Rollout: pilot, golden set, canary, guardrails, human fallback.
Weak: lists services without data flow; no ACL story; no evaluation; invents rule names not in the registry.
Follow-ups: How do you handle ACL changes? What fails first at 10x load? How do you evaluate before launch?

## 5. Final assessment

- Mastery check: 25 items across all modules (concept, predict-the-result including recall/RRF/MRR calculations, short scenarios), pass at 80% (G-7), misses route to module revision. All graded tasks run in frozen-fixture mode.
- Timed coding task (75 minutes): in a provided workspace with fixture data, implement hybrid retrieval (Postgres full-text plus pgvector, RRF) with tenant/ACL filtering and a cited-answer function; hidden tests (pass at >= 70% plus correct stated complexity) check recall threshold on the eval set, no cross-tenant leakage, and citation validity.
- Debugging task (30 minutes): six failing traces with logs; classify root cause and propose the fix for each; scored on rubric.
- Design/canvas capstone (45 minutes): "Enterprise RAG service" architecture with ingestion, retrieval, ACLs, eval and cost notes; the rule engine cites only registry rules R-AI-3 (ingestion and staleness plan), R-AI-4 (injection guard and least-privilege tools) and R-TENANT-2 (tenant and ACL filter on every vector index and cache path), with "stated non-goal" suppression; pass is rubric overall >= 3.5, no dimension < 3, no critical findings.
- Voice mock round (20 minutes; badge "Meets", readiness Senior bar): "Design and productionise a RAG system, then debug a quality regression"; rubric on `rag.architecture`, `rag.retrieval`, `rag.evaluation`, `rag.debugging`; probes adapt to weak areas.
- Badge issued when all components are passed; component retake allowed after revision.

## 6. Course-level risks and freshness notes

- Fast-changing: embedding models and prices, reranker offerings, provider citation/grounding features, managed RAG services (for example Bedrock Knowledge Bases, Azure AI Search, Vertex AI), long-context sizes, agent frameworks (LangChain/LlamaIndex APIs change often), evaluation tool APIs. Teach concepts first, tool names as dated examples.
- pgvector (verified Oct 2026): 0.8.0 introduced iterative scans; 0.8.2 (2026-02-26) patched a parallel HNSW build overflow (CVE-2026-3172); RDS and Aurora 18.4 ship 0.8.2 while some earlier minors still ship 0.8.1. Index limits `vector` 2,000, `halfvec` 4,000, `sparsevec` 1,000 non-zeros, `bit` 64,000. Still `[verify: host-specific]` at each review: the exact version on Cloud SQL and Supabase (RDS and Aurora checked 2026-10).
- BM25 inside Postgres is not native: `ts_rank` is not BM25. Extensions exist (pg_search, pg_textsearch) but were not found on RDS/Aurora extension lists as of Oct 2026 (Cloud SQL and AlloyDB have a preview native BM25 index on pg_textsearch); `[verify: current AWS extension lists]` before telling a learner to rely on one. Sandbox uses pure-Python BM25 or `tsvector`.
- Managed RAG services (Oct 2026 dated examples, no labs): Bedrock Knowledge Bases (hybrid incl. Aurora PostgreSQL, reranking, metadata filters, query decomposition), Azure AI Search (hybrid, semantic ranking, agentic retrieval: 2026-04-01 API GA for minimal extractive retrieval, newer features preview; verified 2026-10), Google RAG Engine (now documented under Gemini Enterprise Agent Platform, formerly Vertex AI) and Agent Search (renamed from Vertex AI Search on 2026-04-22; verified 2026-10). Bedrock hybrid search is supported only for RDS/Aurora, OpenSearch Serverless and MongoDB stores with a filterable text field (verified 2026-10). Pricing is not hard-coded; `[verify: GA/preview status and pricing]` at each review.
- Remaining `[verify]` list: rerank latency ranges and embedding/rerank prices (calculator inputs, no hard-coded figures); evaluation tool APIs (Ragas, DeepEval, TruLens, Phoenix, LangSmith, Langfuse: all six exist per 2026 secondary comparisons; check at each review: current API names against each tool's docs and PyPI page; graded tasks use hand-implemented metrics so nothing blocks on this); OCR/layout parser choices (fixtures only); DPDP and GDPR statements (X1 pointer, counsel review H/P).
- Model names and numeric claims in lessons (dimensions, context sizes, prices) must be pulled from official docs on the authoring date and stamped.
- Pricing/latency figures in the cost lesson are illustrative; the cost calculator must use configurable price inputs.
- Legal items (DPDP Act, GDPR erasure) are awareness only and point to X1; standard disclaimer applies; counsel review pending.
- Overlap risk: B2 M5 (context/memory), B4 (agent loops), B5 (eval depth, safety, LLMOps), B6 (latency in voice), A4 (indexing, RLS). B3.M8 owns RAG metric definitions and failure analysis (R-B3-1); B5.M1-M3 own methodology, judge design and CI gating.
- Sandbox risk: embedding models and ANN tasks must be deterministic and fast; use precomputed fixtures and pinned versions; CI must run all examples.
- Review cadence: every 3 months for tools/models/pgvector; every 12 months for core concepts.

## 7. Open items for boss

All four v1.0 open questions are ruled (R-B3-1 evaluation split, R-B3-2 managed services, R-B3-3 fixtures plus CPU model, R-B3-4 multimodal overview). Remaining real items:

1. Interview weights in LF-2 are `[judgement]`; confirm the Backend-route weighting of B3 (NICE) against the pilot.
2. The sandbox needs a pgvector build at 0.8.2 or later in the P-04 image (security patch); confirm the platform manifest pin.
3. Confirm whether BM25-extension labs are out of scope (current plan: pure-Python BM25 or `tsvector` only) given they are absent from default RDS/Aurora allow-lists.
4. Question-bank gap: this file holds 30 seed verbal questions (3 per module) against the template target of 8 verbal per module (80); the remaining about 50 must be authored at bank build, with owner items for M8 (RAG metrics) first. Not a blocker for starting the M1 build.
5. Reviewer is unassigned: name an SME reviewer, plus counsel for the DPDP/GDPR erasure wording (M7) and an engineer for the AWS extension-list checks before publish.

## 8. Learner-Fit Appendix

Tag key: B = Senior Python Backend, A = AI Engineer, H = hybrid; M/S/N = MUST/SHOULD/NICE (G-3). Course tags: B:N A:M H:M. Backend learners take only the NICE primer slice B3.M1 (lessons 1-3: why RAG, the two paths, RAG vs long context vs fine-tuning vs tools), as the study-route doc assumes. Cluster emphasis: AI-startup and global-product probe M3, M4, M5, M8, M9; IT-services and Big4 probe M1 build-vs-buy, M7 (governance), M10 (enterprise design); Indian-product probes M5 hybrid (Hinglish) and M9 cost.

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge |
|---|---|---|---|---|
| L1 | Simpler 15.5 h; all lessons; B1.M8, A4.M2/M3, B2.M1 remediation links open | 60% guided fixture labs, 40% mastery items | Nothing | None until mastery >= 80% |
| L2 | Simpler 15.5 h, skim known (API calls, basic SQL) | 50% labs, 30% items, 20% verbal | M1.1, M1.2 if band >= 70 | Optional M5 ablation |
| L3 | Deeper 10.5 h; skim foundations, study explain-at-depth lessons (M4, M5, M7, M8) | 50% applied labs, 20% items, 30% voice drills | Per bands | One Challenge per MUST module |
| L4 | Deeper about 7-8 h after skips (Postgres, Redis, API lessons skipped); straight to mastery check where scored | 30% labs, 70% senior-bar probes and voice | M1, M2.1-.2, M4.1 (exact vs ANN) likely; keep M4.4 (pgvector in practice), M5, M7, M8 | All staff probes; adversarial and filtered-recall fixtures |

### LF-2 Module matrix

| Mod | Placement diagnostics (6 each, `placement`) | Bands (>=85 / 70-84 / 50-69 / <50) | B/A/H | Weight % B / A / H | Rehearsal modes | Module readiness (G-6 specialised) |
|---|---|---|---|---|---|---|
| M1 | P1 RAG vs long-context cost; P2 hallucination reduced vs removed; P3 offline vs online path; P4 fine-tune for facts; P5 stage failure map; P6 managed vs own stack | skip / skim D-only / study / S path + B1.M8 | B:N A:M H:M | 12 / 10 / 10 | voice drill, design canvas | decision tree applied to 4 scenarios >= rubric 3.5; voice >= 3.5 on `rag.architecture` |
| M2 | P1 chunk size trade-off; P2 table split; P3 overlap cost; P4 content-hash re-ingest; P5 parent-child; P6 metadata for citation | same | B:N A:M H:M | 8 / 9 / 8 | coding, voice drill | recall-by-chunk-size table plus idempotent re-run; voice >= 3.5 |
| M3 | P1 storage for 5M x 1536; P2 dot vs cosine normalised; P3 Matryoshka truncation; P4 mixed-model index; P5 query/doc prefixes; P6 silent truncation | same | B:N A:M H:M | 8 / 9 / 8 | coding, voice drill | model comparison on labelled set with cost; voice >= 3.5 on `rag.embeddings` |
| M4 | P1 operator-class mismatch; P2 ef_search effect; P3 post-filter fewer than k; P4 HNSW vs IVF; P5 RAM estimate; P6 recall vs exact | same | B:N A:M H:M | 16 / 11 / 13 | coding, voice drill | measured recall/latency table plus filter fix; timed task passed |
| M5 | P1 RRF arithmetic; P2 score-scale fusion; P3 candidate depth; P4 reranker ceiling; P5 query rewriting drift; P6 filter after top-k | same | B:N A:M H:M | 12 / 14 / 13 | coding, voice drill | hybrid+rerank lift shown with ablation; voice >= 3.5 |
| M6 | P1 decorative citation; P2 abstention; P3 lost-in-middle; P4 indirect injection; P5 conflicting sources; P6 faithfulness vs correctness | same | B:N A:M H:M | 8 / 10 / 9 | coding, voice drill | endpoint passes poisoned-doc hidden test; voice >= 3.5 |
| M7 | P1 cache key without tenant; P2 RLS vs app filter; P3 revocation lag; P4 erasure lineage; P5 blue/green swap; P6 per-tenant index choice | same | B:N A:M H:S | 14 / 8 / 10 | design canvas, coding, voice drill | isolation suite incl. cache passes; canvas R-TENANT-2 clean |
| M8 | P1 recall@k vs MRR calc; P2 nDCG calc; P3 recall vs faithfulness (chatbot builders); P4 golden-set leakage; P5 failure taxonomy; P6 context precision | same | B:N A:M H:M | 6 / 14 / 11 | coding, voice drill | harness plus 6-trace diagnosis rubric >= 3.5; voice >= 3.5 on `rag.debugging` |
| M9 | P1 dominant latency; P2 semantic cache risk; P3 cost line item; P4 embedding rate limits; P5 timeout fallback; P6 per-stage trace | same | B:N A:M H:S | 12 / 8 / 10 | coding, voice drill | measured p95 cut with quality held; cost calculator within tolerance |
| M10 | P1 agentic vs static; P2 loop budgets; P3 GraphRAG fit; P4 text-to-SQL vs RAG; P5 baseline-first; P6 reference architecture gaps | same | B:N A:S H:S | 4 / 7 / 8 | design canvas, voice drill | capstone rubric >= 3.5 with R-AI-3, R-AI-4, R-TENANT-2 only |

Weights sum to 100 per target (B: 12+8+8+16+12+8+14+6+12+4; A: 10+9+9+11+14+10+8+14+8+7; H: 10+8+8+13+13+9+10+11+10+8). Rationale `[judgement]`: backend loops probe pgvector/Postgres, multi-tenancy and cost, so B weights M4, M7, M9; AI Engineer loops weight retrieval quality (M5) and evaluation (M8). M7 and M9 tags follow the boss ruling (B:N A:M H:S).

Diagnostic note (boss item 8): at least P3 of M8 and P2 of M1 test recall vs faithfulness for chatbot builders who have shipped bots without retrieval metrics.

### LF-3 Experience bridge ("you already know" -> senior-depth gap)

| Mod | You already know | Gap between using it and explaining it |
|---|---|---|
| M1 | Chatbots calling LLM APIs, stuffing context, AWS services | Naming stage failure modes; cost/latency/freshness/ACL reasoning for RAG vs long context; managed vs own trade-offs |
| M2 | Parsing JSON/CSV, Celery jobs, S3 events | Layout-aware parsing, chunking as a measured choice, idempotent re-ingestion with content hashes |
| M3 | Calling an embeddings endpoint | Storage maths, Matryoshka, quantisation, prefixes, and model-migration planning |
| M4 | Postgres indexes (B-tree, GIN), EXPLAIN, `maintenance_work_mem` | HNSW/IVF internals, recall as a tunable measured against exact search, filtered-ANN behaviour, operator-class matching |
| M5 | Postgres full-text search, Redis, ranking by SQL | Hybrid fusion with RRF, reranker ceilings, ablation discipline, query rewriting drift |
| M6 | Prompting in chatbots, Pydantic output | Citation verification, abstention, indirect injection through documents |
| M7 | RLS, tenant_id columns, Redis keys, IAM | RAG leak paths (cache, rerank, logs, citations), ACL revocation lag, erasure lineage (RLS data model: A4.M2) |
| M8 | pytest, metrics dashboards | Retrieval metrics by hand, golden sets with hard negatives, stage-wise failure funnel (methodology: B5) |
| M9 | Redis caching, p95 tracing, Lambda cost sense | Dominant-stage reasoning, semantic cache risks, RAG unit economics |
| M10 | Tool-calling bots, voice agents | Baseline-first discipline, loop budgets, architecture review with registry rules |

### LF-4 Senior-bar framing

Per-question mid/senior/staff notes sit in the section 4 table. Module summary: mid describes the pipeline; senior states where it fails, how it is measured (recall@k before generation quality) and what it costs; staff sets evaluation gates, tenant/ACL policy, migration plans and cost ownership across teams. Interview-weighted time: retrieval quality, pgvector and evaluation get the most time because AI Engineer loops probe them repeatedly while Backend loops touch them lightly `[judgement]`.

### LF-5 Adaptive hooks

| Module | Remediation links | Challenge extension | Reorder/defer |
|---|---|---|---|
| M1 | B2.M1, B1.M8 | Break-even cost model, long context vs RAG | May move first or skip |
| M2 | M1, A4.M1 | Chunker regression suite | After M1 |
| M3 | B1.M8, M2 | Zero-downtime embedding migration plan | May defer after M4 |
| M4 | A4.M3, M3 | Per-tenant-tier index strategy | Core; with M3 |
| M5 | M3, M4 | Router that skips rerank safely | After M4 |
| M6 | M5, B2.M4 | Claim-level verifier budget | After M5 |
| M7 | A4.M2, A6.M9, X1 | Revocation SLO proof | May defer on Backend route |
| M8 | M5, B5.M1 | Release gate with confidence interval | After M5; not deferrable for A |
| M9 | B2.M9, A7.M10 | Unit economics per answered query | Deferrable for B |
| M10 | M5, M8, B4.M1 | Agentic vs static experiment design | Last; deferrable (A:S, H:S) |

Thresholds: module mastery 80%; readiness (G-6): mastery retake >= 85% at >= 7 days, timed task passed, strict voice drill at the Senior bar (average >= 3.5, no competency < 3) in two sessions at least 48 h apart, no open prerequisite remediation flag. Adaptive engine may reorder M6/M7/M9 and defer M7, M9, M10 on the Backend route.

### LF-6 Applied labs (production-framed)

M1: choose and defend an approach for an HR-policy assistant, a support KB, an IoT runbook search and a contract-review tool. M2: ingest a mixed PDF/HTML/markdown corpus for an enterprise wiki with idempotent re-runs. M3: compare two embedding models and 256-dim truncation on an Indic-English support set. M4: 200k-vector pgvector tuning for a SaaS knowledge base, including a filtered-recall fix. M5: hybrid plus rerank for a product-support search with error-code queries. M6: cited-answer FastAPI endpoint hardened against a poisoned document. M7: multi-tenant document search with RLS, group ACLs and a cache-leak test. M8: eval harness and six failing-trace diagnosis for a customer-support RAG. M9: instrument and cut p95 of a voice-adjacent RAG endpoint; cost calculator for 1M docs. M10: capstone service and canvas for a 20,000-employee assistant.

### LF-7 Currency and quality bar

All modules stamped "Oct 2026"; fixtures keep labs deterministic; version facts per section 6.

| Mod | Memorable visual or analogy | Top 3 misconceptions handled |
|---|---|---|
| M1 | Open-book exam with a librarian: the librarian (retrieval) caps the student (LLM) | RAG removes hallucination; fine-tune to add facts; long context kills RAG |
| M2 | Photocopying a book into index cards: cut mid-sentence and the card lies | One chunk size fits all; splitting tables; re-embedding everything |
| M3 | Map coordinates: the same city has different coordinates on different maps (models), so never mix | More dimensions always better; mixing models; trusting leaderboards |
| M4 | Highway-and-local-roads (HNSW layers) vs sorting mail by region (IVF) | ANN is exact; index ignored by operator mismatch; filters just work |
| M5 | Wide net then expert sorter (funnel); two search committees merged by rank not score | Averaging raw scores; reranker before fixing chunking; no ablation |
| M6 | Footnotes you can click and check, not decoration | Citations prove truth; system prompt stops injection; stuff top-20 |
| M7 | Hotel keycards: wrong card must fail at the door, not at the guest | Filter in the prompt; shared semantic cache; delete the source and done |
| M8 | Failure funnel: where did the right chunk fall out? | "Looks good"; one end-to-end score; leaked synthetic set |
| M9 | Latency waterfall: fix the longest bar first | Tune ANN when LLM dominates; always cache semantically; ignore ingest cost |
| M10 | Research assistant with a stopwatch and a budget | Agents fix retrieval; unbounded loops; GraphRAG by default |

## 9. Boss compliance log

- G-1: Ownership notes row added; pointers (B1.M8, B7.M1, A4.M2/M3, A6.M9, X1, B5.M1-M6, B4.M8) at <= 20 min, no labs. G-2/section 5: header 15.5 S / 10.5 D + 2.5 assessment; module hours sum; semantics sentence present. G-3/G-11: Learner-Fit Appendix LF-1..LF-7 with B/A/H tags. G-4 to G-7: level paths, bands, readiness, thresholds applied. G-8: PostgreSQL 18, tier language, dated Provider Notes. G-9: tags owned vs secondary split. G-10: goal tags, senior-bar notes, 2 staff probes per module, `Q-B3-Mn-k` IDs. G-12: Oct 2026 stamps, no invented prices or latencies.
- R-B3-1: M8 owns metrics and failure analysis; judge to 20-minute pointer (B5.M3); methodology/CI gating pointer B5.M1-M2. R-B3-2: build-vs-buy lesson in M1 with dated Bedrock KB, Azure AI Search, Vertex AI examples, no labs. R-B3-3: fixture mode default; live-CPU labs listed. R-B3-4: multimodal/document AI overview <= 45 min, fixtures.
- Section 8 B3 items 1-9: item 1 tags applied; 2 M1; 3 M3/M4; 4 M8; 5 M7; 6 fixtures; 7 capstone canvas with R-AI-3, R-AI-4, R-TENANT-2; 8 experience bridge and recall-vs-faithfulness diagnostics; 9 `[verify]` closed or narrowed with exact checks (section 6). O-37, O-74, O-76 to O-78 followed.
- Track lead B review (2026-10-06): see `_reviews/TrackLead_B_Report.md`. Applied: outcomes cut to 8 (template 5-8); worked-example line added to all 10 modules plus alt-text note; M6 indirect-injection lesson and lab cut to a B5 pointer (G-1, O-81); M8 judge-building lab replaced by prebuilt-judge use (R-B3-1); Q10.3 limited to registry IDs R-AI-3, R-AI-4, R-TENANT-2; model names removed from weak-answer examples; LF-1 L4 skip list and LF-5 readiness wording aligned to G-6; two [verify] markers given exact checks; cache-augmented generation added to M1.
