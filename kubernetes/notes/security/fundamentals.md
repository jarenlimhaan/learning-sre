## Security Fundamentals
| Risk | Common Causes |
| --- | --- |
| **Exposed Control Plane and API Access:** Unauthorized access to the Kubernetes API server can compromise the entire cluster. | API server accessible over the internet without proper auth.Weak or default credentials being used. |
| **Insecure Workloads:** Running containers as root or with excessive privileges can allow for privilege escalation and other attacks. | Containers running with default root user or running in privileged mode.Mounting sensitive host directories into containers via volumes. |
| **Overly Permissive Roles:** Roles with excessive permissions can lead to data exposure if compromised. | Assigning broad permissions to users or service accounts.Lack of role separation and adherence to the principle of least privilege. |
| **Lack of Network Segmentation and Pod Isolation:** Attackers can move laterally across services and pods once inside the network. | Default network settings allow unrestricted communication between pods.Absence of Network Policies to control traffic flow. |
| **Unsecured Data at Rest and In Transit:** Data stored in etcd or persistent volumes can be accessed if not encrypted. | Default settings without encryption enabled.Use of insecure communication protocols. |

## Security aspects of running k8 clusters
Kubernetes offers many constructs we can use to tackle several of these problems (the list below is not extensive! There are many more tools we can leverage in the Kubernetes ecosystem):
- Role-Based Access Control (RBAC): allows us to apply the principle of least privilege by defining roles and binding them to specific subjects.
- Network Policies: allow us to control traffic flow between pods and network endpoints. We can start by denying all traffic, and then allow only the necessary traffic to flow between Pods.
- Encryption at Rest: allows us to protect the data stored in the cluster's etcd store and persistent volumes.
- Pod Security Standards (PSS): allows us to enforce, warn, or audit certain security standards on a namespace basis when
creating new pods.
- Image Security: allows us to leverage images that have as few as possible known vulnerabilities, and to swiftly
accommodate security patches.