# Module 3: Exploring Cluster Metrics 

Now that we can access Grafana, let's dive into understanding the health and performance of our ARO cluster using the pre-built dashboards.

## Understanding Key Cluster Metrics

When monitoring a Kubernetes/OpenShift cluster, some common areas to observe are:

* **CPU Usage:** Overall and per-node/pod CPU utilization. High CPU can indicate performance bottlenecks.
* **Memory Usage:** Overall and per-node/pod memory consumption. Running out of memory can lead to OOM (Out Of Memory) kills.
* **Disk I/O and Space:** Performance of persistent storage and available disk space, especially for etcd, Prometheus, and worker nodes.
* **Network I/O:** Traffic patterns, bandwidth usage, and potential network bottlenecks.
* **Pod Health:** Number of running, pending, failed pods.
* **API Server Health:** Latency and error rates of the Kubernetes API server, a critical component.
* **etcd Health:** Health of the etcd cluster, which stores all cluster state.

## Using Grafana Dashboards for Cluster Overview

OpenShift provides several default Grafana dashboards that give excellent insights into these areas.

**Activity: Explore Cluster Dashboards**

1.  **Go to Grafana:**
    * Open your Grafana instance as shown in Module 2 (Observe -> Dashboards from the OpenShift Console).

2.  **Find Cluster Dashboards:**
    * Click the **Dashboards** icon on the left sidebar and select **Browse**.
    * Look for dashboards related to cluster health. Common ones include:
        * **Kubernetes / Compute Resources / Cluster**
        * **Kubernetes / API server**
        * **Node Exporter / Nodes** (or similar name, shows individual node metrics)
        * **etcd**
        * **OpenShift-specific dashboards**

3.  **Explore "Kubernetes / Compute Resources / Cluster":**
    * This is often a great starting point.
    * **Time Range:** Notice the time range selector at the top right of Grafana. You can adjust this to see data from the last 5 minutes, 1 hour, 1 day, etc. Try changing it.
    * **Key Panels to Observe:**
        * **CPU Utilization (Total):** Overall CPU usage across the cluster.
        * **Memory Utilization (Total):** Overall memory usage.
        * **CPU Quotas vs Usage:** If quotas are set, how close are you to them?
        * **Pod Capacity and Usage:** Number of pods running vs. allocatable.
        * **Workload specific views:** Often, you can drill down by namespace, workload type, etc.

    ![Grafana Cluster Compute Dashboard](https://via.placeholder.com/700x450.png?text=Grafana+Cluster+Compute+Dashboard)
    *(Image placeholder: Replace with an actual screenshot of a relevant Grafana cluster dashboard)*

4.  **Explore "Node Exporter / Nodes" (or similar):**
    * This dashboard provides detailed metrics for each individual node in your cluster (masters and workers).
    * At the top, you'll likely see a dropdown to select a specific **Node**.
    * **Key Panels:**
        * CPU Usage per Node
        * Memory Usage per Node
        * Disk I/O per Node
        * Network Traffic per Node

5.  **Explore "Kubernetes / API server":**
    * This dashboard shows the health and performance of the Kubernetes API server.
    * **Key Panels:**
        * API Server Request Rate
        * API Server Request Latency (very important for performance)
        * Client HTTP_REQUESTS (by verb and code)

## Quick Look at Prometheus UI for Cluster Metrics

While Grafana is best for visualization, you can also use the Prometheus UI to query specific cluster metrics.

**Activity: Simple PromQL Query**

1.  **Go to Prometheus UI:**
    * Open your Prometheus UI (Observe -> Metrics from the OpenShift Console).

2.  **Example Query - Total Memory Usage by Nodes:**
    * In the "Expression" input box, type:
        ```promql
        sum(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) by (kubernetes_node)
        ```
    * Click **Execute**.
    * Switch to the **Graph** tab if it's not already selected. You should see memory usage per node.
    * **Explanation:**
        * `node_memory_MemTotal_bytes`: Total memory on a node.
        * `node_memory_MemAvailable_bytes`: Available memory on a node.
        * The difference gives used memory.
        * `sum(...) by (kubernetes_node)`: Aggregates this for each node.

3.  **Example Query - CPU Usage (Rate):**
    * This one is a bit more complex as CPU is a counter:
        ```promql
        sum(rate(container_cpu_usage_seconds_total{id="/"}[5m])) by (kubernetes_node)
        ```
    * **Explanation:**
        * `container_cpu_usage_seconds_total{id="/"}`: CPU seconds consumed by the root cgroup on nodes.
        * `rate(...[5m])`: Calculates the per-second average rate of increase over the last 5 minutes.
        * `sum(...) by (kubernetes_node)`: Aggregates per node.

Don't worry too much about mastering PromQL right now. The goal is to see that you *can* query raw metrics directly.

By exploring these dashboards and trying a simple query, you gain visibility into the operational status of your ARO cluster. This is crucial for troubleshooting, capacity planning, and ensuring stability.

**Next Module: [Monitoring Application Metrics](./04-Application-Metrics.md)**