## Headless Services & StatefulSets

* **The Problem:** Standard Services act as random load balancers using a single IP. Databases can't use random load balancing because you must target specific nodes (e.g., Primary vs. Replica).
* **The Solution (`clusterIP: None`):** Disables the load balancer and exposes the direct IP addresses of all underlying pods to CoreDNS.
* **How They Tie Together:**
* **StatefulSet** gives pods permanent **names** (`mysql-0`, `mysql-1`).
* **Headless Service** registers those names in **DNS** (`mysql-0.mysql`, `mysql-1.mysql`).
* **Result:** Replicas can talk directly to each other, and your app can route writes strictly to `mysql-0` and reads to `mysql-1`.