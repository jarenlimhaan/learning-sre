## Continuously monitor container health

Probes are periodic health checks performed by Kubernetes to determine the status of a container. They allow Kubernetes to manage the container lifecycle by checking whether an application has started, remains healthy, and is ready to serve traffic.

### Types of probes

- **Startup probe:** Determines whether the application has finished starting. Until it succeeds, liveness and readiness probes do not run. Repeated failure causes the kubelet to restart the container according to its restart policy.
- **Liveness probe:** Determines whether the application is still healthy. Repeated failure causes the kubelet to restart the container.
- **Readiness probe:** Determines whether the application is ready to accept traffic. Failure removes the Pod from matching Service endpoints without restarting the container.

Liveness and readiness probes continue throughout the container lifecycle after any startup probe succeeds.
