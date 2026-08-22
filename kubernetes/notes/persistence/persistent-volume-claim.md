## Persistent Volume Claims
- A PersistentVolumeClaim is what actually reserves a PV (when using static provisioning) or creates and reserves a PV (when using dynamic provisioning).
- Claims are bound to a single PersistentVolume, and a PersistentVolume can have at most one claim bound to it.
- We can set specific criteria in a claim, so that only PVs that match these criteria are considered for binds.
- Be mindful of the possibility for extra unused capacity!
- Reclamation policies can be Retain, Delete, or Recycle (deprecated).
- Access modes:
    - ReadWriteOnce: the volume can be mounted as read-write by a single node, and can be used by any number of Pods within that node.
    - ReadOnlyMany: the volume can be mounted as read-only by many nodes.
    - ReadWriteMany: the volume can be mounted as read-write by many nodes.
    - ReadWriteOncePod: the volume can be mounted as read-write by a single Pod.


### tldr
Instead of writing a manual `PersistentVolume` (PV) for every individual disk, the workflow operates like this:

#### How It Works in Practice

1. **StorageClass (Centralized Pool)**: The cluster admin sets up a template that points to a cloud or enterprise storage system (e.g., AWS EBS, GCP Persistent Disk, Ceph).
2. **PVC (The Request)**: The developer requests a specific size and access mode (e.g., *"Carve me out 10GB from the fast-storage pool"*).
3. **Automatic Carving**: Kubernetes talks directly to the storage provider, provisions a new 10GB disk, formats it, attaches it to the host node, and creates the underlying PV automatically.
4. **Pod Mount**: The Pod claims that carved-out volume and mounts it to a local path (like `/mnt/local`).

*In static/local setups, you define every single physical volume manually. In production, you define the centralized storage engine once, and Kubernetes automatically carves out individual volumes on-demand whenever a claim is submitted.*