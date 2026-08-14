## Which signals to use

- How these signals complement each other

## Three Pillars of Observability

### Metrics

- Dashboards showing system health
- Alerts on abnormal conditions
- Capacity planning and trending
- Aggregated views across instances

### Logs

- Detailed error messages
- Business logic debugging
- Audit trails
- Raw values and rich context

### Traces

- Debugging slow requests
- Service dependencies
- Finding where errors originate
- Optimizing critical paths

## Observability Workflow

### Metrics alert you

- Your alert fires: HTTP error rate > 1% for 5 minutes.

### Traces show you where

- You look for traces with HTTP status 500 and inspect the trace structure of failing requests.

### Logs explain why

- You filter your logs by the trace ID from one failing request, finding the exact error: "Payment gateway returned 503 Service Unavailable - rate limit exceeded."
