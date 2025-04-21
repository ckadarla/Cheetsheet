Here’s your **Terraform with AWS Cheat Sheet** 🛠️ — tailored for DevOps, SRE, and Cloud Engineers who want to automate AWS infrastructure with Terraform. Includes essential commands, real-world examples, and AWS-specific best practices.

---

# 🚀 Terraform with AWS – Cheat Sheet

---

## ✅ 1. Setup & Initialization

### 📄 Provider Block

```hcl
provider "aws" {
  region = "us-east-1"
}
```

### 🔁 Initialize Terraform

```bash
terraform init
```

---

## 🌐 2. AWS CLI Auth (Local Dev)

Make sure you're authenticated:

```bash
aws configure
```

You can also use AWS named profiles:

```hcl
provider "aws" {
  region  = "us-east-1"
  profile = "my-aws-profile"
}
```

---

## 🧱 3. Common AWS Resources

### 🏗️ VPC

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}
```

### 📡 Subnet

```hcl
resource "aws_subnet" "subnet1" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-1a"
}
```

### 🛣️ Internet Gateway

```hcl
resource "aws_internet_gateway" "gw" {
  vpc_id = aws_vpc.main.id
}
```

---

### 💻 EC2 Instance

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.subnet1.id
  tags = {
    Name = "WebServer"
  }
}
```

---

### 📦 S3 Bucket

```hcl
resource "aws_s3_bucket" "bucket" {
  bucket = "my-terraform-bucket-12345"
  acl    = "private"
}
```

---

### 🔐 IAM Role

```hcl
resource "aws_iam_role" "lambda_exec" {
  name = "lambda-exec-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action    = "sts:AssumeRole"
      Effect    = "Allow"
      Principal = { Service = "lambda.amazonaws.com" }
    }]
  })
}
```

---

## 🧪 4. Terraform CLI Commands

| Command                 | Description                          |
|------------------------|--------------------------------------|
| `terraform init`       | Initialize Terraform directory       |
| `terraform plan`       | Preview changes before apply         |
| `terraform apply`      | Apply infrastructure changes         |
| `terraform destroy`    | Tear down infrastructure             |
| `terraform validate`   | Validate configuration files         |
| `terraform fmt`        | Format code                          |
| `terraform show`       | Show current state                   |
| `terraform output`     | Display output values                |
| `terraform taint`      | Force resource recreation            |

---

## 💾 5. Remote State with S3

```hcl
terraform {
  backend "s3" {
    bucket         = "tf-state-bucket"
    key            = "env/dev/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

### 💡 Tip: Use DynamoDB for state locking

---

## 🔁 6. Terraform Modules

```hcl
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  name   = "my-vpc"
  cidr   = "10.0.0.0/16"
  azs    = ["us-east-1a", "us-east-1b"]
  public_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
}
```

---

## 🧠 7. Best Practices

- ✅ Use `terraform.tfvars` to manage environment configs.
- 🔐 Store secrets in **AWS Secrets Manager** or **SSM Parameter Store**.
- 🔄 Use **Workspaces** for `dev`, `test`, `prod` environments.
- 💡 Combine with **Terragrunt** for DRY code structure.

---

## 🧭 Sample Outputs

```hcl
output "instance_public_ip" {
  value = aws_instance.web.public_ip
}
```

---

## 🔄 Terraform Workspace Commands

```bash
terraform workspace new dev
terraform workspace select prod
```

---
Here’s a **Terraform EKS Module Cheat Sheet** — a quick-start and reference guide for creating and managing EKS clusters using [Terraform AWS EKS module](https://github.com/terraform-aws-modules/terraform-aws-eks).

---

## 📦 **Terraform EKS Module Cheat Sheet**

> ✅ Uses: [terraform-aws-eks](https://github.com/terraform-aws-modules/terraform-aws-eks)  
> ✅ Provider: AWS  
> ✅ Language: HCL (Terraform)

---

### 🧰 **1. Required Providers**

```hcl
provider "aws" {
  region = "us-west-2"
}

provider "kubernetes" {
  host                   = module.eks.cluster_endpoint
  cluster_ca_certificate = base64decode(module.eks.cluster_certificate_authority_data)
  token                  = data.aws_eks_cluster_auth.cluster.token
}
```

---

### 🚀 **2. Minimal EKS Cluster Setup**

```hcl
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "~> 20.0" # use latest stable

  cluster_name    = "my-eks-cluster"
  cluster_version = "1.28"
  subnet_ids      = module.vpc.private_subnets
  vpc_id          = module.vpc.vpc_id

  eks_managed_node_groups = {
    default = {
      instance_types = ["t3.medium"]
      desired_size   = 2
      max_size       = 3
      min_size       = 1
    }
  }

  enable_irsa = true
}
```

---

### 📡 **3. EKS with VPC Module**

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.1.0"

  name = "eks-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-west-2a", "us-west-2b", "us-west-2c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = true
}
```

---

### 🔒 **4. IAM Roles for Service Accounts (IRSA)**

```hcl
module "irsa" {
  source = "terraform-aws-modules/iam/aws//modules/iam-role-for-service-accounts-eks"

  role_name_prefix = "external-dns"
  attach_external_dns_policy = true
  oidc_providers = {
    main = {
      provider_arn               = module.eks.oidc_provider_arn
      namespace_service_accounts = ["kube-system:external-dns"]
    }
  }
}
```

---

### ⚙️ **5. Kubernetes Auth ConfigMap**

```hcl
module "eks" {
  # ...
  manage_aws_auth_configmap = true
  aws_auth_roles = [
    {
      rolearn  = "arn:aws:iam::111122223333:role/TeamRole"
      username = "team"
      groups   = ["system:masters"]
    }
  ]
}
```

---

### 📈 **6. Add-ons & Tools**

#### Enable Add-ons
```hcl
cluster_addons = {
  coredns = {
    resolve_conflicts = "OVERWRITE"
  }
  kube-proxy = {}
  vpc-cni    = {}
}
```

#### Deploy Metrics Server, External DNS, ALB Controller, etc. using Helm:
```hcl
module "helm_addons" {
  source = "git::https://github.com/terraform-aws-modules/terraform-aws-eks.git//modules/kubernetes-addons"

  eks_cluster_id = module.eks.cluster_id

  addons = {
    metrics_server = {
      version = "3.8.2"
      values  = []
    }
  }
}
```

---

### 🚦 **7. Output Examples**

```hcl
output "cluster_name" {
  value = module.eks.cluster_name
}

output "cluster_endpoint" {
  value = module.eks.cluster_endpoint
}
```
---

### 🧪 **8. Useful CLI Commands**

| Task | Command |
|------|---------|
| Get kubeconfig | `aws eks update-kubeconfig --name <cluster-name>` |
| Check nodes    | `kubectl get nodes` |
| IAM mappings   | `kubectl describe configmap aws-auth -n kube-system` |

---

### ✅ **9. Best Practices**

- Always pin module versions
- Use `terraform fmt` and `validate`
- Enable `enable_irsa = true`
- Use separate workspaces/states per environment
- Prefer **managed node groups** unless custom AMIs are needed
- Tag resources using `tags = {}`

---
