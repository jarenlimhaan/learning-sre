# Argo CD Helm value precedence

When the same Helm key is supplied in several places, precedence from lowest to highest is:

1. the chart's own `values.yaml`;
2. files in `valueFiles` (later files override earlier files);
3. the inline string field `values`;
4. the structured field `valuesObject`;
5. `parameters` (equivalent to `--set`).

## Recommended use

- Use `valueFiles` for version-controlled environment configuration.
- Prefer `valuesObject` over the string `values` field for readable structured overrides.
- Use `parameters` sparingly for small leaf values such as an image tag.
- Pin `targetRevision` to a chart version for reproducible deployments.

```yaml
spec:
  source:
    chart: my-chart
    repoURL: https://example.com/charts
    targetRevision: 1.2.3
    helm:
      valueFiles:
        - values-common.yaml
        - values-prod.yaml
      valuesObject:
        replicaCount: 3
      parameters:
        - name: image.tag
          value: "v1.4.0"
```

Here `values-prod.yaml` overrides `values-common.yaml`; `valuesObject.replicaCount` overrides the files; and `parameters.image.tag` overrides that key everywhere else. For duplicate parameters, the last entry wins.
