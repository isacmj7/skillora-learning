# Verification Log A5-A8 (VER-2, verified 2026-10-05)

Source key: PyPI = https://pypi.org/pypi/<pkg>/json (primary registry data). Status: R = resolved, C = corrected, O = left open.

## A5
| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| 1 | RQ release cadence [verify] | RQ 2.12.0 on 2026-08-30; 2.7 to 2.12 released Feb to Aug 2026 | PyPI rq | R: dated fact in M8 and M-queue lines |
| 2 | Arq maintenance-only "since v0.27 in Feb 2025" | CONTRADICTED on date: v0.27.0 was released 2026-02-02, 0.28.0 on 2026-04-16; README says maintenance-only (issue #510), announced Oct 2025 per secondary coverage | PyPI arq; https://github.com/python-arq/arq (README); https://github.com/tastyware/streaq/discussions/108 | C: date fixed in M8 and freshness notes |
| 3 | Celery 5.6 stable; Dramatiq 2.x active | Celery 5.6.3 (2026-03-26); Dramatiq 2.2.1 (2026-09-02) | PyPI | R |
| 4 | Strawberry, Ariadne, Graphene release dates/activity [verify] | Strawberry 0.331.1 (2026-10-04); Ariadne 1.1.0 (2026-06-15); Graphene 3.4.3 (2024-11-09, nothing since) | PyPI | R (release-date based; maintainer identity not checked) |
| 5 | Manifest pins FastAPI/Django/Flask/SQLAlchemy [verify] | Latest: FastAPI 0.142.2, Django 6.1.1, Flask 3.1.3, SQLAlchemy 2.1.3 (so "SQLAlchemy 2.x" holds). Pins are a platform-manifest decision | PyPI | Partly R: facts added; marker kept (manifest not in scope) |
| 6 | Django 6.1 current, 6.0 out of mainstream support, 5.2 LTS | Confirmed: 6.1 released Aug 2026; 6.0 mainstream ended Aug 2026; 5.2 LTS extended support to Apr 2028 | https://www.djangoproject.com/weblog/2026/aug/05/django-61-released/ ; https://www.djangoproject.com/download/ | R (no text change needed) |
| 7 | Django 6.x native async DB coverage [verify] | NOT resolved. docs.djangoproject.com 6.1 async page was unreachable (sandbox proxy); only a Medium article claims native psycopg3 async, not a reliable source; 5.1 docs say DB ops still sync underneath | https://docs.djangoproject.com/en/6.1/topics/async/ (unread) | O: marker kept with reason in two places |
| 8 | OPA, Cedar, OpenFGA status [verify] | OPA CNCF graduated (Feb 2021); core maintainers joined Apple in 2025, governance unchanged. Cedar CNCF Sandbox (accepted 2025-10-08). OpenFGA CNCF Incubating (2025-10-28) | https://www.cncf.io/projects/cedar/ ; https://www.cncf.io/blog/2025/11/11/openfga-becomes-a-cncf-incubating-project/ ; https://www.opensourceforu.com/2025/08/apple-acquires-open-policy-agent-developers-while-cncf-retains-control-of-open-source-project/ | R |
| 9 | OWASP API Top 10 2023 still current | Confirmed, no newer edition | https://owasp.org/API-Security/ (via search results) | R (no change) |
| 10 | OAuth 2.1 draft-15, March 2026 | draft-15 published 2026-03-02; a -16 may exist, not confirmed | https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/ | C: reworded to "draft-15 or later" |
| 11 | OpenAPI 3.2.0, Redis 8 AGPL/Valkey, FastAPI 0.14x | Not re-checked beyond PyPI (FastAPI 0.14x confirmed) | PyPI | No change, noted |

## A6
| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| 12 | Voice latency budget numbers [verify] | Provider-dependent by design; no factual claim | n/a | R: marker replaced by "none quoted as fact, label as placeholder" |
| 13 | MQTT broker limits/pricing [verify] | Same, teaching method only | n/a | R |
| 14 | Managed-service limits table row [verify] | Same | n/a | R |
| 15 | Kafka 4.0 removed ZooKeeper; KIP-932 share groups maturity [verify] | Share groups production-ready in Kafka 4.2 (2026-02-17); 4.3.0 (2026-05-22) and 4.3.1 (2026-06-25) are current | https://www.confluent.io/blog/apache-kafka-4-2-release/ ; https://kafka.apache.org/blog/2026/06/25/apache-kafka-4.3.1-release-announcement/ ; https://kafka.apache.org/downloads | R |
| 16 | "[verify] acceptance by the canvas spec" (line 728) | Internal cross-document acceptance gate, not a factual claim | n/a | Left unchanged intentionally |

## A7
| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| 17 | SnapStart Python 3.12+ and container images (July 2026) | Confirmed; AWS base images 3.12+ automatic, others need label `com.amazonaws.lambda.feature.snapstart="Allow"` or hooks; not in Asia Pacific (New Zealand) and (Taipei) | https://aws.amazon.com/about-aws/whats-new/2026/07/aws-lambda-snapstart-container/ ; https://docs.aws.amazon.com/lambda/latest/dg/snapstart.html | R; exclusions (provisioned concurrency, EFS) and Python 3.15 runtime date not confirmed, so narrow marker kept |
| 18 | Terraform/Terragrunt/Atlantis/HCP licensing [verify] | Terraform BUSL 1.1 (Aug 2023); OpenTofu MPL 2.0; Terragrunt MIT; Atlantis Apache 2.0; HCP Terraform legacy free plan ended 2026-03-31, free tier 500 resources | https://terragrunt.gruntwork.io/docs/community/license/ ; https://github.com/runatlantis/atlantis/discussions/3664 ; https://spacelift.io/blog/terraform-license-change ; https://scalr.com/learning-center/hcp-terraform-free-tier-is-being-discontinued-what-you-need-to-know | R (HCP prices deliberately not taught) |
| 19 | OTel Python logs SDK status [verify] | Logs signal still in development in Python (underscore modules); stabilisation PRs open (#5732, #5733); opentelemetry-sdk 1.45.0 (2026-09-25) | https://github.com/open-telemetry/opentelemetry-python/pull/5732 ; PyPI | R |
| 20 | OTel profiles public alpha since March 2026 | Confirmed (Collector v0.148.0+) | https://opentelemetry.io/blog/2026/profiles-alpha/ | R (no change) |
| 21 | X-Ray SDK maintenance 2026-02-25, EOS 2027-02-25 | Confirmed | https://aws-news.com/article/2025-10-29-announcing-aws-x-ray-sdksdaemon-end-of-support-and-opentelemetry-migration ; https://pypi.org/project/aws-xray-sdk/ | R (no change) |
| 22 | GitHub Actions self-hosted runner fee postponed | Confirmed: $0.002/min announced Dec 2025, postponed within 48h, not in effect | https://www.tenki.cloud/blog/github-actions-runner-pricing-2026 ; https://bex.co/blog/2026/07/08/github-actions-runner-metering-backoff (secondary) | R (no change) |
| 23 | Fargate Firecracker wording [verify] | AWS docs confirm each task has own isolation boundary and kernel; explicit Firecracker-for-Fargate wording only seen in secondary/AWS blog material | https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-security-considerations.html | Partly R: wording corrected; narrow marker kept |
| 24 | LocalStack licensing [verify] | Community edition ended 2026-03-23; unified image needs account and auth token; reports of Apache 2.0 to BSL change from secondary sources only | https://blog.localstack.cloud/2026-upcoming-pricing-changes/ ; https://www.infoq.com/news/2026/02/localstack-aws-community | Partly R; marker kept for exact licence text |

## A8
| # | Original claim | Result | Source | Action |
|---|---|---|---|---|
| 25 | Locust/k6 versions [verify] | Locust 2.46.7 (PyPI, 2026-10-04); k6 2.3.0 (2026-09-21, from search of GitHub releases; secondary) | PyPI locust ; https://github.com/grafana/k6/releases | R |
| 26 | Cosmic Ray maintained [verify] | 8.7.0 released 2026-08-09 | PyPI cosmic-ray | R |
| 27 | mutmut 3.0.2 yanked; current version [verify] | CORRECTED: 3.0.0 to 3.0.4 are all yanked; 3.0.5 and later fine; 3.8.0 current (2026-09-12) | PyPI mutmut | C |
| 28 | PyPI trusted publishing/attestations [verify] | PEP 740 attestations are default in pypa/gh-action-pypi-publish v1.11.0+ with a trusted publisher | https://peps.python.org/pep-0740/ ; https://docs.pypi.org/attestations/publish/v1/ | R |
| 29 | pytest 9.0 (8 Nov 2025); pytest-asyncio 1.0 (26 May 2025) | Dates confirmed on PyPI; current pytest 9.1.1, pytest-asyncio 1.4.0 | PyPI | R |
| 30 | Pyrefly 1.0 stable (May 2026); ty beta | Pyrefly 1.0.0 on 2026-05-12; ty beta since 2025-12-16 (0.0.84 on PyPI) | https://pyrefly.org/blog/v1.0/ ; https://astral.sh/blog/ty | R (no change) |
| 31 | OWASP Top 10:2025 (final Jan 2026), API 2023, LLM 2025 | Web 2025 and API 2023 confirmed; LLM 2025 not re-checked | https://owasp.org/projects/top-ten ; https://github.com/owasp/top10 | R/no change |
| 32 | DPDP Rules 2025 dates | Confirmed: notified 2025-11-13; consent managers 2026-11-13; core obligations 2027-05-13 | https://en.wikipedia.org/wiki/Digital_Personal_Data_Protection_Rules,_2025 (secondary) | R (no change) |
| 33 | "Still [verify] at authoring" list (Python 3.13/3.14 behaviour, pytest changes, etc., line 502) | Authoring-time checklist, not specific claims | n/a | Left as is |

## Open items
1. A5 Django 6.1 native async DB coverage (primary docs unreachable).
2. A5 manifest pins (platform-manifest decision, not external fact).
3. A7 SnapStart exclusions and Python 3.15 runtime date.
4. A7 Fargate-Firecracker explicit wording in current AWS docs.
5. A7 LocalStack exact licence text.
Note: web_fetch timed out and the sandbox allowlist blocked most doc sites, so several confirmations rely on web search results and PyPI.
