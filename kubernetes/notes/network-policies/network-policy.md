## Network Policies
- Network Policies define how pods can communicate with each other and with other network endpoints. They can specify rules for both ingress (incoming) and egress (outgoing) traffic.
- By default, all traffic between pods is allowed in Kubernetes. Network Policies provide one solution to isolate pods and reduce the attack surface in case a pod becomes compromised.
- In order for Network Policies to have any effect, the CNI plugin used in the cluster must support this feature.
    - Examples of solutions that support this include Calico, Cilium and Weave Net.
- A Network Policy includes three main configuration pieces:
    - Which pods the policy applies to.
    - [Optional] Ingress rules.
    - [Optional] Egress rules
