# Local Argo CD installation with Helm

These commands target a local Kubernetes cluster. Ensure `kubectl config current-context` points to the intended cluster before installing.

## Prerequisites

Install `kubectl`, Helm, and the Argo CD CLI. On Windows with WinGet:

```powershell
winget install Kubernetes.kubectl
winget install Helm.Helm
winget install ArgoProject.ArgoCD
```

## Install Argo CD

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

helm upgrade --install argocd argo/argo-cd \
  --namespace argocd \
  --create-namespace \
  --version 8.6.0
```

The pinned chart version makes the lab reproducible. Check for an appropriate newer version before using this in a maintained environment.

```bash
kubectl get pods -n argocd
kubectl port-forward service/argocd-server -n argocd 8080:80
```

In another terminal, retrieve the initial password and log in:

```bash
argocd admin initial-password -n argocd
argocd login localhost:8080 --username admin --insecure
```

Change the initial admin password after first login. For a non-local environment, expose Argo CD with TLS and use SSO/RBAC instead of relying on port forwarding and the built-in admin account.

## Optional: install Argo Rollouts

Argo Rollouts is a separate controller; installing Argo CD does not install it.

```bash
helm upgrade --install argo-rollouts argo/argo-rollouts \
  --namespace argo-rollouts \
  --create-namespace
```

Install the matching `kubectl argo rollouts` plugin by following the upstream instructions for your operating system, then verify with `kubectl argo rollouts version`.
