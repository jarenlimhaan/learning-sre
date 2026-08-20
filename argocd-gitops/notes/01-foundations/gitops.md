# GitOps foundations

GitOps uses a version-controlled repository as the source of truth for a system's desired state. An in-cluster agent such as Argo CD continuously compares that desired state with the live cluster and reconciles differences.

## Problems with a traditional push model

```text
git push -> CI builds and tests -> pipeline runs kubectl apply or helm upgrade
```

- **Configuration drift:** a manual command such as `kubectl scale` makes the live cluster differ from Git.
- **Weak auditability:** cluster state alone does not explain who requested a change, why it happened, or what was previously deployed.
- **Difficult rollback:** recovery may require finding an old artifact and rerunning a pipeline under pressure.
- **Inconsistent environments:** ad hoc changes make promotion between development, staging, and production unpredictable.
- **Broad CI credentials:** a push pipeline often needs direct, write-capable access to the cluster.

## Pull-based GitOps flow

```text
Application code change
-> CI tests and publishes a versioned image
-> configuration repository is updated with the new image tag
-> Argo CD detects the Git change
-> Argo CD compares desired and live state
-> manual or automated sync reconciles the cluster
```

Application code and deployment configuration may share a repository, but separating them can provide clearer permissions and promotion history. Use immutable image digests or unique tags; reusing a mutable tag such as `latest` weakens reproducibility.

## GitOps principles

1. **Declarative:** desired state is expressed as declarations rather than an imperative sequence of commands.
2. **Versioned and immutable:** desired state has durable version history and approved changes.
3. **Pulled automatically:** software agents retrieve desired-state declarations from the source.
4. **Continuously reconciled:** agents observe actual state and work to converge it with desired state.

GitOps does not remove CI, testing, observability, secrets management, or access control. It changes the deployment control loop and makes Git the normal place to propose and audit desired-state changes.
