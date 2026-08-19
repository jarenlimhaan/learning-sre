
## 1. Imperative mgmt. with kubectl

### Main commands

```bash
kubectl create <resource> [config]
kubectl delete <resource>
kubectl expose <resource> <name>
...
kubectl [get | describe | logs ] ...

```

### Pros

* Lowest learning curve
* Commands transparently communicate changes via single word actions
* Single step to make changes to the cluster

### Cons

* Not possible to save templates for creating new objects
* No change review nor audit trail possible
* No records of what has been created / deleted (only what is in the cluster)

---

## 2. Imperative mgmt. with config files

### Main commands

```bash
kubectl create -f <filename>
kubectl delete -f <filename>
kubectl replace -f <filename>

kubectl [get | describe | logs ] ...

```

### Pros

* Configuration files can be committed, reviewed, and audited
* Files provide a template for creating new objects
* Simpler than declarative management

### Cons

* More suitable for single files, rather than directories
* Requires familiarity with the object schemas for each object being managed
* Does not persist updates made outside the configuration files

---

## 3. Declarative mgmt. with config files

### Main commands

```bash
kubectl apply -f <filename>
kubectl diff -f <filename>
kubectl delete -f <filename>

kubectl [get | describe | logs ] ...

```

### Pros

* Persists updates made to live objects even if not reflected in the configuration files
* Better support for automatically identifying necessary operations for each object

### Cons

* Highest learning curve
* Partial updates are more complex to understand and debug
* Live objects' state might not be entirely reflected in the configuration files