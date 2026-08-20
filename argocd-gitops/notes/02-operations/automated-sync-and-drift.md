# Automated sync and drift correction

Automated sync lets Argo CD apply a new desired state without a person clicking **Sync**. A common policy is:

```yaml
spec:
  syncPolicy:
    automated:
      enabled: true
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

## What each option does

- `enabled`: turns on automated sync. Omitting it also enables automation when `automated` is present; setting it explicitly is clearer.
- `prune`: deletes tracked resources that were removed from Git. Without it, obsolete live resources remain.
- `selfHeal`: resyncs when a managed live resource drifts from Git, even when the Git revision has not changed.
- `CreateNamespace=true`: creates `spec.destination.namespace` when it does not exist. This is a sync option, not part of `automated`.

## Safety considerations

- Protect the deployment branch with review and required checks; automated sync makes Git approval the deployment gate.
- Test pruning carefully. A wrong path, ref, or manifest deletion can remove resources from the cluster.
- Use an `AppProject` to restrict allowed repositories, destinations, and resource kinds.
- Use sync windows when deployments must only occur at approved times.
- Automatic sync does not automatically mean automatic rollback. A failed sync is reported; recovery normally requires correcting or reverting Git. Argo Rollouts can abort application traffic promotion based on analysis.
- By default, automated sync does not repeatedly retry the same failed commit forever. A new commit or an explicit retry may be needed after fixing an external cause.

## Drift example

```text
Git says replicas: 3 -> operator manually changes live value to 5
-> Application becomes OutOfSync
-> selfHeal enabled: Argo CD restores replicas: 3
```

Self-healing applies only to resources and fields Argo CD tracks. Configure `ignoreDifferences` for fields intentionally controlled by another controller, and use that feature narrowly so it does not hide meaningful drift.
