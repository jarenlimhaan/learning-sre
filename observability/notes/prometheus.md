## Prometheus

![prom](../images/prom.png)

### TL;DR

- **Prometheus** is an open-source monitoring and alerting system, great for cloud-native apps.
- It uses a **pull model**: Prometheus scrapes `/metrics` endpoints on a schedule.
- Data is stored as **time series**: metric name + labels + timestamp + value.

### Core concepts

- **Metric types**:
  - `counter` (only goes up)
  - `gauge` (up/down value)
  - `histogram` (buckets + count + sum, good for latency)
  - `summary` (quantiles on client side)
- **Labels** add dimensions (e.g., `service="api"`, `instance="10.0.0.1:9100"`).
- **PromQL** is the query language for metrics and alert conditions.

### Common workflow

1. Instrument app or run exporters (Node Exporter, kube-state-metrics, etc.).
2. Configure scrape targets in `prometheus.yml`.
3. Query metrics with PromQL.
4. Build dashboards in Grafana.
5. Create alerts with Alertmanager.

### Useful PromQL examples

```promql
# requests per second (last 5m)
rate(http_requests_total[5m])

# error rate by service
sum(rate(http_requests_total{status=~"5.."}[5m])) by (service)
/
sum(rate(http_requests_total[5m])) by (service)

# 95th percentile latency from histogram
histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le, service))
```

### Pros / limits

- ✅ Simple, reliable, strong Kubernetes integration.
- ✅ Powerful alerting + ecosystem.
- ⚠️ Not ideal for long-term storage by itself (use Thanos/Cortex/Mimir for scale/retention).
- ⚠️ High-cardinality labels can hurt performance/cost.

### Quick best practices

- Keep label cardinality low and controlled.
- Prefer histograms for latency SLOs.
- Alert on symptoms (latency, errors, saturation), not just CPU/memory.
- Record important computed queries as recording rules.
