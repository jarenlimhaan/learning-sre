The drawbacks of traditional push models
- git push -> (ci/cd - build, test, audit, push) -> helm upgrade / kubectl apply
- Configuration drift
    - what happens when a developer runs kubectl scale deployment my-app --replicas=0
    - The live state of the cluster is not different from the configuration stored in git. Nobody knows what the real desired state is, git says one thing but the cluster says another?
- Poor auditability 
    - How do you know who scales the deployment and why? there's little to no audit trail. The source of truth is just the current state of the cluster with no history 
- Difficult rollbacks 
    - **Reverting a bad deployment is often not straightforward, especially when multiple services are involved.** It often requires a high-stress, manual process of finding the last good artifact and re-running a pipeline. This is slow and highly prone to human error during an outage.
- Inconsistent Deployments and Configurations
    - Since configuration might drift, it becomes increasingly harder to have a clear view of each environment configuration, also opening the door to bigger problems when promoting a certain release from a lower to a higher environment.

The Gitops workflow 
- How does the gitop flow tackles these issues 
    - git push -> (ci/cd) -> (updates the k8 configuration files in) different configuration repo (best pratice but can be done within same repo)
    - ArgoCD 
        - looks at the repo continously and check against the live cluster then compare with the repo since it contains the desire state and monitor for any changes, and when there are changes: *Should any difference be identified, marks the resources as out-of-sync and, if configured to do so, automatically reverts back to the state of the configuration repo.*

The Gitops Principle
- Declarative: A system managed by Gitops must have its desired state expressed declaratively 
- Versioned & Immutable: The desired state is stored in a way that enforces immutability, versioning and retains a complete version history 
- Pulled automatically: software agents automatically pull the desired state declarations from the source.
- Continous Reconciled: software agents continously observe the actual system state and attempt to apply the desired state 