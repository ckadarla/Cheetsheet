Here’s a **GKE (Google Kubernetes Engine) cheatsheet** along with its **unique features** explained in simple terms.

---

## 🚀 GKE Cheatsheet (Quick Reference)

### 🔧 Basic Commands
| Task                        | Command |
|-----------------------------|---------|
| Get credentials for GKE     | `gcloud container clusters get-credentials CLUSTER_NAME --region REGION` |
| List clusters               | `gcloud container clusters list` |
| View nodes                  | `kubectl get nodes` |
| View pods                   | `kubectl get pods` |
| View services               | `kubectl get svc` |
| Deploy an app               | `kubectl apply -f app.yaml` |
| Check cluster info          | `kubectl cluster-info` |
| Scale deployment            | `kubectl scale deployment DEPLOYMENT_NAME --replicas=3` |
| Delete deployment           | `kubectl delete deployment DEPLOYMENT_NAME` |

---

### 🌐 GKE Concepts
| Concept           | Description |
|------------------|-------------|
| **Cluster**       | A group of compute VMs running Kubernetes |
| **Node Pool**     | A group of nodes with the same config |
| **Pod**           | Smallest unit in Kubernetes (1+ containers) |
| **Deployment**    | Manages replicas and updates |
| **Service**       | Exposes pods internally or externally |
| **Ingress**       | Routes traffic to services |
| **Autoscaler**    | Adds/removes nodes based on load |
| **DaemonSet**     | Ensures all (or some) nodes run a pod |
| **ConfigMap**     | Stores non-sensitive config data |
| **Secret**        | Stores sensitive config like passwords |

---

### 📦 GKE YAML Sample (Deployment)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: gcr.io/my-project/my-image:latest
        ports:
        - containerPort: 80
```

---

## ✨ Unique Features of GKE (Simple Words)

| Feature | What it Means |
|--------|----------------|
| **Autopilot Mode** | GKE fully manages the infrastructure for you — no need to size or manage nodes. |
| **Node Auto-upgrade** | Automatically updates your Kubernetes nodes to stay secure and current. |
| **Node Auto-repair** | If a VM (node) fails, GKE automatically replaces it. |
| **Horizontal Pod Autoscaling (HPA)** | Automatically adds/removes pods based on CPU or custom metrics. |
| **Cluster Autoscaler** | Automatically adds/removes nodes based on workload. |
| **Built-in Logging/Monitoring** | Native integration with Cloud Logging and Cloud Monitoring. |
| **VPC-native Clusters** | Pods get private IPs directly in the VPC, allowing better network control. |
| **Private Clusters** | Nodes have internal IPs only for better security. |
| **GKE with Anthos** | Supports hybrid (on-prem + cloud) and multi-cloud Kubernetes deployments. |
| **GKE Workload Identity** | Securely allows pods to use GCP services without managing service account keys. |

---

Would you like a **PDF or printable version** of this cheat sheet? Or maybe a **mind map** or visual summary?
