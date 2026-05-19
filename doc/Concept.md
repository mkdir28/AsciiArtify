## Introduction

**AsciiArtify** plans to develop a new software product for converting images to ascii-art using Machine Learning.
In _Concept.md_ file will be presented comparative analysis of three tools for deploying Kubernetes clusters in a local environment - minikube, kind and k3d.

---

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

---

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
      `kind` operates entirely within Docker. This allows automation scripts to instantiate a full cluster, run integration tests, and instantly destroy it `kind delete cluster`. It leaves zero hypervisor artifacts behind, guaranteeing a perfectly clean slate for every automated test run.

   2. **CI/CD Pipeline Integration**
      `kind` does not require a Virtual Machine, `kind` is the industry standard for running inside CI/CD pipelines. It works flawlessly in Docker-in-Docker (DinD) environments (like GitHub Actions, GitLab CI, and CircleCI), allowing teams to automate end-to-end testing against a real Kubernetes API.

   3. **Declarative Infrastructure**
      Complex cluster topologies and custom API server flags can be fully automated via a single YAML configuration file. Guarantees that every developer and CI pipeline provisions the exact same environment.

     ```bash
     kind create cluster --config cluster-config.yaml
     ```

---

- **Additional Feature Kubernetes cluster monitoring:**

   1. **Native Upstream Compatibility**
      `kind` uses standard `kubeadm` to bootstrap its nodes, can automate the deployment of the official Kubernetes `metrics-server` or the `Prometheus/Grafana` stack using standard Helm charts or `kubectl apply` commands.

   2. **Native Upstream Compatibility**
      Every Kubernetes node in `kind` is simply a Docker container running on your host machine, it is possible to leverage native Docker automation tools. A simple `docker stats` command provides immediate, real-time CPU and memory monitoring for your entire cluster infrastructure.
---

- **Additional Feature Kubernetes cluster management:**
   1. **Programmatic Lifecycle Management**
      Provides rapid, script-friendly commands `kind create cluster` , `kind get clusters` , `kind delete cluster` designed for headless operation without requiring interactive user inputs.

   2. **Advanced Topology & Node Management**
      Allows developers to manage and simulate complex production architectures locally. By editing the `kind` config file, developer manage custom port-mappings to the host machine, define High Availability (HA) master nodes or apply specific taints and labels to worker nodes before the cluster even boots.

   3. **Seamless Kubeconfig Automation**
      Upon creation, `kind` automatically generates the necessary cluster certificates, maps the dynamic Docker ports and merges the connection context directly into the host's `~/.kube/config file`. It instantly sets the active context, meaning automated deployment tools are immediately connected to the new cluster.
---

To check full information about kind, click here:

- https://kind.sigs.k8s.io

---

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

---

## Advantages and Disadvantages**


| Criteria | Minikube | Kind (Kubernetes IN Docker) | k3d (k3s in Docker) |
| :--- | :--- | :--- | :--- |
| **Ease of Use** | **Very High** Highly guided experience. Features a massive built-in add-on system (`minikube addons enable`) for easy setup of dashboards and metrics. | **Medium** Geared towards DevOps. Requires Docker knowledge and writing declarative YAML configurations to expose host ports or add ingress. | **High** Provides an excellent out-of-the-box experience. Comes pre-packaged with a LoadBalancer and Traefik Ingress. |
| **Deployment Speed** | **Slow** Usually takes 2-5 minutes to boot, as it pulls heavier images and often relies on full virtualization. | **Fast** Usually ready in 30-60 seconds. Uses lightweight container provisioning. | **Fastest** Usually ready in 15-30 seconds. Uses highly optimized, stripped-down binaries. |
| **Stability** | **Very Stable** Official CNCF project. Perfectly mirrors a heavy, production-grade Kubernetes cluster. | **Highly Stable** Official Kubernetes SIG project. Designed specifically for passing strict upstream Kubernetes conformance tests. | **Stable** Community driven project and backed by Rancher/SUSE's k3s engine. However, its heavily modified architecture can occasionally cause minor edge case discrepancies. |
| **Docs & Community** | [**Excellent**](https://minikube.sigs.k8s.io/docs/) The oldest and most popular tool. Contains tutorials, StackOverflow answers and community guides exist. | [**Very Good**](https://kind.sigs.k8s.io) The industry standard for enterprise CI/CD. The official Kubernetes documentation is robust. | [**Good**](https://k3d.io/stable/) Rapidly growing, but a smaller community than Minikube. Often requires relying on upstream `k3s` documentation for deep troubleshooting. |
| **Config Complexity** | **Easy - Medium** Simple for single-node setups, but configuring VM drivers, resource limits, and network bridges can become difficult. | **Medium** Requires writing custom []`kind-config.yaml`](https://kind.sigs.k8s.io/docs/user/configuration/) files to simulate multi-node architectures or map host ports. | **Easy** Features a highly programmable CLI. Complex multi-node setups and port-mappings can be done instantly via single-line [terminal commands](https://k3d.io/v5.4.6/usage/commands/k3d_cluster_create/) or also via [YAML](https://k3d.io/v5.1.0/usage/configfile/) file configuration. |
| **Advantages** | - 100% K8s conformance.<br>- Supports many drivers (Docker, VMs, Bare-metal).<br>- Built-in GUI dashboard. | - Pure Kubernetes environment.<br>- Flawless multi-node simulation.<br>- The absolute best tool for automated CI/CD pipeline testing.<br>- Clean, ephemeral environments. | - Incredibly low footprint (runs on 512MB RAM).<br>- Built-in local registry support.<br>- Lightning-fast iteration cycles. |
| **Disadvantages** | - Massive resource hog (requires high CPU/RAM).<br>- Multi-node cluster support is still considered somewhat experimental. | - Data is strictly ephemeral (deleted upon restart) by default.<br>- Lacks built-in Ingress/LoadBalancer (requires MetalLB). | - Not 100% "pure" K8s (replaces `etcd` with SQLite, strips out legacy cloud providers).<br>- Might fail when testing highly specific Kubernetes internals. |


**Demo**

**Conclusions**
