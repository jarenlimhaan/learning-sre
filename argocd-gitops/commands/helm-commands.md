helm upgrade --install argocd argo/argo-cd \
  --namespace argocd \
  --create-namespace \

helm upgrade argo-rollouts argo/argo-rollouts --install --namespace argo-rollouts --create-namespace

## Prometheus 
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade --install prometheus prometheus-community/prometheus \
  --version 27.49.0 \
  --namespace monitoring \
  --create-namespace \
  --values values.yaml