winget install Helm.Helm

helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

helm upgrade argocd argo/argo-cd --version 8.6.0 --install --create-namespace -n argocd

winget install ArgoProj.ArgoRollouts

