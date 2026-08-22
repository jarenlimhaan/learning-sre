# kubectl command reference

Replace values inside `<angle-brackets>` before running a command. Add `-n <namespace>` to namespaced resources when they are not in your current namespace.

## Context and namespaces

```bash
# Check which cluster kubectl currently targets
kubectl config current-context
kubectl config get-contexts

# Switch context
kubectl config use-context <context-name>

# List namespaces and set the default namespace for the current context
kubectl get namespaces
kubectl config set-context --current --namespace=<namespace>
```

Always check the current context before applying or deleting resources.

## Create, compare, apply, and delete

```bash
# Validate and render locally without changing the cluster
kubectl apply --dry-run=client -f <yaml-file> -o yaml

# Show the difference between the file and live state
kubectl diff -f <yaml-file>

# Declaratively create or update resources
kubectl apply -f <yaml-file>
kubectl apply -f <directory>

# Delete resources declared in a file
kubectl delete -f <yaml-file>

# Generate a starter Pod manifest without creating it
kubectl run nginx --image=nginx:1.27.0 \
  --dry-run=client -o yaml > pod.yaml
```

Review `diff` before applying important changes. Be especially careful with `delete -f` and the active context.

## Inspect resources

```bash
# Common resource listings
kubectl get pods
kubectl get deployments
kubectl get replicasets
kubectl get services
kubectl get all -n <namespace>

# Include details such as node, Pod IP, and image
kubectl get pods -o wide

# Continuously watch a resource list
kubectl get pods --watch

# Inspect configuration, status, conditions, and recent events
kubectl describe pod <pod-name>
kubectl get pod <pod-name> -o yaml

# List recent namespace events in timestamp order
kubectl get events --sort-by=.metadata.creationTimestamp

# Find API resource names, short names, and whether they are namespaced
kubectl api-resources
kubectl explain deployment.spec.strategy
```

`get` gives a summary, `describe` combines useful details and events, and `-o yaml` shows the complete API object.

## Labels and selectors

```bash
# Display a label as a column
kubectl get pods -L app

# Equality-based and set-based selectors
kubectl get pods -l app=color-api
kubectl get pods -l 'environment in (dev,staging)'

# Add or replace a label
kubectl label pod <pod-name> environment=dev --overwrite
```

## Logs, shell access, and port forwarding

```bash
# Logs for a single-container Pod
kubectl logs <pod-name>
kubectl logs <pod-name> --follow

# Select a container in a multi-container Pod
kubectl logs <pod-name> -c <container-name>

# Logs from the previous crashed container instance
kubectl logs <pod-name> -c <container-name> --previous

# Execute a command or open an interactive shell
kubectl exec <pod-name> -- printenv
kubectl exec -it <pod-name> -c <container-name> -- sh

# Reach a local-only Pod or Service
kubectl port-forward pod/<pod-name> 8080:80
kubectl port-forward service/<service-name> 8080:80
```

## Deployments and rollouts

```bash
kubectl get deployments
kubectl scale deployment/<deployment-name> --replicas=<count>
kubectl set image deployment/<deployment-name> \
  <container-name>=<image>:<tag>

kubectl rollout status deployment/<deployment-name>
kubectl rollout history deployment/<deployment-name>
kubectl rollout undo deployment/<deployment-name>
kubectl rollout restart deployment/<deployment-name>
```

Imperative commands such as `scale` and `set image` change live state. If Git or YAML is your source of truth, update it too or the next apply/reconciliation may overwrite the change.

## Services and temporary debugging

```bash
# Create a Service in front of an existing Pod
kubectl expose pod <pod-name> --type=ClusterIP --port=80

# Inspect Service selectors and EndpointSlices
kubectl describe service <service-name>
kubectl get endpointslices -l kubernetes.io/service-name=<service-name>

# Start a temporary interactive debugging Pod and remove it on exit
kubectl run curl-debug --rm -it --restart=Never \
  --image=curlimages/curl -- sh
```

If a Service has no endpoints, compare its selector with the labels on ready Pods.

## ConfigMaps and Secrets

```bash
kubectl get configmaps
kubectl describe configmap <configmap-name>

# Generate YAML first so it can be reviewed
kubectl create configmap app-config \
  --from-literal=DEFAULT_COLOR=blue \
  --dry-run=client -o yaml

kubectl get secrets

# Learning example only: literal values may be saved in shell history
kubectl create secret generic db-creds \
  --from-literal=username=db_user \
  --from-literal=password=db_pass
```

Do not commit real Secret values. Base64 encoding is not encryption; use appropriate secret management and RBAC for real environments.

## Storage

```bash
kubectl get persistentvolumes
kubectl get persistentvolumeclaims --all-namespaces
kubectl describe pvc <claim-name>
kubectl get storageclasses
```

When a claim remains `Pending`, inspect its events, requested access mode, storage class, size, and available matching volumes.

## Resource usage

```bash
# Requires Metrics Server
kubectl top nodes
kubectl top pods -A

# Show requests and limits configured on a Pod
kubectl describe pod <pod-name>
```
