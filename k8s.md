Sure! Here's a clean, copy-ready **Kubernetes Cheat Sheet** – structured for fast reference and real-world usage:

---

# 🧠 Kubernetes Cheat Sheet

## ✅ Core Concepts
| **Component**  | **Description** |
|----------------|-----------------|
| **Pod**        | Smallest unit that runs one or more containers. |
| **ReplicaSet** | Ensures specified number of pod replicas are running. |
| **Deployment** | Declarative updates for Pods and ReplicaSets. |
| **StatefulSet**| Manages stateful applications. |
| **DaemonSet**  | Runs one pod per node (useful for log collectors, etc). |
| **Service**    | Stable endpoint to access pods (ClusterIP, NodePort, LoadBalancer). |
| **Ingress**    | HTTP/HTTPS routing to services. |
| **ConfigMap**  | Pass non-sensitive config data to pods. |
| **Secret**     | Pass sensitive data (passwords, keys) to pods. |
| **Namespace**  | Virtual clusters within a K8s cluster. |
| **Volume / PVC** | Persistent storage for pods. |
| **Job / CronJob** | Run tasks to completion or on a schedule. |

---

## 🛠️ kubectl Basics

```bash
kubectl version                                # Show client and server versions
kubectl cluster-info                           # Show cluster info
kubectl get nodes                              # List all nodes
kubectl get all                                # Show all resources in namespace
kubectl get pod -o wide                        # Pods with node & IP info
kubectl describe pod <pod-name>                # Detailed pod info
kubectl delete pod <pod-name>                  # Delete a pod
kubectl exec -it <pod-name> -- /bin/sh         # Access a running container
kubectl logs <pod-name>                        # View logs
```

---

## 🧱 Workloads

### 🌀 Deployments

```bash
kubectl create deployment nginx --image=nginx
kubectl get deployments
kubectl scale deployment nginx --replicas=3
kubectl rollout status deployment nginx
kubectl rollout undo deployment nginx
kubectl delete deployment nginx
```

---

### 🧰 Pods

```bash
kubectl run test-pod --image=nginx --restart=Never
kubectl get pods
kubectl describe pod <pod>
```

---

### 📦 Services

```bash
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl get svc
```

---

## 📁 Configs

### ConfigMap

```bash
kubectl create configmap my-config --from-literal=ENV=prod
kubectl describe configmap my-config
```

### Secret

```bash
kubectl create secret generic my-secret --from-literal=password=MyP@ss
kubectl get secrets
kubectl describe secret my-secret
```

---

## 🌐 Networking

### Ingress

```bash
kubectl apply -f ingress.yaml
kubectl get ingress
```

### Port Forwarding

```bash
kubectl port-forward svc/my-service 8080:80
```

---

## 💾 Storage

```bash
kubectl get pvc                         # PersistentVolumeClaims
kubectl get pv                          # PersistentVolumes
kubectl describe pvc <name>
```

---

## ⏱️ Jobs & CronJobs

```bash
kubectl create job pi --image=perl -- perl -Mbignum=bpi -wle "print bpi(2000)"
kubectl create cronjob hello --image=busybox --schedule="*/1 * * * *" -- echo Hello
```

---

## 📜 YAML Example: Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 2
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
          image: nginx:1.21
          ports:
            - containerPort: 80
```

---

## 🔐 RBAC Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

---

## 🧹 Clean Up

```bash
kubectl delete pod <pod-name>
kubectl delete deploy <deployment-name>
kubectl delete svc <service-name>
kubectl delete ns <namespace-name>
kubectl delete -f file.yaml
```

---

## 📦 Useful Shortcuts

```bash
alias k='kubectl'
alias kgp='kubectl get pods'
alias kgs='kubectl get svc'
alias kaf='kubectl apply -f'
alias kdf='kubectl delete -f'
```
---
### 🚀 **1. Deployment**
- **Use it when**: Your app is **stateless** – doesn't need to remember previous state (like most web apps or APIs).
- **Pods**: Identical, interchangeable.
- **Pod names**: Random and not stable.
- **Storage**: Shared or ephemeral, not persistent by pod identity.
- **Scaling**: Easy horizontal scaling – just increase the replica count.
- **Use Cases**: Nginx, frontend apps, stateless microservices, APIs.

🧠 **Think of it like**: Spinning up identical fast-food counters – any one can serve the next customer.

---

### 🧾 **2. StatefulSet**
- **Use it when**: Your app is **stateful** – it needs to **remember** things or maintain identity.
- **Pods**: Have **unique**, **sticky identities** (e.g., `app-0`, `app-1`, ...).
- **Pod names**: Predictable and **stable**.
- **Storage**: Each pod gets its **own persistent volume** (PVC) that **sticks** with it even if pod is recreated.
- **Scaling**: Ordered, controlled scaling and rolling updates.
- **Use Cases**: Databases (MySQL, Cassandra, MongoDB), Kafka, Zookeeper.

🧠 **Think of it like**: Assigning hotel rooms – each guest (pod) has their own room (volume) and identity.

---

### 🔍 TL;DR Comparison Table:

| Feature              | **Deployment**                  | **StatefulSet**                        |
|----------------------|----------------------------------|----------------------------------------|
| App Type             | Stateless                        | Stateful                                |
| Pod Identity         | Not preserved                    | Stable, persistent                      |
| Pod Name             | Random (e.g., `web-xyz`)         | Predictable (e.g., `db-0`, `db-1`)      |
| Storage              | Shared/Ephemeral                 | Dedicated Persistent Volumes (PVCs)     |
| Pod Order            | Not guaranteed                   | Start/stop/update in defined order      |
| Use Case             | Web servers, API apps            | Databases, Kafka, Zookeeper             |

