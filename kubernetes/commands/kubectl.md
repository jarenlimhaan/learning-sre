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
