
# Kubernetes Notes - Abhishek Veeramalla Free DevOps Course

**Course Playlist**: [Kubernetes Series](https://www.youtube.com/playlist?list=PLdpzxOOAlwvJdsW6A0jCz_3VaANuFMLpc)

---

## 📌 Overview

These notes cover the **fundamental shift** from Docker to Kubernetes:

**→ **Understanding **Pods** (the smallest deployable unit)
**→ **ReplicaSets** and **Deployments** (production-ready way to run apps)

###  Key Mindset Change

> Stop thinking in containers.
> Start thinking in **Pods** wrapped by **Deployments**.

---

# Kubernetes Pods | Deploy Your First App

## 1. Container vs Pod

| Aspect         | Docker Container               | Kubernetes Pod                             |
| -------------- | ------------------------------ | ------------------------------------------ |
| Definition     | Smallest unit in Docker        | Smallest **deployable** unit in Kubernetes |
| What it runs   | One process/application        | Wrapper around **1 or more containers**    |
| Networking     | Separate network per container | Shared network (`localhost`, `127.0.0.1`)  |
| Storage        | Volumes per container          | Shared volumes                             |
| Lifecycle      | Independent                    | Containers live/die **together**           |
| Management     | Imperative (`docker run`)      | Declarative (YAML)                         |
| Kubernetes use | Not used directly              | Kubernetes **deploys Pods only**           |

👉 **Note**:

* 99% of Pods run a **single container**
* Multi-container Pods are used for:

  * Sidecars (logging, monitoring)
  * Init containers

---

## 2. Why YAML instead of Docker CLI?

* Declarative approach
* Version control (Git)
* Reusable & standardized
* Cleaner than long CLI commands

---

## 3. Simple nginx Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx-container
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

---

## 4. Essential kubectl Commands

```bash
kubectl create -f pod.yaml
kubectl get pods
kubectl get pods -o wide
kubectl describe pod <name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/bash
kubectl delete pod <name>
kubectl port-forward pod/nginx 8080:80
```

### 🔗 Access Methods

* `minikube ssh` + curl Pod IP
* `kubectl port-forward`

---

#  Kubernetes Deployment | ReplicaSets

## 1. Pod vs ReplicaSet vs Deployment

| Object     | Responsibility                         | Self-Healing | Scaling | Rolling Updates | Real-world Usage |
| ---------- | -------------------------------------- | ------------ | ------- | --------------- | ---------------- |
| Pod        | Runs your application                  | ❌            | ❌       | ❌               | Rarely used      |
| ReplicaSet | Maintains number of Pods               | ✅            | ✅       | ❌               | Rarely used      |
| Deployment | Manages ReplicaSet + advanced features | ✅            | ✅       | ✅               | **Production**   |

###  Simple Analogy

* Pod = Worker
* ReplicaSet = Babysitter
* Deployment = Manager

---

## 2. Why not use ReplicaSet directly?

ReplicaSet only maintains **count**.

Deployment adds:

* Rolling updates (zero downtime)
* Rollbacks
* Revision history
* Pause/Resume

👉 **Rule**: Always use **Deployment**

---

## 3. Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

---

## 4. Auto-Healing – How it Works

* You set `replicas: 3`
* Deployment → creates ReplicaSet
* ReplicaSet → creates 3 Pods
* If a Pod dies → new Pod is created instantly

### 🧪 Demo

```bash
kubectl delete pod <pod-name>
```

---

## 5. Scaling a Deployment

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

### What happens:

* Desired state updated
* ReplicaSet adjusts Pods
* New Pods follow same template

---

## 6. Essential Commands

```bash
kubectl create -f deployment.yaml
kubectl get all
kubectl get rs
kubectl describe deployment <name>
kubectl scale deployment <name> --replicas=5
kubectl delete deployment <name>
```

---

#  Interview Questions Covered

* Difference between Docker Container and Kubernetes Pod?
* Pod vs ReplicaSet vs Deployment?
* Why not use ReplicaSet directly?
* How does auto-healing work?
* What happens during scaling?

---

# 💡 Pro Tips

* Use `kubectl get all` for full visibility
* Never manage Pods directly
* Store YAML files in Git
* Use `kubectl describe` → Events for debugging
* **Minikube** is perfect for learning

---

# | Kubernetes Pods | Deploy Your First App

**Video**: Abhishek Veeramalla Free DevOps Course (YouTube)
**Focus**: Understanding Pods (the smallest deployable unit in Kubernetes) + hands-on deployment of your first app (nginx) using Minikube and kubectl

---

## 📌 Overview

* Understanding **Pods** (smallest deployable unit in Kubernetes)
* Hands-on deployment using **Minikube + kubectl**
* Video is mostly **practical after initial theory**
* Runs on **local single-node cluster (Minikube)**

---

## ⚙️ Prerequisites

* Days 30–32

  * Docker vs Kubernetes
  * Kubernetes Architecture
  * Basic Setup

---

# 🧠 1. Core Concepts

## 🔹 Container vs Pod

* Docker deploys containers directly (`docker run`)
* Kubernetes **never deploys raw containers**
* Smallest unit in Kubernetes = **Pod**
* Pod = wrapper around **one or more containers**

👉 Most Pods run **1 container (99% cases)**

### Multi-container Pods used for:

* Sidecar containers (logging, monitoring)
* Init containers

---

## 🔹 Shared Features Inside a Pod

Containers inside the same Pod share:

* Network namespace → communicate via `localhost / 127.0.0.1`
* Storage volumes
* Same lifecycle (all die together)

---

## 🔹 Why YAML instead of Docker CLI?

* Declarative (not imperative)
* Version-controlled in Git
* Standardized, reusable, reviewable
* Cleaner than long `docker run` commands

---

## 🔹 Simple nginx Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx-container
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

---

# 🛠️ 2. Setup: kubectl + Minikube

| Step             | Command / Action                   | Notes                              |
| ---------------- | ---------------------------------- | ---------------------------------- |
| Install kubectl  | Official script from kubernetes.io | Verify: `kubectl version --client` |
| Install Minikube | Download from minikube.sigs.k8s.io | —                                  |
| Start cluster    | `minikube start`                   | Uses Docker driver by default      |
| Verify           | `kubectl get nodes`                | Node should be **Ready**           |

---

# 🚀 3. Create & Deploy Your First Pod

### Step 1: Create file

`pod.yaml` (use YAML shown above)

### Step 2: Deploy

```bash
kubectl create -f pod.yaml
```

### Step 3: Check status

```bash
kubectl get pods
kubectl get pods -o wide
```

---

# ⚡ 4. Essential kubectl Commands

| Command                  | Purpose                | Example                               |
| ------------------------ | ---------------------- | ------------------------------------- |
| kubectl get pods         | List all pods          | —                                     |
| kubectl get pods -o wide | Show IP & node         | —                                     |
| kubectl describe pod     | Detailed info + Events | `kubectl describe pod nginx`          |
| kubectl logs             | View logs              | `kubectl logs nginx`                  |
| kubectl exec             | Enter container        | `kubectl exec -it nginx -- /bin/bash` |
| kubectl delete pod       | Delete pod             | `kubectl delete pod nginx`            |

---

# 🌐 5. Accessing the Application

Since no LoadBalancer is used, two simple options:

## 🔹 Option 1 – Minikube SSH

```bash
minikube ssh
curl <pod-ip>
```

(Pod IP from `kubectl get pods -o wide`)

---

## 🔹 Option 2 – Port Forward (Recommended)

```bash
kubectl port-forward pod/nginx 8080:80
```

👉 Open in browser:
`http://localhost:8080`

---

# 🧩 6. Debugging & Common Issues

* Pod not running →
  `kubectl describe pod <name>` → check **Events**

* Common errors:

  * ImagePullBackOff
  * CrashLoopBackOff

* Application issues →
  `kubectl logs <pod-name>`

* Container-level debugging →
  `kubectl exec -it <pod-name> -- /bin/bash`

👉 Important:
Pods **do not auto-heal or auto-scale**
If a Pod dies, it stays dead

---

# ⭐ 7. Key Advantages of Pods

* Shared network & storage
* Stable cluster IP (via kube-proxy)
* Declarative configuration
* Foundation for:

  * Deployments
  * ReplicaSets
  * Services

---

# 🎯 8. Interview Questions Covered

* Difference between Docker container and Kubernetes Pod?
* Why do multi-container Pods use `localhost`?
* How do you debug a failing Pod?
* Pod vs Deployment?

---


