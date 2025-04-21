![image](https://github.com/user-attachments/assets/0fc8ec3b-6345-4275-ac2e-9a343cedaedd){ width=50% }

## 🧠 **Kubernetes Basic Architecture**
### 🔹 **1. Master Node (Control Plane)** – *The Brain 🧠*
Responsible for managing the whole cluster – scheduling, scaling, maintaining desired state.
**Key components:**
| Component        | Role |
|------------------|------|
| **API Server**   | Front door to K8s – all requests (kubectl, tools) go here |
| **Scheduler**    | Decides which node runs a pod |
| **Controller Manager** | Ensures desired state (e.g., restart a crashed pod) |
| **etcd**         | Cluster’s key-value store – holds config & state data |
---
### 🔹 **2. Worker Nodes** – *The Muscles 💪*
These run your actual applications (pods/containers).
**Key components:**
| Component        | Role |
|------------------|------|
| **kubelet**      | Talks to API server, runs/monitors pods |
| **kube-proxy**   | Handles network routing for pods/services |
| **Container Runtime** | Runs containers (e.g., containerd, Docker) |
---
### 🔹 **3. Pod** – *The Smallest Deployable Unit 📦*
- Wraps one or more containers.
- All containers in a pod share **network** and **storage**.
- Managed by controllers like Deployment, StatefulSet, etc.
---
### 🔹 **4. Node & Cluster**
- **Node** = A single machine (VM or physical).
- **Cluster** = Group of nodes managed by Kubernetes.
---
### 🔹 **5. Add-ons (Optional but Important)**
- **Dashboard** – Web UI
- **CoreDNS** – Internal DNS service
- **Ingress Controller** – Manage external access to services
- **Metrics Server** – Collects resource metrics (CPU, memory)

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
---
## 🌐 What is **Ingress** in Kubernetes?
**Ingress** is a Kubernetes object that **manages external access** to services inside your cluster, typically over **HTTP and HTTPS**.
Think of it as a **smart gateway** that:
- Listens for web requests.
- Understands URLs, domains, paths, etc.
- Routes traffic to the right app/service inside the cluster.
---
## 🧭 How Ingress Works (Flow)
1. 🌍 **User** hits a URL → `https://myapp.company.com`
2. 📥 **DNS** maps it to the **Ingress Controller**’s external IP.
3. 🔁 **Ingress Controller** (like NGINX, Traefik, etc.) receives the request.
4. 📜 It looks up **Ingress rules** (path/domain-based routing).
5. 🚀 Forwards the request to the correct **Kubernetes Service**.
6. 📦 The service sends it to the correct **Pod** (your app).
---
## 🧩 Components Involved
| Component | Role |
|----------|------|
| **Ingress Resource** | YAML object that defines the routing rules. |
| **Ingress Controller** | Software that implements the rules (e.g., NGINX, Traefik). |
| **Service** | K8s service that exposes your Pod. |
| **Pod** | Where your application runs. |
---
## 🔁 Example: Ingress YAML
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: myapp.company.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: myapp-service
            port:
              number: 80
```
### What this does:
- If someone goes to `http://myapp.company.com`, they get routed to the `myapp-service` (which routes to your Pod).
---
## 🔐 Optional Features
- **TLS/SSL termination**
- **Path-based routing**: `/app1` → app1 service, `/app2` → app2 service.
- **Host-based routing**: `app1.company.com` → app1, `app2.company.com` → app2.
- **Rewrite URLs**, add headers, rate limiting (depends on controller).
---
## 🔥 Common Ingress Controllers
- NGINX Ingress Controller (most popular)
- Traefik
- HAProxy
- Istio Ingress Gateway (if using Istio)
---
#### 💡 TL;DR
**Ingress** is the "traffic cop" that decides which service gets which web request. You define rules in an Ingress resource, and the Ingress Controller routes traffic accordingly.
---
Awesome! Let’s demystify **Istio** and **Service Mesh** in a simple way 👇
---
## 🧩 What is a **Service Mesh**?
A **Service Mesh** is a **dedicated infrastructure layer** that helps manage communication between **microservices** in a distributed system (like in Kubernetes).
It handles things like:
- 🚦 **Traffic routing & load balancing**
- 🔒 **Security (mTLS, authorization)**
- 👀 **Observability (metrics, logs, traces)**
- 🔁 **Resilience (retries, timeouts, circuit breakers)**

> **Without changing your application code!** 🎯
---
## 🌐 What is **Istio**?
**Istio** is a popular **open-source service mesh** that runs on Kubernetes.  
It provides all the features of a service mesh by injecting a **sidecar proxy** (Envoy) alongside each microservice.
---
## 🧠 How Istio Works (High-Level Architecture)
1. ✅ **Istio Control Plane** (Istiod):
   - Configures and manages the mesh.
   - Applies routing, security, and policies.
2. 🔁 **Data Plane**:
   - Each **Pod** gets an **Envoy Proxy** sidecar.
   - All service-to-service communication goes **through the sidecars**.
---
### 📦 Istio Components
| Component | Role |
|----------|------|
| **Envoy Proxy** | Sidecar proxy that intercepts all traffic to/from the service |
| **Istiod** | Brain of Istio – controls configuration, policy, certificates |
| **Ingress/Egress Gateway** | Handles external traffic in/out of the mesh |
---
## 🔐 Key Istio Features
| Feature | Description |
|--------|-------------|
| 🔁 **Traffic Management** | Canary deployments, traffic splitting, mirroring, failovers |
| 🔐 **Security (mTLS)** | Encrypts service-to-service traffic automatically |
| 👀 **Observability** | Metrics (Prometheus), logs, tracing (Jaeger, Zipkin) |
| 🧪 **Policy Enforcement** | RBAC, quotas, allow/deny rules |
| 🔄 **Fault Injection** | Simulate errors or delays for testing resilience |
---
## 🚦 Example: Traffic Routing with Istio
You can split traffic like this in an Istio `VirtualService`:
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
      weight: 80
    - destination:
        host: reviews
        subset: v2
      weight: 20
```
## 🧠 TL;DR
| Term | Meaning |
|------|---------|
| **Service Mesh** | Layer that handles service-to-service communication features |
| **Istio** | A service mesh implementation for Kubernetes |
| **Sidecar Proxy** | A small proxy container that handles all app traffic |
| **Istiod** | The control plane that manages everything |
---
