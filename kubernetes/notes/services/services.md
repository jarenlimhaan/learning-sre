## Services

A Service provides a stable virtual IP and DNS name for a changing set of backend Pods. Its selector normally determines the ready Pods placed into EndpointSlices. The Service sends traffic to those endpoints; it does not create or monitor the Pods itself.

![Service routing traffic to Pods](../images/image.png)

## Types of service
**Abstract pod details to facilitate connectivity**

| Service Type | Behavior | When to use |
| --- | --- | --- |
| `ClusterIP` | Provides a cluster-internal virtual IP; this is the default type. | Internal service-to-service communication and backends exposed through Gateway or Ingress. |
| `NodePort` | Opens the same port, normally from `30000-32767`, on every node and forwards it to the Service. | Labs or integrations that need direct `<NodeIP>:<NodePort>` access. |
| `LoadBalancer` | Requests an external load balancer from a supported cloud/controller implementation. Usually builds on `NodePort`. | Direct external TCP/UDP exposure where a load-balancer integration exists. |
| `ExternalName` | Returns a DNS `CNAME` for an external hostname. It has no selector, virtual IP, proxying, or port forwarding. | Giving in-cluster clients a Kubernetes-style DNS alias for an external name. |

Within the same namespace, clients can use `<service-name>`. Across namespaces, use `<service-name>.<namespace>` or the full name `<service-name>.<namespace>.svc.cluster.local`.
