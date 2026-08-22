## Manage stateful applications more effectively
- StatefulSets maintain stable identities for each pod, ensuring that even if a pod is restarted, it retains its unique identity and connection to persistent storage. More specifically, StatefulSets provide:
    - A stable, unique network identity for each pod created as part of the StatefulSet.
    - Consistent and stable persistent storage associated with each pod, also across restarts.
- Ordered pod creation, scaling, and deletion, should that be necessary when working
with databases or replicated services.
- We can specify a PersistentVolumeClaim template in the Pod definition, and as long as this stays the same, the Pod will use the same claim and have access to the same data.
    - Each replica in a StatefulSet will have its own PersistentVolumeClaim, so data is not shared across different replicas.
    - By default, StatefulSet PVCs are retained when Pods or the StatefulSet are deleted. `persistentVolumeClaimRetentionPolicy` can change retention during scaling or deletion; the PV's reclaim policy then governs its backing storage.
- Pod names and networking identities are also stable, and will follow the following pattern: `<statefulset name>-<ordinal id>`
- We can use headless services to expose the pods via more stable domains.

**StatefulSets do NOT replace Deployments unless your application specifically requires persistent state or unique network identities.**

## Deployment vs. StatefulSet

* **Use Deployments UNLESS...**
* You need **individual, dedicated persistent storage** for every single pod instance.
* You need **stable, predictable hostnames** (`app-0`, `app-1`) instead of random string IDs.
* You need **ordered startup and shutdown**. This is the default `OrderedReady` behavior; `podManagementPolicy: Parallel` can relax creation/scaling order.
* You need **direct pod-to-pod networking** (using a Headless Service).



If none of those conditions apply—like in web APIs, microservices, frontends, or stateless backends—stick with a **Deployment**. They are simpler, faster to scale, and easier to manage.
