**Introduction**

**AsciiArtify** plans to develop a new software product for converting images to ascii-art using Machine Learning.
In _Concept.md_ file will be presented comparative analysis of three tools for deploying Kubernetes clusters in a local environment - minikube, kind and k3d.


**Features**
### minikube

- **Automation Capabilities:**

  1. **Automated Creation:** Starts a fully configured local Kubernetes cluster in a virtual machine or container with one command =>
     ```bash
     minikube start
     ```

  2. **Resource Allocation:** Automatically detects and allocates fixed baseline of hardware resources (CPU, Memory, Disk).

  3. **Automated Updates:** Kubernetes version changes can be automated by recreating or updating local clusters with a specified Kubernetes release using Minikube commands.

  4. **LoadBalancer Emulation:** one of the commands can automatically exposes `LoadBalancer` services on the local machine without requiring manual `kubectl` port-forward commands.
     ```bash
     minikube tunnel
     ```

  5. **Ingress Configuration:** Automates the deployment of local ingress controllers via `add-ons` to route host traffic to your services using standard domain mapping.

  6. **Local Registry**: Automates pushing locally built container images straight to the cluster's internal registry.
     ```bash
     minikube addons enable registry
     ```

  7. **Auto-Scaling Simulations:** When used in tandem with the Kubernetes Horizontal Pod Autoscaler (HPA) and the metrics server addon, Minikube automatically scales your application pods up or down based on CPU/Memory thresholds.

  8. **Automated Testing:** **_Minikube_** configs are fully parseable by standard tooling, developers commonly use it in automated CI/CD pipelines (e.g., using GitHub Actions, Jenkins, or ArgoCD) to deploy and test code before pushing it to staging or production.

  9. **Scripted Setup:** Full lifecycle management can be automated using shell scripts or configuration tools like `Ansible` to instantly spin up matching test environments for entire engineering teams.

---

- **Additional Feature Kubernetes cluster monitoring:**

  To monitor a **_Minikube_** cluster, enable the built-in Kubernetes Metrics Server for real-time `kubectl top` commands and launch the native web dashboard for a visual overview. For advanced observability, deploy a full monitoring stack like `Prometheus` and `Grafana`.

  1. **Metrics Server:**

     Collects resource metrics (CPU and Memory utilization) from the cluster's nodes and pods. This enables developers to test resource-backed features locally, such as the Horizontal Pod Autoscaler (HPA) or `kubectl top` commands.

     Enable the Metrics Server:

     ```bash
     minikube addons enable metrics-server
     ```

     View Node Metrics:

     ```bash
     kubectl top node
     ```

     View Pod Metrics:

     ```bash
     kubectl top pod
     ```

  2. **Kubernetes Dashboard:** **_Minikube_** can automatically enable the Kubernetes Dashboard, configure the required local proxy access and launch the dashboard in the default web browser.

     ```bash
     minikube dashboard
     ```

  3. **Prometheus & Grafana:**

     Minikube includes a pre-configured add-on that deploys Prometheus (for time series data collection and alerting) and Grafana (for better visual dashboards). This allows for deep application performance monitoring (APM) simulation locally.

     Activation:

     ```bash
     minikube addons enable prometheus-grafana
     ```

---

- **Additional Feature Kubernetes cluster management:**

  1. **Cluster Lifecycle Management**

     Provisions and starts a fully configured local cluster.

     ```bash
     minikube start
     ```

     Checks the health and connection state of the local node.

     ```bash
     minikube status
     ```

     Gracefully powers down the cluster, preserving its state and data.

     ```bash
     minikube stop
     ```

     Completely wipes the local cluster data and frees host resources.

     ```bash
     minikube delete
     ```

  2. **Profile management (multi-cluster support)**

     Enables instant environment switching (e.g., dev vs test) on a single machine without interference.

     Spins up a completely isolated cluster instance.

     ```bash
     minikube start -p <name>
     ```

     Displays all currently configured local cluster profiles.

     ```bash
     minikube profile list
     ```

  3. **Add-on Management System**

     Deploys optional infrastructure modules (e.g., `ingress`, `metrics-server`, `registry`) via automated manifests or Helm components.

     ```bash
     minikube addons enable <feature>
     ```

  4. **Networking & Service Management**

     Automatically opens/routes internal cluster services to your host browser.

     ```bash
     minikube service <service-name>
     ```

     Emulates cloud LoadBalancer IPs by bridging the cluster network to the host network.

     ```bash
     minikube tunnel
     ```

     Logs directly into the Minikube node engine for low-level filesystem and container runtime debugging.

     ```bash
     minikube ssh
     ```

  5. **Resource & Configuration Tuning**

     Sets hard resource limits during initialization.

     ```bash
     minikube start --cpus=2 --memory=4096
     ```

     Configures the underlying hypervisor/runtime provider (Docker, VirtualBox, Hyper-V, etc.).

     ```bash
     minikube start --driver=<type>
     ```

---

To check full information about minikube, click here:

- https://minikube.sigs.k8s.io/docs/

### kind


### k3s



**Advantages and Disadvantages**

**Demo**

**Conclusions**
