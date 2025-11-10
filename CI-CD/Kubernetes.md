## **Kubernetes: Beginner Tutorial**

### 🧠 1. What Is Kubernetes?

Kubernetes (also called **K8s**) is an **open-source container orchestration system**.
It helps you **run, scale, and manage applications** that are packaged in containers (e.g., Docker containers).

Think of Kubernetes as:

> A manager that automatically decides **where, when, and how** to run your application’s containers — and makes sure they stay healthy.

---

### ⚙️ 2. Why Use Kubernetes?

| Problem                       | How Kubernetes Helps                                  |
| ----------------------------- | ----------------------------------------------------- |
| Servers crash or restart      | It **restarts containers** automatically.             |
| You need to handle more users | It **scales up** your app automatically.              |
| You deploy multiple versions  | It **rolls out updates** and can **rollback** safely. |
| Your app runs on many servers | It **balances the load** and manages networking.      |

---

### 🧩 3. Kubernetes Architecture Overview

```
+-----------------------------------------------------+
|                     Cluster                         |
|-----------------------------------------------------|
|  Control Plane             |     Worker Nodes       |
|-----------------------------------------------------|
|  - API Server              |  - Kubelet             |
|  - Scheduler               |  - Container Runtime   |
|  - Controller Manager      |  - Kube Proxy          |
|  - etcd (database)         |  - Pods (your apps)    |
+-----------------------------------------------------+
```

#### Control Plane

The **brain** of Kubernetes. It decides what should happen.

#### Nodes

The **machines (VMs or physical)** where your app’s containers actually run.

#### Pods

The **smallest deployable unit** — usually contains **one container** (sometimes more if tightly coupled).

---

### 📦 4. Basic Workflow

1. You write your app → package it in a **Docker image**.
2. You describe how to run it in a **YAML file** (Kubernetes manifest).
3. You run:

   ```bash
   kubectl apply -f app.yaml
   ```
4. Kubernetes creates the **Pods**, keeps them running, and replaces them if they crash.

---

### 🧱 5. Common Kubernetes Objects

| Object         | Description                               | YAML Example       |
| -------------- | ----------------------------------------- | ------------------ |
| **Pod**        | Runs containers                           | `kind: Pod`        |
| **Deployment** | Manages Pods, handles scaling and updates | `kind: Deployment` |
| **Service**    | Exposes your app on a network             | `kind: Service`    |
| **Ingress**    | Routes HTTP traffic to services           | `kind: Ingress`    |
| **ConfigMap**  | Stores configuration values               | `kind: ConfigMap`  |
| **Secret**     | Stores sensitive data (passwords, tokens) | `kind: Secret`     |
| **Namespace**  | Isolates environments (dev, prod, etc.)   | `kind: Namespace`  |

---

### 🧰 6. Basic Commands

| Command                       | Description             |
| ----------------------------- | ----------------------- |
| `kubectl get pods`            | List running Pods       |
| `kubectl get services`        | List Services           |
| `kubectl describe pod <name>` | Show Pod details        |
| `kubectl logs <pod>`          | See container logs      |
| `kubectl apply -f file.yaml`  | Create/update from YAML |
| `kubectl delete -f file.yaml` | Delete resources        |

---

### 🧭 7. Example: Simple App Deployment

**`deployment.yaml`**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: nginx:latest
        ports:
        - containerPort: 80
```

**`service.yaml`**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 80
  type: LoadBalancer
```

Apply:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

You now have a **scalable web app** running with **3 replicas**, accessible via the Service.

---

### **Kubernetes Glossary**

| Term                   | Meaning                                                    |
| ---------------------- | ---------------------------------------------------------- |
| **Cluster**            | A group of machines (nodes) managed by Kubernetes.         |
| **Node**               | A single machine (physical or virtual) inside the cluster. |
| **Pod**                | The smallest deployable unit; runs one or more containers. |
| **Container**          | A lightweight, isolated runtime environment for an app.    |
| **Deployment**         | Manages Pods, updates, and scaling automatically.          |
| **ReplicaSet**         | Ensures a specific number of Pods are running.             |
| **Service**            | Exposes Pods to other parts of the app or the internet.    |
| **Ingress**            | Provides HTTP/HTTPS routing to services.                   |
| **Namespace**          | Logical separation within the cluster (like folders).      |
| **ConfigMap**          | Stores non-secret configuration data.                      |
| **Secret**             | Stores sensitive data (passwords, tokens, certs).          |
| **Volume**             | Provides persistent storage for Pods.                      |
| **Kubelet**            | Agent running on each Node to manage containers.           |
| **etcd**               | The distributed database storing cluster state.            |
| **API Server**         | The front-end for the Kubernetes control plane.            |
| **Scheduler**          | Decides which Node runs a Pod.                             |
| **Controller Manager** | Ensures the system matches the desired state.              |
| **kubectl**            | Command-line tool to interact with the cluster.            |

## **Helm — “The Package Manager for Kubernetes”**

### 🔍 What It Is

Helm is like **apt** (for Ubuntu) or **npm** (for Node.js), but for **Kubernetes apps**.

It helps you:

* Install complex applications (like PostgreSQL, Nginx, Grafana, etc.)
* Manage versioned releases
* Simplify long YAML manifests

Instead of deploying 10 YAML files manually, Helm bundles them into **one package** called a **Chart**.

---

### 🧩 Key Concepts

| Concept         | Description                                                             |
| --------------- | ----------------------------------------------------------------------- |
| **Chart**       | A package that defines a Kubernetes app (contains templates + configs). |
| **Release**     | A running instance of a Chart in your cluster.                          |
| **Values.yaml** | Configuration file — lets you customize your app easily.                |

---

### 📦 Example: Installing Nginx via Helm

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-nginx bitnami/nginx
```

* This downloads the **nginx chart** from the Bitnami repo
* Creates all necessary resources (Deployment, Service, ConfigMap, etc.)
* And gives you one simple command to uninstall later:

  ```bash
  helm uninstall my-nginx
  ```

---

### ⚙️ Why Use Helm

✅ Reusable templates — no more YAML repetition
✅ Easy upgrades & rollbacks
✅ Versioned releases
✅ Works great for CI/CD (e.g. with ArgoCD)

---

## **Argo — GitOps and Workflow Automation for Kubernetes**

There are **two major Argo projects** you’ll encounter:

### 1️⃣ **Argo CD**

A **GitOps continuous delivery tool** for Kubernetes.

* You keep your desired cluster state (YAML files or Helm charts) in **Git**.
* ArgoCD automatically **syncs** your cluster to match that Git state.
* If someone manually changes the cluster, ArgoCD **detects drift** and can restore it automatically.

#### 🔁 Typical GitOps Flow:

1. You push a new commit to GitHub (e.g., change image tag to v2.0)
2. ArgoCD detects it
3. It applies the change to your cluster automatically

📊 It also provides a **web UI** where you can visualize:

* Apps
* Health status
* Sync state (in sync / out of sync)

#### Example ArgoCD Architecture

```
Git repo → ArgoCD → Kubernetes cluster
```

---

### 2️⃣ **Argo Workflows**

Another tool in the Argo family — used for **running jobs, data pipelines, or batch processes** in Kubernetes.
It lets you define complex multi-step tasks (like CI/CD pipelines or ML model training).

But when people say *“Argo”*, they usually mean **ArgoCD**.

---

### 🧠 Helm vs ArgoCD — Comparison

| Feature          | **Helm**                                            | **Argo CD**                                |
| ---------------- | --------------------------------------------------- | ------------------------------------------ |
| Purpose          | Package manager (install apps)                      | Continuous delivery (automate deployments) |
| Storage          | Local or Helm repo                                  | Git repository (single source of truth)    |
| Usage            | Manual or via CI/CD                                 | Automated (GitOps approach)                |
| Rollback         | Yes (helm rollback)                                 | Yes (sync to previous Git commit)          |
| UI               | CLI-based (optional plugins)                        | Full visual dashboard                      |
| Common Together? | ✅ Yes — ArgoCD can deploy Helm charts automatically |                                            |

---

### 🚀 Typical Real-World Setup

Most Kubernetes teams use both:

```
Developer → Commit to Git (with Helm chart)
        ↓
      Argo CD → Applies Helm chart → Kubernetes cluster
```

This approach:

* Keeps everything **declarative**
* Ensures **consistent environments**
* Allows **automatic, auditable deployments**

---

## **Quick Glossary**

| Term            | Description                                                |
| --------------- | ---------------------------------------------------------- |
| **Helm Chart**  | Packaged Kubernetes YAML templates                         |
| **Release**     | One installed instance of a Chart                          |
| **Values.yaml** | Config file to customize a Helm chart                      |
| **Argo CD**     | GitOps tool that syncs cluster state with Git              |
| **GitOps**      | Deployment model where Git is the “source of truth”        |
| **Sync**        | The process of applying Git-defined changes to the cluster |
| **Drift**       | When the live cluster differs from Git configuration       |

---

Would you like me to create a **hands-on mini-lab** where you deploy a sample app using **Helm + ArgoCD** (either locally with Minikube or in the cloud)?
That would make these concepts click in practice.


Kubernetes, как сказано в официальной документации, это система для автоматизации процесса развертывания, масштабирования и управления контейнеризированными приложениями с открытым кодом. Я бы дополнил это утверждение, сказав, что Kubernetes это скорее фреймворк, который помогает построить отказоустойчивую и масштабируемую платформу управления контейнерами. Используется декларативный подход — мы описываем, что требуется достичь, а не как.

[Docker](https://www.docker.com/):

- Docker is the container technology that allows you to containerize your applications.
- Docker is the core of using other technologies.

[Docker Compose](https://docs.docker.com/compose/)

- Docker Compose allows configuring and starting multiple Docker containers.
- Docker Compose is mostly used as a helper when you want to start multiple Docker containers and doesn't want to start each one separately using docker run ....
- Docker Compose is used for starting containers on the same host.
- Docker Compose is used instead of all optional parameters when building and running a single docker container.

[Docker Swarm](https://docs.docker.com/engine/swarm/)

- Docker swarm is for running and connecting containers on multiple hosts. 
- Docker swarm is a container cluster management and orchestration tool. 
- It manages containers running on multiple hosts and does things like scaling, starting a new container when one crashes, networking containers ...
- Docker swarm is docker in production.  
    It is the native docker orchestration tool that is embedded in the Docker Engine.
- The docker swarm file named stack file is very similar to a Docker compose file.

[Kubernetes](https://kubernetes.io/)

- Kubernetes is a container orchestration tool developed by Google. 
- Kubernete's goal is very similar to that for Docker swarm.

[Docker Cloud](https://cloud.docker.com/)

- A [paid](https://cloud.docker.com/pricing/) enterprise docker service that allows you to build and run containers on cloud servers or local servers. 
- It provides a Web UI and a central control panel to run and manage containers while providing all the docker features in a user-friendly Web interface.

Docker-compose

(from the docs): Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

Compose has commands for managing the whole lifecycle of your application:

- Start, stop, and rebuild services
- View the status of running services
- Stream the log output of running services
- Run a one-off command on a service

Kubernetes

(from [Introduction to Kubernetes](https://www.edx.org/course/introduction-to-kubernetes)): Kubernetes is a container orchestrator like Docker Swarm, Mesos Marathon, Amazon ECS, Hashicorp Nomad. Container orchestrators are the tools which group hosts together to form a cluster, and help us make sure applications:

- are fault-tolerant,
- can scale, and do this on-demand
- use resources optimally
- can discover other applications automatically, and communicate with each other
- are accessible from the external world
- can update/rollback without any downtime.

The kind of problems Kubernetes tries to solve:

- 12-factor apps,
- Automatic binpacking,
- Self-healing mechanisms,
- Horizontal scaling,
- Service discovery and Load balancing,
- Automated rollouts and rollbacks,
- Blue-Green deployments / Canary deployments
- Secrets and configuration management,
- Storage orchestration

- The Kubernetes Master is a collection of three processes that run on a single node in your cluster, which is designated as the master node. Those processes are: [kube-apiserver](https://kubernetes.io/docs/admin/kube-apiserver/), [kube-controller-manager](https://kubernetes.io/docs/admin/kube-controller-manager/) and [kube-scheduler](https://kubernetes.io/docs/admin/kube-scheduler/).
- Each individual non-master node in your cluster runs two processes:
-      [kubelet](https://kubernetes.io/docs/admin/kubelet/), which communicates with the Kubernetes Master.
-      [kube-proxy](https://kubernetes.io/docs/admin/kube-proxy/), a network proxy which reflects Kubernetes networking services on each node.

The basic Kubernetes objects include:

- [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)
- [Service](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Volume](https://kubernetes.io/docs/concepts/storage/volumes/)
- [Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

Kubernetes also contains higher-level abstractions that rely on [Controllers](https://kubernetes.io/docs/concepts/architecture/controller/) to build upon the basic objects, and provide additional functionality and convenience features. These include:

- [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
- [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
- [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
- [Job](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)

- Service discovery and load balancing  
    Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
- Storage orchestration  
    Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
- Automated rollouts and rollbacks  
    You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- Automatic bin packing  
    You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
- Self-healing  
    Kubernetes restarts containers that fail, replaces containers, kills containers that don’t respond to your user-defined health check, and doesn’t advertise them to clients until they are ready to serve.
- Secret and configuration management  
    Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.

Master components

kube-apiserver

The API server is a component of the Kubernetes [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane.

The main implementation of a Kubernetes API server is [kube-apiserver](https://kubernetes.io/docs/reference/generated/kube-apiserver/). kube-apiserver is designed to scale horizontally—that is, it scales by deploying more instances. You can run several instances of kube-apiserver and balance traffic between those instances.

etcd

Consistent and highly-available key value store used as Kubernetes’ backing store for all cluster data.

If your Kubernetes cluster uses etcd as its backing store, make sure you have a [back up](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster) plan for those data.

You can find in-depth information about etcd in the official [documentation](https://etcd.io/docs/).

kube-scheduler

Component on the master that watches newly created pods that have no node assigned, and selects a node for them to run on.

Factors taken into account for scheduling decisions include individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference and deadlines.

kube-controller-manager

Component on the master that runs [controllers](https://kubernetes.io/docs/concepts/architecture/controller/).

Logically, each [controller](https://kubernetes.io/docs/concepts/architecture/controller/) is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.

These controllers include:

- Node Controller: Responsible for noticing and responding when nodes go down.
- Replication Controller: Responsible for maintaining the correct number of pods for every replication controller object in the system.
- Endpoints Controller: Populates the Endpoints object (that is, joins Services & Pods).
- Service Account & Token Controllers: Create default accounts and API access tokens for new namespaces.

cloud-controller-manager

[cloud-controller-manager](https://kubernetes.io/docs/tasks/administer-cluster/running-cloud-controller/) runs controllers that interact with the underlying cloud providers. The cloud-controller-manager binary is an alpha feature introduced in Kubernetes release 1.6.

cloud-controller-manager runs cloud-provider-specific controller loops only. You must disable these controller loops in the kube-controller-manager. You can disable the controller loops by setting the --cloud-provider flag to externalwhen starting the kube-controller-manager.

cloud-controller-manager allows the cloud vendor’s code and the Kubernetes code to evolve independently of each other. In prior releases, the core Kubernetes code was dependent upon cloud-provider-specific code for functionality. In future releases, code specific to cloud vendors should be maintained by the cloud vendor themselves, and linked to cloud-controller-manager while running Kubernetes.

The following controllers have cloud provider dependencies:

- Node Controller: For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
- Route Controller: For setting up routes in the underlying cloud infrastructure
- Service Controller: For creating, updating and deleting cloud provider load balancers
- Volume Controller: For creating, attaching, and mounting volumes, and interacting with the cloud provider to orchestrate volumes

Node Components

Kubelet

An agent that runs on each node in the cluster. It makes sure that containers are running in a pod.

The kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn’t manage containers which were not created by Kubernetes.

kube-proxy

[kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/) is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/)concept.

kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

kube-proxy uses the operating system packet filtering layer if there is one and it’s available. Otherwise, kube-proxy forwards the traffic itself.

Container Runtime

The container runtime is the software that is responsible for running containers.

Kubernetes supports several container runtimes: [Docker](http://www.docker.com), [containerd](https://containerd.io), [cri-o](https://cri-o.io/), [rktlet](https://github.com/kubernetes-incubator/rktlet) and any implementation of the [Kubernetes CRI (Container Runtime Interface)](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md).