## Limitations of Deployments in Kubernetes

Why are Deployments not always suitable for production workloads?

Traditional Kubernetes Deployments have several drawbacks when being updated:
1. Too fast and "all or nothing"

    Once a RollingUpdate begins, it proceeds as quickly as it can create new pods. There is no concept of pausing the rollout to observe the new version's behavior. If you deploy a version with a bug, the RollingUpdate can continue replacing stable pods with broken ones, escalating a small problem into a full outage.

2. "Success" is defined poorly

    A readiness probe only confirms that a pod is ready to accept traffic; it does not guarantee the application is functionally correct. As long as the readiness endpoint returns 200 OK, Kubernetes considers the pod "Ready" and continues the rollout.

3. No real traffic control

    The RollingUpdate strategy ties instance scaling directly to traffic shifting. As soon as a new pod becomes "Ready", it's added to the Service's load-balancing pool and begins receiving a portion of live user traffic.

4. Rollbacks are another risky RollingUpdate

    When you trigger a rollback, Kubernetes doesn't instantly switch back to the old version. It performs another RollingUpdate in reverse, replacing the bad pods with the previous version, which can reintroduce instability during the rollback.

Consider using advanced rollout controllers (e.g., Argo Rollouts) or service mesh traffic control for gradual, observable rollouts with automated analysis and safe rollback strategies.

## The Rollout CRD

A `Rollout` has a Pod template and selector similar to a `Deployment`, but adds canary and blue-green strategies, pauses, promotion, traffic routing, and analysis.

Migration commonly starts by changing `apiVersion` from `apps/v1` to `argoproj.io/v1alpha1`, changing `kind` from `Deployment` to `Rollout`, and replacing the strategy. Before applying it, also verify that:

- the Argo Rollouts controller and CRDs are installed;
- the selector still matches the Pod-template labels;
- Services select the intended Rollout Pods;
- no Deployment and Rollout simultaneously own the same selector;
- readiness probes and capacity settings are suitable for the chosen strategy.

## Canary rollout flow

The container image is defined in the Rollout's pod template. When the image or another pod-template field changes and the manifest is applied again, Argo Rollouts creates a new ReplicaSet instead of modifying existing pods.

For five replicas with `setWeight: 20` and `pause: {}`:

- Before the update: 5 old pods (100%).
- During the canary: 4 old pods (about 80%) and 1 new pod (about 20%).
- After promotion: 5 new pods (100%) and the old ReplicaSet is scaled down.

```bash
kubectl apply -f rollout.yaml
kubectl argo rollouts get rollout simple-color-app --watch
kubectl argo rollouts promote simple-color-app
```

Without a traffic-routing provider, the weight is approximated using the number of ready pods behind the Kubernetes Service.

## Argo CD Application vs Rollout

An Argo CD `Application` tells Argo CD where the Kubernetes manifests are stored and where to apply them:

```text
Git repository and path -> destination cluster and namespace
```

A `Rollout` is one of those manifests. It tells Kubernetes which container image to run and how to release a new version, such as with a canary deployment.

```text
Argo CD Application -> GitHub manifests -> Rollout -> Pods and containers
```

The Rollout does not point to GitHub directly. It is stored in Git and points to a container image in a registry such as Docker Hub or GHCR:

```yaml
containers:
  - name: color-app
    image: example/color-app:1.0.0
```

Typical GitOps flow:

```text
Code change -> CI builds and pushes image -> image tag updated in rollout.yaml
-> change pushed to Git -> Argo CD applies it -> Argo Rollouts performs the canary release
```
