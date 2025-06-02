# OpenShift 4.18 on Azure (ARO) - Monitoring & Metrics Workshop

Welcome to this workshop on exploring the monitoring and metrics capabilities of OpenShift 4.18 on Azure (ARO).

This 2-hour workshop will guide you through understanding and utilizing the built-in monitoring stack within your ARO cluster. We will focus on how to access, interpret, and leverage the metrics provided by OpenShift for both cluster and application monitoring.

**Target Audience:** Developers, SREs, and anyone interested in understanding OpenShift monitoring fundamentals.
**Prerequisites:**
* An existing Azure Red Hat OpenShift (ARO) 4.18 cluster.
* `oc` (OpenShift CLI) tool installed and configured to access your ARO cluster.
* Kubeconfig file for your ARO cluster.
* A modern web browser.

**Workshop Modules:**

1.  **[Introduction to OpenShift Monitoring](./01-Introduction.md)** (15 minutes)
    * Overview of the built-in monitoring stack.
    * Key components: Prometheus & Grafana.
    * The role of the Cluster Monitoring Operator.
2.  **[Accessing Monitoring Dashboards](./02-Accessing-Dashboards.md)** (25 minutes)
    * Navigating to the OpenShift Web Console.
    * Accessing Grafana dashboards.
    * Brief tour of the Prometheus UI.
3.  **[Exploring Cluster Metrics](./03-Cluster-Metrics.md)** 
    * Understanding key cluster-level metrics.
    * Using default Grafana dashboards for cluster overview.
    * Identifying resource utilization (CPU, Memory, Storage).
4.  **[Monitoring Application Metrics](./04-Application-Metrics.md)**
    * Understanding how applications expose metrics.
    * Deploying a sample application that exposes metrics.
    * Viewing application-specific metrics in Grafana/Prometheus.
5.  **[Wrap-up and Next Steps](./05-Wrap-up.md)** 
    * Recap of key learnings.
    * Brief mention of alerting (out of scope for hands-on).
    * Pointers for further exploration.

**Important Notes:**

* This workshop focuses on the **default, out-of-the-box monitoring capabilities** of ARO.
* We will **not** be covering advanced customization of Prometheus, Grafana, or setting up alerting rules in detail.
* Ensure your ARO cluster is healthy and accessible before starting.

Let's get started!
