# Bases and Overlays

## Compose and customize different environments

- A **base** is a set of common Kubernetes resources, such as Deployments, Services, and ConfigMaps, shared across multiple environments.
- Bases are reusable and provide the foundation for environment-specific configurations.
- An **overlay** references a base and layers environment-specific changes on top of it, such as replica counts, image tags, or additional resources.
- When building an overlay, Kustomize combines the base resources with the overlay's customizations to generate the final manifests. The base files remain unchanged.

## Directory structure

```text
proj/
├── base/
│   ├── kustomization.yaml
│   ├── deployment.yaml
│   └── service.yaml
└── overlays/
    ├── dev/
    │   ├── kustomization.yaml
    │   └── dev-replicas.yaml
    └── prod/
        ├── kustomization.yaml
        └── prod-replicas.yaml
```

## Define the shared base

`proj/base/kustomization.yaml`:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - deployment.yaml
  - service.yaml
```

The base lists the shared resource manifests without needing to know which overlays will use them.

## Customize each environment

`proj/overlays/dev/kustomization.yaml`:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - ../../base

patches:
  - path: dev-replicas.yaml
```

`proj/overlays/prod/kustomization.yaml`:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - ../../base

patches:
  - path: prod-replicas.yaml
```

Paths are relative to the `kustomization.yaml` file that contains them. Both overlays reference the same base, but each applies its own patch.

For a base Deployment named `nginx`, `dev-replicas.yaml` can contain:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 1
```

The production patch uses the same resource identity with `replicas: 3`. The patch changes the replica count while retaining the other Deployment fields from the base.

## Preview and apply

Run from the directory containing `proj/`:

```bash
# Render the development manifests
kubectl kustomize proj/overlays/dev

# Apply the production configuration
kubectl apply -k proj/overlays/prod
```

In this repository, the corresponding overlay directories are `kubernetes/manifests/kustomize/overlays/dev` and `kubernetes/manifests/kustomize/overlays/prod`.

See [Transformations](transformations.md) for customizations that can be declared directly in `kustomization.yaml`.
