# Workshop Prerequisites

Before you begin this workshop, please ensure you have the following:

* **Azure Red Hat OpenShift (ARO) 4.18 Cluster:**
    * An active ARO 4.18 cluster must be installed and accessible.
    * You need administrative access to this cluster, or at least permissions to create projects, deploy applications, and manage monitoring resources like `ServiceMonitor` and `PrometheusRule`.
* **`oc` (OpenShift CLI):**
    * The OpenShift Command Line Interface (`oc`) must be installed on your local machine.
    * It should be configured to connect to your ARO cluster. You can verify this by running `oc whoami` and `oc cluster-info`.
* **`kubectl` (Kubernetes CLI):**
    * The Kubernetes Command Line Interface (`kubectl`) should also be installed. It is often included with the `oc` CLI download.
* **GitHub Account (Optional but Recommended):**
    * If you plan to use your own sample applications hosted on GitHub or fork any provided examples, you will need a GitHub account.
* **Basic Understanding:**
    * A fundamental understanding of containers (e.g., Docker or Podman).
    * Basic knowledge of Kubernetes concepts (Pods, Services, Deployments, Namespaces).
    * Familiarity with OpenShift concepts is beneficial.
* **Tools (Optional):**
    * A text editor or IDE (like VS Code) for viewing and editing YAML files.
    * A web browser to access the OpenShift Web Console and your ARO cluster.

Once you have all these prerequisites in place, you are ready to start the workshop!
