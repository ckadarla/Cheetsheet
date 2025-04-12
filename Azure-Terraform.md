Absolutely! Here's a concise and powerful **Terraform with Azure Cheat Sheet** 📘 – tailored for provisioning and managing Azure resources using Terraform, including CLI usage, provider blocks, key resources, and real-world examples.

---

# 🛠️ Terraform with Azure – Cheat Sheet

---

## ⚙️ 1. Initialize Terraform for Azure

### 📄 Provider Configuration
```hcl
provider "azurerm" {
  features {}
}
```

### 🪄 Initialize Terraform
```bash
terraform init
```

---

## 📤 2. Authenticate with Azure

**Option 1:** Use Azure CLI (most common for local dev)

```bash
az login
```

**Option 2:** Service Principal

```bash
az ad sp create-for-rbac --name "terraform-sp" --role="Contributor" \
  --scopes="/subscriptions/<SUBSCRIPTION_ID>"
```

Then configure in Terraform:
```hcl
provider "azurerm" {
  features {}
  subscription_id = "<SUBSCRIPTION_ID>"
  client_id       = "<APP_ID>"
  client_secret   = "<PASSWORD>"
  tenant_id       = "<TENANT_ID>"
}
```

---

## 📦 3. Basic Resource Group Example

```hcl
resource "azurerm_resource_group" "example" {
  name     = "rg-terraform-demo"
  location = "East US"
}
```

---

## ☁️ 4. Common Azure Resources

### ✅ Storage Account

```hcl
resource "azurerm_storage_account" "example" {
  name                     = "storageacctdemo"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

---

### ⚡ App Service Plan + Web App

```hcl
resource "azurerm_app_service_plan" "example" {
  name                = "example-asp"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  sku {
    tier = "Standard"
    size = "S1"
  }
}

resource "azurerm_app_service" "example" {
  name                = "example-app-service"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  app_service_plan_id = azurerm_app_service_plan.example.id
}
```

---

### 🚀 Azure Kubernetes Service (AKS)

```hcl
resource "azurerm_kubernetes_cluster" "aks" {
  name                = "aks-cluster"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  dns_prefix          = "aksdemo"

  default_node_pool {
    name       = "default"
    node_count = 2
    vm_size    = "Standard_DS2_v2"
  }

  identity {
    type = "SystemAssigned"
  }
}
```

---

## 🔐 Azure Key Vault with Secrets

```hcl
resource "azurerm_key_vault" "example" {
  name                        = "tf-keyvault-demo"
  location                    = azurerm_resource_group.example.location
  resource_group_name         = azurerm_resource_group.example.name
  tenant_id                   = "<TENANT_ID>"
  sku_name                    = "standard"
}

resource "azurerm_key_vault_secret" "secret" {
  name         = "MySecret"
  value        = "super-secret"
  key_vault_id = azurerm_key_vault.example.id
}
```

---

## 🗂️ 5. Terraform Commands Summary

| Command                         | Purpose                                      |
|----------------------------------|----------------------------------------------|
| `terraform init`               | Initialize Terraform config                  |
| `terraform plan`               | Show execution plan                          |
| `terraform apply`              | Apply infrastructure changes                 |
| `terraform destroy`            | Tear down infrastructure                     |
| `terraform fmt`                | Format Terraform code                        |
| `terraform validate`           | Validate configuration files                 |
| `terraform output`             | Show output values                           |
| `terraform state list`         | List all resources in current state          |
| `terraform taint`              | Force recreation of a resource               |

---

## 💾 6. Remote State in Azure Storage

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "terraform-rg"
    storage_account_name = "tfstatestorageacct"
    container_name       = "tfstate"
    key                  = "terraform.tfstate"
  }
}
```

Initialize with:

```bash
terraform init
```

---

## 🧪 7. Useful Patterns

### 🚩 Conditional Resource Creation

```hcl
resource "azurerm_public_ip" "maybe" {
  count               = var.create_ip ? 1 : 0
  name                = "public-ip"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  allocation_method   = "Static"
}
```

---

### 🧩 Modules Usage

```hcl
module "network" {
  source              = "./modules/network"
  resource_group_name = azurerm_resource_group.example.name
}
```

---

## 🧠 Tips & Best Practices

- ✅ Use **remote state** and **state locking**.
- 🔐 Use **Key Vault** to manage secrets securely.
- 🌍 Define variables & outputs for modularity.
- 🧪 Use `terraform plan` before every `apply`.
- 🧼 Use `terraform destroy` carefully in prod!

---

Would you like this as:

- 📄 A **PDF**
- 🗃️ A **multi-page guide**
- 📘 A combined **Azure DevOps + Terraform cheatsheet**?

Let me know and I’ll generate it for you!
