## ConfigMap
- ConfigMaps can be used to store non-sensitive data in key-value pairs and decouple this data from the Pod definitions and lifecycle.
- ConfigMaps can be referenced in multiple ways:
    - Passed as environment variables
    - Passed as files via volume mounts
- Data cannot exceed 1MB in size.
- Pods must be in the same namespace as the ConfigMaps they reference.
    - Exception: when fetching values directly via the Kubernetes API.
- We can also set a ConfigMap as immutable so that it cannot be updated and must be deleted and recreated.
