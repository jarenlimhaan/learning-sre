## Can't we get by with just Docker?

* Docker and Docker Compose are useful for individual hosts and development environments, but they do not by themselves provide cluster-wide scheduling and reconciliation across many machines.

| Challenge                | Docker                                                                                                                       | Kubernetes                                                                                                                               |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Container Scheduling** |  Manual assignment of containers to servers<br> Time consuming and error-prone                                             |  Declarative definition of criteria for node (server) assignment<br> Handled automatically by Kubernetes based on resource definitions |
| **Load Balancing**       |  No built-in load balancing                                                                                                 |  Services provide built-in load balancing across Pods                                                                                   |
| **Scaling Applications** |  Lacks automation for horizontal scaling<br> Requires low-level adjustments for directing load to newly created containers |  Horizontal Pod Autoscaler can scale pods horizontally and automatically through real-time metrics                                      |
| **Self-Healing**         |  Lacks automation for detecting and restarting unhealthy workloads                                                          |  Health checks and continuous monitoring allow for considerably more robust self-healing mechanisms                                     |
| **Service Discovery**        |  No built-in solution for service discovery at scale                                                                           |  Internal DNS allows for robust service discovery mechanisms through Services     |
| **Configuration Management** |  Requires low-level configuration and adjustments, and can become cumbersome for larger applications and multiple environments |  ConfigMaps and Secrets allow decoupling and scaling of application configuration |

## What is Kubernetes?

* **Open-source tool**, which has become the **de facto standard for container orchestration**.

  * Kubernetes is designed to be easily extensible, and as a result the K8s ecosystem has grown to thousands of different tools.

* **Goal:** address the complexities of **running containers in production**. As such, the tool is designed around:

| Principle       | Description                                                                      |
| --------------- | -------------------------------------------------------------------------------- |
| **Automation**  | Reduces the need for manual intervention in deploying and managing applications. |
| **Declarative** | Declare the final, desired state and let Kubernetes figure out how to get there. |
| **Scalability** | Effortlessly scales applications horizontally to meet demand.                    |
| **Resilience**  | Ensures that applications are always available, even in the face of failures.    |

* A Kubernetes cluster has many components, and they are either placed in the **control plane** or in **worker nodes**:

  * **Control plane:** It's the brain of the cluster, containing the components responsible for managing the entire system.
  * **Worker nodes:** They are the machines where your applications actually run.
