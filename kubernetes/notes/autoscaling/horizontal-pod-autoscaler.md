# Horizontal Pod Autoscaler (HPA)

## Automatically scale workloads horizontally

The Horizontal Pod Autoscaler changes the number of Pod replicas for a scalable workload, such as a Deployment or StatefulSet, based on observed metrics.

```text
Higher load → HPA increases replicas
Lower load  → HPA decreases replicas
```

HPA scales **horizontally** by adding or removing Pods. It does not increase the CPU or memory assigned to existing Pods; changing those values is vertical scaling.

## How it works

1. The HPA controller periodically reads metrics.
2. It compares the current metric value with the configured target.
3. It calculates a desired replica count.
4. It updates the target workload through its `scale` subresource.
5. The workload controller creates or removes Pods to reach that count.

For a resource-utilization target, the calculation is approximately:

```text
desired replicas = ceil(current replicas × current utilization / target utilization)
```

Kubernetes includes tolerances and stabilization behavior to avoid repeatedly scaling up and down because of small metric fluctuations.

## Requirements

- A metrics provider must be available. CPU and memory scaling commonly uses Metrics Server through the resource metrics API.
- Containers must normally define resource `requests` when the HPA uses percentage-based CPU or memory utilization.
- The target must expose the `scale` subresource. Deployments and StatefulSets support it; DaemonSets do not use HPA.
- The HPA and its target workload must be in the same namespace.

Check whether resource metrics are available:

```bash
kubectl top nodes
kubectl top pods
kubectl get apiservice v1beta1.metrics.k8s.io
```

## CPU scaling example

The Deployment must define a CPU request:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: color-api
spec:
  replicas: 2
  selector:
    matchLabels:
      app: color-api
  template:
    metadata:
      labels:
        app: color-api
    spec:
      containers:
        - name: color-api
          image: lmacademy/color-api:1.2.1
          resources:
            requests:
              cpu: 100m
            limits:
              cpu: 500m
```

The HPA can then target average CPU utilization:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: color-api
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: color-api
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

`averageUtilization: 70` means average CPU usage across the selected Pods should remain near 70% of their requested CPU. If each container requests `100m`, the target is approximately `70m` of average usage per Pod.

## Useful commands

```bash
# Generate a basic CPU-based HPA
kubectl autoscale deployment color-api \
  --cpu-percent=70 \
  --min=2 \
  --max=10

# Inspect current and target metrics
kubectl get hpa
kubectl get hpa --watch
kubectl describe hpa color-api

# Inspect the target workload and resource usage
kubectl get deployment color-api
kubectl top pods

# Remove the autoscaler
kubectl delete hpa color-api
```

## Metric types

With `autoscaling/v2`, an HPA can use:

- **Resource metrics:** CPU or memory usage for Pods.
- **Container resource metrics:** usage from a particular container in each Pod.
- **Pod metrics:** a custom metric averaged across Pods.
- **Object metrics:** a metric describing another Kubernetes object.
- **External metrics:** metrics from systems outside Kubernetes, such as queue depth.

Custom and external metrics require an appropriate metrics adapter; Metrics Server only supplies basic CPU and memory resource metrics.

## Scaling behavior

The optional `behavior` field controls how quickly scaling occurs and can configure stabilization windows and rate limits.

```yaml
behavior:
  scaleDown:
    stabilizationWindowSeconds: 300
  scaleUp:
    stabilizationWindowSeconds: 0
```

A longer scale-down stabilization window helps prevent premature removal of replicas during brief drops in traffic.

## Common problems

- **`<unknown>` metrics:** Metrics Server may be unavailable, Pods may be too new, or resource requests may be missing.
- **HPA does not scale:** verify that load reaches the Pods, current utilization exceeds the target, and the maximum replica count has not been reached.
- **Replicas constantly fluctuate:** configure scaling behavior and choose targets based on observed workload patterns.
- **Manual replica changes do not persist:** the HPA continues reconciling the replica count. Avoid treating `spec.replicas` as fixed while HPA manages the workload.
- **Memory scaling may not recover:** applications do not always release memory after load decreases, so memory is often a less responsive scaling signal than CPU or request-rate metrics.

HPA changes the number of application Pods. It does not add worker nodes when the cluster lacks capacity; node provisioning requires a separate node autoscaling mechanism.
