# Argo CD core concepts

Argo CD is a Kubernetes controller that continuously compares manifests from a source with resources in a destination cluster. It reports differences and, when requested or configured, synchronizes the cluster to the desired state.

![Argo CD architecture](../../images/arch.png)

## Main components

- **API server:** serves the UI, CLI, REST, and gRPC APIs; handles authentication and coordinates requests.
- **Repository server:** caches repositories and renders Kubernetes manifests from plain YAML, Helm, Kustomize, and supported plugins.
- **Application controller:** compares desired and live state, evaluates health, and performs sync operations.
- **ApplicationSet controller:** optionally generates many `Application` resources from templates and generators.
- **Redis:** provides disposable caching used by Argo CD components; Git and the cluster remain the sources of durable state.

## The Application CRD

![Argo CD Application custom resource](../../images/application-resource.png)

An `Application` is the declarative contract connecting a source to a destination:

1. **Project (`spec.project`):** the `AppProject` whose security boundaries apply.
2. **Source (`spec.source` or `spec.sources`):** repository/chart, revision, and path or chart name used to produce desired manifests.
3. **Destination (`spec.destination`):** cluster and namespace where resources should run.
4. **Sync policy (`spec.syncPolicy`):** optional automation and sync behavior such as pruning or self-healing.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/example/platform-config.git
    targetRevision: main
    path: apps/guestbook
  destination:
    server: https://kubernetes.default.svc
    namespace: guestbook
```

The `Application` usually lives in the Argo CD control-plane namespace. The resources it manages live in the destination namespace. An `AppProject` is separate from a Kubernetes namespace: it is an Argo CD security and organization boundary.

## Application versus workload manifests

| Concept | Argo CD `Application` | Kubernetes manifests |
| --- | --- | --- |
| Purpose | Tells Argo CD what to render, where to deploy it, and how to sync | Defines the actual workload and supporting resources |
| Typical kinds | `Application` | `Deployment`, `Rollout`, `Service`, `ConfigMap` |
| Reconciler | Argo CD application controller | The relevant Kubernetes controller |
| Location | Usually the Argo CD namespace | The destination namespace, or cluster-scoped |

Argo CD does not build container images. CI normally tests code and publishes an image; Git then records the desired image tag or digest for Argo CD to deploy.
