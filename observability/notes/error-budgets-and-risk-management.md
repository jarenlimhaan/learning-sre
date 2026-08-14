## Error Budget

The error budget is the difference between 100% and the set SLO. If your SLO is 99.9% availability, your error budget is 0.1% failure. Over a 30-day month:

*  **Total minutes:** 43,200
*  **Allowed downtime:** 43.2 minutes
*  **That's your error budget**

---

### Error Budget > 0?

*  Push risky features faster
*  Deploy more frequently
*  Run experiments in production
*  Take calculated risks

---

### Error Budget <= 0?

*  Freeze risky deployments
*  Focus on reliability improvements
*  Pay down technical debt
*  Investigate and fix the root causes of recent outages

---

## Explanation: How Error Budgets Work

An **Error Budget** is a Site Reliability Engineering (SRE) concept used to manage the trade-off between **feature speed** and **system stability**.

1. **Defining the Margin:** 100% uptime is impractical and expensive. If an application targets a 99.9% Service Level Objective (SLO), the remaining 0.1% is the acceptable threshold for failure (43.2 minutes of downtime per month).
2. **Governor for Deployments:** The budget regulates developer velocity:
* **Budget Available ($> 0$):** High system stability allows engineering to deploy new features quickly and take deployment risks.
* **Budget Exhausted ($\le 0$):** The system has breached its failure allowance. Feature releases are paused, and engineering effort shifts entirely to fixing bugs, improving infrastructure, and reducing technical debt until reliability recovers.