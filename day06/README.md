# 🚀 TerraWeek Day 06 – Advanced Terraform & Capstone Project

## 👩‍💻 Author

**Sakshi Saxena**

Senior Software Engineer | DevOps & Cloud Enthusiast

AWS | Terraform | Docker | Kubernetes | CI/CD | Linux | GitHub Actions

---

# 📌 Project Repository

🔗 **GitHub Repository:** https://github.com/Sakshi-Saxena/terraform-2tier-app/tree/main

---

# 🎯 Capstone Project

## AWS 2-Tier Infrastructure using Terraform

This project provisions a production-style **2-tier application infrastructure on AWS** using Terraform and follows Infrastructure as Code (IaC) best practices.

The infrastructure includes:

* Custom VPC
* Public and Private Subnets
* Internet Gateway
* NAT Gateway
* Auto Scaling Group
* EC2 Instances
* Security Groups
* Amazon S3 Bucket
* Remote State Management
* Security Scanning
* Terraform Testing
* Cost Estimation
* CI/CD using GitHub Actions

---

# 🏗️ Architecture Diagram

```text
                           Internet
                               │
                      Internet Gateway
                               │
         ┌─────────────────────────────────┐
         │                                 │
 ┌──────────────┐                  ┌──────────────┐
 │ Public Subnet│                  │ Public Subnet│
 │     AZ-1     │                  │     AZ-2     │
 └──────────────┘                  └──────────────┘
         │                                 │
         └────────── Auto Scaling Group ───┘
                               │
         ┌─────────────────────────────────┐
         │                                 │
 ┌──────────────┐                  ┌──────────────┐
 │Private Subnet│                  │Private Subnet│
 │     AZ-1     │                  │     AZ-2     │
 └──────────────┘                  └──────────────┘
                               │
                         Amazon S3 Bucket
```

---

# 📚 Learning Goals Achieved

## ✅ Workspaces & Environments

Implemented Terraform workspaces to manage multiple environments.

Commands used:

```bash
terraform workspace list
terraform workspace new staging
terraform workspace select staging
terraform workspace show
```

### Workspace Usage

```hcl
locals {
  instance_type = terraform.workspace == "prod" ? "t3.medium" : "t3.micro"
}
```

### Workspaces vs Separate Backends

#### Workspaces

✅ Simple to manage

✅ Same codebase

❌ Shared backend

❌ Less isolation

#### Separate State Backends

✅ Better isolation

✅ Recommended for production

❌ More configuration overhead

---

# ✅ Quality Gates

### Terraform Formatting

```bash
terraform fmt -recursive
```

### Terraform Validation

```bash
terraform validate
```

### Terraform Testing

```bash
terraform test
```

Implemented native Terraform tests using:

```text
tests/vpc.tftest.hcl
```

### Plan-based Test

* Generates execution plan only.
* Does not create resources.
* Faster and safe for CI.

### Apply-based Test

* Creates real infrastructure.
* Verifies actual deployments.
* Slower and incurs cost.

---

# 🔒 Security Scanning

Used:

```bash
trivy config .
```

Security issues fixed:

* Restricted SSH ingress
* Enabled S3 encryption
* Enabled S3 versioning
* Enabled S3 public access block
* Enforced IMDSv2
* Encrypted EBS volumes

---

# 💰 Cost Estimation

Used:

```bash
terraform plan -out=tfplan

infracost breakdown --path=tfplan
```

Estimated monthly cost:

| Resource      | Approx Cost |
| ------------- | ----------- |
| NAT Gateway   | ~$32        |
| EC2 t2.micro  | ~$8         |
| S3            | <$1         |
| Data Transfer | ~$2         |
| Total         | ~$42/month  |

---

# ⚙️ CI/CD using GitHub Actions

Implemented three separate pipelines.

---

## 1️⃣ Terraform CI Pipeline

Runs automatically on push to `main`.

Steps:

* Terraform Format Check
* Terraform Init
* Terraform Validate
* Terraform Test
* Trivy Security Scan
* Terraform Plan
* Infracost Cost Estimation

---

## 2️⃣ Terraform Apply Pipeline

Manual workflow dispatch.

Steps:

* Terraform Init
* Terraform Apply

---

## 3️⃣ Terraform Destroy Pipeline

Manual workflow dispatch.

Steps:

* Terraform Init
* Terraform Destroy

---

# 📂 Project Structure

```text
terraform-2tier-app/
│
├── backend.tf
├── providers.tf
├── versions.tf
├── variables.tf
├── terraform.tfvars
├── outputs.tf
├── main.tf
├── locals.tf
├── data.tf
│
├── modules/
│   └── security-group/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
│
├── tests/
│   └── vpc.tftest.hcl
│
├── user-data/
│   └── install.sh
│
└── .github/
    └── workflows/
        ├── terraform.yml
        ├── terraform-apply.yml
        └── terraform-destroy.yml
```

---

# 🏆 Capstone Requirements Checklist

## ✅ Custom Module

Implemented:

```text
modules/security-group
```

---

## ✅ Registry Module

Implemented:

```text
terraform-aws-modules/vpc/aws
```

---

## ✅ Remote State with Native S3 Locking

Implemented:

* Amazon S3 Backend
* Native `.tflock` locking
* S3 Versioning Enabled

---

## ✅ Variables and Outputs

No hard-coded values.

Used:

* Variables
* Locals
* Outputs
* Terraform Workspaces

---

## ✅ Quality Gates

Implemented:

* terraform fmt
* terraform validate
* terraform test
* trivy config

---

## ✅ GitHub Actions Workflow

Implemented:

* CI Pipeline
* Apply Pipeline
* Destroy Pipeline

---

## ✅ Documentation

Included:

* Architecture Diagram
* Project Structure
* Deployment Steps
* Cleanup Instructions

---

# 🚫 Provisioners

This project intentionally avoids:

* local-exec
* remote-exec

Instead uses:

✅ User Data

✅ Terraform Modules

Following HashiCorp's recommended practices.

---

# 🚀 Deployment Instructions

### Initialize

```bash
terraform init
```

### Format

```bash
terraform fmt -recursive
```

### Validate

```bash
terraform validate
```

### Test

```bash
terraform test
```

### Security Scan

```bash
trivy config .
```

### Plan

```bash
terraform plan
```

### Apply

```bash
terraform apply
```

---

# 🧹 Cleanup

Destroy infrastructure:

```bash
terraform destroy -auto-approve
```

Verified:

✅ VPC deleted

✅ EC2 deleted

✅ ASG deleted

✅ Security Groups deleted

✅ NAT Gateway deleted

✅ No orphaned resources

---

# 🌟 Key Takeaways

* Infrastructure as Code using Terraform
* Modular Terraform Design
* Remote State Management
* Security Scanning
* Cost Estimation
* Terraform Testing
* CI/CD Automation
* Production Best Practices
* AWS Networking Fundamentals

---

# 🙏 Special Thanks

Huge thanks to **TrainWithShubham** and **Shubham Londhe** for simplifying Terraform concepts and making Infrastructure as Code approachable through the TerraWeek challenge.

#TerraWeekChallenge #Terraform #AWS #DevOps #CloudComputing #InfrastructureAsCode #GitHubActions #Automation
