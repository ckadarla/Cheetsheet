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
### 📄 GCP backend with remote statefile
```
terraform {
  backend "gcs" {
    bucket  = "my-terraform-state-bucket-123"
    prefix  = "terraform/state"
  }
}
```
```
resource "google_storage_bucket" "terraform_state" {
  name     = "my-terraform-state-bucket-123"
  location = "US"
  force_destroy = true
  versioning {
    enabled = true
  }
  uniform_bucket_level_access = true
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


Here’s a set of **sample Terraform code snippets** for various **GCP components**: IAM, Compute Instance, VPC, VPC Service Controls (VPCSC), Cloud Storage, Filestore (GCP Storage), GKE, GCR, and Cloud Run.

Each example uses the Terraform `google` provider (>= v4.0 recommended).

---

## ✅ 1. Provider Setup (common for all)
```hcl
provider "google" {
  project = "your-gcp-project-id"
  region  = "us-central1"
  zone    = "us-central1-a"
}
```

---

## ✅ 2. IAM Binding
```hcl
resource "google_project_iam_member" "viewer" {
  project = "your-gcp-project-id"
  role    = "roles/viewer"
  member  = "user:example@example.com"
}
```

---

## ✅ 3. Compute Instance
```hcl
resource "google_compute_instance" "vm_instance" {
  name         = "example-instance"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }

  network_interface {
    network = "default"
    access_config {}
  }
}
```

---

## ✅ 4. VPC Network
```hcl
resource "google_compute_network" "vpc_network" {
  name                    = "custom-vpc"
  auto_create_subnetworks = false
}

resource "google_compute_subnetwork" "subnet" {
  name          = "custom-subnet"
  ip_cidr_range = "10.0.0.0/16"
  region        = "us-central1"
  network       = google_compute_network.vpc_network.id
}
```

---

## ✅ 5. VPCSC (Service Perimeter)
```hcl
resource "google_access_context_manager_access_policy" "policy" {
  parent = "organizations/123456789012"
  title  = "example-policy"
}

resource "google_access_context_manager_service_perimeter" "perimeter" {
  name         = "accessPolicies/${google_access_context_manager_access_policy.policy.name}/servicePerimeters/perimeter-1"
  parent       = google_access_context_manager_access_policy.policy.name
  title        = "Perimeter 1"
  perimeter_type = "PERIMETER_TYPE_REGULAR"

  status {
    resources = ["projects/your-gcp-project-id"]
    restricted_services = ["storage.googleapis.com"]
  }
}
```

---

## ✅ 6. Cloud Storage (GCS Bucket)
```hcl
resource "google_storage_bucket" "bucket" {
  name     = "my-terraform-bucket-123"
  location = "US"
  uniform_bucket_level_access = true
}
```

---

## ✅ 7. Filestore (Cloud Storage option)
```hcl
resource "google_filestore_instance" "filestore_instance" {
  name       = "example-filestore"
  zone       = "us-central1-c"
  tier       = "STANDARD"
  network {
    network = "default"
    modes   = ["MODE_IPV4"]
    reserved_ip_range = "10.0.0.0/29"
  }

  file_shares {
    name       = "share1"
    capacity_gb = 1024
  }
}
```

---

## ✅ 8. GKE (Google Kubernetes Engine)
```hcl
resource "google_container_cluster" "primary" {
  name     = "gke-cluster"
  location = "us-central1"
  remove_default_node_pool = true
  initial_node_count       = 1

  network    = "default"
  subnetwork = "default"
}

resource "google_container_node_pool" "primary_nodes" {
  name       = "primary-node-pool"
  cluster    = google_container_cluster.primary.name
  location   = google_container_cluster.primary.location

  node_config {
    machine_type = "e2-medium"
  }

  initial_node_count = 1
}
```

---

## ✅ 9. GCR (Google Container Registry)
GCR is **automatically enabled** when pushing images. You only need to **enable the API** if you haven't already:

```hcl
resource "google_project_service" "container_registry" {
  project = "your-gcp-project-id"
  service = "containerregistry.googleapis.com"
}
```

Then tag/push Docker images like:
```sh
docker tag my-image gcr.io/your-gcp-project-id/my-image
docker push gcr.io/your-gcp-project-id/my-image
```

---

## ✅ 10. Cloud Run
```hcl
resource "google_cloud_run_service" "default" {
  name     = "cloud-run-service"
  location = "us-central1"

  template {
    spec {
      containers {
        image = "gcr.io/cloudrun/hello"
      }
    }
  }

  traffic {
    percent         = 100
    latest_revision = true
  }
}

resource "google_cloud_run_service_iam_member" "public_invoker" {
  location    = google_cloud_run_service.default.location
  service     = google_cloud_run_service.default.name
  role        = "roles/run.invoker"
  member      = "allUsers"
}
```

---

If you want these as a **ready-to-run modular repo**, I can help you organize them into a structured folder with a shared provider block, variables, outputs, etc. Want that?
