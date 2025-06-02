# Module 2: Accessing Monitoring Dashboards (25 minutes)

Now we'll learn how to access the built-in monitoring tools: the OpenShift Web Console for an overview, Grafana for detailed dashboards, and the Prometheus UI for querying metrics.

## 1. The OpenShift Web Console

The OpenShift Web Console provides a high-level overview of your cluster's status and is the primary entry point to access more detailed monitoring interfaces.

**Steps:**

1.  **Log in to your ARO Cluster's Web Console:**
    * You can typically find the console URL from your ARO cluster's Azure portal page or via the `oc` command:
        ```bash
        oc whoami --show-console
        ```
    * Open the URL in your browser and log in with your credentials (e.g., `kubeadmin` or your configured identity provider).

2.  **Navigate to Monitoring:**
    * Once logged in, on the left-hand navigation pane, click on **Observe**.
    * Here you'll find links to **Dashboards**, **Metrics**, and **Alerts**.

    ![OpenShift Console Observe Section](https://via.placeholder.com/600x400.png?text=OCP+Console+Observe+Section)
    *(Image placeholder: Replace with an actual screenshot of the OCP console's "Observe" section)*

## 2. Accessing Grafana Dashboards

Grafana is where you'll find detailed, visual dashboards for your cluster and applications.

**Steps:**

1.  **From the OpenShift Web Console:**
    * In the **Observe** section, click on **Dashboards**.
    * This will typically open a new tab or redirect you to the Grafana instance managed by OpenShift.
    * You should be automatically logged into Grafana with your OpenShift credentials.

2.  **Exploring Grafana:**
    * You'll land on the Grafana home page or a default dashboard.
    * Click the **Dashboards** icon (usually a set of four squares) on the left sidebar.
    * Select **Browse** (or "Manage" in older versions).
    * You'll see a list of folders and dashboards. Many useful dashboards are pre-built and organized into folders like "OpenShift", "Kubernetes", "Prometheus", etc.

    ![Grafana Dashboards List](https://via.placeholder.com/600x400.png?text=Grafana+Dashboards+List)
    *(Image placeholder: Replace with an actual screenshot of the Grafana dashboards list)*

    We will explore specific dashboards in the next module.

## 3. Accessing the Prometheus UI

The Prometheus UI allows you to directly query metrics using PromQL, view service discovery status, and check the status of Prometheus itself.

**Steps:**

1.  **From the OpenShift Web Console:**
    * In the **Observe** section, click on **Metrics**.
    * This will take you to the Prometheus UI.

2.  **Alternative - Finding the Route (if needed):**
    * The Prometheus service (`prometheus-k8s`) is exposed via a route in the `openshift-monitoring` namespace. You can find it using the `oc` CLI:
        ```bash
        oc get routes -n openshift-monitoring
        ```
    * Look for the route associated with `prometheus-k8s` and open its URL in your browser. You might need to log in with your OpenShift credentials.

3.  **Exploring the Prometheus UI:**
    * **Graph:** This is where you can enter PromQL queries and see results as graphs or tables.
    * **Alerts:** Shows the current state of configured alerts (we aren't setting these up, but you can see predefined ones).
    * **Status:** Provides information about:
        * **Runtime & Build Information:** Details about the Prometheus server.
        * **Command-Line Flags:** How Prometheus was started.
        * **Configuration:** The current Prometheus configuration (useful for advanced debugging).
        * **Rules:** Configured recording and alerting rules.
        * **Targets:** Shows all the endpoints (services, pods) that Prometheus is scraping metrics from and their status (Up/Down). This is very useful for troubleshooting if metrics are missing.
        * **Service Discovery:** Shows active service discovery mechanisms.

    ![Prometheus UI Graph](https://via.placeholder.com/600x400.png?text=Prometheus+UI+Graph+Page)
    *(Image placeholder: Replace with an actual screenshot of the Prometheus UI Graph page)*

We now know how to reach the primary interfaces for monitoring. In the next modules, we'll use these to explore cluster and application metrics.

**Next Module: [Exploring Cluster Metrics](./03-Cluster-Metrics.md)**