## Resource quota, limits and requests
- Resource Quotas, Requests, and Limits ensure that resources such as CPU, memory, and storage are distributed fairly among applications and prevent a single application or namespace from consuming excessive resources.

### Resource Quotas
- policy applied at the Namespace level that defines hard limits on the number of resources (such as CPU, memory, and object counts) that can be consumed by all objects within that namespace.
![alt text](../images/quota.png)

### Requests and Limits
- defined at the container level within Pods, and specify how much of each resource a container needs.
- Requests specify the minimum amount of CPU or memory a container should always have available after scheduling, and
- Limits specify the maximum amount of CPU or memory that a container is allowed to use.
![alt text](../images/limit.png)
