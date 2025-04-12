Here’s your **Kubernetes Cheat Sheet** – a compact guide for quick reference, perfect for beginners and seasoned K8s engineers alike.

---

## 🧠 **Core Concepts**

| Concept | Description |
|--------|-------------|
| **Pod** | Smallest unit. Runs one or more containers. |
| **ReplicaSet** | Ensures a specified number of pod replicas are running. |
| **Deployment** | Manages ReplicaSets, provides declarative updates. |
| **Service** | Stable networking endpoint for a group of pods. |
| **ConfigMap / Secret** | Inject configs or sensitive data into pods. |
| **Namespace** | Isolated environments within a cluster. |
| **Ingress** | HTTP(S) routing to services inside the cluster. |
| **Volume** | Persistent or ephemeral storage attached to a pod. |
| **Job / CronJob** | One-off or scheduled batch tasks. |

---

## 🛠️ **kubectl Basics**

### Cluster & Context
```bash
kubectl config view                            # Show kubeconfig details
kubectl config get-contexts                    # List contexts
kubectl config use-context my-context          # Switch context
```

### Namespace
```bash
kubectl get ns                                 # List namespaces
kubectl create ns my-namespace                 # Create namespace
kubectl config set-context --current --namespace=my-namespace
```

---

## 🚀 **Workloads**

### Pods
```bash
kubectl get pods [-n <ns>]                     # List pods
kubectl describe pod <pod>                     # Pod details
kubectl logs <pod>                             # Pod logs
kubectl exec -it <pod> -- /bin/sh              # Shell into a pod
```

### Deployments
```bash
kubectl create deploy nginx --image=nginx
kubectl get deploy                             # List deployments
kubectl rollout restart deploy <name>          # Restart pods
kubectl scale deploy <name> --replicas=3       # Scale
kubectl delete deploy <name>                   # Delete deployment
```

---

## 🔧 **Services & Networking**

```bash
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl get svc                                # List services
kubectl get endpoints                          # Show service-pod mapping
```

### Ingress
```bash
kubectl apply -f ingress.yaml
kubectl get ingress
```

---

## 📦 **Configs & Secrets**

### ConfigMap
```bash
kubectl create configmap my-config --from-literal=env=prod
kubectl get configmaps
```

### Secret
```bash
kubectl create secret generic my-secret --from-literal=password=123
kubectl get secrets
```

---

## 📂 **Volumes & Storage**

```bash
kubectl get pvc                                # Persistent Volume Claims
kubectl describe pvc <name>
kubectl get pv                                 # Persistent Volumes
```

---

## ⏲️ **Jobs & CronJobs**

```bash
kubectl create job my-job --image=busybox -- echo "Hello"
kubectl create cronjob my-cron --image=busybox --schedule="*/1 * * * *" -- echo "Run"
```

---

## 🧪 **Debugging & Monitoring**

```bash
kubectl describe <resource> <name>
kubectl logs <pod>
kubectl top pod                                # Requires metrics-server
kubectl get events                             # Show recent events
```

---

## 🧾 **YAML Sample: Deployment**

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

## 🧹 **Clean-up**

```bash
kubectl delete pod <name>
kubectl delete deploy <name>
kubectl delete ns <name>
kubectl delete -f <file>.yaml
```

---

## 🔐 **RBAC Example**

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

Would you like this cheat sheet as a **printable PDF**, a **wallpaper-style image**, or formatted into an **interactive page** for easier access?
