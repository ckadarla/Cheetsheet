![image](https://github.com/user-attachments/assets/7e1d07dd-4265-4d84-9b9a-f293b6ee17aa)

---

## 🌐 **1. Overview**

| Feature       | AKS (Azure) | EKS (AWS)       | GKE (Google Cloud) |
|---------------|-------------|------------------|---------------------|
| Launch Year   | 2018        | 2018             | 2015                |
| Fully Managed | Yes         | Control Plane only (Nodes separate) | Yes |
| CNCF Certified| ✅          | ✅               | ✅                  |
| Multi-Zone    | ✅          | ✅               | ✅                  |
| Hybrid Support| Azure Arc   | Outposts, EKS Anywhere | Anthos              |

---

## ⚙️ **2. Control Plane Management**

| Feature                     | AKS                      | EKS                         | GKE                         |
|----------------------------|---------------------------|------------------------------|------------------------------|
| Managed Control Plane      | ✅ Free                   | ✅ $0.10/hr per cluster      | ✅ Free                      |
| SLA for Control Plane      | 99.95% (with AZs)         | 99.95% (multi-AZ)           | 99.95% (regional)           |
| Version Upgrades           | Manual or Auto            | Manual (via CLI/API)        | Auto or Manual              |
| Master Node Access         | No access (fully managed) | No access (fully managed)   | No access (fully managed)   |

---

## 🧱 **3. Node Management**

| Feature                | AKS                       | EKS                        | GKE                        |
|------------------------|----------------------------|-----------------------------|-----------------------------|
| Auto Node Provisioning | ✅ with VMSS               | ❌ (manual with ASG)        | ✅ Autopilot / Node Pools   |
| Spot/Preemptible Nodes | ✅                         | ✅                         | ✅                         |
| Node OS Options        | Linux, Windows            | Linux, Bottlerocket, Windows | Linux, Windows             |
| GPU Support            | ✅                         | ✅                         | ✅                         |

---

## 📦 **4. Pricing**

| Feature           | AKS                          | EKS                              | GKE                         |
|-------------------|-------------------------------|-----------------------------------|-----------------------------|
| Control Plane     | Free                          | $0.10/hour/cluster               | Free (Standard), Premium costs |
| Node Costs        | Pay for VMs only              | Pay for VMs + control plane      | Pay for VMs (Autopilot = pay per pod) |
| Autopilot Option  | ❌                            | ❌                               | ✅ (per-pod billing)         |

---

## 🔒 **5. Security & Identity**

| Feature                          | AKS                          | EKS                              | GKE                              |
|----------------------------------|-------------------------------|-----------------------------------|----------------------------------|
| IAM Integration                  | Azure AD, RBAC               | IAM + RBAC, IRSA                 | IAM + GKE RBAC                   |
| Network Policies                 | ✅ Calico / Azure-native     | ✅ via CNI plugins               | ✅ Built-in                      |
| Secrets Management               | Azure Key Vault              | AWS Secrets Manager/SSM         | Secret Manager / KMS             |
| Pod-level IAM (IRSA equivalent) | Azure Workload Identity      | ✅ IRSA                          | ✅ Workload Identity             |

---

## 📡 **6. Networking**

| Feature               | AKS                                 | EKS                                 | GKE                                |
|-----------------------|--------------------------------------|--------------------------------------|-------------------------------------|
| CNI Type              | Azure CNI or Kubenet                 | VPC CNI                             | GKE CNI (native)                    |
| Ingress Controller    | App Gateway, NGINX, Traefik          | ALB Ingress Controller, NGINX       | Built-in GKE Ingress + NGINX       |
| Load Balancer Support | Standard & Internal LB               | ELB/NLB/ALB                         | L7/L4 native                        |
| Private Cluster       | ✅ (via subnet control)              | ✅ (API server in VPC)              | ✅                                  |

---

## 📈 **7. Monitoring & Logging**

| Feature              | AKS                            | EKS                                 | GKE                                 |
|----------------------|---------------------------------|--------------------------------------|--------------------------------------|
| Native Monitoring    | Azure Monitor + Container Insights | CloudWatch + Container Insights    | Cloud Monitoring + Logging           |
| Fluentd/Fluentbit    | Supported                        | Supported                           | Pre-installed                        |
| Prometheus/Grafana   | Supported via Add-ons            | Supported via Helm/EKS Blueprints   | GKE integration / Open Source        |

---

## 🔁 **8. CI/CD Integration**

| Feature               | AKS                         | EKS                         | GKE                            |
|-----------------------|------------------------------|------------------------------|--------------------------------|
| Native CI/CD          | GitHub Actions, Azure DevOps | CodePipeline, CodeBuild     | Cloud Build, GitHub Actions    |
| Helm Support          | ✅                          | ✅                          | ✅                             |
| GitOps Support        | Azure Arc + Flux            | ArgoCD, Flux (via Blueprints) | Config Sync (Anthos), ArgoCD   |

---

## 🧠 **9. Smart Features & AI/ML**

| Feature              | AKS                          | EKS                            | GKE                            |
|----------------------|-------------------------------|----------------------------------|--------------------------------|
| Autoscaling          | Cluster + HPA + VMSS         | Cluster + HPA                  | Cluster + HPA + Autopilot      |
| AI/ML Integration    | Azure ML                     | SageMaker, Kubeflow on EKS     | Vertex AI, Kubeflow on GKE     |
| Spot Automation      | VMSS w/ Spot, Karpenter (custom) | EC2 Spot + Karpenter          | Autopilot + Preemptible Nodes |

---

## 🧩 **10. Developer Experience & Ecosystem**

| Feature            | AKS                             | EKS                             | GKE                             |
|--------------------|----------------------------------|----------------------------------|----------------------------------|
| CLI Tool           | `az aks`                         | `eksctl`, `aws eks`              | `gcloud container`              |
| Terraform Support  | ✅                             | ✅                             | ✅                             |
| IDE Integrations   | Visual Studio Code, IntelliJ    | IntelliJ, Cloud9, VS Code       | Cloud Shell, VS Code            |
| Marketplace Addons | Azure Marketplace               | Helm Charts, AWS Marketplace    | GKE Marketplace                 |

---

## ✅ **When to Choose What?**

| Use Case | Best Option |
|----------|-------------|
| Deep Azure integration / hybrid | **AKS** |
| Full AWS stack / custom control | **EKS** |
| Fast setup / best auto features | **GKE** |
| Per-pod billing simplicity      | **GKE Autopilot** |
| Multi-cloud with Anthos/Arc     | GKE / AKS         |
| Strong security integration     | All (tie)         |

---

