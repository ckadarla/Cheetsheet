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

Would you like me to generate a **PDF version** of this cheat sheet?  
I can also bundle this with **Azure**, **GCP**, and **Kubernetes Terraform** cheat sheets into a full guide if you'd like!
