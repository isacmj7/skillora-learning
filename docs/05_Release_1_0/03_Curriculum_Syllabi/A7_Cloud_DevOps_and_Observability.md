---
name: A7 — Cloud, DevOps and Observability
course_id: A7
track: A (Backend)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
owner: SME agent A7
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-04, HAR-11 (see _reviews/Boss_Final_Approval.md)
---

# A7 — Cloud, DevOps and Observability (Track A)

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | A7 — Cloud, DevOps and Observability / Track A (Senior Python Backend Mastery) |
| Audience | Senior Python backend / AI engineers (L3–L4) who ship services and are expected to own deployment, runtime behaviour and incidents. Interviewers at IT-services, consulting-tech, Big 4 and product companies probe "how does your code run in production and how do you know it is healthy". Level semantics follow G-4: Simpler path serves L1-L2, Deeper path serves L3-L4 (full Learner-Fit Appendix in section 8) |
| Prerequisites | Working Python services; basic Linux shell and Git; HTTP fundamentals. A5 is recommended first (dotted edge A5 to A7; A7 builds on API and backend architecture), and A7 is recommended before A6. Assumed learner baseline (from the profile): daily Docker, Redis, Bitbucket Pipelines, AWS Lambda and S3 |
| Placement skip rule | Per module, the 5-8 diagnostic items set the G-5 band: >= 85 skip lessons and take the mastery check; 70-84 skim (Deeper-only lessons plus mastery check); 50-69 study; < 50 deep-study (Simpler path plus prerequisite remediation). Reference learner (daily Docker, Redis, Bitbucket Pipelines, Lambda, S3): expected skip bands in M1 (lessons 1-2), M4 (Lambda basics, lesson 1), M5 (S3, lesson 1) and M8 (Bitbucket, part of lesson 2); expected study or deep-study in M3, M6, M7, M9, M10 and in ECS/Fargate, SQS/SNS/EventBridge semantics, IAM evaluation, VPC and PrivateLink, Terraform state, SLO maths and burn-rate alerts. No band skips verbal seed questions or the course final assessment |
| Outcomes (each tied to a competency) | 1. Build a small, secure, cache-friendly production image for a Python service and explain layers, multi-stage builds and runtime hardening (`cloud.containers`). 2. Describe and debug a Kubernetes Deployment end to end: pod lifecycle, probes, resources, Service, Ingress or Gateway, config and secrets, HPA (`cloud.k8s`). 3. Choose between Lambda, ECS/Fargate, EKS and EC2 for a workload with cost, latency, scaling and ops trade-offs, and design reliable event-driven integrations with SQS, SNS and EventBridge including idempotency and DLQs (`cloud.aws-compute`, `cloud.aws-messaging`). 4. Reason about IAM least privilege, VPC subnets, security groups and private connectivity to data stores, and triage a hung Python service with TCP, TLS, DNS and Linux evidence (`cloud.aws-security`, `cloud.networking`, `ops.linux-triage`). 5. Write and review Terraform with remote state, modules and a safe plan/apply workflow, and explain drift and state-locking (`devops.iac`). 6. Design a CI/CD pipeline with tests, image build, scanning, OIDC-based cloud auth and a blue/green or canary rollout with automatic rollback (`devops.cicd`). 7. Instrument a Python service with structured logs, RED/USE metrics and OpenTelemetry traces, define SLIs/SLOs and write burn-rate alerts that avoid alert fatigue (`ops.observability`). 8. Lead a simulated incident (triage, mitigate, communicate, write a blameless postmortem) and reason about infrastructure cost levers and basic multi-cloud equivalents (`ops.incident`, `cloud.cost`, `cloud.multicloud`) |
| Estimated effort | **24 h Simpler / 15 h Deeper** module time, plus **3.0 h** assessment allowance (final mastery, observability task, canvas task, voice mock, revision loops). Simpler = L1-L2 route with full scaffolding and prerequisites; Deeper = L3-L4 route with foundations skimmed per placement, hours net of typical skip-ahead (harder per hour, not longer). Module hours are in section 3 and sum to the header (G-2, section 5 effort table). Optional M3 Challenge lesson (up to 2 h) is outside these totals |
| Interview coverage tags | `cloud.containers`, `cloud.k8s`, `cloud.aws-compute`, `cloud.aws-messaging`, `cloud.aws-data`, `cloud.aws-security`, `cloud.networking`, `cloud.multicloud`, `cloud.cost`, `devops.iac`, `devops.cicd`, `devops.deploy`, `ops.observability`, `ops.slo`, `ops.incident`, `ops.linux-triage`. Secondary tags (no credit): `sec.secrets` (owner A8), `design.ai-systems` (owner A6). Feeds system-design (A6) rounds on failure modes, scaling and observability, and the AI-system designs (LLM gateway, voice platform) |
| Company-style clusters served | IT-services, global-IT-consulting, Big4, global-product, Indian-product, AI-startup |
| IDE / lab environment | No live cluster, Docker daemon or cloud account in 1.0 (R-A7-2). Labs use the P-11 simulators: Dockerfile linter and image-size analyser (no daemon); Kubernetes manifest validator plus scripted `kubectl` step-through simulator; Terraform static plan analysis or mocked provider; CI YAML linters; IAM policy evaluator; in-process OpenTelemetry collector and Prometheus-style metrics evaluator; AWS emulator per spike S-1 (preferred: in-process moto-style emulator; LocalStack only after licensing review, see section 6). **Every lab states what it does not model** (IAM enforcement, service limits, timing, networking, real failure behaviour). No cost to the learner |
| Badge and certificate | Course badge "Cloud and DevOps Ready (Senior)": all 10 module badges, final assessment 80 percent or higher, voice round at "Meets" (average >= 3.0, none < 2; G-7), and evidence artefacts (hardened Dockerfile, K8s manifests, Terraform module, pipeline YAML, OTel-instrumented service with an SLO and alert, one postmortem). Certificate carries the "last reviewed" date |
| Authoring owner | SME agent A7 |
| Reviewer | To be assigned |
| Review cadence | Every 6 months for the course; M3 (Kubernetes), M4 (Lambda and serverless), M7 (Terraform) and M9 (OpenTelemetry) every 3 months because versions and deprecations move fast. Version facts stamped "as of October 2026" |

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice tasks | Pitfalls and misconceptions interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | Containers and Docker for production (skip band expected for lessons 1-2; gap in lessons 3-4) | Build a minimal, cache-efficient, non-root image and explain what is inside an image and a running container | Image layers and union filesystem, build cache order, `.dockerignore`, multi-stage builds, slim vs distroless vs Alpine (musl wheel issues for Python), pinned digests, `PID 1` and signal handling (`exec` form, `tini`), non-root user, read-only root filesystem, capabilities, healthchecks, BuildKit cache mounts, secrets at build time, `uv` or `pip` install patterns, namespaces and cgroups as the actual isolation primitives | Layer stack diagram with cache invalidation step-through; namespaces and cgroups sketch; multi-stage flow | Shrink a 1.2 GB Python image to under 150 MB and reorder layers for cache hits (graded on size and layer analysis); fix an image that ignores SIGTERM; remove a baked-in secret; add a healthcheck | "A container is a lightweight VM"; copying source before installing dependencies; running as root; `latest` tags; secrets in `ENV` or in an earlier layer (still recoverable); Alpine always smaller and better for Python |
| M2 | Compose essentials and container supply-chain security | Run a multi-service stack locally and secure the image supply chain | Compose essentials only (healthcheck-gated `depends_on`, profiles, override files, dev vs prod parity; depth trimmed to fund M6 networking), image scanning (scanner names are examples, any tool with SBOM and VEX support), SBOMs, image signing concepts (Sigstore/cosign), provenance and SLSA build levels (L1 provenance exists, L2 hosted platform signs it, L3 hardened platform; slsa.dev v1.0), base-image patching cadence, registry choice (ECR), tag immutability, digest deploys | Compose topology diagram; supply-chain pipeline flow | Compose an API + Postgres + Redis + worker stack with healthcheck-ordered startup; interpret a scan report and decide what to fix; write a Compose override for debugging | `depends_on` means "ready"; Compose as a production orchestrator; ignoring transitive CVEs vs panicking over unreachable ones; scan only at build, never at runtime |
| M3 | Kubernetes essentials (gap; weight by JD analyser) | Deploy, expose, configure, scale and debug a stateless Python service on Kubernetes | Architecture (API server, etcd, scheduler, kubelet, controllers), Pod lifecycle, ReplicaSet and Deployment rolling updates (`maxSurge`, `maxUnavailable`), Services (ClusterIP, NodePort, LoadBalancer), Ingress and the **Gateway API** direction, ConfigMap and Secret (base64 is not encryption; external secrets), probes (liveness, readiness, startup), requests and limits and QoS classes, OOMKilled vs CPU throttling, HPA (CPU, memory, custom metrics) and KEDA concept, PodDisruptionBudget, graceful shutdown (`preStop`, `terminationGracePeriodSeconds`), Jobs and CronJobs, namespaces and RBAC basics, `kubectl` debugging flow (describe, logs, events, exec, port-forward), StatefulSet and PV concepts at overview level; **optional Challenge lesson (up to 2 h, awareness only): operators and CRDs, service mesh, CNI and kube-proxy networking internals** (R-A7-1) | Control-loop (reconciliation) animation; rolling-update step-through; request path Ingress to Pod; probe state machine | Write Deployment, Service, ConfigMap, HPA YAML for a FastAPI app (validated statically, then stepped through a scripted `kubectl` simulator); fix three broken manifests (wrong selector, failing readiness probe, OOMKilled); predict rollout behaviour for given `maxSurge` and `maxUnavailable` | Liveness probe that checks the database (causes restart storms); no readiness probe so traffic hits cold pods; limits equal to requests misunderstanding; CPU limit throttling causing latency; "Secrets are secure by default"; HPA needs requests set; Ingress controller vs Ingress resource confusion; believing Ingress is removed (the community ingress-nginx controller reached end-of-life on 24 March 2026, repository read-only, no further CVE patches; the Ingress API itself remains; migration paths are Gateway API implementations with the ingress2gateway 1.0 tool or vendor-supported controllers; checked October 2026) |
| M4 | AWS compute and serverless (skip band expected for Lambda basics; gap in ECS/Fargate, API Gateway, cost modelling) | Choose and operate the right AWS compute model and tune Lambda for production | Lambda execution model (init, invoke, freeze), cold starts and mitigation (provisioned concurrency, SnapStart for Python (Python 3.12 and later, and container-image functions since July 2026 per AWS announcements, verified 2026-10 against AWS What's New and the Lambda docs: AWS base images on Python 3.12+ work directly, other container images need the label com.amazonaws.lambda.feature.snapstart="Allow" or runtime hooks, and two commercial Regions are excluded; **[verify: exclusions for provisioned concurrency and EFS, and the Python 3.15 runtime date, in the AWS docs at each review]**), smaller packages, init-time work, ARM/Graviton), memory-to-CPU coupling, concurrency (reserved, provisioned), sync vs async vs stream/queue invocations, retries and DLQ or destinations, Lambda in a VPC, container-image Lambdas, Powertools for AWS Lambda (Python), API Gateway (REST vs HTTP API vs ALB vs Function URL), authorisers, throttling, payload limits, ECS on Fargate vs EC2 launch type, task definitions, services, task roles vs execution roles, ALB integration, autoscaling, EKS vs ECS decision, App Runner (closed to new customers from 30 April 2026, maintenance mode; AWS points to ECS Express Mode; checked October 2026), Lambda runtime and billing changes (Python 3.14 managed runtime available; init phase billed like invoke time since 1 August 2025; always read deprecation dates from the runtimes page) | Lambda lifecycle step-through; compute decision tree (Lambda vs Fargate vs EKS vs EC2); API Gateway type comparison matrix | Tune a Lambda handler (move client init out of handler, right-size memory using supplied timing data); write an idempotent async handler with a DLQ; write an ECS task definition and identify the IAM role mistakes; cost-compare Lambda and Fargate for three traffic profiles using a calculator task | "Lambda scales infinitely" (account and burst limits, downstream DB connections exhausted); cold start only matters for Java; using one IAM role for everything; Lambda in VPC always slow (outdated, but connection pooling and NAT cost still matter); picking EKS for a three-service team; ignoring the 15-minute limit and payload limits (quote current numbers from the Lambda quotas page at review; never hard-code) |
| M5 | AWS data, messaging and integration (skip band expected for S3 basics; gap in SQS/SNS/EventBridge semantics, RDS/Aurora operations) | Design reliable event-driven integrations and operate managed data stores | S3 (storage classes, lifecycle, presigned URLs, versioning, consistency, event notifications, multipart, encryption SSE-KMS), SQS (standard vs FIFO, visibility timeout, long polling, DLQ and redrive, batching, partial batch failure for Lambda), SNS fan-out and filtering, SNS plus SQS pattern, EventBridge (buses, rules, pipes, scheduler, schema), Kinesis vs SQS vs MSK decision at overview (Kafka-style essentials -> A6.M4 Challenge, O-111), idempotency and outbox only as AWS primitives (SQS FIFO dedup window, conditional writes; implementation is owned by A5.M8, pointer of 20 minutes or less), at-least-once delivery as SQS exposes it, RDS and Aurora (Multi-AZ vs read replica, failover behaviour, RDS Proxy for Lambda, parameter groups, backups and PITR, Aurora Serverless v2), ElastiCache with Valkey or Redis OSS engines (cluster mode, eviction, failover, TLS and auth; Valkey priced lower than Redis OSS on AWS pricing pages, checked October 2026, never hard-code prices), DynamoDB concepts at overview | SQS visibility-timeout and DLQ timeline; SNS to SQS fan-out diagram; Multi-AZ failover vs read-replica comparison | Build a producer, queue, Lambda consumer with partial batch response and DLQ redrive in the emulator; attach the A5.M8 idempotent consumer to the SQS Lambda consumer through a conditional write (reused by `ref`, not re-implemented); presigned upload flow | "SQS guarantees exactly-once" (only FIFO dedup within a window; consumers still need idempotency); visibility timeout shorter than processing time; Multi-AZ is not a read replica; unbounded retries with no DLQ; one connection per Lambda invocation to RDS |
| M6 | AWS identity, networking, secrets and Linux network triage (gap) | Apply least privilege, reason about private connectivity and blast radius, and triage a hung or unreachable Python service from TCP, TLS, DNS and process evidence | IAM (users vs roles, policies, identity vs resource policies, trust policies, `AssumeRole`, permission boundaries, SCP concept, policy evaluation logic, conditions, IAM Access Analyzer, short-lived credentials, OIDC federation for CI), KMS basics, VPC (CIDR, public vs private subnets, route tables, IGW, NAT gateway cost, security groups vs NACLs, VPC endpoints and PrivateLink, Multi-AZ layout, peering vs Transit Gateway overview), ALB vs NLB, CloudFront and WAF at overview, Route 53 routing policies, TLS with ACM, shared responsibility model; **KMS and Secrets Manager vs SSM Parameter Store service specifics** (rotation, caching in a handler, cost; secrets practice -> A8.M8); **Networking and Linux triage fundamentals (new lesson, O-110):** TCP handshake, retransmits and congestion at interview depth, TLS handshake and certificate chains, DNS resolution and TTL pitfalls in long-lived pools, HTTP/2 and HTTP/3 differences, sockets and file descriptors, ephemeral port exhaustion and TIME_WAIT, `top`, `ss`, `lsof`, `strace`-style triage for a hung Python service | Subnet and route-table diagram with request flow; IAM policy evaluation flowchart; PrivateLink vs NAT path comparison; TCP handshake and TLS handshake ladder diagrams; triage decision flow for a hung service | Given captured `ss`, `top` and `strace`-style outputs, name the cause of a hung service (fd exhaustion, port exhaustion, DNS stall, blocked thread); rotate a secret and cache it safely in a handler; fix an over-broad IAM policy to least privilege using Access Analyzer-style findings; diagnose "Lambda cannot reach RDS" and "private subnet cannot reach S3" from a given topology; design a three-tier VPC and annotate security groups | `Action: *` and `Resource: *`; confusing security groups (stateful, allow-only) with NACLs (stateless); NAT gateway for all S3 traffic when a gateway endpoint is free; sharing access keys instead of roles; trust policy vs permission policy; assuming explicit allow beats deny; "the network is slow" with no `ss` or packet evidence; blaming the app for ephemeral port exhaustion caused by no connection reuse; secrets in Lambda environment variables unencrypted |
| M7 | Infrastructure as Code with Terraform (gap; Terraform sole IaC, OpenTofu note) | Author, structure and safely operate Terraform for a small AWS stack | HCL basics, providers, resources, variables, outputs, locals, data sources, `for_each` vs `count`, modules, remote state (S3 backend with native locking via `use_lockfile`, introduced in 1.10 and stable in 1.11; the DynamoDB lock arguments are deprecated; checked October 2026), workspaces vs directory-per-environment, plan/apply workflow in CI, drift detection, `import` blocks, `moved` blocks and refactoring, lifecycle (`prevent_destroy`, `create_before_destroy`, `ignore_changes`), sensitive values in state, Terraform vs OpenTofu context (Terraform under BSL 1.1 since August 2023, IBM acquired HashiCorp in 2025 with no licence change; OpenTofu is the Linux Foundation MPL 2.0 fork with extras such as state encryption; checked October 2026; commands are interchangeable for the course), `terraform test` with `mock_provider`, policy as code (OPA/Sentinel concept); **one table-level comparison lesson (CloudFormation, CDK, Pulumi; no labs; R-A7-4)** | State and plan/apply lifecycle diagram; module composition diagram; drift scenario step-through | Write a module for S3 bucket + SQS + Lambda with least-privilege role; read a plan and identify a destructive change; split state to reduce blast radius; fix a `count` index-shift replace using `for_each` or `moved` | Committing state or secrets to Git; one giant state file; `count` with lists causing resource recreation; manual console changes causing drift; `terraform apply` from laptops; assuming `destroy` is never run by pipelines; ignoring that secrets land in state |
| M8 | CI/CD, pipeline security gates and deployment strategies (skip band expected for Bitbucket basics; gap in GitHub Actions, OIDC, progressive delivery) | Design a pipeline from commit to safe production rollout with rollback | Pipeline stages (lint, test, build, scan, push, deploy, verify), caching and parallelism, GitHub Actions concepts (workflows, jobs, reusable workflows, matrices, environments and approvals, OIDC to AWS, pinning actions by SHA), Bitbucket Pipelines (steps, pipes, deployments, variables, OIDC), trunk-based development, feature flags, environments and promotion, deployment strategies (recreate, rolling, blue/green, canary, shadow), database migration compatibility (expand and contract), automated rollback on SLO signals, GitOps (Argo CD or Flux concept), release versioning, DORA metrics, secret handling in CI, **owns pipeline security gates** (SAST, dependency, secret and image scan stages, gate policy and exceptions; O-54; local tooling and Python dependency supply chain -> A8.M8), GitHub Actions pricing and runner-fee changes are volatile (never teach prices; the announced self-hosted runner fee was postponed, checked October 2026) | Pipeline flow diagram; blue/green vs canary traffic-shift animation; expand-and-contract migration timeline | Write a GitHub Actions workflow with OIDC role assumption, test matrix and a gated production deploy; translate it to Bitbucket Pipelines; design a canary with promotion and abort criteria; plan a column rename with zero downtime | Long-lived cloud keys in CI secrets; deploying the migration and code in one irreversible step; canary without metrics to judge it; "blue/green is free" (double capacity, data-layer compatibility); rollback that ignores schema changes; third-party actions on mutable tags |
| M9 | Observability: logs, metrics, traces and SLOs (gap) | Instrument a service and turn telemetry into SLOs and actionable alerts | Three pillars plus events and profiles, structured JSON logging with correlation IDs (Python `logging`, `structlog`), log levels and cost, PII in logs, metrics types (counter, gauge, histogram), RED and USE and the four golden signals, cardinality and why labels explode cost, Prometheus model (pull, PromQL basics, histograms and percentiles, `rate`), distributed tracing (spans, context propagation, W3C `traceparent`, sampling head vs tail), OpenTelemetry (API, SDK, Collector, OTLP, auto-instrumentation for FastAPI, Django, requests, SQLAlchemy, Celery, semantic conventions, traces, metrics and logs are stable in the specification, SDK maturity varies by language (check the Python logs SDK: still in development as of 2026-10, with stabilisation PRs open in opentelemetry-python (SDK 1.45.0), verified 2026-10), profiles in public alpha since March 2026, checked October 2026), CloudWatch (Logs Insights, metrics, alarms, embedded metric format; X-Ray SDKs in maintenance mode since 25 February 2026 with end of support 25 February 2027, AWS recommends OpenTelemetry or ADOT, X-Ray the backend continues), Grafana, Datadog and similar vendor landscape at overview, SLI, SLO, error budget, multi-window burn-rate alerts, symptom-based vs cause-based alerting, alert fatigue, dashboards that answer questions, synthetic checks; **20-minute pointer lesson only:** GenAI spans and LLM cost metrics -> B5.M7, voice per-stage latency and WER sampling -> B6, agent traces -> B4.M8 (no duplicated labs; R-A7-3) | Trace waterfall reading step-through; histogram-to-percentile explanation; error-budget burn graph; OTel pipeline diagram (SDK, Collector, backends) | Instrument a FastAPI app with OTel traces and a custom histogram; add correlation IDs to logs across an async boundary; compute an SLO and error budget from sample data; write a burn-rate alert rule; find the slow span in a supplied trace | Averages instead of percentiles; high-cardinality labels (user ID); logging everything at DEBUG in production; alerting on CPU instead of user pain; dashboards without an owner or question; 100 percent SLO targets; sampling that drops the errors; confusing monitoring with observability |
| M10 | Reliability, incident response, infrastructure cost (FinOps) and multi-cloud awareness (gap) | Handle an incident end to end, reason about cost, and map AWS concepts to Azure and GCP | Reliability patterns (timeouts, retries with backoff and jitter, circuit breakers, bulkheads, load shedding, graceful degradation, health checks), multi-AZ and multi-region (RPO, RTO, pilot light, warm standby, active-active), backups and restore testing, chaos and game days, incident lifecycle (detect, triage, mitigate, communicate, resolve, review), severity levels, on-call practice, runbooks, blameless postmortems and action items, MTTR vs MTTD, **owns infrastructure cost levers and FinOps** (NAT and data transfer, idle resources, rightsizing, Savings Plans and Spot concepts, tagging and showback, budgets and anomaly detection, unit economics; supplies inputs to A6.M5 shared cost rubric card; LLM token cost -> B2.M9 via template X3), well-architected pillars, **multi-cloud at table level only** (Azure: App Service, Functions, AKS, Blob, Service Bus, Entra ID, Key Vault, Monitor and Application Insights; GCP: Cloud Run, Functions, GKE, Cloud Storage, Pub/Sub, IAM, Secret Manager, Cloud Monitoring), Cloud Run as a serverless-container reference, portability and lock-in trade-offs (service names change often; re-check each review) | AWS-Azure-GCP concept map table; incident timeline template; retry-storm diagram; RPO/RTO strategy ladder | Run a scripted incident simulation (given dashboards, logs and a bad deploy) and write the timeline and postmortem; add backoff with jitter and a circuit breaker to a client; estimate monthly cost for a given architecture and find the three biggest savings; map a given AWS design to Azure and GCP services | Blaming individuals; fixing without mitigating first (rollback before root cause); retries without jitter amplifying outages; "multi-cloud means resilience" without cost and complexity analysis; DR plans never tested; ignoring data-transfer and NAT costs; claiming multi-region without addressing data replication |

## 3. Per-module detail blocks

Question-bank targets for every module follow the authoring template: 12 MCQ / concept checks, 6 code-output or predict-the-result, 4 coding or config tasks with hidden tests (graded by static validators or simulators, see section 1), 8 verbal questions with follow-up trees (3 seeded in section 4), 3 scenario or design questions. Mastery check per module: 6-10 items, pass threshold 80 percent, revision loop on misses. Item IDs follow `Q-A7-Mn-nn` (G-10); lesson IDs are `A7.Mn.k`. Hours are Simpler / Deeper (G-2: Deeper is net of typical skip-ahead, harder per hour, not longer) and sum to 24.0 S / 15.0 D. Every lab states what its simulator does not model.

**Ownership notes (G-1).** OWNS and POINTS per module (pointers are one-way, at most 20 minutes, no lab, at most one `ref` item):

| Module | OWNS | POINTS |
|---|---|---|
| M1 | Image layers, build cache, runtime hardening, cgroup-aware worker sizing | Build-time secrets practice -> A8.M8 (O-61) |
| M2 | Container supply chain: scan, SBOM, signing, provenance, registry and tagging (O-52) | Python dependency supply chain -> A8.M8 (O-53) |
| M3 | Kubernetes essentials; pod lifecycle and graceful shutdown on K8s (O-26 part) | Process-level graceful shutdown -> A2.M5; operators, mesh, CNI internals are awareness only (depth is 1.1) |
| M4 | Lambda, ECS/Fargate, API front doors, compute choice (O-51) | Worker job semantics -> A2.M9 (O-15); retry and backoff implementation -> A5.M4 (X2, O-19) |
| M5 | AWS queues and eventing: SQS, SNS, EventBridge, Kinesis (O-23); RDS, Aurora, ElastiCache operations (O-36) | Delivery-semantics theory, saga, CDC, exactly-once reasoning -> A6.M4 (O-21); idempotent consumer and outbox implementation -> A5.M8; queue vs log broker and Kafka-style essentials -> A6.M4 Challenge (O-22, O-111); DB replication and pooling concepts -> A4.M7 (O-32); migrations and PITR -> A4.M8-M9 (O-34) |
| M6 | IAM evaluation, VPC, KMS, Secrets Manager and SSM specifics, **networking and Linux triage (O-110)** | Secrets practice -> A8.M8 (O-61); OAuth2/OIDC protocol and JWT design -> A5.M6 (O-44) |
| M7 | Terraform and the IaC comparison table | none |
| M8 | Pipeline design, **security gates** (SAST, dependency, secret and image scan stages; O-54), deployment strategies, feature flags (O-27) | Local scanner usage (pip-audit, bandit, semgrep, ruff security rules, secret scanning) -> A8.M8 |
| M9 | Generic OTel, metrics, tracing, SLOs, platform-level structured logging (O-55) | Stdlib logging mechanics -> A1.M8 (O-7); LLM observability and drift -> B5.M7 (O-82); voice metrics -> B6; agent traces -> B4.M8; load-testing method -> A8.M10 (O-63) |
| M10 | DR implementation, restore testing, incidents, postmortems (O-25); **infrastructure cost levers and FinOps (O-56)** | DR tiers at design level -> A6.M5 (O-24); LLM token cost -> B2.M9 (X3); retry and circuit-breaker implementation -> A5.M4 (X2) |

### M1 — Containers and Docker for production (2.0 S / 0.75 D)
- **Lessons:** A7.M1.1 Images, layers and the build cache **[SKIP-AHEAD band]**. A7.M1.2 Multi-stage builds and slim base images **[SKIP-AHEAD band]**. A7.M1.3 What a container actually is: namespaces, cgroups, PID 1 and signals. A7.M1.4 Hardening: non-root, read-only filesystem, capabilities, digests, build-time secrets.
- **Simpler:** guided Dockerfile refactor with before/after size; analogy-light cgroup limits. **Deeper:** inspect layers with a tool, BuildKit cache and secret mounts, reproducible builds, musl vs glibc wheels.
- **Mastery focus:** cache-order reasoning, secret layer leaks, signal handling, cgroup-aware sizing of gunicorn/uvicorn workers.
- **Badge:** hardened Dockerfile passing size, non-root, healthcheck and signal tests (Dockerfile linter and image-size analyser; not modelled: real runtime, kernel isolation, registry behaviour).

### M2 — Compose essentials and container supply-chain security (1.5 S / 1.0 D)
- **Lessons:** A7.M2.1 Compose for multi-service local stacks and dev/prod parity (essentials only). A7.M2.2 Scanning, SBOMs and triage by reachability. A7.M2.3 Signing, provenance and SLSA levels. A7.M2.4 Registry and tagging strategy (ECR, immutable tags, digest pinning).
- **Simpler:** run the provided stack and read one scan report. **Deeper:** triage CVEs by reachability, set patch SLAs, design sign-and-verify in the pipeline.
- **Mastery focus:** startup ordering vs readiness, scan-gate trade-offs, tag mutability risks.
- **Badge:** Compose file with healthcheck ordering (YAML validator; not modelled: real start timing) plus a written scan-triage decision.

### M3 — Kubernetes essentials (3.0 S / 2.0 D)
- **Lessons:** A7.M3.1 Architecture and the reconciliation loop. A7.M3.2 Pods, Deployments and rolling updates. A7.M3.3 Services, Ingress/Gateway API and networking basics. A7.M3.4 Config, secrets, probes and resources. A7.M3.5 Autoscaling, disruption budgets, graceful shutdown and the debug workflow. **A7.M3.6 Challenge (optional, up to 2 h, outside module hours; R-A7-1):** operators and CRDs, service mesh, CNI and kube-proxy internals, awareness only.
- **Simpler:** annotated YAML walkthroughs, one concept per lesson, guided debug of a crashing pod. **Deeper:** QoS and eviction order, CPU throttling mechanics, topology spread, HPA tuning, network policies, KEDA for queue-driven scaling.
- **Mastery focus:** probe design, resource sizing, rollout mechanics, symptom-to-cause debugging.
- **JD-driven weighting rule:** the placement engine multiplies M3 time by the JD analyser output: Kubernetes (EKS, AKS, GKE) required -> 1.0 of Deeper hours; nice-to-have -> 0.6 (lessons .1, .2, .4 only); absent -> 0.25 (primer: .1 plus probes from .4). With no JD supplied the default is the Learner-Fit tag (B:S).
- **Badge:** passing manifest set (Deployment, Service, ConfigMap, HPA, PDB) plus three fixed broken-manifest scenarios (manifest validator and scripted `kubectl` step-through; not modelled: scheduler, real networking, node pressure, rollout timing).

### M4 — AWS compute and serverless (2.5 S / 1.5 D)
- **Lessons:** A7.M4.1 Lambda internals and cold starts **[SKIP-AHEAD band for basics]**. A7.M4.2 Lambda concurrency, retries, async patterns and Powertools. A7.M4.3 API Gateway, ALB and Function URL choices. A7.M4.4 ECS on Fargate. A7.M4.5 Choosing compute and cost modelling.
- **Simpler:** decision tree with worked examples. **Deeper:** concurrency maths (concurrency = rate x duration), RDS connection exhaustion, Fargate sizing and autoscaling, SnapStart and init optimisation.
- **Mastery focus:** limits and consequences, task role vs execution role, choosing the API front door.
- **Badge:** tuned Lambda, ECS task definition review, written compute recommendation with cost reasoning (emulator per S-1 plus calculator; not modelled: real limits, throttling, cold-start timing, IAM enforcement).

### M5 — AWS data, messaging and integration (2.5 S / 1.5 D)
- **Lessons:** A7.M5.1 S3 design and security **[SKIP-AHEAD band]**. A7.M5.2 SQS, SNS and EventBridge patterns and service selection (Kinesis vs SQS at overview; Kafka-style essentials are the A6.M4 Challenge). A7.M5.3 DLQ, redrive and replay operations on AWS (idempotency and outbox design are owned by A5.M8: pointer). A7.M5.4 RDS/Aurora operations and RDS Proxy. A7.M5.5 ElastiCache and Secrets Manager usage.
- **Simpler:** one reliable queue-consumer pattern. **Deeper:** FIFO groups, ordering vs parallelism, EventBridge Pipes, Aurora failover and client retry, cache stampede and TTL jitter.
- **Mastery focus:** delivery semantics, failure handling, which service for which integration.
- **Badge:** working producer-queue-consumer with DLQ redrive and the reused A5.M8 idempotency tests (emulator per S-1; not modelled: real visibility timing, throughput quotas, failover).

### M6 — AWS identity, networking, security and Linux network triage (3.5 S / 2.25 D)
- **Lessons:** A7.M6.1 IAM mental model and policy evaluation. A7.M6.2 Roles, trust policies, OIDC federation, KMS, Secrets Manager vs SSM Parameter Store. A7.M6.3 VPC layout, routing, security groups and NACLs. A7.M6.4 Private connectivity (endpoints, PrivateLink) and edge/TLS (Route 53, ALB/NLB, CloudFront, WAF, ACM). **A7.M6.5 Networking fundamentals for backend engineers (new; O-110):** TCP handshake, retransmits and congestion at interview depth, TLS handshake and chains, DNS resolution and TTL pitfalls, HTTP/2 and HTTP/3. **A7.M6.6 Linux triage of a hung Python service (new; O-110):** sockets and file descriptors, ephemeral port exhaustion and TIME_WAIT, `top`, `ss`, `lsof`, `strace`-style evidence. Lessons .5 and .6 account for +2.0 S / +1.5 D inside the module hours, offset by trimmed Compose and multi-cloud.
- **Simpler:** pre-built topologies with guided tracing; captured outputs with a cause checklist. **Deeper:** SCPs, permission boundaries, cross-account access, KMS key policies, Transit Gateway, unguided evidence-to-cause triage.
- **Mastery focus:** tracing a packet or an authorisation decision, least-privilege rewrites, naming a cause from `ss`/`top`/`strace` text.
- **Badge:** least-privilege policy set, two solved connectivity diagnoses, two solved hung-service triages (IAM policy evaluator, topology step-through, captured-output analyser; not modelled: real IAM enforcement, packet timing, kernel behaviour).

### M7 — Infrastructure as Code with Terraform (2.25 S / 1.5 D)
- **Lessons:** A7.M7.1 HCL, resources and the plan/apply model. A7.M7.2 Variables, modules and composition. A7.M7.3 Remote state, locking and environment layout. A7.M7.4 Safe changes: refactoring, imports, lifecycle, drift. A7.M7.5 Terraform in CI, testing, and IaC alternatives (one table-level comparison of CloudFormation, CDK and Pulumi; no labs; R-A7-4).
- **Simpler:** single-module build with guided plan reading. **Deeper:** state surgery, provider pinning and upgrades, policy as code, `terraform test` with `mock_provider`.
- **Mastery focus:** reading plans for danger, state design, secrets in state.
- **Badge:** reviewed module plus plan-review exercise finding all destructive changes (static plan analysis or mocked provider; not modelled: real provider behaviour, eventual consistency, quotas).

### M8 — CI/CD, pipeline security gates and deployment strategies (2.0 S / 1.25 D)
- **Lessons:** A7.M8.1 Pipeline anatomy, quality gates and **security gates** (SAST, dependency, secret and image scan stages, gate policy, exceptions with expiry). A7.M8.2 GitHub Actions and Bitbucket Pipelines side by side **[SKIP-AHEAD band for Bitbucket]**. A7.M8.3 Secure cloud auth with OIDC and secret hygiene. A7.M8.4 Rolling, blue/green, canary, feature flags. A7.M8.5 Safe database changes and rollback (expand and contract).
- **Pointer:** local usage of the scanners named in .1 -> A8.M8 (A8 does not design pipelines).
- **Simpler:** one deployable pipeline, one canary diagram. **Deeper:** GitOps pull model, progressive delivery controllers, DORA metrics, monorepo pipelines.
- **Mastery focus:** strategy choice under data-layer constraints, rollback truthfulness.
- **Badge:** pipeline YAML passing lint and policy checks plus rollout plan with abort criteria (CI YAML linters; not modelled: runner behaviour, real OIDC trust exchange, deploy timing).

### M9 — Observability: logs, metrics, traces and SLOs (2.75 S / 1.75 D)
- **Lessons:** A7.M9.1 Logging that helps at platform level (JSON, correlation IDs across services and queues, cost, PII; handler mechanics are A1.M8, not re-taught). A7.M9.2 Metrics, histograms, percentiles and cardinality. A7.M9.3 Tracing and OpenTelemetry in Python. A7.M9.4 SLIs, SLOs, error budgets and burn-rate alerts. A7.M9.5 **Pointer lesson (20 minutes, no lab):** GenAI spans and LLM cost metrics -> B5.M7; voice per-stage latency and WER sampling -> B6; agent traces -> B4.M8 (R-A7-3; the former "debugging AI workloads" depth is removed). The generic "debugging with telemetry" practice moves into .3 and .4.
- **Simpler:** instrument one endpoint, read one trace. **Deeper:** tail sampling, exemplars, Collector pipelines, exporting to multiple backends, cost control of telemetry.
- **Mastery focus:** choosing the right signal, percentile reasoning, alert design.
- **Badge:** instrumented service with an SLO document and a burn-rate alert rule that passes simulated outage and no-outage tests (in-process OTel collector and Prometheus-style evaluator; not modelled: real backend ingestion, sampling at scale, clock skew).

### M10 — Reliability, incident response, cost and multi-cloud awareness (2.0 S / 1.5 D)
- **Lessons:** A7.M10.1 Resilience patterns in infrastructure and client code (implementation of retry/backoff/breaker is A5.M4 X2; here: retry budgets, load shedding, health checks, failure injection). A7.M10.2 DR implementation, RPO and RTO, backup restore testing (design-level DR tiers are A6.M5). A7.M10.3 Incident command, communication and blameless postmortems. A7.M10.4 Infrastructure cost levers and FinOps (feeds the A6.M5 shared cost rubric card). A7.M10.5 Multi-cloud awareness: AWS, Azure and GCP concept map at table level (30 minutes, trimmed per boss offset).
- **Simpler:** concept map table and one scripted incident. **Deeper:** retry budgets, metastable failure, game-day design, unit economics per request.
- **Mastery focus:** mitigate-first discipline, honest cost and lock-in trade-offs, equivalence mapping accuracy.
- **Badge:** completed incident simulation with timeline and postmortem plus a cost review (scripted incident and cost calculator; not modelled: real pressure, partial information, real billing data).

## 4. Seed interview questions (3 per module, 30 total)

Format per item: difficulty, competency tag, company-style cluster, ideal answer outline, common weak answers, follow-up probes. These are style-of-question items, not questions from any named company. Difficulty words map to the data-model integer scale 1-5 at import: easy 1, easy-medium 2, medium 3, medium-hard 4, hard 5 (same mapping as Track C); calibrated after the pilot.

### M1 — Containers

**M1-Q1. "Walk me through how you would build a production Docker image for a FastAPI service and why."** (Medium; `cloud.containers`; IT-services, global-product)
- Ideal: multi-stage build, builder installs dependencies/wheels, runtime stage is slim; order layers so dependency install is cached and source copied last; `.dockerignore`; non-root user; pinned base image (tag plus digest); no secrets in layers (use BuildKit secret mounts); `exec`-form CMD so signals reach the process; healthcheck and worker count sized to cgroup limits; scan in CI; mention reproducibility.
- Weak: "just use python:latest and pip install"; only talks about size; ignores security and signals.
- Probes: Why not Alpine? What happens to a secret you `COPY` then `RUN rm`? How would you cut build time in CI?

**M1-Q2. "What is a container, really? How does it differ from a VM?"** (Medium; `cloud.containers`; global-IT-consulting, Big4)
- Ideal: a process (or group) with isolated views via Linux namespaces (pid, net, mnt, uts, ipc, user) and resource limits via cgroups; shares host kernel; image is a read-only layered filesystem with a writable top layer; VMs virtualise hardware with their own kernel; trade-offs (startup, density, isolation strength, kernel attack surface); mention that stronger isolation exists (gVisor, Firecracker microVMs as AWS documents for Lambda).
- Weak: "lightweight VM"; cannot name namespaces or cgroups.
- Probes: What does `docker stop` do and why might a container take 10 s to stop? How does a memory limit manifest to your Python process? Can a container escape and what reduces that risk?

**M1-Q3. "Your container works locally but is OOM-killed in production. How do you investigate?"** (Hard; `cloud.containers`; global-product, Indian-product)
- Ideal: confirm OOM via exit code 137 and orchestrator events; compare limit vs actual RSS; check whether workers (gunicorn processes times memory) multiplied beyond limit; Python memory not returned to OS promptly, fragmentation, large payloads and caches; reproduce with the same limit locally; measure with metrics and heap profilers; fixes: reduce workers, stream responses, bound caches, raise limit with justification, recycle workers (`max-requests`); add memory alerts before kill.
- Weak: "increase the memory limit" with no diagnosis.
- Probes: Difference between container memory limit and what the JVM/Python sees? Why might RSS grow without a leak? Would you set a limit equal to the request?

### M2 — Compose and supply chain

**M2-Q1. "How do you make a local multi-service development environment reliable for a team?"** (Medium; `cloud.containers`; IT-services, Indian-product)
- Ideal: Compose with named services, healthchecks and `depends_on: condition: service_healthy`; seeded data and migrations as one-shot services; override files for dev tooling and hot reload; env files with no real secrets; pinned image versions; document one-command start and reset; parity with prod config via the same env var contract; profiles for optional services; limits of Compose (not prod orchestrator).
- Weak: "just docker compose up"; no mention of readiness or secrets.
- Probes: What if migrations race with API startup? How do you share test data? When do you move to a cluster-based dev environment?

**M2-Q2. "A scanner reports 40 high CVEs in your image. What do you do?"** (Medium; `cloud.containers`; Big4, global-IT-consulting)
- Ideal: triage rather than panic: is the package in the runtime image, reachable, exploitable, fix available; rebuild on a patched slim/distroless base to remove most OS noise; update dependencies; use VEX or documented exceptions with expiry; gate on fixable criticals; rescan regularly since new CVEs appear post-build; SBOM for inventory; patch SLA by severity.
- Weak: "fail the build on any CVE" or "ignore them".
- Probes: How do you handle a CVE with no fix? How do you prove to an auditor what ran in production? Where in the pipeline do you scan?

**M2-Q3. "How would you secure the container supply chain from commit to cluster?"** (Hard; `cloud.containers`; global-product, Big4)
- Ideal: protected branches and reviews; pinned and verified dependencies; hardened CI runners and OIDC instead of static keys; build in isolated ephemeral environment; SBOM and provenance attestations; sign images (Sigstore/cosign concept) and enforce verification with an admission policy; immutable tags and digest deploys; private registry with scanning; least-privilege pull roles; runtime controls (read-only FS, non-root, seccomp).
- Weak: only "scan images".
- Probes: What does image signing not protect against? How do you handle third-party GitHub Actions? What is SLSA at a high level (build levels 1-3, slsa.dev v1.0)?

### M3 — Kubernetes

**M3-Q1. "Explain what happens from `kubectl apply` of a Deployment until traffic reaches a new pod."** (Medium; `cloud.k8s`; global-IT-consulting, global-product)
- Ideal: API server authenticates, validates, persists to etcd; Deployment controller creates ReplicaSet; ReplicaSet controller creates Pods; scheduler binds to a node on resources and constraints; kubelet pulls image and starts containers via the runtime; probes run; readiness passes then endpoints are added to the Service (EndpointSlice); kube-proxy/dataplane routes; Ingress/Gateway routes external traffic; rolling update respects `maxSurge`/`maxUnavailable`.
- Weak: "it starts a container on a server".
- Probes: What if the image pull fails? Where does readiness come in? What is the reconciliation loop?

**M3-Q2. "How do you design liveness, readiness and startup probes for a Python API that depends on a database?"** (Hard; `cloud.k8s`; global-product, Indian-product)
- Ideal: liveness answers "should this process be restarted" and must be cheap and independent of dependencies; readiness answers "can it serve traffic now" and may include critical dependency checks but avoid cascading removal during a shared dependency blip (consider degraded mode); startup probe protects slow starts (model loading, migrations); thresholds and timeouts tuned; graceful shutdown: `preStop` sleep, handling SIGTERM, drain in-flight requests, `terminationGracePeriodSeconds`; failure mode: DB check in liveness causes restart storms.
- Weak: one `/health` endpoint used for all probes.
- Probes: What happens when all pods fail readiness? How do long requests interact with rolling updates? How would LLM or model-loading services change this?

**M3-Q3. "Pods are Pending, then CrashLoopBackOff, then OOMKilled in turn. How do you debug each, and how would you size resources?"** (Hard; `cloud.k8s`; IT-services, Indian-product)
- Ideal: Pending: `describe pod` events, insufficient CPU/memory, taints, node selectors, PVC binding, quota; CrashLoopBackOff: `logs --previous`, exit codes, config or secret missing, failing probes, command errors; OOMKilled: limit vs usage, `kubectl top`, metrics history; sizing from observed percentiles with headroom, requests for scheduling and limits for protection; QoS classes; avoid CPU limits causing throttling or justify them; use VPA recommendations or load tests; HPA requires requests.
- Weak: "restart the pod" or "increase limits".
- Probes: Why might latency rise with CPU limits though utilisation is low? What does QoS Guaranteed buy? How would you autoscale a queue worker?

### M4 — AWS compute and serverless

**M4-Q1. "When would you choose Lambda, ECS Fargate, EKS or EC2 for a Python service?"** (Medium; `cloud.aws-compute`; global-IT-consulting, Big4, IT-services)
- Ideal: Lambda for spiky, event-driven, short tasks, low ops, pay per use, but limits (duration, payload, cold start, connection management, concurrency); Fargate for long-running containerised APIs and workers without managing nodes, predictable cost at steady load; EKS when you need Kubernetes ecosystem, portability, complex scheduling, and have platform skill; EC2 for special hardware, tuning, or lowest cost at steady scale with ops burden; decision factors: traffic profile, team skill, latency SLOs, state, cost crossover, compliance, existing tooling; give a concrete example with numbers approach.
- Weak: "Lambda is cheaper" or "Kubernetes for everything".
- Probes: At what traffic does Fargate beat Lambda and how would you compute it? How do you handle a 20-minute job? What about GPU inference?

**M4-Q2. "Your Lambda behind API Gateway has p99 latency spikes and sometimes overwhelms the database. Diagnose and fix."** (Hard; `cloud.aws-compute`; global-product, AI-startup)
- Ideal: separate cold start from downstream latency using traces and init duration logs; reduce cold starts: initialise clients outside handler, smaller package, lazy imports, provisioned concurrency for critical paths, SnapStart on supported Python runtimes, ARM; memory increase raises CPU and may cut duration; DB: connection storms from concurrency, use RDS Proxy or HTTP-based data API, reserved concurrency as a cap, queue buffering; API Gateway throttling and timeouts; observability with Powertools and X-Ray/OTel.
- Weak: "add more memory" only.
- Probes: How do you compute needed concurrency? What does reserved concurrency do to the rest of the account? How do you test this before production?

**M4-Q3. "Explain the ECS task role vs task execution role and how a service gets traffic and scales."** (Medium; `cloud.aws-compute`; IT-services, Big4)
- Ideal: execution role used by ECS agent to pull images, write logs, fetch secrets for injection; task role is what the application code assumes to call AWS APIs; service maintains desired count across AZs, integrates with ALB target groups and health checks; autoscaling with target tracking on CPU, memory or ALB request count per target; deployment types (rolling, blue/green via CodeDeploy) and circuit breaker rollback; logging via awslogs/Firelens.
- Weak: confuses the roles or says "one role".
- Probes: Where do secrets get injected? What happens when health checks fail during deploy? How do you size Fargate tasks?

### M5 — AWS data, messaging and integration

**M5-Q1. "Design a reliable asynchronous job pipeline with SQS and Lambda. What can go wrong?"** (Hard; `cloud.aws-messaging`; global-product, Indian-product)
- Ideal: producer sends to SQS (consider SNS fan-out); Lambda event source mapping with batch size and partial batch failure reporting; visibility timeout well above the function timeout (AWS guidance for Lambda consumers: at least six times the function timeout, plus the batching window; re-read at review); DLQ with `maxReceiveCount` and redrive; idempotent consumer (idempotency key, conditional write); poison messages; ordering needs (FIFO groups) and throughput trade-offs; backpressure via max concurrency; monitoring queue age, DLQ depth; outbox pattern on the producer to avoid dual-write loss.
- Weak: "SQS is exactly-once so no duplicates".
- Probes: Message processed but the delete failed, what happens? How do you replay the DLQ safely? How do you preserve per-customer ordering without serialising everything?

**M5-Q2. "SQS vs SNS vs EventBridge vs Kinesis: how do you choose?"** (Medium; `cloud.aws-messaging`; global-IT-consulting, Big4, IT-services)
- Ideal: SQS point-to-point queue for work distribution and buffering; SNS pub/sub fan-out with filtering, often with SQS subscribers; EventBridge event bus with content-based routing, SaaS and AWS service events, schedules, schema registry, lower throughput than streams; Kinesis (or MSK) ordered, replayable streams for high-throughput analytics and multiple consumers at their own pace; retention and replay differences; delivery and ordering semantics; cost and operational shape; example per service.
- Weak: lists definitions without selection criteria.
- Probes: Need replay of last 7 days? Need 100k events per second? Need to route by payload field to many targets?

**M5-Q3. "Explain RDS Multi-AZ, read replicas and Aurora. How does your application behave during a failover?"** (Medium; `cloud.aws-data`; IT-services, global-product)
- Ideal: Multi-AZ gives synchronous standby for HA and automatic failover (not for read scaling in the classic instance deployment; the Multi-AZ DB cluster option has two readable standbys); read replicas are asynchronous, for read scaling, with lag; Aurora has shared distributed storage, fast failover and up to many replicas; failover causes DNS endpoint flip and dropped connections: app must use retries with backoff, short DNS caching, connection pool validation; RDS Proxy smooths connections and failover; backups, PITR, parameter changes, maintenance windows.
- Weak: "Multi-AZ gives you read replicas".
- Probes: How long might failover take and how do you test it? What goes wrong with replica reads after a write? When pick Aurora Serverless v2?

### M6 — AWS identity, networking and security

**M6-Q1. "Explain how IAM decides whether a request is allowed, and how you would apply least privilege to a Lambda."** (Medium; `cloud.aws-security`; Big4, global-IT-consulting)
- Ideal: default deny; explicit deny always wins; allow needs a matching allow in identity or resource policy (cross-account needs both sides); boundaries and SCPs cap permissions; conditions; role with a trust policy for who may assume and a permission policy for what it can do; Lambda execution role scoped to exact resource ARNs and actions; no wildcards; separate roles per function; use Access Analyzer and CloudTrail to refine; temporary credentials rather than keys.
- Weak: "give it AdministratorAccess to get it working".
- Probes: Identity vs resource policy difference? What is `iam:PassRole` risk? How do you audit unused permissions?

**M6-Q2. "A Lambda in a private subnet cannot reach S3 or the internet, and another cannot reach RDS. Troubleshoot."** (Hard; `cloud.networking`; IT-services, global-product)
- Ideal: check subnet route tables (NAT gateway or gateway endpoint), VPC endpoint for S3 (gateway, free) and policies, security group egress and RDS inbound rule referencing the Lambda's security group on the DB port, NACLs (stateless, ephemeral ports), DNS resolution settings, endpoint policies, function in correct subnets across AZs, timeouts and ENI limits; use Reachability Analyzer and flow logs; mention NAT cost and why endpoints reduce it.
- Weak: "open all ports to 0.0.0.0/0".
- Probes: Security group vs NACL differences? Why does a NAT gateway bill surprise teams? How would PrivateLink help with a third-party API?

**M6-Q3. "Design the network for a three-tier web app on AWS for high availability and least exposure."** (Medium; `cloud.networking`; Big4, IT-services)
- Ideal: VPC with CIDR planning, at least two AZs; public subnets for ALB and NAT, private app subnets, isolated data subnets; security group chain (ALB to app to DB); WAF and CloudFront in front; endpoints for AWS services; TLS with ACM; Route 53 health-based routing; egress control; logging via flow logs; bastion replaced by SSM Session Manager; consider per-AZ NAT cost vs resilience trade-off.
- Weak: everything in public subnets; single AZ.
- Probes: What do you do if the CIDR overlaps with on-premises? How would you connect to on-premises (VPN, Direct Connect)? Where does KMS fit?

### M7 — Terraform

**M7-Q1. "How does Terraform state work and how do you manage it safely in a team?"** (Medium; `devops.iac`; global-IT-consulting, global-product)
- Ideal: state maps config to real resources and stores attributes and dependencies; remote backend (S3 with encryption, versioning and locking) so teams share one truth and avoid concurrent applies; sensitive data lands in state so restrict access; split state by domain/environment to limit blast radius; never edit by hand, use `state mv`, `import`, `moved` blocks; plan in CI, apply from pipeline with approval; handle drift via refresh and scheduled plans.
- Weak: "a file Terraform needs"; local state committed to Git.
- Probes: Two engineers apply simultaneously, what happens? How do you recover a corrupted or lost state? Why are secrets in state a concern?

**M7-Q2. "A plan shows `forces replacement` on a database. What do you do?"** (Hard; `devops.iac`; Big4, global-product)
- Ideal: stop and read the diff to find the attribute forcing replacement; do not apply blindly; check whether change is intended; options: revert config, use `lifecycle.ignore_changes` or `prevent_destroy` guards, make in-place alternative, `moved` for renames, snapshot before; schedule with migration plan; add policy checks in CI for destroy of stateful resources; explain `create_before_destroy` limits with unique names.
- Weak: "just apply, Terraform handles it".
- Probes: How do you rename a resource without recreating? How would you automate detection of destructive plans? What about provider upgrades causing diffs?

**M7-Q3. "Structure Terraform for dev, staging and prod across several services. Trade-offs of workspaces vs directories vs modules?"** (Hard; `devops.iac`; global-product, global-IT-consulting)
- Ideal: reusable versioned modules for patterns; per-environment root configs with separate state and backends; workspaces share config and can hide environment differences, riskier for prod isolation; separate accounts per environment with role assumption; variables and tfvars per environment; pinned provider and module versions; avoid deep module nesting; tooling (Terragrunt, Atlantis, HCP Terraform or alternatives; licensing: Terraform is BUSL 1.1 since Aug 2023, OpenTofu is MPL 2.0, Terragrunt is MIT, Atlantis stays Apache 2.0, verified 2026-10; never teach HCP Terraform prices, its legacy free plan ended 31 March 2026 and the free tier is capped at 500 managed resources); policy checks and cost estimation in PRs.
- Weak: copy-paste directories with no modules.
- Probes: How do you promote a change from dev to prod? How do you share outputs between stacks? Terraform vs CDK vs CloudFormation?

### M8 — CI/CD and deployment strategies

**M8-Q1. "Design a CI/CD pipeline for a Python microservice deployed to AWS."** (Medium; `devops.cicd`; IT-services, Indian-product, global-IT-consulting)
- Ideal: PR checks (lint, type check, unit tests, coverage, dependency and secret scanning) with caching and parallelism; build once, tag by commit SHA, push to registry, scan and sign; deploy to staging automatically, run integration/smoke tests, gated promotion to prod; CI authenticates with OIDC to a role (no long-lived keys); migrations step ordering; progressive rollout with health verification and automated rollback; notifications; metrics (lead time, failure rate); same artefact promoted across environments.
- Weak: one YAML that tests and deploys with static keys; rebuild per environment.
- Probes: How do you keep the pipeline fast? How do you handle a failing flaky test? How would this differ on Bitbucket Pipelines vs GitHub Actions?

**M8-Q2. "Blue/green vs canary vs rolling: when do you use each, and what are the hidden costs?"** (Hard; `devops.deploy`; global-product, Big4)
- Ideal: rolling: simple, mixed versions coexist, slow rollback; blue/green: two full environments, instant switch and rollback, double capacity, session and data compatibility; canary: gradual traffic shift with metrics comparison, needs good SLIs, traffic routing support, longer rollout; shadow traffic for validation; feature flags decouple deploy from release; data layer must be backward compatible in all strategies (expand and contract); define abort criteria upfront; AWS tooling examples (ECS/CodeDeploy, Lambda aliases with weighted routing, ALB weighted target groups, Argo Rollouts concept).
- Weak: definitions only, no data or metrics discussion.
- Probes: How do you decide the canary is healthy? What if the new version writes data the old version cannot read? How would you roll back a Lambda?

**M8-Q3. "Design the pipeline stages that ship a breaking database change with zero downtime."** (Hard; `devops.deploy`; Indian-product, global-product) (expand-contract SQL, locks and online DDL are owned by A4.M8-Q2; `ref`)
- Ideal: pipeline-level sequencing only: migration job as a separate, gated stage that runs before the code deploy and is rehearsed on production-sized data in a pre-production stage; compatible old and new code versions during the rollout (blue-green or canary stage); feature flag stage; pause-and-verify gates (replication lag, error rate, SLO burn) between expand, deploy, backfill and contract stages; automated rollback at each stage; contract step as a later, separate pipeline run after a soak.
- Weak: "run migration at night with downtime page"; one pipeline stage doing everything.
- Probes: Where do approvals sit? What gates the contract stage? How does the pipeline stop if the backfill lags? (DDL details and rename handling: A4.M8.)

### M9 — Observability

**M9-Q1. "What is the difference between monitoring and observability, and how do logs, metrics and traces complement each other?"** (Medium; `ops.observability`; IT-services, global-IT-consulting, Big4)
- Ideal: monitoring checks known failure conditions; observability lets you ask new questions of system behaviour from its telemetry; metrics: cheap aggregates for trends and alerting; logs: detailed events for context; traces: request path across services for latency attribution; correlation via trace IDs in logs and exemplars linking metrics to traces; cost and cardinality trade-offs; an example investigation flow from alert to metric to trace to log.
- Weak: "three pillars" recited without a workflow.
- Probes: Which would you alert on? How do you correlate across services? What would you add first to a service with none?

**M9-Q2. "Define SLIs, SLOs and an error budget for an API, and design the alerting."** (Hard; `ops.slo`; global-product, Indian-product, AI-startup)
- Ideal: SLIs from user experience (availability as good/total requests, latency percentile under threshold, correctness/freshness where relevant); SLO target with window (for example 99.9 percent over 30 days) and its error budget in minutes or requests; choose targets from user need and cost, not 100 percent; multi-window multi-burn-rate alerts (fast burn pages, slow burn tickets); budget policy (freeze releases when exhausted); alert on symptoms, route by severity, runbook links; example numbers computed correctly (99.9 percent of 30 days is about 43 minutes).
- Weak: alert on CPU above 80 percent; "five nines" with no reasoning.
- Probes: Why percentiles not averages? How do you SLO an LLM-backed endpoint? How do you avoid alert fatigue?

**M9-Q3. "How does distributed tracing work, and how would you instrument a FastAPI plus Celery plus Postgres system with OpenTelemetry?"** (Hard; `ops.observability`; global-product, AI-startup)
- Ideal: trace is a tree of spans; trace and span IDs; context propagation via headers (W3C `traceparent`) and message metadata for queues; SDK in each service with exporter to Collector over OTLP; auto-instrumentation libraries for FastAPI, SQLAlchemy/psycopg, requests, Celery plus manual spans for business operations; resource attributes (service name, version, environment); sampling (head vs tail, keep errors and slow traces); Collector for batching, redaction and routing; log correlation; async context pitfalls (context lost across tasks or thread pools); avoid PII in attributes; overhead considerations.
- Weak: "install a library and traces appear".
- Probes: How does context cross the queue? Head vs tail sampling trade-off? How do you control telemetry cost?

### M10 — Reliability, incident response, cost and multi-cloud

**M10-Q1. "Production is down after a deploy 10 minutes ago. Walk me through your first 30 minutes."** (Medium; `ops.incident`; IT-services, Big4, global-product)
- Ideal: acknowledge and declare severity, assign incident commander and communicator; assess impact from SLIs and dashboards; check recent changes; mitigate first (rollback or flag off) before root cause; communicate status on a cadence to stakeholders/customers; preserve evidence (logs, timelines); verify recovery by metrics; schedule blameless postmortem with timeline, contributing factors, action items with owners; note decisions made under uncertainty.
- Weak: starts debugging code while users are down; no communication.
- Probes: Rollback is risky because of a schema change, now what? How do you run comms for a client-facing outage? What goes in the postmortem?

**M10-Q2. "A service depends on a flaky downstream. What infrastructure-level resilience do you put in place?"** (Hard; `ops.incident`; global-product, Indian-product) (retry, backoff and jitter mechanics are the X2 shared lesson owned by A5.M4 and Q-A5-M4-03; `ref`)
- Ideal: infrastructure and platform controls: timeouts and connection limits at the proxy or mesh layer, circuit breaking and outlier ejection at the load balancer or service mesh, bulkheads through separate queues, pools or task definitions, autoscaling limits so that scaling does not amplify load, load shedding at the edge, graceful degradation and fallbacks, SLO burn alerts, chaos and fault-injection game days; where retries live in the call chain is decided by the A5.M4 rules.
- Weak: "just add more instances"; retries at every layer.
- Probes: How does a retry storm show up in your dashboards? Which layer owns timeouts? How do you rehearse the failure?

**M10-Q3. "Our AWS bill doubled. How do you find out why, and how do you explain AWS, Azure and GCP equivalents when a client uses a different cloud?"** (Medium; `cloud.cost`, `cloud.multicloud`; Big4, global-IT-consulting, IT-services)
- Ideal: Cost Explorer or CUR grouped by service, account, tag; look for NAT and data transfer, idle or oversized instances, log ingestion, unbounded storage, Lambda concurrency growth, new resources; budgets and anomaly detection; remediation (rightsizing, lifecycle policies, endpoints, Savings Plans or Spot where suitable); tagging governance and showback. Multi-cloud: map by capability not by name (Lambda to Azure Functions or Cloud Functions/Cloud Run functions, ECS/Fargate to Azure Container Apps or Cloud Run, EKS to AKS or GKE, S3 to Blob or Cloud Storage, SQS/SNS to Service Bus or Pub/Sub, IAM roles to Entra ID with managed identities or GCP service accounts, CloudWatch to Azure Monitor or Cloud Monitoring, Secrets Manager to Key Vault or Secret Manager); honest about differences in networking and identity models; lock-in and portability trade-offs; do not claim expertise beyond experience.
- Weak: "turn things off" or claims all clouds are the same.
- Probes: How do you attribute cost per team or per feature? When is multi-cloud justified? What stays portable (containers, Terraform, OTel) and what does not?

### 4.1 Goal tags, senior-bar notes and staff-level probes (G-10, boss item 8)

Goal tag format `B:x A:x H:x` (B Senior Python Backend, A AI Engineer, H hybrid). Note column: mid / senior / staff signal. Labels `Mn-Qk` are seed labels: the importer assigns the canonical `Q-A7-Mn-nn` number and keeps the label as a legacy ID (data model section 3).

| ID | Goal tag | Mid / senior / staff |
|---|---|---|
| M1-Q1 | B:M A:S H:M | Lists multi-stage and non-root / adds cache order, digests, secret mounts, exec-form signals / adds CI build-time budget, base-image patch cadence and a team-wide template image |
| M1-Q2 | B:S A:S H:S | "Process with namespaces" / maps cgroup limit to Python behaviour and `docker stop` timing / adds isolation strength trade-offs (microVMs) and kernel attack surface |
| M1-Q3 | B:M A:S H:M | Raises the limit / shows 137 evidence and workers x RSS arithmetic / adds memory alerts before kill, worker recycling policy and a capacity model |
| M2-Q1 | B:S A:N H:S | `docker compose up` / healthcheck-gated start, migrations as one-shot service / adds onboarding time budget and when to leave Compose |
| M2-Q2 | B:S A:N H:S | Scan and fix all / triage by reachability and fix availability / adds VEX, exception expiry, patch SLAs and audit evidence |
| M2-Q3 | B:S A:N H:S | "Scan images" / signing, provenance, OIDC build / adds admission verification, SLSA target level and what signing does not prove |
| M3-Q1 | B:S A:N H:S | "Starts a container" / names controllers, scheduler, kubelet, EndpointSlice / adds failure at each hop and rollout budget |
| M3-Q2 | B:M A:N H:S | One `/health` / separate liveness and readiness, no dependency in liveness / adds shutdown sequencing, shared-dependency blip policy, slow model-loading starts |
| M3-Q3 | B:S A:N H:S | Restart or raise limits / `describe`, `--previous`, exit codes / adds percentile-based sizing, QoS and throttling trade-offs |
| M4-Q1 | B:M A:S H:M | "Lambda cheaper" / criteria-driven choice with limits / adds cost crossover numbers, team skill and migration path |
| M4-Q2 | B:M A:M H:M | Adds memory / separates cold start from downstream, concurrency x duration / adds RDS Proxy, reserved-concurrency blast radius, load-test plan |
| M4-Q3 | B:M A:N H:S | Confuses roles / roles, ALB, autoscaling, circuit breaker / adds least-privilege per role and deploy-failure behaviour |
| M5-Q1 | B:M A:S H:M | Queue plus Lambda / visibility timeout, DLQ, partial batch, idempotency / adds outbox, replay safety, per-key ordering without serialising |
| M5-Q2 | B:M A:S H:S | Definitions / selection by delivery, fan-out, replay / adds cost shape, quotas and when to hand off to a stream platform (A6.M4 Challenge) |
| M5-Q3 | B:S A:N H:S | "Multi-AZ gives replicas" / HA vs read scaling, DNS flip, app retries / adds failover test plan and replica-lag effects on correctness |
| M6-Q1 | B:S A:N H:S | Admin to unblock / default deny, explicit deny, policy types / adds PassRole risk, Access Analyzer loop, SCP and boundary design |
| M6-Q2 | B:S A:S H:S | Opens 0.0.0.0/0 / walks route, endpoint, SG, NACL, DNS / adds Reachability Analyzer, NAT cost and PrivateLink options |
| M6-Q3 | B:M A:N H:S | Single AZ public / tiers, SG chain, endpoints / adds NAT-per-AZ cost vs resilience, CIDR planning, on-prem links |
| M7-Q1 | B:S A:N H:N | "A file Terraform needs" / remote state, locking, blast radius / adds state split strategy, recovery and access control |
| M7-Q2 | B:S A:N H:N | Applies anyway / reads the diff and guards stateful resources / adds CI policy that blocks destroys of stateful types and a snapshot gate |
| M7-Q3 | B:S A:N H:N | Copy-paste dirs / modules, separate state and accounts / adds promotion flow, version pinning and licensing awareness |
| M8-Q1 | B:M A:N H:S | One YAML with keys / OIDC, build once, gated promotion / adds security gates policy, DORA metrics, pipeline speed budget |
| M8-Q2 | B:M A:N H:S | Definitions / hidden costs, abort criteria / adds metrics-judged canary design and data compatibility |
| M8-Q3 | B:M A:N H:S | Night downtime / staged pipeline with gates and compatible versions / adds soak-gated contract stage, lag-based pause and rollback at each stage |
| M9-Q1 | B:S A:S H:S | Recites pillars / alert-to-trace-to-log workflow / adds telemetry cost, cardinality budget, ownership |
| M9-Q2 | B:S A:S H:S | "CPU above 80" / SLI, SLO, budget arithmetic / adds multi-window burn rates, budget policy and SLOs for non-deterministic endpoints (pointer B5.M7) |
| M9-Q3 | B:S A:S H:S | "Install a library" / propagation, sampling, Collector / adds queue context propagation, tail-sampling cost, PII redaction |
| M10-Q1 | B:S A:N H:S | Debugs code first / mitigate first, roles, comms / adds schema-blocked rollback, stakeholder cadence, learning loop |
| M10-Q2 | B:S A:S H:S | "Add more instances" / timeouts, breaking and bulkheads at infrastructure level / adds scaling limits, shedding and game-day rehearsal |
| M10-Q3 | B:S A:N H:S | "Turn things off" / Cost Explorer by tag and service, NAT and transfer / adds unit economics, showback, honest multi-cloud mapping limits |

**Staff-level probes (two per module, appended to each module's verbal tree):**

| Module | Probe 1 | Probe 2 |
|---|---|---|
| M1 | How do you roll out a base-image CVE fix across 40 services without 40 pull requests? | When is a rootless or microVM runtime worth its cost? |
| M2 | Design a patch SLA and exception policy an auditor accepts but developers do not route around. | How do you detect a compromised third-party action or base image after the fact? |
| M3 | How would you decide between one large cluster and several, per environment and per team? | Your readiness checks all pass but p99 doubles during rollouts: find it. |
| M4 | A team wants EKS for three services: what questions and evidence do you ask for? | Model the Lambda vs Fargate crossover and name the assumptions that flip it. |
| M5 | Design DLQ ownership and replay governance so poison messages never become silent data loss. | An EventBridge rule change broke a downstream team: what guardrails prevent that? |
| M6 | Design org-level guardrails (SCPs, boundaries) that let teams self-serve IAM safely. | A p99 stall occurs only after a DNS change: what evidence would you collect and in what order? |
| M7 | How do you migrate 200 hand-built resources into Terraform without an outage? | Design the module versioning policy for 30 teams. |
| M8 | How do you reduce lead time from 3 days to 3 hours without raising change failure rate? | Define the exception process for a failing security gate during an incident fix. |
| M9 | Your telemetry bill doubled: what do you cut and what must you protect? | Define SLOs for a service with three tiers of customer. |
| M10 | Run a game day for a region loss: scope, safety, success criteria. | Present a cost reduction plan to a CFO without harming reliability. |

## 5. Final assessment

| Component | Description |
|---|---|
| Mastery check | 30 items drawn across all modules (mix of MCQ, predict-the-result, config-fix), 80 percent pass, weak-module revision loop |
| Timed hands-on task (60–75 min) | Given a failing Python service stack: fix the Dockerfile and a broken Kubernetes manifest, repair a pipeline YAML, correct an over-broad IAM policy and a Terraform plan hazard; hidden tests validate each artefact |
| Observability task | Instrument a supplied service with OpenTelemetry, define an SLO and a burn-rate alert, then locate the slow span in a provided trace |
| System-design canvas task | "Design a deployable, observable, cost-aware platform for an event-driven AI voice or LLM gateway service on AWS": canvas must show compute choice, queue and DLQ, data stores, VPC and IAM boundaries, CI/CD and rollout path, telemetry and SLOs, DR targets, cost drivers; auto-evaluation checks against A6 registry IDs only: R-SPOF-1 (single points of failure), R-QUEUE-2 (missing DLQ or retry limit), R-IDEMP-1 (no idempotency), R-AUTH-1 (no auth path), R-OBS-1 (no observability), plus rubric scoring (overall 3.5 or higher, no dimension below 3, no critical rule findings; G-7) |
| Voice mock round (20–25 min) | Interviewer persona runs three of the seed questions with probes, then an incident-triage scenario; scored on rubric for competency, structure and trade-off articulation with transcript evidence; strict mode |
| Certificate | Awarded on passing all components; evidence portfolio attached |

## 6. Course-level risks and freshness notes

Verified or restated as of **October 2026** (official or primary sources via web search on 2026-10-05; items still needing a human or review-date check remain **[verify]**):
- **Ingress-nginx:** community controller reached end-of-life 24 March 2026, repository read-only, no further CVE patches; ingress2gateway 1.0 released 20 March 2026 (Kubernetes SIG-Network blog). The Ingress API itself remains. Verified.
- **Lambda SnapStart:** supported for Python 3.12 and later; extended to container-image functions in 2026 (AWS What's New, July 2026). Exact runtime list, Python 3.15 runtime date and exclusions (provisioned concurrency, EFS) must be re-read from the AWS docs at each review (AWS docs, re-check per review). Init-phase billing since 1 August 2025 and Python 3.14 managed runtime as stated in M4. Quotas (duration, payload) are never hard-coded.
- **Terraform:** S3 native locking with `use_lockfile` (experimental 1.10, GA 1.11) and deprecated DynamoDB arguments. Verified against HashiCorp S3 backend docs. HCP Terraform, Terragrunt and Atlantis licensing: Terraform BUSL 1.1, Terragrunt MIT, Atlantis Apache 2.0 (verified 2026-10, project licence pages); HCP Terraform pricing is volatile and never taught.
- **OpenTelemetry:** traces, metrics and logs stable in the specification; Python logs SDK is still in development (stabilisation PRs open, verified 2026-10) and profiles entered public alpha in March 2026 (opentelemetry.io blog). X-Ray SDK maintenance dates and App Runner status as stated in the module table.
- **Other fast-changing items (3-month review):** Kubernetes minor and API deprecations (current minus one, G-8); AWS service launches and renames (ElastiCache Valkey, RDS Multi-AZ DB cluster options); GitHub Actions and Bitbucket Pipelines features and pricing; scanner tool names, SLSA definitions (slsa.dev v1.0); Azure and GCP service names; Firecracker and gVisor wording (AWS documents Firecracker microVMs for Lambda; Fargate wording: AWS docs state each task has its own isolation boundary and does not share a kernel, **[verify: whether current AWS docs name Firecracker for Fargate]**). Never hard-code prices; use the calculator with an "as of" stamp.
- **Stable content (12-month review):** container fundamentals, IAM evaluation logic, VPC and TCP/TLS/DNS fundamentals, delivery semantics, SLO maths, incident practice.
- **Emulation risk (R-A7-2, S-1):** no live cluster, Docker daemon or cloud. Emulators diverge from real behaviour (IAM enforcement, limits, timing, networking); every lab states what it does not model. LocalStack ended its Community edition on 23 March 2026: one unified image now needs an account and auth token (free Hobby plan is non-commercial), verified 2026-10 from LocalStack's blog and secondary coverage; **[verify: exact licence text under spike S-1]**; the moto-style in-process emulator is preferred.
- **Overlap risk:** A5, A6, A8, A2, A1, B5, B6 boundaries are enforced by the ownership table in section 3.
- **Multi-cloud depth risk:** Azure and GCP content is table-level only; interview answers must not claim hands-on depth.
- **Learner-fit risk:** over-investing in Kubernetes for roles that mostly use Lambda/ECS; M3 is weighted by the JD analyser rule in section 3.
- **No fabricated figures:** all numbers in tasks come from supplied datasets or SLO arithmetic shown; no hiring-outcome claims.

## 7. Readiness criterion (G-6 specialised)

A learner is "interview-ready" on an A7 tag (`cloud.containers`, `cloud.k8s`, `cloud.aws-compute`, `cloud.aws-messaging`, `cloud.aws-data`, `cloud.aws-security`, `cloud.networking`, `devops.iac`, `devops.cicd`, `devops.deploy`, `ops.observability`, `ops.slo`, `ops.incident`, `ops.linux-triage`, `cloud.cost`, `cloud.multicloud`) when: module mastery check at or above 85 percent on a retake at least 7 days after first pass; the module's timed hands-on task passed; strict-mode voice drill at the Senior bar (average 3.5 or above, no competency below 3) in two sessions at least 48 hours apart, including one incident or triage scenario; no open remediation flag on prerequisites. The canvas task needs rubric 3.5 or above with no critical rule findings (registry rule IDs only). Badge ("Meets") thresholds remain lower (G-7).

## 8. Learner-Fit Appendix

Reference learner: senior engineer, 11+ years, about 4 years Python, daily Docker, Redis, Bitbucket Pipelines, AWS Lambda and S3, FastAPI/Django/Flask, SQL, LLM APIs and voice agents, targeting Senior Python Backend and AI Engineer roles at IT-services, Big 4 and product companies. Rules apply to any learner. Bands from each module's placement items (G-5): **>= 85 skip lessons and take the mastery check; 70-84 skim; 50-69 study; < 50 deep-study**. No band waives the course final assessment or verbal seed questions.

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 (new to containers and cloud) | Simpler, 24 h plus 3 h assessment; M1 to M10 in order, prerequisites (Linux shell, HTTP) linked; M3, M7, M9 core lessons first pass | 70 percent guided, 30 percent unguided | none | none until M1, M4, M6 pass |
| L2 (junior-mid, uses Docker or one cloud with guidance) | Simpler, about 20 h; skim known M1 and M4.1 lessons | 50/50 guided and unguided | known M1, M4.1, M5.1 lessons | one Challenge per passed module (M3.6 allowed) |
| L3 (experienced, cannot always explain) | Deeper, about 12-14 h; skim M1, M2.1, M8.2; study "explain at depth" lessons in M3-M7, M9 | 70 percent unguided, timed configs, simulator triage | banded lessons | SCP design, tail-sampling cost, state-surgery case |
| L4 (senior, owns it in production; reference learner) | Deeper, about 11-13 h expected at full weight after skip (M1 skipped, M4.1, M5.1, M8.2 skipped; M2 skimmed; M3, M6, M7, M9, M10 studied); a hybrid route applies the 0.6 SHOULD multiplier on top (G-3; Learner Journey baseline 10.8 h, expected 7.5 h) `[judgement]`; mastery checks, then senior-bar probes and voice drills | 80 percent timed and voice; labs only where the check misses | M1 lessons, S3, Lambda basics, Bitbucket | staff probes (section 4.1), postmortem write-up, M3.6 Challenge |

### LF-2 Module matrix

Diagnostics are `Q-A7-Mn-D1..Dk` (placement-tagged items from the MCQ and predict buckets). Weight is percent of cloud/DevOps interview time per target (B Backend, A AI Engineer, H hybrid; each column sums to 100); rationale: backend and services/Big 4 rounds probe compute choice, messaging, IAM and incidents most; AI-engineer rounds lean on deployability, observability and cost `[judgement]`. Tags describe route inclusion (G-3), weights describe interview content. Course-level: B:S A:N H:S.

| Module | Diagnostics (count: topics) | Tags | Weight B / A / H | Rehearsal modes | Module readiness |
|---|---|---|---|---|---|
| M1 | 6: layer order and cache bust, secret in earlier layer, exec vs shell CMD, OOM exit 137, Alpine wheels, non-root volume ownership | B:S A:S H:S | 8 / 10 / 9 | voice, coding | check 85, hardened image tests pass (skip band expected) |
| M2 | 6: `depends_on` readiness, CVE reachability, tag mutability, SBOM vs VEX, SLSA level meaning, signing limits | B:S A:N H:N | 5 / 5 / 5 | voice, design canvas | check 85, scan-triage decision at rubric 3.5 |
| M3 | 8: probe purpose, maxSurge arithmetic, requests vs limits, QoS, CPU throttle symptom, HPA prerequisite, Ingress vs controller, Secret not encrypted | B:S A:N H:S | 12 / 6 / 9 | voice, coding (manifest fixes), design canvas | check 85, 3 broken manifests fixed, voice Senior bar |
| M4 | 7: concurrency = rate x duration, init vs handler, reserved vs provisioned, task vs execution role, API Gateway type, SnapStart fit, Fargate vs Lambda crossover | B:M A:S H:M | 14 / 14 / 14 | voice, design canvas, coding | check 85, Lambda tune task, compute memo at 3.5 |
| M5 | 8: visibility timeout, DLQ redrive, partial batch response, FIFO dedup scope, SNS to SQS fan-out, Multi-AZ vs replica, RDS Proxy purpose, S3 presigned flow | B:M A:S H:S | 12 / 9 / 11 | voice, coding, design canvas | check 85, consumer idempotency tests pass |
| M6 | 8: explicit deny, trust vs permission policy, SG vs NACL, gateway endpoint vs NAT, `PassRole`, ephemeral ports, DNS TTL in pools, fd exhaustion signature | B:S A:N H:S | 12 / 8 / 10 | voice, coding (triage), design canvas | check 85, 2 connectivity and 2 hung-service diagnoses correct |
| M7 | 6: state purpose, `count` shift, `moved` vs `state mv`, `use_lockfile`, secrets in state, `forces replacement` | B:S A:N H:N | 8 / 5 / 7 | voice, coding, project deep-dive | check 85, plan review finds all destructive changes |
| M8 | 7: OIDC vs static keys, build once promote, canary abort criteria, blue/green cost, expand and contract order, pinned actions by SHA, gate exception policy | B:M A:N H:S | 11 / 8 / 10 | voice, design canvas, behavioural | check 85, pipeline lint plus rollout plan at 3.5 |
| M9 | 8: percentile vs average, cardinality, W3C `traceparent`, head vs tail sampling, error budget arithmetic, burn-rate windows, symptom alerting, context loss across async | B:S A:S H:S | 10 / 20 / 15 | voice, coding, project deep-dive | check 85, SLO doc plus alert passes outage and no-outage tests |
| M10 | 6: mitigate before root cause, RPO vs RTO, jitter, retry storm, NAT and transfer cost, restore testing | B:S A:N H:S | 8 / 15 / 10 | voice, behavioural, design canvas | check 85, incident simulation timeline and postmortem at 3.5 |

### LF-3 Experience bridge ("you already know" -> gap to senior depth)

| Module | You already know | Gap between using it and explaining it at senior depth |
|---|---|---|
| M1 | Docker daily, writing Dockerfiles for FastAPI/Django | cache-order proofs, secrets in layers, PID 1 and signals, cgroup-aware workers |
| M2 | Compose for local dev, Bitbucket builds | healthcheck-ordered startup, CVE reachability triage, SBOM, signing, provenance |
| M3 | seen Deployments in team repos, maybe `kubectl logs` | probes, QoS, rolling-update arithmetic, debug flow, Gateway API after ingress-nginx retirement |
| M4 | Lambda handlers, API Gateway triggers | concurrency maths, RDS connection exhaustion, ECS/Fargate roles, honest Lambda vs Fargate vs EKS cost reasoning |
| M5 | S3 uploads, Redis, maybe SQS consumers | visibility timeout and DLQ design, SNS/EventBridge semantics, Multi-AZ vs replica, failover behaviour in the app |
| M6 | IAM roles attached to Lambda, security groups copied from examples | policy evaluation, trust vs permission, VPC routes and endpoints, TCP/TLS/DNS and `ss`/`strace` evidence |
| M7 | read Terraform in a repo, maybe CloudFormation | state, locking, `count` vs `for_each`, plan danger reading, environment layout |
| M8 | Bitbucket Pipelines steps and deployments | OIDC, GitHub Actions, security gate policy, canary and blue/green with data compatibility, expand and contract |
| M9 | logs in CloudWatch, perhaps Sentry | percentiles, cardinality, OTel propagation, SLO and burn-rate maths, symptom alerts |
| M10 | on-call participation, AWS bill noticed | mitigate-first command, postmortem craft, restore proof, unit economics, honest multi-cloud mapping |

### LF-4 Senior-bar framing

Per-question mid / senior / staff notes are in section 4.1. Course-wide: mid answers name services and features; senior answers state failure modes, limits, a measurement and a rollback; staff answers add blast radius, org-level guardrails, cost, and how the design is operated and taught across teams.

### LF-5 Adaptive hooks

| Item | Rule |
|---|---|
| Mastery thresholds | module check 80 percent (badge); 85 percent on a retake after at least 7 days (readiness) |
| Remediation links | M2 -> M1; M3 -> M1; M4 -> M1 and A2.M9 (workers); M5 -> M4 and A6.M4 (semantics); M6 -> M4; M7 -> M6; M8 -> M2, M6, M7 and A4.M8 (migrations); M9 -> M4 and A1.M8 (logging mechanics); M10 -> M9 and A5.M4 (retries) |
| Challenge extensions | M3.6 operators, mesh, CNI awareness; M5 per-key ordering design; M6 SCP and boundary design; M7 state split plan; M9 tail-sampling cost model; M10 game-day design |
| Engine may reorder or defer | M3 weight by JD analyser; M7 and M2 deferrable on A-route (A:N); M9 and M10 may run before M3 for AI-engineer routes; M1 -> M3 and M4 -> M5 order fixed; M6 before M7 and M8 |
| Route multipliers | B route: M1-M10 at 1.0 where tag M, 0.6 where S (core lessons chosen by diagnostic); A route: S modules at 0.6, N modules at 0.25 (primer lessons only: M2.2, M3.1, M6.4, M7.1, M8.1, M10.3); H route: S 0.6 |

### LF-6 Applied labs (production-framed, one per module)

| Module | Lab |
|---|---|
| M1 | Shrink and harden the image of an MQTT-to-HTTP IoT ingestion service (SIMULATOR) |
| M2 | Triage a scan report for a SaaS API image and decide the gate policy (DESIGN) |
| M3 | Deploy a FastAPI AI-assistant backend with probes, HPA and a failing rollout (VALIDATOR plus scripted kubectl) |
| M4 | Tune a voice-agent webhook Lambda and choose Lambda vs Fargate for the session service (EMULATOR plus calculator) |
| M5 | Reliable notification pipeline: SQS, Lambda consumer, partial batch, DLQ redrive, A5.M8 idempotency reused (EMULATOR) |
| M6 | Diagnose a stalled enterprise integration service from `ss`, `top`, `strace` captures plus a private-subnet connectivity fault (SIMULATOR) |
| M7 | Terraform module for the notification pipeline with a destructive-plan review (STATIC PLAN) |
| M8 | Bitbucket-to-GitHub translation with OIDC, security gates and a canary abort plan for a SaaS service (LINTER) |
| M9 | Instrument an LLM-gateway-style FastAPI service for traces and RED metrics, define an SLO and burn-rate alert (IN-PROCESS COLLECTOR; GenAI specifics point to B5.M7) |
| M10 | Bad-deploy incident simulation for a client-facing enterprise API plus a cost review of its AWS bill (SCRIPTED) |

### LF-7 Currency and quality bar

| Module | Stamp | Memorable visual or analogy | Top 3 misconceptions handled |
|---|---|---|---|
| M1 | 2026-10 | Stack of transparent sheets (layers) with a cache line that breaks at the first changed sheet | container is a lightweight VM; secret removed in a later layer is gone; Alpine is always better |
| M2 | 2026-10 | Food supply chain with sealed, labelled packages | `depends_on` means ready; all CVEs are equal; scan once at build is enough |
| M3 | 2026-10 | Thermostat control loop (desired vs actual) | DB check in liveness is safe; Secrets are encrypted; Ingress was removed |
| M4 | 2026-10 | Restaurant: food truck (Lambda), own kitchen (Fargate), hotel banquet hall (EKS) | Lambda scales infinitely; one role is fine; cold start only hits Java |
| M5 | 2026-10 | Post office with a redelivery slip (visibility timeout) and dead-letter shelf | SQS is exactly-once; Multi-AZ is a read replica; retry forever is safe |
| M6 | 2026-10 | Building with badge reader (IAM), floor plan (VPC) and a doctor's stethoscope (`ss`) | wildcard policy is fine temporarily; SG equals NACL; "network is slow" without evidence |
| M7 | 2026-10 | Architect's blueprint vs a surveyor's register of what exists (config vs state) | state is just a cache; `count` is fine for lists; apply from a laptop is OK |
| M8 | 2026-10 | Motorway lane switch with a spotter (canary) and a reversible on-ramp | blue/green is free; rollback ignores schema; static keys in CI are fine |
| M9 | 2026-10 | Hospital: vitals chart (metrics), patient notes (logs), route through wards (traces) | averages are enough; label by user ID; monitoring equals observability |
| M10 | 2026-10 | Fire drill with a single incident commander | fix root cause before mitigating; multi-cloud means resilience; DR plan untested is a plan |

## 9. Open items for boss

1. Spike S-1 (moto-style vs LocalStack) must close before M4 and M5 labs are authored; confirm default to moto-style if the licensing review is not finished by that date.
2. Resolved (Track A lead review): the A6.M4 Challenge is platform-neutral (partitions, consumer groups, offsets, rebalances, compaction, exactly-once scope) and names no AWS service, so A7.M5 owns the Kinesis vs SQS vs MSK selection criteria at overview level (O-23) and points to A6.M4 only for the concepts; no A6 change needed.
3. Confirm the JD analyser output format (Kubernetes required / nice-to-have / absent) available to the placement engine at 1.0 launch for the M3 weighting rule.

## 10. Boss compliance log

| Decision | Applied |
|---|---|
| G-1, O-23, O-24, O-25, O-26, O-27, O-36, O-51 to O-56, O-110 | Ownership notes table in section 3; pointers to A1, A2, A4, A5, A6, A8, B2, B4, B5, B6 one-way and capped |
| G-2 | Header 24 S / 15 D plus 3.0 h assessment; module hours sum exactly; S/D semantics sentence; M3.6 Challenge outside totals |
| G-3, G-11 | Learner-Fit Appendix LF-1 to LF-7; B/A/H tags per module (LF-2) and per seed question (4.1) |
| G-4, G-5, G-6, G-7 | Level paths, bands, readiness (section 7), thresholds 80 module, 85 readiness, Senior bar voice |
| G-8 | Kubernetes, Terraform, OTel at current minus one and stable signals only; no model names; Python 3.14 context |
| G-9, G-10 | Hyphenated tags with one owner; `ops.linux-triage` is registered (A7-owned); IDs `Q-A7-Mn-nn`; goal tags, senior-bar notes, 2 staff probes per module |
| G-12 | Dated October 2026, no invented statistics, `[judgement]` marked |
| R-A7-1 | Optional M3.6 Challenge lesson up to 2 h, awareness only |
| R-A7-2 | P-11 simulators, S-1 decision pending, "not modelled" per lab; no kind-like cluster or local LocalStack text |
| R-A7-3 | M9.5 20-minute pointer to B5.M7, B6, B4.M8; M9 L5 depth removed |
| R-A7-4 | Terraform sole IaC; table-level comparison inside M7.5; OpenTofu and S3 locking resolved |
| R-A7-5 | M10.4 owns cost levers; inputs feed A6.M5 cost rubric card; LLM token cost points to B2.M9 |
| R-A8-2 | M8.1 owns pipeline security gates; pointer to A8.M8; M2 owns container supply chain |
| Section 8 items 1-12 | Items 1-12 applied: bands and tags, simulator text, M9 pointer, M10 FinOps, M8 gates, M2/M6 ownership, M3 Challenge, Terraform, verification (ingress-nginx, SnapStart, locking verified; others restated), experience bridge, M6 networking and Linux triage lesson with offset, JD weighting rule |

- Track A lead review (2026-10-05, TRACK-LEAD-A): full review; stale tag note removed, SQS visibility-timeout guidance made precise, L4 hours restated, seed-label mapping and difficulty mapping stated, open item 2 resolved. Verdict: Approve with conditions (S-1 emulator spike, remaining [verify] items). See _reviews/TrackLead_A_Report.md.
