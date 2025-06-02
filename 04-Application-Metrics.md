# Module 4: Monitoring Application Metrics 

Beyond cluster health, a key aspect of monitoring is understanding how your own applications are performing. OpenShift's monitoring stack can scrape metrics from applications, provided they expose them in a Prometheus-compatible format.

## How Applications Expose Metrics for Prometheus

Prometheus typically discovers and scrapes metrics from HTTP(S) endpoints (often `/metrics`) that expose data in a specific text-based format. Many popular frameworks and libraries offer "Prometheus client libraries" to help instrument your application code.

**Common ways applications provide metrics:**

1.  **Built-in /metrics endpoint:** Some applications (like Spring Boot with Actuator, Quarkus with Micrometer) can expose a `/metrics` endpoint automatically or with minimal configuration.
2.  **Exporters:** For applications that don't natively expose Prometheus metrics (e.g., databases, message queues, hardware), a separate "exporter" process is run alongside them. The exporter queries the application and translates its internal metrics into the Prometheus format.
3.  **Custom Instrumentation:** Using Prometheus client libraries (e.g., for Go, Python, Java) to define and expose custom application-specific metrics.

## ServiceMonitors and PodMonitors

To tell Prometheus *where* to find these application metrics, OpenShift uses Custom Resources (CRs):

* **`ServiceMonitor` (most common for user workloads):**
    * Defines how Prometheus should monitor a set of services.
    * It typically selects services based on labels and specifies the path, port, and interval for scraping metrics.
    * The Cluster Monitoring Operator watches for `ServiceMonitor` objects in user-defined projects (if configured, or by default for some core services) and automatically configures Prometheus to scrape the targeted services.

* **`PodMonitor`:**
    * Similar to `ServiceMonitor`, but targets pods directly based on labels, rather than services. Useful in cases where a service might not be present or appropriate.

**Enabling User-Defined Project Monitoring:**

By default, the main cluster Prometheus instance (`prometheus-k8s`) monitors core cluster components. To allow it to monitor your own applications in your projects, you might need to enable monitoring for user-defined projects.

As of OpenShift 4.x, this is often enabled by default or can be configured in the `cluster-monitoring-config` ConfigMap in `openshift-monitoring`. For this workshop, we'll assume your ARO environment allows user workload monitoring or we'll use an example that's auto-discovered.

If you deploy an application and its `ServiceMonitor` correctly, metrics should automatically appear.

## Deploying a Sample Application with Metrics

Let's deploy a simple application that already exposes Prometheus metrics. We'll use a pre-built example.

**Activity: Deploy and Monitor a Sample App**

1.  **Create a New Project (Namespace):**
    * Using the OpenShift Web Console: Go to **Home** -> **Projects** -> **Create Project**.
        * Name: `app-monitoring-demo`
        * Display Name: `App Monitoring Demo`
        * Click **Create**.
    * Or using the `oc` CLI:
        ```bash
        oc new-project app-monitoring-demo --display-name="App Monitoring Demo"
        ```

2.  **Deploy the Sample Application:**
    * We'll deploy a simple Go application that exposes custom metrics and has a `ServiceMonitor`.
    * Create a file named `sample-app.yaml` with the following content:

        ```yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: prom-example-app
          labels:
            app: prom-example-app
        spec:
          replicas: 1
          selector:
            matchLabels:
              app: prom-example-app
          template:
            metadata:
              labels:
                app: prom-example-app
              annotations:
                # This annotation helps Prometheus discover the /metrics endpoint even without a ServiceMonitor for very simple cases,
                # but a ServiceMonitor is the recommended approach for robust configuration.
                prometheus.io/scrape: 'true'
                prometheus.io/path: '/metrics'
                prometheus.io/port: '8080'
            spec:
              containers:
              - name: prom-example-app
                image: quay.io/brancz/prometheus-example-app:v0.4.0 # A known image that exposes metrics
                ports:
                - name: web # Port name must match Service port name for ServiceMonitor
                  containerPort: 8080
                  protocol: TCP
        ---
        apiVersion: v1
        kind: Service
        metadata:
          name: prom-example-app-svc
          labels:
            app: prom-example-app # Label to be selected by ServiceMonitor
        spec:
          ports:
          - name: web # Port name must match one in Deployment and ServiceMonitor
            port: 8080
            protocol: TCP
            targetPort: web
          selector:
            app: prom-example-app
          type: ClusterIP
        ---
        apiVersion: [monitoring.coreos.com/v1](https://monitoring.coreos.com/v1)
        kind: ServiceMonitor
        metadata:
          name: prom-example-app-sm
          labels:
            team: frontend # Just an example label for the ServiceMonitor itself
        spec:
          selector:
            matchLabels:
              app: prom-example-app # Selects the Service with label app=prom-example-app
          endpoints:
          - port: web # Matches the 'name' of the port in the Service
            path: /metrics # The path where metrics are exposed
            interval: 15s # Scrape interval
        ```

    * Apply this YAML to your `app-monitoring-demo` project:
        ```bash
        oc apply -f sample-app.yaml -n app-monitoring-demo
        ```

3.  **Verify Deployment:**
    * Check that the pod is running:
        ```bash
        oc get pods -n app-monitoring-demo
        ```
        Wait for the `prom-example-app-...` pod to be in `Running` state.
    * Check the service and service monitor:
        ```bash
        oc get svc,servicemonitor -n app-monitoring-demo
        ```

4.  **Verify Metrics Endpoint (Optional but Recommended):**
    * Port-forward to the service to access the `/metrics` endpoint directly from your local machine:
        ```bash
        oc port-forward svc/prom-example-app-svc 8080:8080 -n app-monitoring-demo
        ```
    * Open your browser or use `curl` to access `http://localhost:8080/metrics`. You should see a list of metrics.
        ```bash
        # In another terminal
        curl http://localhost:8080/metrics
        ```
    * Look for metrics like `version`, `http_requests_total`, `example_random_gauge`. Stop the port-forward (Ctrl+C) when done.

## Viewing Application Metrics

### In Prometheus UI

1.  **Check Prometheus Targets:**
    * Go to the Prometheus UI (Observe -> Metrics).
    * Navigate to **Status** -> **Targets**.
    * In the search box, you can try filtering by `app-monitoring-demo` or `prom-example-app-sm`.
    * You should eventually see an endpoint for your `prom-example-app-svc` in the `app-monitoring-demo` namespace, and its state should be `UP`. This might take a few minutes after the `ServiceMonitor` is created.

2.  **Query Application Metrics:**
    * Once the target is up, go to the **Graph** page in Prometheus.
    * Enter one of the metric names you saw from the `/metrics` endpoint. For example:
        * `version{namespace="app-monitoring-demo"}` (This is a gauge with app version info)
        * `rate(http_requests_total{namespace="app-monitoring-demo", handler="/metrics"}[2m])` (Rate of requests to the /metrics endpoint of your app)
        * `example_random_gauge{namespace="app-monitoring-demo"}` (A sample gauge from the app)
    * Click **Execute**. You should see the data for your application.

    ![Prometheus Query App Metric](https://via.placeholder.com/700x450.png?text=Prometheus+Query+for+App+Metric)
    *(Image placeholder: Replace with an actual screenshot of a Prometheus query for an app metric)*

### In Grafana (Using Ad-hoc Queries or Custom Dashboards)

While OpenShift doesn't auto-generate specific dashboards for every custom application, you can:

1.  **Use the "Kubernetes / Compute Resources / Namespace (Workloads)" Dashboard:**
    * Go to Grafana -> Dashboards -> Browse.
    * Open the "Kubernetes / Compute Resources / Namespace (Workloads)" dashboard.
    * Select your `app-monitoring-demo` namespace from the `Namespace` dropdown at the top.
    * This will show you CPU, memory, and network usage for the pods in your application's namespace, but not the custom application metrics (like `http_requests_total` specific to the app's code).

2.  **Create a New Panel with a Custom Metric (Brief Demo):**
    * For a quick view of a specific application metric in Grafana:
        * Open any dashboard or create a new one (Dashboard icon -> New Dashboard -> Add new panel).
        * In the new panel, ensure the **Data source** is set to `Prometheus` (or similar, often the default).
        * In the **Metrics browser** or query field, enter your PromQL query, e.g., `rate(http_requests_total{namespace="app-monitoring-demo", pod=~"prom-example-app-.*"}[5m])`.
        * You should see the graph update with your application's metric.
        * You can then customize the visualization (title, legend, type of graph).

    *This gives a taste of customization but building full dashboards is a more involved process.*

    ![Grafana Adhoc App Metric](https://via.placeholder.com/700x450.png?text=Grafana+Panel+with+App+Metric)
    *(Image placeholder: Replace with an actual screenshot of a Grafana panel showing a custom app metric)*

By deploying an application that exposes metrics and creating a `ServiceMonitor`, you've successfully integrated your application into OpenShift's monitoring system. This allows you to track its performance and behavior alongside cluster metrics.

**Cleanup (Optional - if you want to remove the demo app):**
```bash
oc delete -f sample-app.yaml -n app-monitoring-demo
oc delete project app-monitoring-demo
```

**Next Module: [Wrap-up and Next Steps](./05-Wrap-up.md)**