## Data plane
* **kubelet:** agent that runs on each worker node and communicates with the control plane. It ensures that containers are running in the pods as defined by the pod specifications.

* **Container Runtime:** responsible for running the containers on each worker node. Kubernetes supports several runtimes, such as Docker, containerd, or CRI-O.

* **kube-proxy:** manages network rules on each worker node. It maintains network connectivity and load balancing for services, ensuring that requests are routed to the appropriate pods.

* **Other K8s Objects:** higher-level Kubernetes objects such as ReplicaSets, Deployments, Jobs, Services, StatefulSets, among others.
