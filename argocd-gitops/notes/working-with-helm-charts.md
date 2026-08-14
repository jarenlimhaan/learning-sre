## Argo CD is a helm template engine 
![helm](../images/helm-chart.png)
- * Argo CD is a `helm template` engine.
    - This is the single most important concept to understand. When you manage a Helm chart with Argo CD, **it does not run `helm install` or `helm upgrade**`.
- As such, there is no Helm history.
- There is also no `helm install`, `helm upgrade`, or `helm uninstall`.
- You can use Argo CD to deploy both charts stored in your own repositories, as well as charts that are publicly available. 