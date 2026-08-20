# Argo CD command reference

```bash
# Connect to a locally port-forwarded API server
argocd login localhost:8080 --username admin --insecure

# Inspect the active context and applications
argocd context
argocd app list
argocd app get <app-name>

# Compare and synchronize
argocd app diff <app-name>
argocd app sync <app-name>
argocd app wait <app-name> --sync --health

# View application history and roll back to a history ID
argocd app history <app-name>
argocd app rollback <app-name> <history-id>
```

Run `argocd app diff` before a manual sync when you want to review the exact change. In a GitOps workflow, prefer reverting Git over making long-lived imperative changes through the CLI.
