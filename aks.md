Here’s your **AKS (Azure Kubernetes Service) Cheat Sheet** 🧠 – focused on **unique AKS capabilities**, tools, integrations, and Azure-specific examples.

---

# 🚀 AKS Cheat Sheet (Azure Kubernetes Service)

---

## 🔧 Create AKS Cluster

```bash
az group create -n aks-rg -l eastus

az aks create \
  --resource-group aks-rg \
  --name myAKSCluster \
  --node-count 3 \
  --enable-addons monitoring \
  --generate-ssh-keys
```

> ✅ `--enable-addons monitoring`: Enables **Azure Monitor for containers** (Log Analytics).

---

## 🔑 Get AKS Credentials (Kubeconfig)

```bash
az aks get-credentials --resource-group aks-rg --name myAKSCluster
```

> This updates your `~/.kube/config` with AKS credentials.

---

## 👥 Azure AD Integration

Enable Azure AD for **RBAC**:

```bash
az aks create \
  --resource-group aks-rg \
  --name myAKSCluster \
  --enable-aad \
  --aad-admin-group-object-ids <group-id> \
  --enable-azure-rbac \
  --enable-managed-identity
```

> Integrates AKS with **Microsoft Entra ID (Azure AD)**.

---

## 🔐 Managed Identities (MSI)

Assign permissions to the AKS-managed identity:

```bash
az role assignment create \
  --assignee <client-id> \
  --role "Reader" \
  --scope /subscriptions/<sub-id>/resourceGroups/aks-rg
```

> Used for accessing Azure services securely (like Key Vault, Storage).

---

## 📦 Ingress with Application Gateway

AKS supports **AGIC (App Gateway Ingress Controller)**:

```bash
az aks enable-addons \
  --addons ingress-appgw \
  --name myAKSCluster \
  --resource-group aks-rg \
  --app-gateway-name myAppGateway \
  --appgw-subnet-cidr "10.2.0.0/24"
```

> Native Azure L7 Load Balancer integration.

---

## 🧠 Azure Monitor Integration

View logs & metrics for AKS cluster:

```bash
az monitor log-analytics workspace show \
  --resource-group aks-rg \
  --workspace-name <workspace-name>
```

Query pod logs in Log Analytics:

```kusto
KubePodInventory
| where ClusterName == "myAKSCluster"
```

---

## 🔐 Azure Key Vault CSI Driver

Mount secrets directly into pods:

```yaml
volumes:
  - name: secrets-store-inline
    csi:
      driver: secrets-store.csi.k8s.io
      readOnly: true
      volumeAttributes:
        secretProviderClass: "azure-kv-example"
```

> Enable CSI driver using Azure CLI or ARM.

---

## 🧱 Azure CNI & Network Policies

Use **Azure CNI** for IP-per-pod:

```bash
az aks create \
  --network-plugin azure \
  --vnet-subnet-id <subnet-id> \
  --service-cidr 10.0.0.0/16 \
  --dns-service-ip 10.0.0.10 \
  --docker-bridge-address 172.17.0.1/16
```

Enable **Calico network policies**:

```bash
az aks create --enable-network-policy
```

---

## 🏗️ Node Pools (Linux & Windows)

```bash
az aks nodepool add \
  --resource-group aks-rg \
  --cluster-name myAKSCluster \
  --name winpool \
  --os-type Windows \
  --node-count 2 \
  --node-vm-size Standard_D2s_v3
```

> Use different pools for Windows, GPUs, etc.

---

## 📜 ARM/Bicep Template Example (AKS Cluster)

```bicep
resource aks 'Microsoft.ContainerService/managedClusters@2023-01-01' = {
  name: 'myAKS'
  location: 'eastus'
  properties: {
    dnsPrefix: 'myaksdns'
    agentPoolProfiles: [
      {
        name: 'nodepool1'
        count: 2
        vmSize: 'Standard_DS2_v2'
        osType: 'Linux'
        mode: 'System'
      }
    ]
    identity: {
      type: 'SystemAssigned'
    }
    kubernetesVersion: '1.28.3'
    enableRBAC: true
  }
}
```

---

## 🧪 Dev Tools

| Tool             | Purpose                             |
|------------------|-------------------------------------|
| `az aks browse`  | Open K8s dashboard (deprecated)     |
| `kubelogin`      | Azure AD-enabled kubectl access     |
| `az aks nodepool`| Manage node pools                   |
| `helm`           | Helm charts with Azure support      |
| `draft`          | Azure tool for K8s app scaffolding  |

---

## 🔥 Useful AKS-Specific Add-ons

| Add-on                 | Feature                                 |
|------------------------|-----------------------------------------|
| `monitoring`           | Azure Monitor integration               |
| `ingress-appgw`        | Application Gateway + AGIC              |
| `azure-keyvault-secrets-provider` | Mount secrets via CSI         |
| `virtual-node`         | Virtual Kubelet with Azure Container Instances |
| `open-service-mesh`    | Native service mesh with Azure support  |

---

## 🧹 Clean-Up

```bash
az aks delete --name myAKSCluster --resource-group aks-rg --yes --no-wait
az group delete --name aks-rg --yes --no-wait
```

---

Would you like this in **PDF**, **Markdown**, or as part of a **multi-cloud K8s cheat sheet** (AKS, EKS, GKE)?
