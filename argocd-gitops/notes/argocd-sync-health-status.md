## Sync and Health Statuses

- Sync status refers to whether the live state, or the deployed kubernetes resources, matched the desired state, or the configuration files in the application repository 
    - This doesn't tell you whether the application is working correctly or not 
---
| Sync Status | Description |
| --- | --- |
| **Synced** | The Live State **matches** the Desired State |
| **OutOfSync** | The Live State **does not match** the Desired State. **Configuration drift** has happened. |
| **Progressing** | The `Application` is currently undergoing a sync operation. Is temporary. |

- Health status refers to whether the live state, or the deployed kubernetes resources, is in a healthy status. Argo CD has built-in health checks for different Kubernetes resources.
---
| Health Status | Description |
| --- | --- |
| **Healthy** | All the resources associated with the application are in a good state. |
| **Degraded** | At least one resource is in a failed or unhealthy state. The application might be perfectly `Synced`, but still be `Degraded`. |