## How does Kubernetes manage the resource deletion order?

- **Purpose:** Propagation policies are essential for managing and deleting resources in Kubernetes.

### Main types of propagation policies

#### Foreground

- The owner resource enters a `deletion in progress` state.
- Dependent resources (for example, pods) are deleted first.
- Finally, the owner resource is deleted.

#### Background

- The owner resource is deleted immediately.
- Dependent resources are cleaned up by the garbage collector afterward.
- This allows for immediate removal of the owner without waiting on its dependents.

#### Orphan

- The owner resource is deleted, but the dependent resources remain in the cluster, becoming `orphaned`.

### Default behavior

Do not assume one universal default for every client and resource type. For example, `kubectl delete` uses background cascading by default, while API behavior can depend on the request and resource. Set `--cascade=foreground`, `--cascade=background`, or `--cascade=orphan` explicitly when deletion order matters.


### Relation to Argo CD

Argo CD uses Kubernetes deletion propagation when pruning or deleting managed resources. The `PrunePropagationPolicy` sync option can explicitly select `foreground`, `background`, or `orphan`. `PruneLast=true` can defer pruning until other resources have synced and become healthy. These settings affect deletion behavior; they do not replace owner references or finalizers.
