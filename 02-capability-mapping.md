# 02 - OpenChoreo ↔ BuildPiper / REMS / SpendSmart Capability Mapping

## Executive Summary

BuildPiper already covers much of the application delivery and platform engineering foundation. REMS covers observability, RCA, SLOs, and incident response. SpendSmart covers cloud spend visibility, cost anomaly detection, budgeting/optimization, and FinOps actions.

The opportunity is not to rebuild these products. The opportunity is to create a unified application context layer and executive-grade product experience across them.

```text
BuildPiper:  Application -> Build -> Deploy -> Environment
REMS:        Runtime -> Signals -> RCA -> Incident / Runbook
SpendSmart: Infrastructure -> Usage -> Cost -> Budget / Optimization

Needed:
Application -> Deployment -> Environment -> Runtime -> Health -> Cost -> Action
```

## Capability Inventory

### BuildPiper

Verified from official BuildPiper sources:

| Area | Capability |
|---|---|
| Core product | AI-powered DevSecOps and platform engineering control plane |
| Application delivery | Application/microservice onboarding, build/deploy workflows, release packages |
| CI/CD | Secure CI pipelines, templates/blueprints, job execution history, approvals |
| Deployment | Canary deployments, rollbacks, versioned configs, deployment analytics |
| Environments | Dynamic/ephemeral environments through EnvOps |
| Kubernetes operations | KubeOps, managed Kubernetes, namespaces, services, deployments, pods, ingress, secrets/config visibility |
| Governance | RBAC, gated pipelines, audit trail, Jira/ServiceNow integrations |
| Security | Code/container/security scans, SBOM validation, credential leak detection, secret management |
| Observability/intelligence | DORA metrics, maturity/velocity/deploy insights, OpenTelemetry, AIOps, Ask Olly |
| AI | Ask Olly, BuildPiper MCP, AI-powered build/deploy log analysis |
| Interfaces | BuildPiper UI, BPCTL CLI, Platform API |
| Integrations | Public site claims 100+ integrations |

### REMS

| Area | Capability |
|---|---|
| Core product | AI-powered observability / AIOps / SRE platform |
| Telemetry | Metrics, logs, traces via Prometheus, Loki, Tempo, OpenTelemetry |
| Detection | Alert correlation, anomaly/root-cause investigation |
| SLOs | SLO, SLI, error budget visibility |
| Incident response |severity classification, role assignments, RCA documentation |
| RCA | SAVVY-powered root cause analysis in plain English |
| Remediation | Runbook recommendations and execution workflows |
| Dashboards | Unified observability views, service explorer, operational dashboards |

### SpendSmart

| Area | Capability |
|---|---|
| Core product | Cloud cost optimization / FinOps solution |
| Cost visibility | Spend transparency across AWS, Azure, GCP |
| Cost allocation | Breakdowns by service, team, environment |
| Anomaly detection | AI-driven cost anomaly detection |
| Optimization | Rightsizing, idle resource cleanup, schedule recommendations |
| Reporting | Multi-cloud reporting, CFO/CTO dashboards |
| Tech foundation | ClickHouse, Grafana, Athena; API-ready integration |

## Capability Mapping Table

Legend: REUSE REUSE, ADAPT ADAPT, BUILD BUILD, NOT NEEDED NOT NEEDED, UNKNOWN UNKNOWN

| OpenChoreo Capability | OpenChoreo Description | BuildPiper | REMS | SpendSmart | Existing Coverage | Notes |
|---|---|---|---|---|---|---|
| Application/component model | Projects and components define deployable app units | App/service onboarding | Service context likely | Cost by product/team/env | ADAPT ADAPT | Need one canonical application object |
| ComponentType/golden paths | Platform-defined workload templates | CI/CD blueprints, templates | No | No | ADAPT ADAPT | BuildPiper templates can become golden paths |
| Traits | Optional operational capabilities | Partial via pipeline/env/security config | Partial via SLO/RCA | Partial via cost policies | BUILD BUILD | Need composable add-ons across products |
| CI/build workflows | Workflow plane or external CI | Strong CI/CD | No | No | REUSE REUSE | BuildPiper is primary asset |
| Deployment/CD | Releases, bindings, promotion | DeployX, release packages, canary/rollback | Health gates possible | Cost gates possible | ADAPT ADAPT | Need health/cost-aware promotion |
| Environment management | Environments bound to data planes | EnvOps | SLO/environment context | Cost by environment | ADAPT ADAPT | Need shared environment ID |
| Runtime/Kubernetes management | Data planes run workloads | KubeOps | Runtime telemetry | Infra cost | REUSE REUSE | BuildPiper strong for runtime ops |
| Observability plane | Logs, metrics, traces, alerts | OpenTelemetry/AIOps | Strong observability stack | No | REUSE REUSE | REMS should own deep observability |
| Domain-centric telemetry | Telemetry enriched by namespace/project/component | Partial | Service topology | Cost allocation | ADAPT ADAPT | Metadata mapping is key gap |
| SRE agent | RCA/remediation agent | Ask Olly | OLLY/RCA/remediation | No | ADAPT ADAPT | Merge Ask Olly + REMS into app-aware SRE agent |
| FinOps agent | Budget alert cost optimization | Limited cost savings | No | Strong | ADAPT ADAPT | SpendSmart can power agent, needs app mapping |
| Resource provisioning | ResourceTypes and Resources | EnvOps/CloudOps/DBOps partial | No | No | ADAPT ADAPT | Need self-service resource model |
| Secrets/config | Secret refs, external secret integrations | Secret management | No | No | REUSE REUSE | Validate full secret-provider coverage |
| Networking/endpoints | Gateway, endpoints, policies | KubeOps/ingress | Observability impact | No | ADAPT ADAPT | Need app-level endpoint model |
| Security/authz | OIDC, RBAC, ABAC, hierarchical access | RBAC, SSO, gated controls | Unknown | Approval workflows | ADAPT ADAPT | Need unified permission model |
| Audit/governance | Platform-wide policies and audit | Strong audit/gates | Incident records | Cost approvals | ADAPT ADAPT | Need cross-product audit trail |
| Portal | Backstage-based IDP | BuildPiper UI/self-service portal | Dashboards | Dashboards | ADAPT ADAPT | Need unified IA, not three portals |
| CLI/API | occ, OpenAPI, GitOps, MCP | BPCTL, Platform API, MCP | APIs unclear | API-ready | ADAPT ADAPT | Need unified APIs and agent tools |
| GitOps | Declarative state option | GitOps module | No | No | REUSE REUSE | BuildPiper public docs mention GitOps |
| Multi-plane architecture | Control/data/workflow/observability separation | BP gateway/UI/API/runtime modules | Observability stack | Cost stack | ADAPT ADAPT | Adopt principle, not exact architecture |
| Open-source/CNCF distribution | Public OSS project | Commercial platform | Commercial solution | Commercial solution | NOT NEEDED NOT NEEDED | Not required unless GTM strategy changes |

## Competitive Read: BuildPiper vs OpenChoreo

| Dimension | OpenChoreo | BuildPiper-Led Unified Platform |
|---|---|---|
| Core identity | Open-source Kubernetes IDP | Enterprise application delivery and operations control plane |
| Strength | Clean platform abstraction model | Mature delivery, governance, hybrid workload breadth |
| AI | SRE/FinOps agents through MCP | Ask Olly + REMS + SpendSmart can create stronger app-aware agents |
| Scope | Kubernetes-first | Kubernetes + VMs + databases + hybrid/multi-cloud |
| Gap | Enterprise breadth and commercial packaging | Unified product model across delivery, ops, and cost |

## Strategic Takeaway

We likely have enough product assets to compete on the core outcome. The missing asset is not another pipeline, dashboard, or cost report. It is the shared application graph that lets every product answer: "Which application, in which environment, running on which infrastructure, with what health and cost impact?"

