# Sync, operation, and health status

Argo CD reports separate dimensions. Do not treat `Synced` as proof that an application works.

## Sync status: does live match Git?

| Status | Meaning |
| --- | --- |
| **Synced** | The tracked live resources match the rendered desired state. |
| **OutOfSync** | At least one tracked resource differs, is missing, or should be pruned. |
| **Unknown** | Argo CD cannot determine the comparison status, often because rendering or cluster access failed. |

`Progressing` is a **health** status, not a sync status. The current sync action is reported separately as an operation phase such as `Running`, `Succeeded`, `Failed`, or `Error`.

## Health status: are the resources functioning?

Argo CD evaluates supported resource types and aggregates their health into the Application health.

| Status | Meaning |
| --- | --- |
| **Healthy** | Resources have reached their expected healthy state. |
| **Progressing** | A resource is still rolling out or waiting to become ready. |
| **Degraded** | A resource has failed or entered an unhealthy state. |
| **Suspended** | Work is intentionally paused, such as a suspended Job or Rollout. |
| **Missing** | A desired resource does not exist in the cluster. |
| **Unknown** | Health cannot be assessed. Custom resources may need a custom health check. |

Common combinations:

- `Synced + Healthy`: Git matches the cluster and resources appear healthy.
- `Synced + Degraded`: the desired configuration was applied, but the workload is failing.
- `OutOfSync + Healthy`: the current workload may be healthy, but it differs from Git.
- `Synced + Progressing`: the desired configuration is applied and the rollout is not finished yet.
