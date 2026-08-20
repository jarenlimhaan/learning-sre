# Deployment strategies

## Recreate

All old Pods stop before new Pods start. This is simple but causes downtime, so it is mainly useful when old and new versions cannot run together.

## Rolling update

A Kubernetes `Deployment` gradually replaces old Pods with new Pods. `maxSurge` and `maxUnavailable` control capacity and availability. It is built in and works well for routine stateless changes, but does not provide explicit pauses, metric analysis, or precise traffic weighting.

## Blue-green

Two complete versions run at the same time:

- **active/blue** receives production traffic;
- **preview/green** is tested before promotion;
- promotion switches the active Service to the new ReplicaSet;
- rollback switches traffic back while the old ReplicaSet is still available.

![Blue-green deployment](../../images/blue-green.png)

Blue-green gives a fast traffic switch and a realistic preview environment, but temporarily requires capacity for both versions. Database changes must remain compatible with both versions during the transition.

## Canary

A small portion of traffic or replicas receives the new version first. The rollout proceeds through steps such as 10%, 25%, and 50%, with pauses or automated analysis between steps. This limits blast radius but takes longer and needs trustworthy health signals.

Without a traffic provider, Argo Rollouts approximates weights using Pod counts. With a service mesh, ingress controller, or Gateway API provider, traffic weight can be controlled separately from replica count.

## Choosing a strategy

| Need | Typical choice |
| --- | --- |
| Simplicity and no extra controller | Rolling update |
| Old and new versions cannot coexist | Recreate |
| Instant cutover and easy preview testing | Blue-green |
| Gradual exposure with metrics-based decisions | Canary |

No strategy makes an incompatible database migration safe automatically. Prefer backward-compatible, expand-and-contract schema changes.
