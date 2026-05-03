
---

# 📘 | Kubernetes Deployment & ReplicaSets

**Source**: Abhishek Veeramalla — Free DevOps Course (YouTube)

This lesson bridges the gap from basic Pods to production‑ready Kubernetes by introducing **ReplicaSets** and **Deployments**.

---

## 📌 Key Learning Outcomes
- Pod vs ReplicaSet vs Deployment  
- Auto‑healing in Kubernetes  
- Hands‑on demo: create, scale, delete, auto‑recover  

---

#  1. Core Concepts

### 🔹 Why Pods Alone Are Not Enough
- Pod = smallest deployable unit  
- If a Pod dies → it stays dead  
- No automatic scaling  
- No rolling updates  
- No self‑healing  

---

### 🔹 ReplicaSet (RS) — *Keeper of Desired State*
- Ensures a fixed number of Pod replicas are always running  
- Auto‑heals: if a Pod dies, RS creates a new one instantly  
- Supports scaling up/down  
- Rarely used directly in production (low‑level object)  

---

### 🔹 Deployment — *High‑Level Recommended Object*
- Manages ReplicaSets → which manage Pods  
- When you create a Deployment:
  - It creates a ReplicaSet  
  - ReplicaSet creates Pods  

**Extra superpowers:**
- Rolling updates (zero downtime)  
- Rollbacks  
- Pause/resume  
- Revision history  

---

## 📊 Quick Comparison

| Feature           | Pod                          | ReplicaSet           | Deployment                          |
| ----------------- | ---------------------------- | -------------------- | ----------------------------------- |
| Self‑healing      | No                           | Yes                  | Yes (via ReplicaSet)                |
| Scaling           | Manual only                  | Yes                  | Yes (preferred)                     |
| Rolling updates   | No                           | No                   | Yes                                 |
| Rollback          | No                           | No                   | Yes                                 |
| How to create     | `kubectl create -f pod.yaml` | Rarely used directly | `kubectl create -f deployment.yaml` |
| Real‑world usage  | Rarely                       | Almost never         | 99% of production workloads         |

---

# 🧾 2. Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3                    # desired number of Pods
  selector:
    matchLabels:
      app: nginx
  template:                      # Pod template
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

✅ Ready to copy‑paste and apply with `kubectl create -f deployment.yaml`.

---

## 🔑 Key Notes
- `spec.replicas` → tells ReplicaSet how many Pods to keep  
- `spec.selector` + `template.metadata.labels` → how RS identifies Pods  
- `template` section = Pod spec (from Day‑33)  

---

# ⚙️ 3. Hands‑On Demo Commands

| Step              | Command                                                  | What happens                             |
| ----------------- | -------------------------------------------------------- | ---------------------------------------- |
| Create Deployment | `kubectl create -f deployment.yaml`                      | Creates Deployment → ReplicaSet → 3 Pods |
| Check everything  | `kubectl get all`                                        | Shows Deployment, ReplicaSet, Pods       |
| See ReplicaSet    | `kubectl get rs`                                         | Shows the ReplicaSet                     |
| Scale             | `kubectl scale deployment nginx-deployment --replicas=5` | Creates more Pods                        |
| Auto‑healing demo | `kubectl delete pod <pod-name>`                          | New Pod created instantly                |
| Check details     | `kubectl describe deployment nginx-deployment`           | Shows events and status                  |
| Delete everything | `kubectl delete deployment nginx-deployment`             | Deletes all resources                    |

---

## 💡 Pro Tips
- Always run `kubectl get all` after changes  
- Watch `READY` and `RESTARTS` columns  
- New Pod appears in **< 1 second**  
- Naming pattern:
  - Deployment → `nginx-deployment`  
  - ReplicaSet → `nginx-deployment-abc123`  
  - Pods → auto‑generated  

---

# 🔄 4. Auto‑Healing in Action
- Deployment running with 3 Pods  
- Delete one Pod manually  
- ReplicaSet detects mismatch  
- New Pod created instantly  

👉 Desired state is always maintained.

---

# 🎯 5. Interview Prep
- Difference between Pod, ReplicaSet, and Deployment  
- Why ReplicaSet isn’t used directly  
- How auto‑healing works  
- What happens when you scale a Deployment  

---


