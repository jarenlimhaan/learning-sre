# Kustomize Transformations

## Straightforward ways of customizing resources

Transformations are fields in `kustomization.yaml` that customize included resources without requiring a separate patch for each resource. They can be used in a base or an overlay.

| Field | Example | Effect |
| --- | --- | --- |
| `namespace` | `namespace: dev` | Set the namespace for namespaced resources. |
| `namePrefix` | `namePrefix: dev-` | Prepend a string to resource names. |
| `nameSuffix` | `nameSuffix: -01` | Append a string to resource names. |
| `commonLabels` | `app: nginx`, `tier: frontend` | Add common labels to resources and supported selector fields. |
| `commonAnnotations` | `project: ecommerce`, `team: finance` | Add common annotations to resources. |
| `images` | `name`, `newName`, `newTag`, `digest` | Change matching container image names, tags, or digests without creating patches. |

## Namespace

```yaml
namespace: dev
```

Assigns namespaced resources, such as Deployments and Services, to `dev`. Cluster-scoped resources do not become namespaced. This field does not create the namespace; create it separately or include a Namespace manifest in `resources`.

## Name prefix and suffix

```yaml
namePrefix: dev-
nameSuffix: -01
```

A resource named `nginx` becomes `dev-nginx-01`. Kustomize also updates recognized references to renamed resources.

## Common labels

The slide uses this syntax:

```yaml
commonLabels:
  app: nginx
  tier: frontend
```

These labels are added to resource metadata and supported selectors and Pod templates. Selector changes affect which Pods a Service or workload selects, so choose these labels carefully.

## Common annotations

```yaml
commonAnnotations:
  project: ecommerce
  team: finance
```

Adds the same annotations to resources. Annotations store descriptive metadata and are not used as label selectors.

## Images

```yaml
images:
  - name: nginx
    newName: example/nginx
    newTag: "1.27.0"
```

- `name` identifies the image to match in the resource manifests.
- `newName` replaces the image name or repository; it can be omitted when only changing the tag.
- `newTag` sets the image tag. The version above is an example from the slide.
- `digest` can specify an image digest instead of selecting a version by tag.

## Example development overlay

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - ../../base

namespace: dev
namePrefix: dev-
nameSuffix: -01

commonAnnotations:
  project: ecommerce
  team: finance

images:
  - name: nginx
    newTag: "1.27.0"
```

Preview the output from the repository root:

```bash
kubectl kustomize kubernetes/manifests/kustomize/overlays/dev
```

This command renders the existing overlay on disk. The example above illustrates fields you can add to it.

See [Bases and Overlays](bases-and-overlays.md) for how environments share a common resource configuration.
