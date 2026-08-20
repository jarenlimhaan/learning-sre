## Annotations
- Annotations are key-value pairs attached to Kubernetes objects, just like labels. Unlike labels, annotations are not supposed to store identifying metadata, and they are often used by tools or the Kubernetes system itself.
- Common use-cases:
    - Tool-specific metadata and configuration: external tools (monitoring systems, logging agents) leverage annotations to attach custom data to resources (configurations, metrics collection endpoints, etc.)
    - Configuration for ingress controllers: used to configure ingress controllers (traffic routing, SSL termination, security settings). For example, you can define path rewrites or custom load balancing rules via annotations.
    - Storing build and version information: annotations can store metadata such as build timestamps, version numbers, or Git commit hashes.
    - Runtime configuration for operators: Kubernetes operators or controllers can use annotations to customize runtime behavior for specific resources.
