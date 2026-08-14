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

### Default policy

- The default propagation policy in Kubernetes is **Background**.
- If no specific policy is set, Kubernetes deletes the owner and then cleans up its dependents in the background.


### Relation to Argo CD
- These propagation policies are relevant to understanding how resource management works within the Argo CD UI and can cater to various use cases in Kubernetes.