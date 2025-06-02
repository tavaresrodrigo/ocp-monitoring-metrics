# Module 1: Introduction to OpenShift Monitoring

We'll get an overview of how monitoring works in OpenShift, especially within an Azure Red Hat OpenShift (ARO) environment.

## Built-in Monitoring Stack

OpenShift Container Platform includes a pre-configured, pre-installed, and self-updating monitoring stack. This stack is based on **Prometheus** and **Grafana**. 

**Key benefits of the built-in stack:**

* **Comprehensive:** Provides metrics for the cluster itself, OpenShift components, and user-defined applications.
* **Integrated:** Seamlessly works with OpenShift and its security model.
* **Managed:** In ARO, much of the lifecycle of the monitoring stack is managed for you.

## Core Components

Let's briefly look at the main players:

### 1. Prometheus

* **What it is:** An open-source systems monitoring and alerting toolkit.
* **Its role in OpenShift:**
    * **Collects metrics:** Scrapes metrics from various endpoints (nodes, pods, services).
    * **Stores metrics:** Persists these metrics in a time-series database.
    * **Provides a query language:** PromQL allows you to query and aggregate the collected data.
    * **Handles alerts:** While we won't configure alerts today, Prometheus is responsible for evaluating alerting rules.

OpenShift runs several Prometheus instances:

* **Cluster Prometheus (`prometheus-k8s`):** Monitors cluster components, nodes, and user-defined workloads. This is our primary focus.
* **User Workload Prometheus (`prometheus-user-workload`):** Can be enabled to allow users to monitor their own applications in a more isolated way (optional, generally not needed for basic app monitoring shown later).

### 2. Grafana

* **What it is:** An open-source platform for monitoring and observability, widely used for visualizing time-series data.
* **Its role in OpenShift:**
    * **Visualizes metrics:** Provides dashboards to display data collected by Prometheus.
    * **Pre-built dashboards:** OpenShift comes with many useful dashboards out-of-the-box for cluster and component monitoring.
    * **Customizable:** You can create your own dashboards (though we won't cover this in detail today).

## The Cluster Monitoring Operator (CMO)

This is a crucial component that automates the setup and maintenance of the monitoring stack.

* **Manages the Stack:** The CMO installs, configures, and updates Prometheus, Grafana, and other related components.
* **Default Configuration:** In ARO, the CMO comes pre-configured with sensible defaults, making it easy to get started.
* **Key Namespace:** Most monitoring components run in the `openshift-monitoring` namespace.

## Monitoring in ARO

Azure Red Hat OpenShift (ARO) leverages this standard OpenShift monitoring stack. This means:

* The core components (Prometheus, Grafana) are available and managed.
* You get immediate insights into your cluster's health and performance.
* While Azure has its own monitoring solutions (like Azure Monitor), ARO provides in-cluster monitoring capability by default. For deeper integration, you might explore sending metrics to Azure Monitor, but that's beyond today's scope.

**What we won't cover in detail (but good to know):**

* **Alertmanager:** The component that handles alerts fired by Prometheus, routing them to various notification channels. While alerting is a critical part of monitoring, configuring it is a separate topic.
* **Customizing Prometheus scraping or Grafana dashboards extensively.**
* **`prometheus-user-workload` setup.**

Now that we have a basic understanding, let's move on to accessing these tools!

**Next Module: [Accessing Monitoring Dashboards](./02-Accessing-Dashboards.md)**