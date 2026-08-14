## Service level indicator (SLI)
- A service level indicator is a carefullt selected metric that directly measure a service's behavior from the user's perspective

### Examples of good SLI

| Service | SLI | Description |
| --- | --- | --- |
| API Svc | Availability | Percentage of non-5xx responses |
| API Svc | Latency | Percentage of completion within 500ms |
| Batch jobs | Throughput | Percentage of timely completion |
| Batch jobs | Correctness | Percentage of correct completion |
| DB | Availability | Percentage of queries that succeed |
| DB | Latency | Percentage of completion within 100ms |