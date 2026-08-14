## Google's Four Metrics that Apply to Almost Every Service

### Latency: How long does it take?

- Measure response time for requests
- Distinguish between successful request latency and failed request latency
- Track percentiles (p50, p95, p99) not just averages.

### Traffic: How much demand?

- Measure requests per second, queries per second, or whatever units make sense for your system.
- Helps normalize other metrics and understand system capacity.

### Errors: What's the rate of failures?

- Measure the percentage of requests that fail.
- Define errors explicitly (are 4xx responses also errors? Are timeouts errors?)

### Saturation: How "full" is the system?

- Measure resource utilization: CPU, memory, disk, network bandwidth, database connections.
- High saturation indicates you're approaching limits and should expect degraded performance soon.

---

## Explanation

These four measurements are known as **The Four Golden Signals** from Google's SRE book. If you can only monitor four things in a service, it should be these.

### 1. Latency

Measures execution speed.

- **Key practice:** Always track successful and failed requests separately. A $500$ error that fails immediately in $2\text{ms}$ will artificially lower your average latency if mixed with successful requests that take $200\text{ms}$.
- **Key practice:** Use percentiles ($p50, p95, p99$) to capture tail latency instead of arithmetic means.

### 2. Traffic

Measures incoming system load.

- **Common metrics:** HTTP requests per second (RPS), transactions per second (TPS), or concurrent users.
- **Purpose:** Provides context for changes in other signals. For example, if latency spikes while traffic stays flat, you have a software/dependency problem. If latency spikes during a $3\times$ traffic surge, you have a capacity problem.

### 3. Errors

Measures system reliability.

- **Common metrics:** HTTP $5xx$ status codes, unhandled exceptions, or dropped connections.
- **Explicit definitions:** SRE teams must clarify edge cases:
- HTTP $404$ (Not Found) caused by a bad user link is usually not a system error.
- HTTP $429$ (Too Many Requests) or upstream timeouts **are** system errors.

### 4. Saturation

Measures how close resources are to maximum capacity.

- **Common metrics:** CPU utilization, RAM usage, database connection pool limits, or disk IOPS.
- **Early warning indicator:** Most services suffer non-linear latency increases long before reaching $100\%$ saturation (e.g., latency degrades sharply once CPU passes $80\%$). Monitoring saturation allows autoscalers or engineers to react before failures cascade.
