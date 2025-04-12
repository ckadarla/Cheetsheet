Here’s a detailed yet compact **Terraform with GCP Cheat Sheet** 📘—perfect for provisioning and managing Google Cloud resources using Terraform. It includes provider setup, authentication, common resources, real examples, and best practices.

---

# ☁️ Terraform with GCP – Cheat Sheet

---

## ✅ 1. Setup & Provider Configuration

### 🔌 Authenticate using Google Cloud SDK
```bash
gcloud auth application-default login
```

### 📄 Provider Block
```hcl
provider "google" {
  project = "your-gcp-project-id"
  region  = "us-central1"
}
```

---

## 📦 2. Common GCP Resources

### 📁 Google Project
```hcl
resource "google_project" "my_project" {
  name       = "My Project"
  project_id = "my-terraform-project"
  org_id     = "123456789012"
}
```

---

### 🌐 VPC Network
```hcl
resource "google_compute_network" "vpc_network" {
  name                    = "my-vpc"
  auto_create_subnetworks = false
}
```

---

### 📡 Subnet
```hcl
resource "google_compute_subnetwork" "subnet" {
  name          = "my-subnet"
  ip_cidr_range = "10.0.1.0/24"
  region        = "us-central1"
  network       = google_compute_network.vpc_network.id
}
```

---

### 💻 Compute Engine VM
```hcl
resource "google_compute_instance" "vm_instance" {
  name         = "demo-instance"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }

  network_interface {
    network    = google_compute_network.vpc_network.id
    subnetwork = google_compute_subnetwork.subnet.id
    access_config {}
  }
}
```

---

### ☁️ GCS Bucket
```hcl
resource "google_storage_bucket" "bucket" {
  name     = "my-tf-bucket-2025"
  location = "US"
  force_destroy = true
}
```

---

### 🔐 Service Account
```hcl
resource "google_service_account" "sa" {
  account_id   = "terraform-sa"
  display_name = "Terraform Service Account"
}
```

---

### 🎯 IAM Binding
```hcl
resource "google_project_iam_binding" "viewer" {
  project = "my-gcp-project-id"
  role    = "roles/viewer"
  members = [
    "serviceAccount:${google_service_account.sa.email}"
  ]
}
```

---

## 💾 3. Remote State (GCS Bucket)

### Backend Configuration
```hcl
terraform {
  backend "gcs" {
    bucket  = "my-terraform-state"
    prefix  = "dev/state"
  }
}
```

Initialize:
```bash
terraform init
```

---

## 🔁 4. Terraform CLI Commands

| Command                 | Description                          |
|------------------------|--------------------------------------|
| `terraform init`       | Initialize Terraform config          |
| `terraform plan`       | Show execution plan                  |
| `terraform apply`      | Apply infrastructure changes         |
| `terraform destroy`    | Destroy infrastructure               |
| `terraform validate`   | Validate code syntax                 |
| `terraform fmt`        | Format configuration files           |
| `terraform output`     | Display output values                |

---

## 🧩 5. Modules Example

```hcl
module "gke" {
  source  = "terraform-google-modules/kubernetes-engine/google"
  project_id = "your-gcp-project-id"
  name       = "gke-cluster"
  region     = "us-central1"
  network    = google_compute_network.vpc_network.name
  subnetwork = google_compute_subnetwork.subnet.name
}
```

---

## 📤 6. Outputs

```hcl
output "instance_ip" {
  value = google_compute_instance.vm_instance.network_interface[0].access_config[0].nat_ip
}
```

---

## 🧠 7. Tips & Best Practices

- 🗃️ Use **remote state backend** with state locking for teams.
- 🔐 Avoid hardcoding credentials—use service accounts securely.
- 🧪 Use `terraform plan` with `-out` before applying in prod.
- 🧼 Organize code with modules and workspaces (`dev`, `prod`, etc).
- 📍 Store secrets in **Secret Manager** or **Vault**, not in code.

---

Would you like a **PDF** of this cheat sheet? I can also combine this with AWS, Azure, Kubernetes, and GCP-specific cheat sheets into one comprehensive 
**Terraform Master Guide**. Just let me know!
