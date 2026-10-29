# Skillora Learning — Master Product & Build Prompt

> **Document family:** behavior is governed by `Skillora_Learning_Master_Specification_v2.md` (wins on conflict). This file provides curriculum depth, data model, tutor behavior and phases. Diagrams, flow charts and level-adaptive visuals are defined in `Skillora_Learning_Visual_Learning_System.md` and are **required** for every lesson (see §39 and §39A). Web/Mobile clients: v2 prompts.

> **Scope note:** the topic lists in this file (sections 9–19) describe the **full long-term curriculum**. They are **seed content for the Admin knowledge base**, not code or prompt requirements to implement all at once. Build order and MVP content scope are defined in `Skillora_Learning_MVP_Roadmap_and_Delivery_Plan.md` (start with 2–3 flagship tracks). India relevance, localisation and compliance: `Skillora_Learning_India_Market_and_Compliance_Specification.md`. Non-functional and operational requirements: `Skillora_Learning_Engineering_Quality_and_Operations_Specification.md`.

> **SaaS-ready:** the product launches fully free but is designed as a SaaS (multi-tenant, entitlement-based gating, billing adapters, paid courses/bundles/programs/cohorts, premium Resume Studio and Interview module later). See `Skillora_Learning_SaaS_Monetization_Architecture.md` and `Skillora_Learning_Resume_Studio_Specification.md`.

> **Admin-managed:** all content, knowledge, master data and configuration described in this file is created and maintained through the web Admin Portal (`Skillora_Learning_Admin_Portal_Specification.md`), not in code.

> **Release 1.0 curriculum mapping:** for 1.0 the seed curriculum below is delivered as **Track A (Senior Python Backend, 8 courses), Track B (AI Engineer, 7 courses), Track C (Interview Craft, 4 courses)** defined in `../../05_Release_1_0/01_Scope/Skillora_Learning_Release_1_0_Web_Scope_and_Founder_Curriculum.md` §3 with syllabi in `05_Release_1_0/03_Curriculum_Syllabi/`. 1.0 is **web only**; mobile and Resume Studio are 1.1+. **Billing Lite (time-boxed passes, voice credit packs, GST invoices, refunds) and a 7-day free trial (+7 on activation) are in 1.0** (`../../05_Release_1_0/01_Scope/Skillora_Learning_Release_1_0_Billing_Lite_and_Trial_Specification.md`); recurring billing and subscriptions are later. Other branches in this file stay as Admin seed content for later releases.

## 1. SYSTEM ROLE

You are the principal architect, product strategist, senior full-stack engineer, AI/LLM engineer, curriculum architect, interview expert, DevOps engineer, cloud architect, security engineer, QA engineer, UX designer, and technical writer responsible for designing and building **Skillora Learning**.

Treat this document as the **master product specification and reusable build prompt**.

You may be asked to design the product, create architecture, generate code, create database schemas, design APIs, build UI, implement learning engines, implement coding execution, implement interview simulation, create curriculum, import knowledge, write tests, deploy the system, review an existing implementation, or extend the platform.

Do not assume the initial implementation is the final product. Skillora Learning must be designed as an **extensible technical learning and career-intelligence platform** whose knowledge, curriculum, users, technologies, and capabilities can grow indefinitely.

---

# 2. PRODUCT IDENTITY

**Product:** Skillora Learning

**Preferred domain:** skilloralearning.com

**Category:** Personalized Technical Learning + Practice + Knowledge + Interview + Career Intelligence Platform.

**Vision:**

> **Learn → Understand → Practice → Build → Assess → Interview → Improve → Master → Continuously Evolve**

Skillora Learning must be substantially more than a static online course. It combines:

1. Structured learning academy.
2. Continuously extensible technical knowledge base.
3. Personalized learner profiles.
4. Coding practice environment.
5. Project and engineering labs.
6. Adaptive assessments.
7. JD-aware interview preparation.
8. Realistic mock interviews.
9. Resume/CV intelligence.
10. Career and skill-gap analysis.
11. Knowledge graph connecting technologies and concepts.
12. Extensible AI tutor/mentor.
13. DevOps and Cloud learning.
14. GenAI and Multi-Agent learning.
15. Long-term mastery tracking.

---

# 3. FUNDAMENTAL PRINCIPLES

## 3.1 Extensibility First

The initial curriculum is **not the limit of the platform**.

Support adding:

- branches;
- sub-branches;
- technologies;
- frameworks;
- cloud providers;
- tools;
- programming languages;
- versions;
- interview domains;
- projects;
- coding challenges;
- assessment types;
- knowledge sources;
- career tracks.

Adding new content must not require rebuilding the architecture.

## 3.2 Knowledge Graph, Not Static Folders

Represent knowledge conceptually as a connected graph.

Example:

```text
Python
 ├── AsyncIO
 │    └── FastAPI
 │          └── Docker
 │                └── Kubernetes
 │                      └── AWS EKS
 │
 ├── PostgreSQL
 │      └── Database Design
 │             └── System Design
 │
 └── GenAI
       ├── RAG
       ├── Agents
       └── Multi-Agent Systems
```

Every knowledge item should support:

- prerequisites;
- related concepts;
- dependent concepts;
- interview relevance;
- coding relevance;
- production relevance;
- career relevance;
- difficulty;
- version;
- source/provenance.

## 3.3 Profile-Aware Intelligence

The same question must be taught and evaluated differently according to:

- experience;
- existing skills;
- target role;
- target seniority;
- previous answers;
- demonstrated mastery;
- career objectives;
- job description;
- resume claims.

## 3.4 Evidence Over Assumption

Never invent learner experience.

Resume-derived information must be marked extracted/unverified until confirmed by the learner.

Technical knowledge should distinguish:

- verified source material;
- official documentation;
- imported user material;
- established knowledge;
- model inference;
- uncertain information.

## 3.5 Production Quality

Use production-quality thinking for:

- security;
- privacy;
- authentication;
- authorization;
- observability;
- testing;
- data integrity;
- scalability;
- reliability;
- cost;
- maintainability;
- extensibility.

---

# 4. TARGET USERS

Support independent user profiles for:

### Beginner Developer
Fundamentals, guided exercises, simple explanations, prerequisites, gradual difficulty.

### Mid-Level Developer
Deep concepts, production patterns, coding, architecture, interviews.

### Senior Engineer
Advanced concepts, trade-offs, production architecture, system design, senior interviews, leadership/stakeholder scenarios.

### Career Switcher
Gap analysis, prerequisite mapping, accelerated route, targeted projects, interview preparation.

---

# 5. PROFILE ENGINE

Each user must have an independent profile.

## Profile Data

Support:

- name;
- education;
- years of experience;
- job history;
- companies;
- roles;
- responsibilities;
- technical skills;
- frameworks;
- databases;
- cloud technologies;
- DevOps tools;
- AI/GenAI experience;
- projects;
- certifications;
- resume/CV;
- career goals;
- target roles;
- target seniority;
- target industries;
- target companies;
- preferred learning pace;
- preferred explanation depth.

## Learning State

Track:

- completed modules;
- current module;
- mastery score;
- assessment scores;
- coding performance;
- repeated mistakes;
- weak concepts;
- strong concepts;
- revision history;
- interview performance;
- mock interview history;
- forgotten concepts;
- confidence;
- last practiced date.

## Profile Isolation

Personal information must remain isolated between users.

Shared knowledge may be reused.

Private resume, answers, scores, interview history, personal experience, goals, and preferences must remain profile-specific.

---

# 5A. COMMERCE, SaaS & PREMIUM-READY FEATURES

- **Launch:** every user gets all features free (Launch — Free Access plan). Usage is metered so pricing can be informed by real data.
- **Premium later (admin-configurable):** Resume Studio (AI job-ready resume generation, JD tailoring, cover letters, ATS check, export), Interview Master (mock interviews, voice, JD-driven prep, last-minute mode, system-design interviews), advanced analytics, offline packs, project reviews, certificates.
- **Courses become paid offerings later:** single course, bundle (any combination), learning path/program, role-based track, all-access subscription, cohort programme, add-ons, credit packs, vouchers; with self-paced, drip, cohort, live and hybrid schedules. Course content and sellable offerings are separate layers.
- All gating through a central entitlement service with quotas; all AI calls metered through an AI Gateway; billing through replaceable adapters; multi-tenant from day one.

# 6. RESUME/CV INGESTION

Allow upload of:

- PDF;
- DOCX;
- TXT;
- Markdown;
- other supported document formats.

Extract:

- experience;
- skills;
- projects;
- technologies;
- responsibilities;
- achievements;
- education;
- certifications.

Do not silently trust extracted information.

Show:

**Detected Information → Confirm / Edit / Delete / Add**

Only confirmed information should be treated as verified profile data.

## Resume Claim Risk Engine

Detect claims that may generate interview questions.

Example:

> “Designed scalable microservices architecture.”

Generate questions such as:

- Why microservices?
- What services did you create?
- How did they communicate?
- How did you handle failures?
- How did you scale?
- How did you monitor?
- Why not a monolith?
- How did you handle distributed transactions?

Assign interview risk and preparation priority.

---

# 7. TARGET JOB / JD ENGINE

Allow the learner to provide a job description by:

- pasted text;
- uploaded document;
- structured requirements.

Extract:

- role;
- seniority;
- required technologies;
- preferred technologies;
- responsibilities;
- architecture expectations;
- coding expectations;
- cloud requirements;
- AI requirements;
- leadership requirements;
- behavioral requirements.

Then calculate:

```text
JD Requirement
      ↓
Candidate Profile
      ↓
Current Knowledge
      ↓
Gap Analysis
      ↓
Priority
      ↓
Preparation Route
```

Generate:

1. Must-know topics.
2. High-priority revision.
3. Missing concepts.
4. Coding challenges.
5. System-design questions.
6. Role-specific technical questions.
7. Resume-based questions.
8. Behavioral questions.
9. Mock interview plan.
10. Last-minute revision plan.

---

# 8. COURSE ENGINE

Normal learning flow:

```text
Module 1
   ↓
Lessons (concept + visual)
   ↓
Examples
   ↓
Practice
   ↓
Coding Challenge
   ↓
Assessment
   ↓
Mastery Check ──(not yet)──→ Targeted Revision ──→ back to Practice
   ↓ (pass)
Module 2
   ↓
...
```

The system may intelligently skip material when the learner demonstrates sufficient mastery.

Learner level (Beginner / Developing / Professional / Senior) is tracked **per branch** and drives explanation depth and visual style; see the Visual Learning System.

Learners may request:

- revision;
- prerequisite lesson;
- extra challenge;
- deeper explanation;
- accelerated path.

---

# 9. CORE PYTHON ENGINEERING

Cover:

## Foundations
- syntax;
- variables;
- data types;
- operators;
- control flow;
- functions;
- modules;
- packages;
- exceptions;
- file handling;
- comprehensions;
- iterators;
- generators.

## Intermediate Python
- decorators;
- closures;
- context managers;
- generators;
- iterators;
- dataclasses;
- typing;
- type hints;
- unpacking;
- `*args`;
- `**kwargs`;
- LEGB;
- mutable/immutable behavior;
- shallow/deep copy;
- memory concepts.

## Advanced Python
- object model;
- descriptors;
- metaclasses;
- MRO;
- magic methods;
- concurrency;
- AsyncIO;
- threading;
- multiprocessing;
- GIL;
- performance;
- profiling;
- memory management.

## Engineering Practices
- virtual environments;
- dependency management;
- packaging;
- logging;
- configuration;
- testing;
- linting;
- formatting;
- documentation.

---

# 10. OOP + SOLID

Cover:

- encapsulation;
- abstraction;
- inheritance;
- polymorphism;
- composition;
- aggregation;
- interfaces/protocols;
- dependency injection;
- design principles.

## SOLID

- Single Responsibility;
- Open/Closed;
- Liskov Substitution;
- Interface Segregation;
- Dependency Inversion.

Include Python examples, anti-patterns, refactoring exercises, interview questions, and senior-level architecture examples.

---

# 11. DATA STRUCTURES & ALGORITHMS

Cover:

- arrays/lists;
- strings;
- dictionaries/hash tables;
- sets;
- tuples;
- stacks;
- queues;
- deque;
- linked lists;
- trees;
- BST;
- heaps;
- priority queues;
- graphs;
- tries;
- recursion;
- backtracking.

Algorithms:

- searching;
- sorting;
- binary search;
- two pointers;
- sliding window;
- prefix sums;
- hashing;
- recursion;
- backtracking;
- greedy;
- dynamic programming;
- BFS;
- DFS;
- shortest path;
- topological sort;
- union-find.

## Coding Interview Requirement

The learner must be able to solve common interview problems in **plain Python without IDE autocomplete or special assistance**.

For major topics include:

1. Concept.
2. Pattern recognition.
3. Brute force.
4. Optimized approach.
5. Complexity.
6. Manual coding challenge.
7. Hidden tests.
8. Debugging challenge.
9. Follow-up variations.
10. Interview explanation practice.

---

# 12. BACKEND ENGINEERING

Cover:

- HTTP;
- HTTPS;
- REST;
- API design;
- status codes;
- headers;
- authentication;
- authorization;
- JWT;
- OAuth concepts;
- sessions;
- cookies;
- rate limiting;
- pagination;
- filtering;
- sorting;
- validation;
- serialization;
- caching;
- queues;
- background jobs;
- webhooks;
- idempotency;
- retries;
- resilience;
- microservices;
- event-driven architecture.

---

# 13. FRAMEWORK BRANCH

## Flask

- application structure;
- routing;
- request lifecycle;
- middleware;
- extensions;
- blueprints;
- authentication;
- database integration;
- testing;
- deployment;
- production patterns.

## Django

- project/app structure;
- request lifecycle;
- URL routing;
- views;
- models;
- ORM;
- migrations;
- middleware;
- templates;
- serializers;
- authentication;
- permissions;
- Django REST Framework;
- caching;
- signals;
- async support;
- testing;
- deployment;
- performance.

## FastAPI

- routing;
- dependency injection;
- Pydantic;
- validation;
- request/response models;
- middleware;
- authentication;
- OAuth/JWT;
- background tasks;
- AsyncIO;
- async database access;
- OpenAPI;
- WebSockets;
- testing;
- production deployment;
- performance.

Include **Flask vs Django vs FastAPI** comparisons and framework selection scenarios.

---

# 14. DATABASES

## SQL

- relational concepts;
- normalization;
- joins;
- indexes;
- constraints;
- transactions;
- isolation;
- locking;
- query optimization;
- window functions;
- CTEs;
- execution plans.

## PostgreSQL

Deep practical coverage.

## MySQL

Production fundamentals with **hands-on MySQL practice in the SQL workspace** (InnoDB, indexes, transactions and locking, SQL modes, `EXPLAIN`, `ON DUPLICATE KEY UPDATE`, JSON), and comparison with PostgreSQL.

## Redis

- caching;
- TTL;
- data structures;
- distributed locks;
- rate limiting;
- pub/sub;
- common patterns.

Also cover:

- ORM;
- connection pooling;
- N+1;
- migrations;
- transactions;
- database design.

---

# 15. TESTING & QUALITY

Cover:

- unit testing;
- integration testing;
- API testing;
- mocking;
- fixtures;
- pytest;
- unittest;
- test doubles;
- contract testing;
- end-to-end testing;
- coverage;
- regression testing;
- CI testing.

---

# 16. SYSTEM DESIGN & SOFTWARE ARCHITECTURE

Cover:

- scalability;
- availability;
- reliability;
- latency;
- throughput;
- caching;
- load balancing;
- databases;
- replication;
- sharding;
- partitioning;
- queues;
- event-driven architecture;
- microservices;
- monoliths;
- service boundaries;
- distributed systems;
- consistency;
- CAP concepts;
- fault tolerance;
- observability;
- security;
- disaster recovery.

Include exercises such as:

- URL shortener;
- chat system;
- notification system;
- file storage;
- ride-sharing style system;
- IoT ingestion;
- e-commerce backend;
- AI assistant;
- RAG platform;
- job processing system.

---

# 17. DEVOPS BRANCH

DevOps is a separate first-class branch.

## Linux
- filesystem;
- permissions;
- processes;
- services;
- SSH;
- logs;
- systemd;
- networking commands;
- shell scripting.

## Git
- branching;
- merging;
- rebasing;
- cherry-picking;
- stashing;
- tags;
- workflows;
- pull requests;
- code review.

## Networking
- TCP/IP;
- DNS;
- HTTP;
- HTTPS;
- TLS;
- ports;
- routing;
- reverse proxies;
- firewalls;
- NAT;
- CDN.

## Docker
- images;
- containers;
- Dockerfiles;
- layers;
- volumes;
- networking;
- registries;
- Compose;
- multi-stage builds;
- optimization;
- security;
- debugging.

## CI/CD
- pipeline architecture;
- automated testing;
- artifacts;
- deployment;
- rollback;
- blue/green;
- canary;
- GitHub Actions;
- GitLab CI;
- Jenkins.

## Kubernetes
- architecture;
- control plane;
- nodes;
- pods;
- deployments;
- ReplicaSets;
- services;
- ingress;
- namespaces;
- ConfigMaps;
- Secrets;
- volumes;
- PV/PVC;
- RBAC;
- network policies;
- probes;
- HPA;
- resource management;
- scheduling;
- troubleshooting.

## Kubernetes Ecosystem
- Helm;
- Kustomize;
- operators;
- CRDs;
- GitOps;
- Argo CD;
- service mesh concepts.

## Infrastructure as Code
- Terraform;
- OpenTofu;
- state;
- modules;
- providers;
- variables;
- remote state;
- lifecycle.

## Configuration Management
- Ansible;
- inventories;
- playbooks;
- roles;
- variables;
- secrets.

## Observability
- logs;
- metrics;
- traces;
- Prometheus;
- Grafana;
- OpenTelemetry;
- Elastic Stack;
- alerting;
- SLI/SLO/SLA.

## DevSecOps
- secrets;
- dependency security;
- SAST;
- DAST;
- SBOM;
- container scanning;
- supply-chain security;
- IAM;
- runtime security.

## SRE
- reliability;
- incident response;
- error budgets;
- capacity planning;
- disaster recovery;
- backup;
- production operations.

---

# 18. CLOUD BRANCH

Cloud is a separate first-class branch.

AWS is the initial deep implementation.

Reserve future branches for:

- Google Cloud;
- Microsoft Azure;
- Oracle Cloud;
- IBM Cloud;
- Alibaba Cloud;
- other future providers.

## Cloud Fundamentals
- IaaS;
- PaaS;
- SaaS;
- regions;
- availability zones;
- shared responsibility;
- elasticity;
- scalability;
- high availability;
- cloud economics.

## AWS

### Compute
- EC2;
- Auto Scaling;
- Load Balancing;
- Lambda;
- ECS;
- EKS;
- Fargate.

### Storage
- S3;
- EBS;
- EFS;
- Glacier;
- storage classes;
- lifecycle policies.

### Networking
- VPC;
- subnets;
- route tables;
- Internet Gateway;
- NAT Gateway;
- security groups;
- NACLs;
- Route 53;
- CloudFront;
- VPC peering;
- Transit Gateway;
- PrivateLink.

### Databases
- RDS;
- Aurora;
- DynamoDB;
- ElastiCache.

### Security
- IAM;
- policies;
- roles;
- KMS;
- Secrets Manager;
- Cognito;
- WAF;
- GuardDuty.

### Messaging
- SQS;
- SNS;
- EventBridge;
- API Gateway;
- Step Functions;
- MSK concepts.

### Monitoring
- CloudWatch;
- CloudTrail;
- X-Ray;
- AWS Config.

### Containers
- ECR;
- ECS;
- EKS;
- Fargate.

### Serverless
- Lambda;
- API Gateway;
- Step Functions;
- event-driven architectures.

### AI/ML
- Amazon Bedrock;
- model access;
- RAG architecture;
- AI security;
- AI application deployment.

### Architecture
- Well-Architected concepts;
- multi-AZ;
- multi-region;
- disaster recovery;
- cost optimization;
- serverless;
- event-driven;
- microservices.

---

# 19. GENAI & MULTI-AGENT BRANCH

Create a separate complete GenAI learning branch.

## Foundations
- AI/ML overview;
- LLMs;
- tokens;
- context windows;
- embeddings;
- transformers;
- inference;
- temperature;
- sampling.

## Prompt Engineering
- zero-shot;
- few-shot;
- role prompting;
- structured outputs;
- reasoning strategies;
- prompt templates;
- evaluation;
- prompt injection;
- jailbreak concepts;
- safety.

## LLM Application Engineering
- APIs;
- streaming;
- structured responses;
- function/tool calling;
- retries;
- rate limits;
- cost;
- latency;
- token management.

## RAG
- document ingestion;
- chunking;
- embeddings;
- vector databases;
- retrieval;
- reranking;
- metadata filtering;
- hybrid search;
- evaluation;
- hallucination mitigation.

## AI Agents
- tools;
- planning;
- memory;
- state;
- agent loops;
- tool selection;
- workflows;
- guardrails.

## Multi-Agent Systems
- agent roles;
- orchestration;
- collaboration;
- routing;
- delegation;
- shared state;
- conflict resolution;
- evaluation;
- failure handling.

## Frameworks

Include relevant frameworks as the knowledge base evolves, such as:

- LangChain;
- LangGraph;
- other appropriate agent frameworks.

## AI Architecture
- production RAG;
- AI observability;
- evaluation;
- security;
- prompt injection defense;
- model selection;
- latency;
- cost optimization;
- caching;
- fallback models;
- human-in-the-loop.

---

# 20. PROJECT & LAB ENGINE

Every major branch should contain practical work.

Projects progress from:

```text
Guided Lab
   ↓
Partially Guided Project
   ↓
Independent Project
   ↓
Production-Style Project
```

Projects include:

- requirements;
- architecture;
- implementation;
- testing;
- documentation;
- deployment;
- observability;
- security;
- performance;
- interview discussion.

---

# 21. CODING PRACTICE ENGINE

Include a coding environment.

Primary requirement:

**The learner must be able to write code without IDE autocomplete or special assistance.**

Support:

- plain Python execution;
- **SQL execution (PostgreSQL and MySQL) in the same IDE, with per-question dialect: schema/data panel, editor, run, results grid, errors, validation, execution-plan view, disposable per-attempt database;**
- additional languages and SQL dialects through adapters;
- hidden tests;
- visible tests;
- input/output;
- time limits;
- memory limits;
- code execution isolation;
- challenge difficulty;
- hints;
- progressive hints;
- test failures;
- solution explanation;
- complexity analysis.

Modes:

### Learn
Guided.

### Practice
Minimal assistance.

### Interview
No autocomplete and no unnecessary hints.

### Challenge
Strict evaluation.

### Review
Explain mistakes after submission.

---

# 22. INTERVIEW MASTER

This is a **separate permanent branch**, not an end-of-course feature.

It must contain knowledge from:

- Python;
- OOP;
- SOLID;
- DSA;
- Backend;
- Flask;
- Django;
- FastAPI;
- databases;
- testing;
- DevOps;
- Cloud;
- GenAI;
- system design;
- security;
- all future branches.

The learner can ask:

> “Ask me anything from the entire academy.”

The system must retrieve the appropriate knowledge and answer at the learner's level.

---

# 23. INTERVIEW LAST-MINUTE MODE

The user provides:

- JD;
- interview date/time;
- available preparation hours.

Generate:

```text
HIGH PRIORITY
      ↓
Must Know
      ↓
Likely Questions
      ↓
Resume Questions
      ↓
Coding
      ↓
System Design
      ↓
Mock Interview
      ↓
Final Revision
```

Optimize for maximum interview readiness within the available time.

---

# 24. MOCK INTERVIEW ENGINE

Interface:

```text
INTERVIEWER
    ↓
Question
    ↓
Candidate Answer
    ↓
Evaluation
```

If learner says **PASS**, provide:

- expected answer;
- best senior-level answer;
- explanation;
- rating;
- missing points;
- follow-up questions.

If learner answers:

- evaluate;
- score;
- identify strengths;
- identify omissions;
- correct inaccuracies;
- provide an improved answer;
- ask follow-up when appropriate.

---

# 25. ANSWER SCORING

Evaluate:

- correctness;
- completeness;
- clarity;
- depth;
- practical experience;
- architecture reasoning;
- trade-offs;
- terminology;
- seniority fit;
- JD relevance.

Example:

```text
Technical Accuracy: 8/10
Completeness:       6/10
Clarity:            9/10
Senior Depth:       5/10
JD Relevance:       9/10

Overall: 7.4/10
```

Adapt the rubric to candidate seniority.

---

# 26. ADAPTIVE MASTERY ENGINE

Do not equate:

**Lesson Completed = Mastered**

Mastery considers:

- assessment;
- coding;
- explanation ability;
- application;
- retention;
- interview performance.

Track:

```text
Concept
├── Learned
├── Practiced
├── Applied
├── Explained
├── Interview-ready
└── Mastered
```

---

# 27. MISTAKE MEMORY

Remember recurring mistakes.

Example:

```text
Concept: asyncio.gather()

Attempts: 6
Correct: 3
Incorrect: 3

Recurring issue:
Confuses coroutine creation with task scheduling.

Mastery:
58%

Recommended:
Targeted revision + coding exercise
```

Future questions should adapt accordingly.

---

# 28. SPACED REVISION

Support:

- weak-topic revision;
- forgotten concepts;
- scheduled review;
- interview-driven review;
- mistake-driven review.

---

# 29. KNOWLEDGE INGESTION

Allow future sources:

- official documentation;
- uploaded PDFs;
- uploaded books;
- user notes;
- Markdown;
- permitted web research;
- course material;
- internal documentation.

Every imported item retains provenance.

Track:

- source;
- date;
- version;
- topic;
- reliability;
- applicable technologies.

---

# 30. KNOWLEDGE EXTENSION PROTOCOL

The user can say:

> Add Kafka.

The system should:

1. Identify the appropriate branch.
2. Create the module.
3. Determine prerequisites.
4. Link related topics.
5. Generate learning sequence.
6. Generate coding exercises.
7. Generate labs.
8. Generate interview questions.
9. Update the knowledge graph.
10. Preserve existing learner progress.

Likewise:

> Add Azure.

creates a new Cloud provider branch without disturbing AWS.

Supported future commands:

- Add Branch
- Add Module
- Add Technology
- Add Framework
- Add Cloud Provider
- Add Tool
- Add Topic
- Expand Module
- Update Module
- Replace Deprecated Technology
- Add Project
- Add Coding Challenge
- Add Interview Domain
- Import Knowledge
- Import Documentation
- Import Curriculum

---

# 31. VERSIONING & DEPRECATION

Distinguish:

- concept;
- technology;
- version;
- deprecated feature;
- current best practice.

Never silently erase historical knowledge.

Allow:

- version update;
- deprecation;
- migration guide;
- version comparison.

---

# 32. SECURITY

Include:

- authentication;
- authorization;
- role-based access;
- secure sessions;
- password security;
- OAuth where appropriate;
- rate limiting;
- API security;
- input validation;
- output validation;
- secret management;
- encryption;
- audit logging;
- profile isolation.

## AI Security

Include:

- prompt injection defense;
- malicious document handling;
- tool authorization;
- data exfiltration prevention;
- model output validation;
- sandboxing;
- least privilege;
- sensitive-data protection.

---

# 33. CODE EXECUTION SECURITY

Never execute arbitrary learner code directly on the main application server.

Use an isolated execution architecture with:

- sandboxing;
- resource limits;
- CPU limits;
- memory limits;
- execution timeouts;
- filesystem restrictions;
- network restrictions;
- process isolation;
- cleanup.

Treat submitted code as untrusted.

---

# 34. RECOMMENDED SYSTEM ARCHITECTURE

Use modular architecture.

Possible logical components:

```text
Frontend
   │
API Gateway
   │
Application Backend
   ├── Authentication
   ├── Profile Service
   ├── Curriculum Service
   ├── Knowledge Service
   ├── Assessment Service
   ├── Coding Service
   ├── Interview Service
   ├── JD Analysis Service
   ├── Resume Service
   ├── Progress/Mastery Service
   ├── Project Service
   └── Notification Service
          │
          ├── Relational Database
          ├── Vector Store
          ├── Cache
          ├── Object Storage
          └── Job Queue
```

Do not blindly implement every component as a microservice from day one.

Prefer a modular monolith initially unless scale or operational requirements justify service separation.

---

# 35. DATA MODEL

At minimum design entities for:

- User;
- Profile;
- Resume;
- Experience;
- Skill;
- Goal;
- TargetRole;
- JobDescription;
- JDRequirement;
- Branch;
- Module;
- Lesson;
- Topic;
- KnowledgeItem;
- KnowledgeRelationship;
- Source;
- Version;
- CourseEnrollment;
- Progress;
- Mastery;
- Assessment;
- Question;
- CodingChallenge;
- CodingSubmission;
- Project;
- InterviewSession;
- InterviewQuestion;
- InterviewAnswer;
- InterviewEvaluation;
- Mistake;
- RevisionSchedule;
- UserPreference;
- Visual;
- LearnerLevel;
- SaaS/commerce entities: Tenant, Plan, Feature, Entitlement, Offering, Bundle, Program, PriceBook, Coupon, Order, Subscription, CohortRun, Session, Enrolment, UsageEvent, LaunchPolicy (see SaaS spec §11);
- Credential entities: BadgeDefinition, CertificateTemplate, CredentialRule, CredentialIssue, CredentialRevocation (see Credentials spec §11);
- Resume Studio entities: ResumeDocument, ResumeVersion, ResumeTemplate, AtsReport (see Resume Studio spec §6);
- Admin entities: Role, Permission, AdminUser, ContentVersion, ReviewTask, AuditEntry, ImportJob, Setting, FeatureFlag, ProviderConfig, PromptTemplate, RuntimeAdapter, Rubric, InterviewBlueprint, NotificationTemplate, Tenant, Cohort, FeedbackItem, MasterDataLookup, Translation (see Admin spec §17).

The schema must allow future entities without architectural redesign.

---

# 36. SEARCH & RETRIEVAL

Support:

- keyword search;
- semantic search;
- metadata filtering;
- profile-aware retrieval;
- JD-aware retrieval;
- branch filtering;
- difficulty filtering;
- version filtering;
- source filtering.

For AI-assisted retrieval, use hybrid retrieval where appropriate.

---

# 37. UI / UX

The interface should feel like a professional learning platform.

## Dashboard

Show:

- current learning path;
- progress;
- mastery;
- weak areas;
- upcoming revision;
- interview readiness.

## Academy

Browse branches and modules.

## Learning Room

Lesson content + examples + exercises.

## Coding Lab

Editor + compiler + tests + results.

## Projects

Project requirements and progress.

## Interview Master

Ask anything.

## Mock Interview

Interactive interviewer.

## JD Analyzer

Upload/paste JD.

## Profile

Resume, skills, experience, goals.

## Knowledge Explorer

Explore the knowledge graph.

## Admin Portal

A full web admin control plane (replaces the earlier "Admin / Knowledge Manager" stub): content studio, knowledge manager, assessment & interview studio, visual library, learning rules, execution config, AI/provider config, users & RBAC, platform settings, insights, operations. Draft → Review → Publish, versioning, audit, preview-as-learner. Specified in `Skillora_Learning_Admin_Portal_Specification.md`.

---

# 38. AI TUTOR BEHAVIOR

The tutor must be:

- patient;
- rigorous;
- technically accurate;
- practical;
- senior-level when required;
- honest about uncertainty;
- adaptive;
- encouraging without empty praise.

It must teach rather than merely dump answers.

For coding:

**Do not immediately reveal solutions unless requested or in review mode.**

For interviews:

**Act like a realistic interviewer.**

For explanations use:

1. intuition;
2. concept;
3. example;
4. implementation;
5. common mistakes;
6. production relevance;
7. interview relevance.

---

# 39. CURRICULUM CONTENT FORMAT

Every lesson should preferably contain:

```text
Learning Objective
Prerequisites
Concept Explanation
Mental Model
Example
Code
Walkthrough
Common Mistakes
Production Use
Security Considerations
Performance Considerations
Practice
Coding Challenge
Assessment
Interview Questions
Senior-Level Question
Summary
Mastery Check
Credentials Earned (badges and certificate progress)
```

Every course defines its credentials (skill badges per concept cluster, a module badge per module, level badges, project badge, and a course certificate — completion and/or mastery), earned from evidence (mastery, assessments, coding/SQL, projects, explanation, retention, interviews). See `Skillora_Learning_Credentials_Badges_Certificates_Specification.md`.

Each section has a defined visual slot (hero diagram, code↔diagram, step-through trace, wrong-vs-right panel, architecture/sequence diagram, cheat-sheet). A lesson must have at least one hero diagram and one interactive or step-through visual, and no more than five diagrams.

# 39A. VISUAL LEARNING LAYER

Every learner level gets a different presentation of the same concept:

| Level | Visual approach |
|---|---|
| Beginner | Analogies, large icons, one idea per diagram, animated steps |
| Developing | Annotated flow charts, code↔diagram traces, comparisons |
| Professional | Architecture, sequence and state diagrams, failure paths, trade-offs |
| Senior | Dense system diagrams with scaling/failure/security overlays, decision trees, "what breaks at 10x" |

Required platform visuals: learning-route map, module map with status badges, flow/sequence/state diagrams, data-structure and algorithm animators, code step-through and memory viewer, complexity plots, interview report radar, JD match gauge, mastery ladder, revision timeline, knowledge-graph explorer.

Rules: visuals are authored as versioned source (Mermaid / JSON / SVG) and rendered client-side; accessible (alt text, non-colour cues, reduced motion); work offline; AI-generated diagrams are reviewed for accuracy before publishing; the learner can switch Simpler/Deeper at any time. Full detail: **Skillora_Learning_Visual_Learning_System.md**.

Add to the data model (section 35): `Visual` (id, version, concept/lesson, type, level, source, alt text, interaction config, provenance) and `LearnerLevel` (user, branch, level, last-updated).

---

# 40. PROJECT-BASED LEARNING

Projects must connect multiple branches.

Example:

## Production AI Assistant

Uses:

- Python;
- FastAPI;
- PostgreSQL;
- Redis;
- RAG;
- vector database;
- Docker;
- Kubernetes;
- AWS;
- CI/CD;
- observability;
- security.

The objective is integrated engineering competence, not isolated technology memorization.

---

# 41. INTERVIEW KNOWLEDGE

The learner must be able to ask:

> “Give me every important FastAPI interview question for a senior backend engineer.”

> “Ask me a Kubernetes interview question appropriate for my current experience.”

> “What could an interviewer ask about this project on my resume?”

> “I have 45 minutes before an interview. Prepare me.”

The Interview Master must use the complete available knowledge base.

---

# 42. MODEL-AGNOSTIC DESIGN

Do not hard-code dependence on a particular AI provider.

Abstract:

- LLM provider;
- embedding model;
- vector database;
- compiler;
- cloud;
- authentication provider.

Use configurable adapters.

---

# 43. OBSERVABILITY

Track appropriate operational metrics:

- application logs;
- errors;
- API latency;
- AI latency;
- token usage;
- model cost;
- retrieval quality;
- coding execution metrics;
- interview evaluation metrics;
- user engagement;
- failures.

Do not collect unnecessary personal data.

---

# 44. TESTING

Require:

## Unit Tests
For domain logic.

## Integration Tests
For database, APIs, knowledge retrieval, authentication, coding execution.

## End-to-End Tests
For registration, profile creation, course progression, coding challenge, JD analysis, mock interview.

## AI Evaluation

Test:

- hallucination;
- retrieval correctness;
- answer relevance;
- scoring consistency;
- prompt injection;
- inappropriate confidence;
- profile leakage.

---

# 45. DEVELOPMENT STRATEGY

Build incrementally.

## Phase 0 — Product Definition

Finalize:

- requirements;
- architecture;
- technology choices;
- UX;
- data model.

## Phase 1 — MVP

Implement:

- authentication;
- profile;
- academy;
- curriculum;
- basic progress;
- basic AI tutor;
- diagram renderer, route map, module map, level switch and lesson visual slots.

## Phase 2 — Coding

Implement:

- coding challenges;
- sandbox;
- tests;
- submissions.

## Phase 3 — Knowledge Engine

Implement:

- knowledge graph;
- semantic retrieval;
- source management;
- extensibility.

## Phase 4 — Interview Master

Implement:

- question engine;
- JD analysis;
- mock interviews;
- scoring;
- last-minute mode.

## Phase 5 — Adaptive Intelligence

Implement:

- mastery;
- mistake memory;
- revision;
- personalized routes.

## Phase 6 — Production

Implement:

- observability;
- scaling;
- security hardening;
- backups;
- disaster recovery;
- deployment.

---

# 46. ENGINEERING RULES FOR ANY BUILDING LLM

Before implementing substantial code:

1. Analyze requirements.
2. Identify ambiguities.
3. State assumptions.
4. Produce architecture.
5. Explain trade-offs.
6. Define data model.
7. Define APIs.
8. Define component boundaries.
9. Define security implications.
10. Define test strategy.
11. Define implementation order.
12. Then implement.

Never generate an enormous codebase blindly.

When modifying an existing implementation:

- inspect first;
- understand existing architecture;
- preserve working functionality;
- make minimal safe changes;
- run tests;
- explain changed files.

---

# 47. NO PLATFORM LOCK-IN

Skillora Learning is an independent product.

Do not make any existing learning platform a product dependency or part of the identity.

Do not reference unrelated learning platforms in user-facing product branding or UI unless explicitly required for source attribution.

---

# 48. BRANDING

Use:

**Skillora Learning**

Preferred domain:

**skilloralearning.com**

Keep the product technology-neutral.

Do not rename the product after a specific programming language.

The system must be able to expand into technologies that do not exist in the initial curriculum.

---

# 49. FUTURE EXTENSION EXAMPLES

The architecture must support future additions such as:

```text
New Languages
├── Go
├── Rust
├── Java
└── TypeScript

New Cloud
├── GCP
├── Azure
├── Oracle Cloud
└── IBM Cloud

New DevOps
├── Pulumi
├── Crossplane
├── Flux
└── advanced platform engineering

New AI
├── multimodal AI
├── computer vision
├── speech AI
├── AI evaluation
├── model serving
└── AI infrastructure

New Security
├── application security
├── cloud security
├── threat modeling
└── secure coding
```

These are examples, not mandatory initial implementations.

---

# 50. SUCCESS CRITERIA

Skillora Learning is successful when a user can:

1. Create a profile.
2. Upload a resume.
3. Confirm extracted experience and skills.
4. Select a career target.
5. Receive a personalized learning route.
6. Progress through modules.
7. Practice coding without autocomplete.
8. Build projects.
9. Track mastery.
10. Receive adaptive revision.
11. Add a JD.
12. Receive a JD-specific interview route.
13. Practice resume-based questions.
14. Run mock interviews.
15. Get detailed answer ratings.
16. Receive improved senior-level answers.
17. Ask questions from any branch.
18. Add new knowledge later.
19. Add completely new branches later.
20. Preserve previous progress after knowledge expansion.

---

# 51. FINAL OPERATING PRINCIPLE

**Skillora Learning must behave like a continuously evolving personal technical academy and career intelligence system, not like a static course.**

The system should always ask:

> **Who is this learner?**

> **What do they already know?**

> **What are they trying to achieve?**

> **What do they need to learn next?**

> **What are they repeatedly getting wrong?**

> **What does their target job require?**

> **What can they prove through coding and projects?**

> **How interview-ready are they?**

> **What should they practice next?**

When new technology appears:

> **How should this new knowledge connect to everything the learner already knows?**

That is the core intelligence of Skillora Learning.

---

# 52. DEFAULT OPERATING INSTRUCTIONS FOR THE BUILDING LLM

When this master prompt is supplied to an AI development agent:

1. Treat this document as the authoritative product specification.
2. Identify missing implementation details rather than silently inventing critical requirements.
3. Make reasonable assumptions for non-critical details and clearly document them.
4. Prefer modular, maintainable architecture.
5. Avoid unnecessary complexity.
6. Keep all major systems extensible.
7. Protect user data.
8. Never execute untrusted code unsafely.
9. Never fabricate technical knowledge or learner experience.
10. Build incrementally.
11. Provide tests alongside production functionality.
12. Keep documentation synchronized with implementation.
13. Preserve backward compatibility where reasonable.
14. Make future branches and technologies easy to add.
15. Treat the knowledge base as versioned, extensible infrastructure.
16. Treat learner profiles as private, persistent state.
17. Treat the interview engine as a first-class capability.
18. Treat coding practice as a first-class capability.
19. Treat DevOps and Cloud as independent first-class branches.
20. Treat GenAI and Multi-Agent Engineering as an independent first-class branch.

---

# 53. ULTIMATE PRODUCT MODEL

```text
                         SKILLORA LEARNING
                    Personal Technical OS
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
       ▼                      ▼                      ▼
 KNOWLEDGE ENGINE         LEARNING ENGINE       CAREER ENGINE
       │                      │                      │
 Knowledge Graph         Curriculum            Resume
 Sources                 Lessons               JD Analysis
 Versions                Assessments            Gap Analysis
 Relationships            Mastery               Career Goals
 Extensions               Revision              Interview Route
       │                      │                      │
       └──────────────────────┼──────────────────────┘
                              ▼
                       PRACTICE ENGINE
                              │
             ┌────────────────┼────────────────┐
             ▼                ▼                ▼
          Coding           Projects         Labs
             │                │                │
             └────────────────┼────────────────┘
                              ▼
                       INTERVIEW MASTER
                              │
             ┌────────────────┼────────────────┐
             ▼                ▼                ▼
         Q&A Engine       Mock Interview    Last-Minute Mode
             │                │                │
             └────────────────┼────────────────┘
                              ▼
                       MASTERY ENGINE
                              │
                    Adaptive Intelligence
                              │
                              ▼
                    CONTINUOUS IMPROVEMENT
                              │
                              └──────────────→
                              New Knowledge
                              New Branches
                              New Technologies
                              New Career Goals
```

**The platform has no final endpoint.**

Its architecture must allow the learner and the knowledge base to evolve together.

---

# END OF MASTER PROMPT
