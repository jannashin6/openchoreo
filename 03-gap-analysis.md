# 03 - Gap Analysis

## Executive Summary

To deliver OpenChoreo’s fundamental value proposition, we do not need to rebuild OpenChoreo. We need to unify our existing delivery, operations, and FinOps products around one application-centric operating model.

The biggest gap is context continuity.

Today, the likely state is:

```text
BuildPiper knows delivery.
REMS knows operational health.
SpendSmart knows cost.
```

The target state is:

```text
Every application has one lifecycle record:
Owner -> repo -> build -> deployment -> environment -> runtime -> health -> incidents -> cost -> recommended action
```

## 1. Capability Gaps

| Gap | Why It Matters | Current Evidence | Recommendation |
|---|---|---|---|
| Canonical application model | Without it, unified UX becomes stitched navigation | BuildPiper has app/service concepts; REMS/SpendSmart context must align | Define application as the root object |
| Cross-product environment model | Health and cost must map to dev/stage/prod/runtime | BuildPiper EnvOps exists; SpendSmart has environment cost breakdowns | Create shared environment IDs and metadata |
| App-aware FinOps agent | OpenChoreo has FinOps agent; SpendSmart has cost intelligence | SpendSmart supports anomaly detection, optimization, actions | Build FinOps agent on SpendSmart with BuildPiper runtime context |
| Composable platform capabilities | OpenChoreo has ComponentTypes/Traits | BuildPiper has templates/blueprints, but not a cross-product trait model | Start with curated "application profiles" before full trait system |
| Unified resource model | OpenChoreo has ResourceTypes/Resources | BuildPiper has EnvOps/CloudOps/DBOps; SpendSmart has infra visibility | Build lightweight Resource Catalog for MVP |
| Agent permission model | Agents need scoped, auditable actions | BuildPiper has RBAC; REMS/SpendSmart approvals exist | Extend RBAC to agent actions and human approvals |
| Unified action layer | Recommendations must become governed action | Pieces exist in pipelines, runbooks, approvals, OpenOps | Create action framework for deploy, rollback, runbook, resize, schedule |

## 2. Integration Gaps

| Integration Gap | Missing Relationship | Impact |
|---|---|---|
| BuildPiper -> REMS | Deployment/release events linked to incidents, SLOs, logs, traces | SRE cannot quickly see whether a deployment caused degradation |
| BuildPiper -> SpendSmart | Workloads/environments linked to cloud spend | Teams cannot see cost impact of deployments or environments |
| REMS -> SpendSmart | Reliability incidents linked to cost/resource anomalies | Cost spikes and reliability degradation remain separate investigations |
| BuildPiper -> REMS -> SpendSmart | App lifecycle linked end-to-end | No single application operating view |
| Agent orchestration | Ask Olly, REMS AI, SpendSmart AI operate with shared context | Agents may produce narrow recommendations instead of lifecycle-aware actions |

Target relationship:

```text
Application
  v
Repository
  v
Build / Release
  v
Deployment
  v
Environment
  v
Runtime resources
  |-- Health / SLO / events / RCA
  |-- Cost / budget / optimization
  v
Governed action
```

## 3. Experience Gaps

| Gap | User Symptom | Required Product Move |
|---|---|---|
| Separate product experiences | Users jump between BuildPiper, REMS, SpendSmart | Unified application detail page |
| Separate terminology | "Service," "application," "resource," "environment" may mean different things | Shared glossary and object model |
| Separate dashboards | Delivery, health, and cost are viewed independently | Application cockpit with tabs/views |
| Separate workflows | Deploy, diagnose, and optimize happen in different tools | Guided lifecycle actions |
| No unified executive view | Leadership cannot see delivery + reliability + cost together | Portfolio dashboard |
| Fragmented agent experience | AI assistants answer within product silos | Unified app-aware SRE/FinOps assistant |

## 4. Data / Context Gaps

| Relationship | Likely Current State | Gap |
|---|---|---|
| Application -> Repository | BuildPiper likely has | Validate completeness |
| Repository -> Build | BuildPiper has | Reuse |
| Build -> Deployment | BuildPiper has | Reuse |
| Deployment -> Environment | BuildPiper/EnvOps has | Standardize shared IDs |
| Environment -> Runtime | BuildPiper/KubeOps has | Reuse/adapt |
| Runtime -> Events | REMS has telemetry, but app linkage must be validated | Build metadata bridge |
| Runtime -> Infrastructure | BuildPiper/SpendSmart likely both know parts | Normalize resource identity |
| Infrastructure -> Cost | SpendSmart has | Reuse |
| Cost -> Application | SpendSmart supports team/product/env breakdowns; app-level attribution requires validation | Build app cost attribution |
| Incident -> Deployment | REMS + BuildPiper integration required | Build deployment-impact correlation |

## 5. Platform / Architecture Gaps

| Platform Gap | Why It Is Justified | MVP Need |
|---|---|---|
| Unified application catalog | Root object for all product experiences | P0 |
| Metadata/context graph | Connects delivery, ops, infra, cost | P0 |
| Cross-product API layer | Lets UI/agents query all products consistently | P0 |
| Shared identity/permissions | Required for governed actions | P0/P1 depending existing SSO |
| Unified audit trail | Necessary when agents recommend or execute actions | P1 |
| Event bus or integration layer | Needed for deployment, alert, cost, and action events | P1 |
| Notification layer | Needed for incidents, budgets, approvals | P1 |
| Agent action policy engine | Required before autonomous remediation | P1 |

## 6. UX Gaps

| UX Surface | Why It Matters | MVP Scope |
|---|---|---|
| Application Overview | Single place to understand an app | Must have |
| Environment View | Shows what runs where | Must have |
| Deployment Timeline | Connects changes to outcomes | Must have |
| Health / Events View | Brings REMS context into app page | Must have |
| Cost View | Brings SpendSmart context into app page | Must have |
| Action Center | Converts recommendations into approved actions | Should have |
| Portfolio Dashboard | Leadership view across apps | Should have |
| Agent Workspace | SRE/FinOps conversational assistant with evidence | Should have |

## Prioritization Matrix

| Gap | User Impact | Business Value | Complexity | Existing Asset | Priority |
|---|---|---|---|---|---|
| Canonical application catalog | Very high | Very high | Medium | BuildPiper app/service model | P0 |
| Shared environment/runtime identity | Very high | High | Medium | EnvOps/KubeOps | P0 |
| BuildPiper + REMS deployment-to-incident correlation | Very high | Very high | Medium | DeployX + REMS | P0 |
| SpendSmart app/environment cost attribution | High | Very high | Medium-high | SpendSmart breakdowns | P0 |
| Unified application overview | Very high | Very high | Medium | Existing UIs/dashboards | P0 |
| SRE agent integrated with deployment context | High | High | Medium | Ask Olly + REMS | P1 |
| FinOps agent integrated with runtime context | High | High | Medium | SpendSmart + KubeOps | P1 |
| Unified action/approval framework | High | High | High | BuildPiper approvals, REMS runbooks, SpendSmart approvals | P1 |
| Resource catalog/self-service infra | Medium | High | High | EnvOps/CloudOps/DBOps | P1 |
| ComponentType/Trait-like abstraction system | Medium | Medium | High | Templates/blueprints | P2 |
| Full GitOps declarative platform model | Medium | Medium | High | BuildPiper GitOps | P2 |
| CNCF/open-source positioning | Low for near-term enterprise product | Low/strategic | High | None | P2/Not needed |

## Most Important Recommendation

Build the unified context layer first. Without it, the product becomes a set of embedded dashboards. With it, BuildPiper, REMS, and SpendSmart can become one application operating system.

