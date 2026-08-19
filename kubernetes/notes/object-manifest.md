## Code Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80

```

---

## Field Descriptions

### `apiVersion`

The `apiVersion` field defines which API group and the respective version of the API is being used to create the object.

### `kind`

The `kind` field defines which exact object or resource is being managed by the configuration file. It must be supported by the specified `apiVersion`.

### `metadata`

The `metadata` field defines data that is used to uniquely identify the object, such as name, namespace, labels, and annotations. K8s may also add information to the `metadata` section.

### `spec`

The `spec` field defines the actual configuration for the objects being managed by the configuration file. The exact shape of the configuration varies according to both the `apiVersion` and the `kind` fields.