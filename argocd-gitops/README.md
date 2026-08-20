# Argo CD and GitOps Learning Guide

This directory is a learning path for GitOps with Argo CD and progressive delivery with Argo Rollouts. Start with the foundations, then follow the numbered note folders in order.

## Recommended reading order

1. **Foundations**
   - [GitOps principles](notes/01-foundations/gitops.md) — why pull-based, continuously reconciled delivery is useful.
   - [Argo CD core concepts](notes/01-foundations/argocd-core-concepts.md) — components, Applications, desired state, and live state.
2. **Day-to-day operations**
   - [Sync and health status](notes/02-operations/sync-and-health-status.md) — how to interpret application state.
   - [Automated sync and drift](notes/02-operations/automated-sync-and-drift.md) — automation, pruning, and self-healing.
   - [Deletion and propagation](notes/02-operations/deletion-and-propagation.md) — what happens to dependent resources during deletion.
3. **Sources and packaging**
   - [Working with Helm](notes/03-sources-and-packaging/working-with-helm-charts.md) — how Argo CD renders charts.
   - [Helm value precedence](notes/03-sources-and-packaging/helm-value-precedence.md) — which override wins.
   - [Private repositories](notes/03-sources-and-packaging/private-repositories.md) — HTTPS and SSH authentication.
4. **Application orchestration**
   - [Projects, hooks, and sync waves](notes/04-orchestration/projects-hooks-and-sync-waves.md) — guardrails and deployment ordering.
5. **Progressive delivery**
   - [Deployment strategies](notes/05-progressive-delivery/deployment-strategies.md) — rolling, recreate, blue-green, and canary trade-offs.
   - [Argo Rollouts](notes/05-progressive-delivery/argo-rollouts.md) — the Rollout CRD and canary flow.
   - [Advanced traffic management](notes/05-progressive-delivery/advanced-traffic-management.md) — weighted and header-based routing.
   - [Automated analysis and promotion](notes/05-progressive-delivery/automated-analysis-and-promotion.md) — Prometheus-backed release decisions.

## Practical references

| When you want to… | Reference |
| --- | --- |
| Install Argo CD locally | [Installation guide](setting-up-argocd/installation.md) |
| Recall a CLI command | [Argo CD](commands/argocd-commands.md), [kubectl](commands/kubectl-commands.md), [Helm](commands/helm-commands.md), or [minikube](commands/minikube-commands.md) commands |
| Create a basic Argo CD Application | [Application example](manifests/application.yaml) |
| Restrict teams and destinations | [AppProject examples](manifests/projects-manifests/) |
| Try a basic canary or blue-green rollout | [Canary](manifests/canary/) or [blue-green](manifests/blue-green/) manifests |
| Add metric-based promotion | [Prometheus canary](manifests/prometheus-canary/) or [blue-green](manifests/prometheus-blue-green/) examples |
| Use weighted/header routing | [Gateway API routing example](manifests/weighted-and-header-based-routing/) |

## Mental model

```text
Git commit (desired state)
        |
        v
Argo CD Application -> renders manifests -> compares desired and live state
        |                                      |
        +---------------- syncs ---------------+
                                               v
                                      Kubernetes resources
                                               |
                         Argo Rollouts optionally controls
                         progressive release of new Pods
```

Argo CD handles continuous delivery from Git. Argo Rollouts handles how a workload moves from an old version to a new one. They complement each other, but neither replaces CI: CI still builds, tests, and publishes the container image.

## Suggested lab path

1. Install Argo CD and log in.
2. Apply a basic `Application` and observe `Synced` versus `Healthy`.
3. Change a manifest in Git and sync it.
4. Enable automated sync, then test drift correction with a harmless manual replica change.
5. Install Argo Rollouts and try blue-green, canary, and finally metric-based analysis.

> Some manifests contain learning-specific names, namespaces, repository URLs, or chart versions. Review those values before applying them to another cluster.
