# Introduction to Kustomize

## Declaratively customize and manage Kubernetes resources

- Kustomize customizes Kubernetes manifests without requiring you to duplicate or directly modify the original YAML files.
- It follows declarative management: you describe the resources, customizations, and patches you want, and Kustomize generates the final manifests.
- It is useful for managing similar applications across environments such as development, staging, and production while reducing duplicated configuration.
- Kustomize uses standard Kubernetes YAML and is built into `kubectl`.

## Key concepts

### Bases and overlays

- A **base** contains the common resources shared by all environments.
- An **overlay** references a base and applies environment-specific customizations.
- For example, a development overlay might use one replica while a production overlay uses three replicas.

### No templating syntax

Kustomize modifies ordinary Kubernetes YAML instead of introducing template expressions. This generally makes configurations easier to read and gives Kustomize a lower learning curve than tools with full templating languages.

### Resource patching

Patches modify selected parts of a resource without copying the complete manifest. Kustomize supports strategic merge patches and JSON patches.

### Labels and annotations

Common labels and annotations can be added to multiple resources, making them easier to group, query, and track.

### ConfigMaps and Secrets

Kustomize can generate ConfigMaps and Secrets from files or literal values. This helps connect environment-specific configuration to otherwise reusable manifests.

## Kustomize versus Helm

| Dimension | Kustomize | Helm |
| --- | --- | --- |
| **Purpose** | Customize existing Kubernetes YAML by layering changes onto a base. | Package, configure, version, and distribute Kubernetes applications. |
| **Complexity** | Uses native YAML without a templating language. | Introduces Go templates, values files, charts, and release management. |
| **Customization** | Supports overlays, patches, name prefixes and suffixes, labels, and annotations. | Supports conditionals, loops, variables, and reusable templates. |
| **Typical use cases** | Environment-specific configuration and targeted patches without duplicating manifests. | Distributing applications, managing dependencies, versioning releases, and advanced parameterization. |

Kustomize is a good fit when you already have valid Kubernetes manifests and want controlled variations of them. Helm is a better fit when you need to package and distribute a configurable application as a versioned chart.

## Common commands

```bash
# Preview the rendered manifests
kubectl kustomize <directory>

# Apply a Kustomize directory
kubectl apply -k <directory>

# Show the difference from the live cluster
kubectl diff -k <directory>

# Delete the rendered resources
kubectl delete -k <directory>
```
