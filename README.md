# Learning SRE

Personal site reliability engineering study notes, command references, and hands-on labs covering Kubernetes, GitOps, autoscaling, and observability.

## Repository map

| Directory | Contents | Starting point |
| --- | --- | --- |
| [kubernetes/](kubernetes/) | Kubernetes concepts, CLI commands, resource manifests, Kustomize examples, and autoscaling labs | [Why Kubernetes?](kubernetes/notes/why-use-k8.md) |
| [argocd-gitops/](argocd-gitops/) | Argo CD, GitOps workflows, Helm, Argo Rollouts, and progressive delivery examples | [Argo CD and GitOps guide](argocd-gitops/README.md) |
| [observability/](observability/) | Reliability concepts, Prometheus, Grafana, Loki, Tempo, OpenTelemetry, and Docker Compose configurations | [Observability concepts](observability/notes/theories/observability.md) |

## Kubernetes

- [Study notes](kubernetes/notes/): architecture, Pods, Deployments, ReplicaSets, Services, configuration, persistence, namespaces, resource management, probes, networking, RBAC, and Pod security.
- [Command references](kubernetes/commands/): kubectl, RBAC, and Kustomize commands.
- [Resource examples](kubernetes/manifests/): individual exercises grouped by topic.
- [Kustomize notes](kubernetes/notes/kustomize/introduction.md): bases, overlays, transformations, generators, and patches; paired with the [NGINX application example](kubernetes/manifests/kustomize/nginx-app/).
- [Horizontal Pod Autoscaler notes](kubernetes/notes/autoscaling/horizontal-pod-autoscaler.md) and [HPA manifests](kubernetes/manifests/hpa/): CPU and custom queue metric examples.

### Queue autoscaling lab series

The lab stack contains a translation frontend, a worker, Redis, redis-exporter, Prometheus, kube-state-metrics, and Grafana. The exercises compare CPU-based scaling with scaling driven by a Redis queue metric exposed through prometheus-adapter.

| Order | Lab | Focus |
| --- | --- | --- |
| 1 | [Deploy the stack](kubernetes/manifests/LAB-01-setup.md) | metrics-server, starter resources, and checking the metrics pipeline |
| 2 | [CPU HPA](kubernetes/manifests/LAB-02-cpu-hpa.md) | Observe CPU utilization alongside queue backlog |
| 3 | [Grafana dashboard](kubernetes/manifests/LAB-03-grafana-dashboard.md) | Inspect queue depth and available worker replicas |
| 4 | [Prometheus adapter](kubernetes/manifests/LAB-04-prometheus-adapter.md) | Expose `translation_queue_length` through the custom metrics API |
| 5 | [Queue-based HPA](kubernetes/manifests/LAB-05-hpa-queue.md) | Observe worker scale-up and scale-down |

Supporting files:

- [starter/](kubernetes/manifests/starter/): Kustomize stack, application and monitoring manifests, Grafana provisioning, and adapter Helm values.
- [hpa-cpu.yaml](kubernetes/manifests/hpa/hpa-cpu.yaml): CPU-based worker autoscaler.
- [hpa-queue.yaml](kubernetes/manifests/hpa/hpa-queue.yaml): queue metric worker autoscaler.
- [generate-traffic.sh](kubernetes/manifests/utils/generate-traffic.sh): continuous translation requests, including deliberately invalid payloads; stop with `Ctrl+C`.
- [hpa-queue.zip](kubernetes/manifests/hpa-queue.zip): bundled lab files.

Have a local Kubernetes cluster, `kubectl`, Helm, Bash, `curl`, and `jq` available. Start with Lab 01 for the metrics-server setup. Run the lab's relative commands from:

```bash
cd kubernetes/manifests

# Preview the starter resources without applying them
kubectl kustomize starter/
```

Lab 05 describes creating a new `hpa-queue.yaml`; the repository already supplies it at `hpa/hpa-queue.yaml`. Use that path when applying or deleting the supplied file. Remove the CPU worker HPA before enabling the queue worker HPA, as described in Lab 02.

For cleanup, uninstall the adapter before deleting the starter stack, since the stack includes its `app` namespace:

```bash
kubectl delete -f hpa/hpa-queue.yaml --ignore-not-found
kubectl delete -f hpa/hpa-cpu.yaml --ignore-not-found
helm uninstall prometheus-adapter -n app
kubectl delete -k starter/
```

## Argo CD and GitOps

Follow the [existing learning guide](argocd-gitops/README.md) for the reading order and lab references. It covers GitOps foundations, sync and health, drift, deletion, Helm sources, private repositories, projects, hooks, sync waves, and progressive delivery.

The directory also includes [installation instructions](argocd-gitops/setting-up-argocd/installation.md), [command references](argocd-gitops/commands/), and [manifests](argocd-gitops/manifests/) for Applications, projects, canary and blue-green rollouts, Prometheus analysis, and traffic routing.

## Observability

- [Reliability and signal concepts](observability/notes/theories/): SLIs, SLOs, SLAs, error budgets, golden signals, logs, and traces.
- [Tool and architecture notes](observability/notes/): Prometheus queries, Loki, Tempo, and OpenTelemetry instrumentation and collection.
- [Setup notes](observability/setting-up/): cluster and SDK setup references.
- [Compose configurations](observability/compose/): application, development, production, and observability service definitions plus collector and backend configuration.
- [Test scripts](observability/test-scripts/): Loki validation and cleanup helpers.
- [Images](observability/images/): diagrams and screenshots used by the notes.

Some Compose configurations depend on application source under `app-versions/`, which is excluded from Git. Check the selected configuration's build paths before running it.

## Working with the examples

Read the relevant note or lab before applying its manifests. Directories contain separate exercises with their own namespaces, image versions, and setup requirements; run each exercise individually. These are learning configurations, including local Grafana demo credentials, and need review before use in a shared environment.
