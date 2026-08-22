## Secrets
- Secrets can be used to store and inject sensitive data into containers. They help us not have to include sensitive information in application code.
- Data is stored in base64-encoded and unencrypted by default, but it is possible to set up encryption at rest for secrets.
- It's best practice to set up RBAC rules with least-privilege permissions, so only the authorized parties are allowed to retrieve or update the values of secrets.
- Accessing Secrets is similar to accessing ConfigMaps (both objects work similarly to each other from the Pod's / container's perspective), and they can be:
    - Passed as environment variables
    - Passed as files via volume mounts
- In addition to generic secrets, Kubernetes also has other specific secret types, such as ServiceAccount tokens, TLS secrets, among others.
- Cloud providers and their managed Kubernetes offerings normally provide native integration with secrets managers for improved and more secure secret storage.

Base64 only makes binary data safe to transport in YAML; anyone who can read the Secret can decode it. Avoid committing real Secret manifests, restrict `get`, `list`, and `watch` permissions, enable encryption at rest, and consider an external secret manager. Secret values injected as environment variables also remain unchanged until the container restarts.
