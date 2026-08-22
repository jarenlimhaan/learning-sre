# Creating a pod
kubectl run nginx --image=nginx:1.27.0

# Get pods
kubectl get pods

# Describe pod
kubectl describe pod nginx

# Get logs
kubectl logs <<pod name>>

# Delete pod from cluster (default)
kubectl delete pod <<pod name>>

# Create a k8 service in front of the pod
kubectl expose pod <pod-name> \
  --type=NodePort \
  --port=80

# Check replica sets
kubectl get rs

# Check deployments
kubectl get deploy

# Check rollout history
kubectl rollout history deployment/<deployment-name>

# Undo rolling update
kubectl rollout undo deployment/<deployment-name>

# Check diff
kubectl diff -f <yaml-file>

# Scaling deployments
kubectl scale deploy <deployment-name> --replicas=n

# Describe pod
kubectl describe pod <pod-name>

# Check matching labels of pods
kubectl get pods -l <label-name>
kubectl get pods -l <key>=<val>

# Check persistent volume
kubectl get persistentvolume

# Check persistent volume claim
kubectl get pvc

kubectl delete -f <yaml-file>
