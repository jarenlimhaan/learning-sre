## Deployments

* ReplicaSets offer a great way to ensure a pre-defined number of pods are running at any given time. However, they do not provide all the needed functionality for managing applications at scale.
* Deployments offer a higher level of abstraction on top of ReplicaSets, and add the following features:
* **Rolling updates**: Gradually replace old pods with new ones without downtime, ensuring the application remains available.
* **Rollbacks**: roll back to a previous version in case something goes wrong during an update. This feature is crucial for maintaining application stability and quickly recovering from errors.
* **Declarative updates** of replicas over time.
* **History and revision control**: keep track of the history of all changes made, allowing you to view and revert to previous versions.
* **Rollout controls**: `maxSurge` and `maxUnavailable` control how quickly Pods are replaced, while readiness probes prevent unready Pods from counting as available.

A Deployment considers a rollout successful based on Kubernetes availability, not business metrics such as error rate or latency. Pauses, traffic-weighted canaries, and automated metric analysis require additional tooling such as Argo Rollouts or a service mesh.
