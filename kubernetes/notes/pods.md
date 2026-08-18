## Pods 

* **Pods** represent a single instance of a running process in your cluster. They:
  * Encapsulate one or more containers.
  * Allow containers to share storage and network resources.
  * Provides multiple options to configure how containers are run (ports, environment variables, volumes, security configuration, among others).

* **Pods provide a higher level of abstraction** than working directly with containers, allowing multiple containers to work together if necessary.
  * Containers running in the same Pod can communicate with each other via `localhost`.
  * They can also read/write to the same volumes.

* **Pods can all communicate with each other by default in Kubernetes.**

* We can set **health probes** in each container so that they are restarted or stop receiving traffic if considered unhealthy.
