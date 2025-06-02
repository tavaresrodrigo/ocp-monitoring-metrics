# Section 1: Introduction to OpenShift Monitoring

Welcome to the first section of our workshop! Here, we'll get an overview of the monitoring capabilities built into Azure Red Hat OpenShift (ARO) 4.18 and learn how to access the monitoring tools.

### 1.1 Overview of ARO 4.18 Monitoring Capabilities

Azure Red Hat OpenShift comes with a comprehensive, pre-configured, and self-updating monitoring stack based on Prometheus. This stack provides monitoring for cluster components and includes the capability to monitor user-defined applications.

* **Core Components:**
    * **Prometheus:** Collects and stores metrics as time-series data. Multiple instances run for cluster and user workloads.
    * **Grafana:** For visualizing metrics and creating dashboards.
    * **Alertmanager:** Handles alerts sent by Prometheus and routes them to configured receivers.
* **Key Projects (Namespaces):**
    * `openshift-monitoring`: Contains the core platform monitoring components (Prometheus, Alertmanager, Grafana, etc., for monitoring the cluster itself).
    * `openshift-user-workload-monitoring`: Contains components (Prometheus, Thanos Ruler) dedicated to monitoring user-defined applications and projects.
* **Operators Involved:**
    * `cluster-monitoring-operator`: Manages the main monitoring stack in `openshift-monitoring`.
    * The user workload monitoring is also managed as part of the overall monitoring solution.

This integrated stack allows administrators and developers to gain insights into cluster performance, resource utilization, and application behavior without needing to set up a separate monitoring infrastructure from scratch.

### 1.2 Accessing Monitoring Consoles

You will primarily interact with OpenShift's monitoring capabilities through the OpenShift Web Console.

1.  **OpenShift Web Console:**
    * Log in to your ARO cluster's Web Console.
    * You can typically find your console URL in the Azure portal for your ARO resource, or by running the following command in your terminal (ensure you are logged into your cluster via the `oc` CLI):
        ```bash
        oc whoami --show-console
        ```
2.  **Navigating to Observe:**
    * Once logged into the Web Console, ensure you are in the **Administrator** perspective (you can switch perspectives using the dropdown menu on the left, under your username).
    * In the left-hand navigation menu, click on **Observe**.
    * Under **Observe**, you will find several important sub-sections:
        * **Metrics:** This is where you can execute custom PromQL (Prometheus Query Language) queries to explore metrics and view ad-hoc graphs.
        * **Dashboards:** This section provides access to pre-built Grafana dashboards for visualizing cluster and application metrics.
        * **Alerting:** Here, you can view currently firing alerts, defined alerting rules, and manage silences.
        * **Targets:** This page shows the status of service endpoints that Prometheus is configured to scrape metrics from. It's useful for verifying if your applications are being correctly monitored.

Spend a few minutes familiarizing yourself with the **Observe** section in the OpenShift Web Console.
