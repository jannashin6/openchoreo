# 01 - OpenChoreo Product Teardown

## Executive Summary

OpenChoreo is an open-source, Kubernetes-native Internal Developer Platform (IDP). It lets developers and AI agents build, deploy, observe, and operate applications, resources, and agentic workloads through higher-level platform abstractions rather than direct Kubernetes primitives.

The product exists because Kubernetes is powerful but too low-level for most application teams. OpenChoreo gives developers self-service workflows while giving platform teams control over infrastructure, environments, policies, observability, and reusable golden paths.

**So what:** OpenChoreo is not simply a CI/CD tool. It is a platform operating model: developer intent enters through a portal, CLI, API, GitOps, or agent interface; the control plane converts that intent into runtime, workflow, networking, security, and observability outcomes.

**Evidence:** OpenChoreo describes itself as a developer platform for Kubernetes with development and platform abstractions, Backstage-powered portal, CI/CD, GitOps, observability, and AI agents. Its architecture docs define a multi-plane model covering control, data, workflow, observability, and experience planes.

## Target Users

| Persona | Needs | Problems Today | What They Do In OpenChoreo | What Is Abstracted |
|---|---|---|---|---|
| Application developer | Ship services without owning platform plumbing | Kubernetes, CI/CD, secrets, endpoints, observability setup | Create projects/components, configure endpoints/dependencies, trigger builds/releases, view status | Kubernetes manifests, networking, pipelines, observability wiring |
| Platform engineer | Provide paved roads without writing bespoke glue | DIY IDPs are costly to build and maintain | Define ComponentTypes, Traits, Workflows, Environments, DataPlanes, ObservabilityPlanes | Repeated platform scaffolding, per-team bespoke templates |
| DevOps / release engineer | Standardize build, deploy, promotion, and GitOps workflows | Fragmented CI/CD and promotion logic | Configure workflow planes, pipelines, release bindings, external CI integrations | Pipeline engine details, promotion state, release rendering |
| SRE / operations | Investigate incidents using platform context | Logs, metrics, traces, deployments, and ownership live apart | Use observability plane, alert rules, SRE agent, domain-centric querying | Cross-tool correlation and raw telemetry hunting |
| Engineering manager | Understand service health and delivery progress | No single view of application lifecycle | Review project/application status, release progress, reliability signals | Tool-by-tool status collection |
| Platform administrator | Govern access and topology | Hard to enforce consistent boundaries | Manage namespaces, RBAC/ABAC, planes, environments, identity integration | Low-level access and cluster exposure management |

**Observation:** The personas are validated by OpenChoreo’s documented experience plane for platform teams, development teams, SREs, and agents, plus its platform/developer API split.

## Core Product Model

OpenChoreo’s model has two layers: platform abstractions and developer abstractions.

```text
Namespace
  |-- Project
  |   |-- Component
  |   |   |-- Workload / endpoints / dependencies
  |   |   |-- ComponentRelease
  |   |   |-- ReleaseBinding -> Environment
  |   |-- Resource
  |       |-- ResourceRelease
  |       |-- ResourceReleaseBinding -> Environment
  |-- Environment -> DataPlane
  |-- WorkflowPlane
  |-- ObservabilityPlane

Platform templates:
  ComponentType + Trait + Workflow + ResourceType
```

| Concept | Meaning | Why It Matters |
|---|---|---|
| Namespace | Organizational boundary for platform resources | Enables isolation and scoped platform configuration |
| Project | Logical application/team boundary | Groups components and resources |
| Component | Deployable software unit | Developer-facing unit of delivery |
| ComponentType | Platform-defined workload template | Encodes golden paths |
| Trait | Optional operational behavior attached to workloads | Makes capabilities composable |
| Environment | Deployment stage linked to a data plane | Separates dev/stage/prod behavior |
| DataPlane | Kubernetes runtime cluster | Runs workloads and enforces runtime semantics |
| WorkflowPlane | Build/GitOps/resource workflow execution layer | Runs automation without coupling it to the control plane |
| ObservabilityPlane | Logs, metrics, traces, alerts | Gives domain-centric operations visibility |
| ReleaseBinding | Pins immutable release to environment | Supports controlled promotion and reduced drift |

**So what:** The product model is the moat. OpenChoreo is not differentiated by having individual tools; it is differentiated by the relationships between application, release, environment, runtime, observability, and governance.

## End-To-End User Journey

| Step | User Action | OpenChoreo Capability | System Action | Output | Dependency |
|---|---|---|---|---|---|
| 1 | Create or select namespace/project | Platform and developer APIs | Creates logical ownership boundary | Project workspace | Platform topology |
| 2 | Create component | Component + ComponentType | Validates component intent against platform template | Component definition | ComponentType |
| 3 | Configure workload | Workload, endpoints, dependencies, traits | Resolves dependencies and operational behavior | Deployable intent | Traits, resources |
| 4 | Build | WorkflowPlane or external CI | Runs CI/build workflow or consumes external artifact metadata | Image/artifact metadata | Argo Workflows, external CI, registry |
| 5 | Release | ComponentRelease | Creates immutable release snapshot | Release candidate | Component state |
| 6 | Deploy/promote | ReleaseBinding | Pins release to environment; renders runtime resources | Environment deployment | Environment, DataPlane |
| 7 | Run | DataPlane | Applies Kubernetes resources, gateway rules, policies | Running workload | Kubernetes, modules |
| 8 | Observe | ObservabilityPlane | Collects logs, metrics, traces, alerts enriched with domain metadata | Health/telemetry view | Observability modules |
| 9 | Operate | Portal, CLI, APIs, MCP, agents | Humans or agents investigate and act within permissions | RCA, remediation, cost optimization | Authz, observability data |

## Major Capability Areas

| Capability | What It Does | Primary User | Why It Matters | Evidence |
|---|---|---|---|---|
| Application/component management | Manages projects, components, endpoints, dependencies | Developers | Creates a higher-level app model above Kubernetes | OpenChoreo GitHub and docs |
| CI/build workflows | Runs CI through workflow plane or integrates external CI | Developers, DevOps | Keeps build automation connected to platform state | Architecture docs |
| Deployment/promotion | Uses immutable releases and bindings to environments | DevOps, platform | Reduces drift and clarifies promotion | Platform abstraction docs |
| Environment management | Maps environments to data planes | Platform engineers | Supports dev/stage/prod and multi-cluster topologies | Deployment topology docs |
| Runtime management | Runs workloads in data planes | Platform engineers | Separates control from execution | Architecture docs |
| Configuration/secrets | Handles environment-specific configuration and secret references | Developers, platform | Keeps sensitive values out of status outputs | ResourceReleaseBinding docs |
| Networking | Gateway topology, endpoints, network policy, optional Cilium guard module | Platform, SRE | Standardizes service exposure and isolation | Architecture docs |
| Observability | Logs, metrics, traces, alerting through observability plane | SRE, developers | Enables domain-centric querying | Observability docs |
| Security/access control | OIDC auth, RBAC, ABAC, hierarchical access | Admins | Required for enterprise governance | Architecture docs |
| Developer experience | Backstage portal, CLI `occ`, APIs, GitOps, MCP | Developers and agents | Supports multiple user modes | Architecture docs |
| Extensibility | Modules, adapters, CRD-based APIs, Backstage plugins | Platform engineers | Lets teams integrate existing tools | Architecture docs |
| AI agents | SRE and FinOps agents plus MCP access | SRE, FinOps, platform | Turns platform context into guided operations | OpenChoreo website and architecture docs |

## UX / Product Experience Teardown

OpenChoreo’s UX decision is to make the application/platform model visible while hiding Kubernetes implementation detail. The portal is Backstage-based and extended for OpenChoreo concepts; the CLI and APIs expose the same underlying model.

| UX Pattern | Product Interpretation | Leadership Takeaway |
|---|---|---|
| Backstage-powered portal | Uses a familiar IDP shell instead of inventing a portal from scratch | Faster adoption for platform teams already aligned to Backstage |
| Domain objects over raw infra | Users see projects, components, environments, releases | Reduces cognitive load for developers |
| Multi-interface access | UI, CLI, API, GitOps, MCP | Serves humans, automation, and agents from the same model |
| Observability by domain | Logs/metrics/traces enriched with namespace/project/component context | Makes ops data actionable for app teams |
| Agent entry points | SRE/FinOps agents operate through MCP and permissions | Makes AI a governed platform actor |

## Architecture / Technical Product Model

OpenChoreo is organized into planes:

```text
Experience Plane
  Portal / CLI / API / GitOps / MCP / Agents
        v
Control Plane
  API server + controllers + authz + desired state
        v
Data Plane(s)        Workflow Plane(s)        Observability Plane(s)
  Runtime workloads    CI/GitOps workflows      Logs / metrics / traces / alerts
```

The Control Plane is the central orchestrator. Data, workflow, and observability planes connect back to it using secure outbound connections. The control plane reconciles desired state while data planes run workloads, workflow planes run automation, and observability planes provide telemetry APIs and MCP access.

**So what:** The architecture is designed for separation of concerns: platform control, runtime execution, automation, and telemetry can scale and evolve independently.

## What OpenChoreo Abstracts

| Underlying Complexity | OpenChoreo Abstraction | User Experience |
|---|---|---|
| Kubernetes deployments, services, policies | Component + ComponentType + Trait | "Create a component and choose capabilities" |
| Multi-cluster runtime topology | Environment + DataPlane | "Deploy to dev/stage/prod" |
| CI engine setup | WorkflowPlane + Workflows | "Run build/deploy workflow" |
| GitOps state management | Declarative APIs + Git source of truth option | "Manage desired state declaratively" |
| Observability setup | ObservabilityPlane + Observer API | "View logs/metrics/traces by app context" |
| Service networking | Endpoints, gateway topology, network policies | "Expose this service safely" |
| Infra provisioning | ResourceType + Resource + bindings | "Request a resource" |
| Operational diagnosis | SRE Agent | "Explain and remediate incident" |
| Cost optimization | FinOps Agent | "Explain budget alert and optimize" |

## Product Strengths

1. **Clear product model:** The component/environment/release/resource model creates a coherent IDP language.
2. **Modular architecture:** Planes keep runtime, workflow, and observability concerns decoupled.
3. **Extensibility:** APIs, CRDs, modules, adapters, and Backstage plugins let teams integrate existing stacks.
4. **Governed self-service:** Developers get autonomy while platform teams define templates, policies, and boundaries.
5. **Agent-native direction:** MCP and built-in SRE/FinOps agents make operations and cost use cases part of the platform, not an afterthought.

## Product Weaknesses / Limitations

| Limitation | Evidence / Basis | Why It Matters |
|---|---|---|
| Operational complexity | Multi-plane Kubernetes deployment with optional modules | Adoption requires serious platform maturity |
| Kubernetes-centric | Product is explicitly a developer platform for Kubernetes | Less natural for enterprises with significant VM/mainframe/non-Kubernetes workloads |
| Early ecosystem maturity | GitHub shows active issues/PRs and recent v1.x releases | Buyers may need confidence in stability/support |
| Learning curve | Many new concepts: planes, bindings, traits, cells, resource types | Product needs strong onboarding and mental model education |
| External CI/observability integration still needs work | Docs support adapters/external systems but require integration | Enterprises with existing tools may face integration effort |

## Product Takeaways

### What Is Worth Adopting

Adopt the application-centric platform model: a unified relationship between application, release, environment, runtime, health, cost, and action.

### What To Avoid Copying

Do not copy OpenChoreo’s Kubernetes-first abstraction stack unless it directly fits our installed base. Our opportunity is broader because BuildPiper already spans microservices, hybrid infrastructure, release engineering, and enterprise governance.

### Real Product Moat

The moat is not "CI/CD plus observability." It is the unified control model that turns fragmented delivery, operations, and cost signals into one governed product experience.

### Table Stakes

Application catalog, CI/CD, environment management, runtime visibility, logs/metrics/traces, RBAC, secrets, audit, APIs, integrations.

### Differentiators

AI SRE workflows, FinOps recommendations, domain-enriched observability, governed self-service resources, and agent interfaces grounded in platform context.

