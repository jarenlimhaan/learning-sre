# Argo CD overriding chart values precedence

When Argo CD renders a Helm chart, values can come from multiple sources. If the same key is defined more than once, the later/higher-priority source wins.

## Precedence order

From lowest to highest priority:

1. Chart `values.yaml`
2. `valueFiles`
3. `values` in the Application spec
4. `valuesObject`
5. `parameters` (`--set`)

## Practical notes

- Use `valueFiles` for environment-specific settings.
- Use `valuesObject` for structured overrides in the Application manifest.
- Use `parameters` for small, targeted overrides.
- If two sources set the same path, the highest-priority source is applied.

## Example

```yaml
spec:
	source:
		chart: my-chart
		repoURL: https://example.com/charts
		targetRevision: 1.2.3
		helm:
			valueFiles:
				- values.yaml
				- values-prod.yaml
			valuesObject:
				replicaCount: 3
			parameters:
				- name: image.tag
					value: "v1.4.0"
```

In this example:

- `values-prod.yaml` overrides `values.yaml`
- `valuesObject.replicaCount` overrides both files
- `parameters.image.tag` overrides any earlier value for `image.tag`


## Argo CD / Helm Values Precedence Hierarchy

Higher numbers indicate higher precedence. When the same parameter is defined in multiple places, values at higher levels override those at lower levels.

### Precedence Order (Lowest to Highest)

1. **The Chart's Own `values.yaml` File**
* Default values defined within the chart repository itself.
* Serves as the base fallback configuration.


2. **Files Specified Under the `valueFiles` Option**
* External value files supplied to override default values.
* **Rule:** Later entries in the `valueFiles` list take precedence over earlier entries.


3. **Entries Specified Under the `valuesObject` Option**
* Inline YAML structured object passed directly in the Argo CD `Application` specification.
* Overrides both the default `values.yaml` and any `valueFiles`.


4. **Entries Specified Under the `parameters` Option**
* Direct key-value parameter overrides (equivalent to `--set` in Helm CLI).
* Highest precedence level; overrides all preceding options.
* **Rule:** Later entries in the `parameters` list take precedence over earlier entries.