Here’s your **EKS (Amazon Elastic Kubernetes Service) Cheat Sheet** 🎯 – focused on **EKS-specific operations**, **AWS integrations**, and **real-world examples** to help you deploy and manage EKS like a pro.

---

# 🐳 EKS Cheat Sheet (Amazon Elastic Kubernetes Service)

---

## 🛠️ Create EKS Cluster (with eksctl)

```bash
eksctl create cluster \
  --name my-eks-cluster \
  --version 1.28 \
  --region us-east-1 \
  --nodegroup-name linux-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3 \
  --managed
```

> ✅ Uses AWS-managed nodes and provisions networking automatically.

---

## 🔑 kubeconfig Setup

```bash
aws eks update-kubeconfig --region us-east-1 --name my-eks-cluster
```

> Adds/updates context in your `~/.kube/config`.

---

## 🧠 EKS IAM Authentication

Use `aws-auth` config map to map IAM roles/users:

```bash
kubectl edit configmap/aws-auth -n kube-system
```

```yaml
mapRoles:
  - rolearn: arn:aws:iam::<account-id>:role/EKSNodeRole
    username: system:node:{{EC2PrivateDNSName}}
    groups:
      - system:bootstrappers
      - system:nodes
```

> Maps IAM roles to Kubernetes RBAC users/groups.

---

## 🌐 VPC & Networking

EKS requires a dedicated VPC or can use an existing one.

```bash
eksctl utils associate-iam-oidc-provider \
  --region us-east-1 \
  --cluster my-eks-cluster \
  --approve
```

> Allows your cluster to use IAM roles for service accounts.

---

## 🔐 IAM Roles for Service Accounts (IRSA)

```bash
eksctl create iamserviceaccount \
  --cluster my-eks-cluster \
  --namespace default \
  --name s3-reader \
  --attach-policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess \
  --approve
```

> Enables fine-grained IAM permissions for pods.

---

## 🚪 Load Balancer Controller (ALB)

### Install ALB Ingress Controller (v2+):

```bash
helm repo add eks https://aws.github.io/eks-charts
helm repo update

kubectl apply -k "github.com/aws/eks-charts/stable/aws-load-balancer-controller/crds?ref=master"

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=my-eks-cluster \
  --set serviceAccount.create=false \
  --set region=us-east-1 \
  --set vpcId=vpc-xxxxxx \
  --set serviceAccount.name=aws-load-balancer-controller
```

---

## 📦 EBS CSI Driver

Enable persistent volumes using EBS:

```bash
eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster my-eks-cluster \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve

eksctl create addon \
  --name aws-ebs-csi-driver \
  --cluster my-eks-cluster \
  --service-account-role-arn arn:aws:iam::<account-id>:role/<role-name> \
  --force
```

---

## 🔁 Node Group Scaling

```bash
eksctl scale nodegroup \
  --cluster my-eks-cluster \
  --name linux-nodes \
  --nodes 4
```

---

## 📜 Sample Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-eks
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-eks
  template:
    metadata:
      labels:
        app: hello-eks
    spec:
      containers:
        - name: hello
          image: public.ecr.aws/nginx/nginx:latest
          ports:
            - containerPort: 80
```

---

## 🧭 EKS Monitoring & Logging

### CloudWatch Logs:

Install Fluent Bit for log forwarding:

```bash
helm install fluent-bit eks/aws-for-fluent-bit -n amazon-cloudwatch \
  --set cloudWatch.enabled=true \
  --set cloudWatch.region=us-east-1 \
  --set serviceAccount.create=false \
  --set serviceAccount.name=fluent-bit
```

> Logs go to CloudWatch Log Groups by namespace/app.

---

## 🔧 Utilities & Tools

| Tool           | Description                           |
|----------------|---------------------------------------|
| `eksctl`       | CLI tool for creating and managing EKS |
| `kubectl`      | Interact with Kubernetes API          |
| `Helm`         | Manage applications via Helm charts   |
| `aws cli`      | Manage AWS IAM, networking, policies  |
| `IAM Roles for SA` | Secure AWS service access by pod  |

---

## 🧹 Delete EKS Cluster

```bash
eksctl delete cluster --name my-eks-cluster --region us-east-1
```

---

## 🚀 **EKS - Elastic Kubernetes Service Cheat Sheet**

---

### 🔧 **Cluster Management (EKS + eksctl)**

> First, install:
> - [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)  
> - [eksctl](https://eksctl.io/introduction/#installation)

| Command | Description |
|--------|-------------|
| `eksctl create cluster --name <cluster-name> --region <region> --nodegroup-name <ng-name> --node-type t3.medium --nodes 3` | Create EKS cluster with a managed node group |
| `eksctl get cluster --region <region>` | List clusters |
| `eksctl delete cluster --name <cluster-name> --region <region>` | Delete a cluster |
| `aws eks update-kubeconfig --name <cluster-name> --region <region>` | Update `kubeconfig` for the cluster |
| `eksctl scale nodegroup --cluster <name> --name <ng-name> --nodes 5 --region <region>` | Scale a node group |

---

### 🧰 **kubectl Basics**

| Command | Description |
|--------|-------------|
| `kubectl get nodes` | List all nodes |
| `kubectl get pods -A` | View all pods in all namespaces |
| `kubectl logs <pod-name>` | View pod logs |
| `kubectl exec -it <pod> -- bash` | Exec into a running pod |
| `kubectl describe pod <pod>` | Detailed info about pod |

---

### 📦 **Deployments & Services**

| Command | Description |
|--------|-------------|
| `kubectl apply -f <file>.yaml` | Deploy resources |
| `kubectl delete -f <file>.yaml` | Delete resources |
| `kubectl get deployments` | List deployments |
| `kubectl get svc` | List services |
| `kubectl expose deployment <name> --type=LoadBalancer --port=80` | Create service for a deployment |
| `kubectl scale deployment <name> --replicas=3` | Scale replicas |
| `kubectl rollout restart deployment <name>` | Restart deployment |

---

### ☁️ **IAM, Networking, and Security**

| Command | Description |
|--------|-------------|
| `eksctl utils associate-iam-oidc-provider --cluster <name> --approve` | Associate IAM OIDC provider (required for IRSA) |
| `eksctl create iamserviceaccount ...` | Create IAM roles for service accounts |
| `kubectl describe configmap aws-auth -n kube-system` | View IAM to RBAC mapping |
| `eksctl create fargateprofile` | Create EKS Fargate profile (for serverless pods) |
| `kubectl auth can-i <verb> <resource>` | Check RBAC permissions |

---

### 📊 **Monitoring & Logs**

| Tool | Use |
|------|-----|
| **CloudWatch Logs** | Capture logs via Fluent Bit or aws-for-fluent-bit |
| **Container Insights** | Full monitoring of nodes/pods |
| `kubectl top nodes` / `top pods` | View metrics (requires Metrics Server) |
| **AWS X-Ray** | Distributed tracing integration with EKS |

---

### 📈 **Upgrades & Maintenance**

| Command | Description |
|--------|-------------|
| `eksctl get cluster --name <name>` | View current cluster version |
| `eksctl upgrade cluster --name <name>` | Upgrade control plane |
| `eksctl upgrade nodegroup --cluster <name> --name <ng-name>` | Upgrade a node group |
| `eksctl drain nodegroup` | Drain nodes for maintenance |

---

### 🛠️ **Helpful Tools**

| Tool | Purpose |
|------|---------|
| `eksctl` | Simplified EKS CLI |
| `kubectl` | Kubernetes CLI |
| `Helm` | Kubernetes package manager |
| `Karpenter` | Cluster autoscaler alternative |
| `aws-iam-authenticator` | Optional – for advanced auth scenarios |

---

### 📄 **Manifest Sample (Deployment + Service)**

```yaml
# deployment.yaml
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
        image: nginx
        ports:
        - containerPort: 80
```

```yaml
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```
