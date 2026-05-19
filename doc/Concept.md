## Introduction

**AsciiArtify** plans to develop a new software product for converting images to ascii-art using Machine Learning.
In _Concept.md_ file will be presented comparative analysis of three tools for deploying Kubernetes clusters in a local environment - minikube, kind and k3d.


## Features
### minikube

- **Supported OS**
    - Linux: Ubuntu, Debian, CentOS, Fedora, etc.
    - macOS: Intel and Apple silicon.
    - Windows: Windows 10/11 and Windows Server.
    - Cloud & CI: GitHub Codespaces

---

- **Supported Architecture**
    - x86-64 / AMD64: Fully supported across Linux, macOS, and Windows.
    - ARM64: Fully supported (Apple M1/M2/M3, Raspberry Pi and modern ARM-based cloud instances).
    - ppc64 (PowerPC): Supported.
    - S390x (IBM Z): Supported

---

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
- **Supported OS**
    - Linux: Ubuntu, Debian, CentOS, Fedora and other modern Linux distributions.
    - macOS: Intel and Apple Silicon (ARM64).
    - Windows: Windows 10/11 and Windows Server (typically via Docker Desktop or WSL2).
    - Cloud & CI: GitHub Codespaces and CI/CD environments supporting Docker.

---

- **Supported Architecture**
    - x86-64 / AMD64: Fully supported across Linux, macOS, and Windows.
    - ARM64: Fully supported (Apple Silicon: M1/M2/M3, ARM cloud instances).
    - ppc64 / ppc64le: Supported on Linux (community/CI support, not always first-class).
    - s390x (IBM Z): Supported on Linux (community/CI support).

---

- **Automation Capabilities:**
1. **Ephemeral Environments**
2. **CI/CD Pipeline Integration**
3. **Declarative Infrastructure**
4. **Declarative Infrastructure**

---

- **Additional Feature Kubernetes cluster monitoring:**

---

- **Additional Feature Kubernetes cluster management:**

---

To check full information about kind, click here:

- https://kind.sigs.k8s.io

### k3d
- **Supported OS**
    - Linux: Ubuntu, Debian, CentOS, Fedora, and other modern Linux distributions with Docker support.
    - macOS: Intel and Apple Silicon (ARM64) with Docker Desktop.
    - Windows: Windows 10/11 (via WSL2 + Docker Desktop).
    - Cloud & CI: GitHub Codespaces and other Docker-based CI environments.

---

- **Supported Architecture**
    - x86-64 / AMD64: Fully supported and primary target architecture.
    - ARM64: Fully supported (Apple Silicon, ARM cloud instances).
    - ppc64 (PowerPC): Not officially supported (may work in custom/community setups).
    - s390x (IBM Z): Not officially supported (very limited or experimental support).

---

- **Automation Capabilities:**
1. **Declarative Infastructure**
      - Automated Setup: Running a single command automatically parses the file to provision complex multi-node structures, network mappings and registries.
      - Shared Team Environment: Teams can commit a k3d-config.yaml file into Git, guaranteeing that every developer and CI runner provisions identical clusters.

2. **CI/CD Pipeline Integration**
      - Automation Mechanism: It is highly compatible with Docker-in-Docker (DinD) architectures. Pipelines can spin up, run integration tests against a real Kubernetes API and destroy the cluster in seconds.

      - CI Native Support: It natively integrates with GitHub Actions (via community actions), GitLab CI and Jenkins, running smoothly on standard, low resource cloud runners.

3. **Built-in Registry Management**
      - Automation Mechanism: Using the `--registry-create` flag during cluster initialization automatically provisions a local Docker registry container and pre configures the cluster’s [`registries.yaml`](https://k3d.io/stable/usage/registries/) file to trust it.

      - Developer Workflow: This automates a local Build, Push, Deploy pipeline without dealing with SSL certificates or authentication overhead.

4. **Custom Node & Resource Scripting**
      - Automation Mechanism: Using [`k3d node create/delete`](https://k3d.io/v5.4.6/usage/commands/k3d_node/) commands, scripts can programmatically simulate node failures, add dedicated agent pools or dynamically scale compute capacity up or down based on current automation scripts.

      - Infrastructure Mimicry: It allows automation engineers to script complex infrastructure testing scenarios (like horizontal node scaling) completely on a local laptop.

---

- **Additional Feature Kubernetes cluster monitoring:**
      1. **Built-in Metrics Routing**
         Automatically bundles the lightweight Kubernetes `metrics-server` component into the runtime engine by default. This enables immediate out of the box usage of native commands like `kubectl top nodes` and `kubectl top pods` without manual installations.

      2. **Local APM Simulation**
         Supports direct exposure of container endpoints, allowing automation scripts or developers to quickly attach Prometheus and Grafana targets to monitor cluster health and memory consumption locally.
---

- **Additional Feature Kubernetes cluster management:**

   1. **Seamless Kubeconfig Injection**
         - Automation Mechanism: By default, k3d cluster create automatically generates the necessary cluster access certificates, extracts the connection string and updates your host machine's local `~/.kube/config` file.

         - Pipeline Readiness: It features an `--update-current-context` flag, ensuring that the very next command executed by an automated script or a tool like `kubectl`, `helm` or `ArgoCD` is instantly aimed at the correct k3d instance.

   2. **Automated Local Workflows**
         - Automation Mechanism: By mounting a local directory containing a raw Kubernetes resource definitions (YAML files) or Helm charts straight into the cluster’s auto-deploy path ([/var/lib/rancher/k3s/server/manifests](https://docs.k3s.io/installation/packaged-components)), k3d watches for file changes.

         - Local GitOps: Whenever an automated code generator or a developer updates a _manifest_ file on the host machine, k3d automatically and instantly applies the changes to the live cluster without any manual execution.

   3. **Cluster Lifecycle & Resource Tuning**
      Provides high speed commands to orchestrate cluster states instantly (`k3d cluster create`, `stop`, `start` and `delete`). Resource limits are governed seamlessly by limiting the host machine's Docker daemon settings.

---

To check full information about k3d, click here:

- https://k3d.io/stable/

**Advantages and Disadvantages**

**Demo**

**Conclusions**
