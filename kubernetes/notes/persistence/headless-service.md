## Headless Services and StatefulSets

A normal Service gives clients one virtual IP and distributes connections across ready endpoints. Some clustered applications instead need to discover or address individual members.

A headless Service sets `clusterIP: None`. Kubernetes does not allocate a virtual IP or load-balance through kube-proxy; DNS returns records associated with the Service's endpoints.

When paired with a StatefulSet:

- the StatefulSet gives Pods stable names such as `mysql-0` and `mysql-1`;
- `spec.serviceName` connects the StatefulSet's network identity to the headless Service;
- Pods can receive stable DNS names such as `mysql-0.mysql.<namespace>.svc.cluster.local`.

Headless discovery does not determine database roles. The database or another controller still decides which member is primary, replica, or eligible for reads and writes.
