## Overview of Pod Security Standards
- Pod Security Standards (PSS) offer a set of guidelines that define different security contexts for pods. They can be used to ensure that pods adhere to specific security standards, reducing the risk of potential vulnerabilities and misconfigurations.
| Profile | Characteristics |
| --- | --- |
| **Privileged** | Imposes no restrictions on the pod’s security settings. This is the least restrictive policy and allows full access to system resources. It is suitable for pods that require elevated privileges, such as those that manage the cluster or perform administrative tasks. |
| **Baseline** | Enforces a minimal set of security restrictions that are suitable for general-purpose workloads. This policy allows common container configurations while preventing escalations in privilege, such as mounting sensitive host paths or modifying the host’s network settings. |
| **Restricted** | Enforces the strictest set of security standards. It is designed for applications that run in highly sensitive or regulated environments and require maximum security controls. |

## Security Levels
- Pod Security Standards (PSS) are enforced via the Pod Security Admission Controller.
    - The controller is enabled by default in later Kubernetes versions.
- The admission control uses labels for configuration, and is set on a namespace basis:
    - pod-security.kubernetes.io/<MODE>: <LEVEL>
    - Available modes:
        - enforce: blocks the creation of pods that violate the security standards.
        - warn: Shows a user-facing warning if violations occur, but still allows the pod to be created.
        - audit: Logs a security violation in Kubernetes audit logs, but still allows the pod to be created.
    - Available levels: privileged, baseline, restricted.
- We should combine PSS with other security mechanisms, such as RBAC, to properly enforce access rules to namespaces that accept privileged workloads.
- It's possible to configure the built-in admission controller to set cluster-wide defaults, and then override these defaults at the namespace level.
