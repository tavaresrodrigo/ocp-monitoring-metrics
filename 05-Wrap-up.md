# Module 5: Wrap-up and Next Steps 

Congratulations on completing this workshop on OpenShift 4.18 on Azure (ARO) Monitoring and Metrics!

## Key Learnings Recap

Over the past ~2 hours, we've covered:

* **Fundamentals of OpenShift Monitoring:** Understanding the roles of Prometheus (collection & storage) and Grafana (visualization).
* **Cluster Monitoring Operator (CMO):** The "engine" that manages the monitoring stack in OpenShift.
* **Accessing Monitoring Tools:** How to navigate to Grafana dashboards and the Prometheus UI via the OpenShift Web Console.
* **Exploring Cluster Metrics:** Viewing overall cluster health, resource utilization (CPU, Memory), and metrics for individual nodes and core components like the API server using pre-built Grafana dashboards.
* **Basic PromQL Queries:** How to run simple queries directly in the Prometheus UI to inspect specific metrics.
* **Application Metrics Monitoring:**
    * How applications can expose metrics for Prometheus.
    * The role of `ServiceMonitor` CRs in telling Prometheus where and how to scrape application metrics.
    * Deploying a sample application and viewing its custom metrics in Prometheus and (briefly) Grafana.

## Alerting (Brief Mention)

While we didn't delve into configuring alerts, it's important to remember that **Alertmanager** is the component within the OpenShift Monitoring stack responsible for handling alerts.

* Prometheus evaluates alerting rules (defined typically as `PrometheusRule` CRs).
* If an alert fires, Prometheus sends it to Alertmanager.
* Alertmanager then de-duplicates, groups, and routes these alerts to various notification channels (e.g., email, Slack, PagerDuty).

Setting up effective alerting is a critical next step after understanding your metrics, but requires careful planning and configuration.

## Further Exploration

This workshop was an introduction. There's much more to explore:

* **Custom Grafana Dashboards:** Learn to build your own dashboards tailored to your specific applications and needs.
* **Advanced PromQL:** Dive deeper into Prometheus's powerful query language for more complex analysis and aggregation.
* **User Workload Monitoring:** For more isolated monitoring of user applications, explore enabling and using the `prometheus-user-workload` instance.
* **Custom Alerting Rules:** Define `PrometheusRule` objects to create alerts for conditions that matter to your applications and cluster.
* **Integrating with Azure Monitor (Optional):**
    * While OpenShift provides a comprehensive built-in stack, you might have reasons to send certain metrics to Azure Monitor for centralized logging or broader Azure ecosystem integration.
    * ARO offers ways to integrate with Azure Monitor, for example, by configuring OpenShift to forward metrics or logs. This often involves additional operators or configurations.
* **Resource Quotas and Limits:** Combine monitoring with setting appropriate resource requests, limits, and quotas to ensure fair resource distribution and stability.
* **OpenShift Logging:** Explore the built-in logging stack (often based on Elasticsearch, Fluentd, Kibana - EFK, or Loki, Promtail, Grafana - LPG) to aggregate and analyze logs from your cluster and applications.

## Resources

* **OpenShift Documentation - Monitoring:** Always refer to the official documentation for the most up-to-date information.
    * [Monitoring overview](https://docs.openshift.com/container-platform/4.18/monitoring/monitoring-overview.html)
    * [Accessing monitoring UIs](https://docs.openshift.com/container-platform/4.18/monitoring/accessing-monitoring-uis.html)
    * [Viewing cluster metrics](https://docs.openshift.com/container-platform/4.18/monitoring/viewing-cluster-metrics.html)
    * [Managing metrics for your own services](https://docs.openshift.com/container-platform/4.18/monitoring/managing-metrics.html)
* **Prometheus Documentation:** [prometheus.io/docs/](https://prometheus.io/docs/introduction/overview/)
* **Grafana Documentation:** [grafana.com/docs/grafana/latest/](https://grafana.com/docs/grafana/latest/)
* **Azure Red Hat OpenShift Documentation:** Check Azure's official ARO documentation for any Azure-specific considerations.

Thank you for participating! We hope this workshop has provided you with a solid foundation for monitoring your workloads on ARO.