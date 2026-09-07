# 04 - Unified Product Concept

## Executive Summary

We should build an application-centric internal platform that unifies BuildPiper delivery, REMS operations, and SpendSmart FinOps into one governed experience.

The product should not copy OpenChoreo’s Kubernetes-native structure. It should adopt OpenChoreo’s strongest idea: a shared platform model that turns scattered tools into a coherent developer, operator, and leadership experience.

## 1. Product Vision

**Give every engineering team one governed place to ship, operate, and optimize every application with confidence.**

Alternative vision options:

1. **Make application ownership visible, actionable, and intelligent from code to cost.**
2. **Turn software delivery, reliability, and cloud spend into one connected operating system.**
3. **Help teams move from fragmented DevOps to accountable application ownership.**

Recommended: the first option is clearest for leadership, product, and engineering.

## 2. Product Concept

The unified product is a lifecycle platform for application teams and platform leaders.

It starts with the application as the root object. From that one object, a user can see and act on:

- source/repository
- build and release status
- deployment history
- environment/runtime status
- logs, metrics, traces, SLOs, events, and incidents
- infrastructure and dependency context
- cost, budget, anomaly, and optimization recommendations
- governed actions such as deploy, rollback, run runbook, scale, schedule, or optimize

**Plain-English concept:**  
An application owner should not need three products to answer three basic questions: "What changed?", "Is it healthy?", and "What is it costing?" The unified platform should answer all three in one place and help the team take the next approved action.

## 3. Target Users

| Persona | Goal | Current Pain | New Platform Experience | Value |
|---|---|---|---|---|
| Application developer | Ship and debug faster | Delivery and runtime context split across tools | View deploys, health, logs, incidents, and cost on the app page | Less waiting, faster recovery |
| SRE / operations | Reduce MTTR | Root cause requires cross-tool correlation | See incidents tied to deployments, topology, SLOs, and runbooks | Faster RCA and safer remediation |
| Platform engineer | Standardize delivery and operations | Golden paths and governance are scattered | Define app profiles, environment rules, permissions, and integrations | Consistent, governed self-service |
| FinOps / cloud owner | Control spend without blocking teams | Cost is not tied cleanly to app ownership | See spend by app/env/team and trigger optimization workflows | Accountable cloud spend |
| Engineering leader | Improve delivery, reliability, cost | No portfolio view of app health and efficiency | Review portfolio delivery, health, cost, and risk | Better operating decisions |

## 4. Unified Mental Model

```text
Organization / Business Unit
  v
Application
  |-- Repository
  |-- Services / Workloads
  |-- Environments
  |   |-- Deployments / Releases
  |-- Runtime Resources
  |   |-- Health / SLOs / Events / Incidents
  |   |-- Cost / Budget / Optimization
  |-- Actions
      |-- Deploy / rollback
      |-- Run remediation
      |-- Scale / schedule
      |-- Approve / audit
```

| Object | Definition | Product Owner |
|---|---|---|
| Application | Root ownership unit for lifecycle, health, and cost | Unified platform |
| Service/Workload | Deployable unit inside an application | BuildPiper |
| Environment | Runtime stage such as dev/stage/prod | BuildPiper/EnvOps |
| Deployment/Release | Versioned change applied to an environment | BuildPiper/DeployX |
| Runtime Resource | Cluster, VM, DB, queue, storage, or cloud resource | BuildPiper + SpendSmart |
| Event/Incident | Operational issue affecting service health | REMS |
| Cost Record | Spend/usage/budget tied to infra/app context | SpendSmart |
| Action | Governed operation triggered by human or agent | Unified platform |

## 5. Unified User Journey

```text
1. Onboard application
   BuildPiper creates app/service context.

2. Configure delivery and environments
   BuildPiper templates, EnvOps, and governance controls define how it ships.

3. Build and deploy
   DeployX runs pipelines, release packages, approvals, deployment strategies.

4. Run in environment
   KubeOps/infra modules expose runtime and resource status.

5. Monitor health
   REMS correlates metrics, logs, traces, SLOs, alerts, and incidents.

6. Understand cost
   SpendSmart attributes spend and anomalies to app/env/team/resource.

7. Investigate
   SRE agent correlates deployments, telemetry, topology, and past incidents.

8. Optimize
   FinOps agent recommends rightsizing, cleanup, scheduling, or budget actions.

9. Take governed action
   User approves deploy, rollback, runbook, scaling, scheduling, or cleanup.
```

## 6. Product Modules

| Module | Purpose | Primary User | Powered By | New Work Required |
|---|---|---|---|---|
| Application Catalog | Root app inventory and ownership | Developers, leaders | BuildPiper | Canonical model, metadata sync |
| Delivery | Build, deploy, release, rollback | Developers, DevOps | BuildPiper DeployX/GitOps | Embed health/cost gates |
| Environments | Manage dev/stage/prod and ephemeral envs | Developers, platform | BuildPiper EnvOps | Shared env identity |
| Runtime / Infrastructure | View workloads, clusters, resources | Platform, SRE | KubeOps/CloudOps/DBOps | Normalize infra-resource mapping |
| Operations / Reliability | Events, incidents, RCA, SLOs | SRE, developers | REMS | Link incidents to deployments/apps |
| Cost / FinOps | Spend, budget, anomaly, optimization | FinOps, leaders | SpendSmart | App-level attribution and actions |
| Agent Workspace | Conversational SRE/FinOps assistant | SRE, FinOps, developers | Ask Olly + REMS + SpendSmart | Unified tool context and permissions |
| Governance | RBAC, approvals, audit, policies | Admins, leaders | BuildPiper + SpendSmart approvals | Cross-product policy and audit model |
| Portfolio Dashboard | Executive view across apps | CTO, VPs, platform leads | All three | Rollup metrics and scoring |

## 7. Reuse Strategy

```text
                 Unified Application Platform
                           |
              Application Context + UX + Actions
                           |
       ┌-------------------┼-------------------┐
       v                   v                   v
   BuildPiper             REMS             SpendSmart
 Build / deploy      Health / SLO / RCA    Cost / budget /
 environments        incidents / runbooks  optimization
 governance
```

| Layer | Should Own |
|---|---|
| BuildPiper | Application onboarding, CI/CD, release packages, deployment strategies, environments, runtime/Kubernetes operations, governance |
| REMS | Telemetry correlation, SLOs, incidents, RCA, runbooks, operational war rooms |
| SpendSmart | Cost visibility, allocation, budgets, anomaly detection, rightsizing, scheduling/cleanup recommendations |
| Unified platform layer | Shared application model, cross-product context graph, integrated UX, agent orchestration, action center, portfolio views |

## 8. Differentiation

Our strongest differentiation should be:

**Application-centric DevOps + SRE + FinOps in one enterprise operating model.**

| Differentiation | Why It Is Credible |
|---|---|
| Broader than Kubernetes | BuildPiper supports modern and legacy/hybrid delivery patterns, not only Kubernetes |
| Stronger enterprise delivery depth | BuildPiper already has pipelines, release packages, approvals, audit, Jira/ServiceNow |
| Deeper SRE story | REMS offers RCA, SLOs, war rooms, runbooks, and open observability foundation |
| Stronger FinOps story | SpendSmart already focuses on cloud spend, anomaly detection, automation, and dashboards |
| Lower build effort | Core capabilities already exist; the major work is unification, not net-new tooling |
| Executive value | The platform can connect engineering velocity, reliability, and cost in one portfolio view |

## 9. MVP Concept

### Must Have

- Unified application catalog
- Application overview page
- Deployment timeline from BuildPiper
- Environment/runtime status from BuildPiper
- Health/events/incidents from REMS
- Cost summary from SpendSmart
- Shared metadata mapping across application, environment, runtime, and cost
- Basic SRE recommendation panel
- Basic FinOps recommendation panel

### Should Have

- Deployment-to-incident correlation
- Cost anomaly tied to app/environment
- SLO-aware deployment health gates
- Runbook recommendations
- Slack/Teams approval workflow
- Portfolio dashboard for leaders
- Unified audit trail

### Later

- Full composable trait system
- Autonomous remediation
- Full self-service resource catalog
- GitOps-first declarative platform model
- Third-party marketplace/plugins
- CNCF/open-source strategy

## 10. Product North Star

**One application, one lifecycle, one trusted place to ship, operate, and optimize.**

## Product Metrics Dashboard

| Metric | Definition | Source | Visualization | Target | Alert Threshold |
|---|---|---|---|---|---|
| North Star: End-to-end managed applications | % production apps with delivery, health, and cost mapped | Unified catalog | Portfolio trend | 70% in 2 quarters | Flat for 30 days |
| Deployment-to-health correlation | % deployments linked to health/SLO change | BuildPiper + REMS | Timeline | 80% | <50% |
| App-level cost attribution | % spend mapped to app/env/team | SpendSmart + catalog | Coverage chart | 75% | <50% |
| AI-assisted RCA usage | % incidents with AI RCA generated | REMS/Ask Olly | Funnel | 60% | <30% |
| Recommendation action rate | % accepted recommendations executed | Unified action layer | Funnel | 25% | <10% |
| MTTR improvement | Median MTTR vs baseline | REMS | Line | -30% | Worsens two periods |
| Cost savings realized | Verified savings from actions | SpendSmart | Trend | Customer-specific | Negative trend |

Review cadence: daily for operational health, weekly for adoption and recommendation conversion, monthly for portfolio outcomes, quarterly for strategic recalibration.

