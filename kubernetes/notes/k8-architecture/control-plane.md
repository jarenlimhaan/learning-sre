## The control plane 
- API Server: exposes the Kubernetes API, which is the main entry point for all administrative tasks (kubectl, dashboard, etc.).
- Scheduler: responsible for placing pods on the most suitable nodes. It selects nodes based on resource availability and other constraints.
- Controller Manager: responsible for running controller processes that handle routine tasks (e.g. ReplicaSet controller, node controller, job controller, etc.).
- Etcd: distributed key-value store that stores all cluster data, including the configuration and state of the cluster. Acts as the single source of truth for the cluster's state.
- Cloud Controller Manager: enables Kubernetes to interact with the underlying cloud infrastructure. It handles tasks such as managing cloud-based load balancers, persistent storage, and node management.