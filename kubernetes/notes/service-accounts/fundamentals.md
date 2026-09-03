## Interact with Kubernetes clusters from within applications and services
- When applications or processes inside Kubernetes pods need to perform actions on resources (such as reading configmaps, writing logs, or scaling deployments), they need permissions to access the Kubernetes API. This access is handled through service accounts rather than traditional user credentials.
- Service accounts authenticate via JWT tokens, which are automatically generated and mounted by Kubernetes inside each pod using a service account.
    - Mounted under /var/run/secrets/kubernetes.io/serviceaccount/token
    - We can use this token to authenticate our requests against the Kubernetes API.
- Some key differences between service accounts and user accounts:
    - User accounts are for humans. Service accounts are for application processes, which (for Kubernetes) run in containers that are part of pods.
    - User account names must be unique across all namespaces of a cluster. Service accounts are namespaced: two different namespaces can contain ServiceAccounts that have identical names.
    - User and Service accounts have different lifecycles, and Service Accounts are more lightweight and easier to create. This makes it easier for workloads to follow the principle of least privilege.