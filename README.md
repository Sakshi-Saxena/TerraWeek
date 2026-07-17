# 🚀 TerraWeek Challenge 2026 – My Terraform Learning Journey

## 👩‍💻 Author

**Sakshi Saxena**

Senior Software Engineer | DevOps & Cloud Enthusiast

AWS | Terraform | Docker | Kubernetes | CI/CD | Linux | GitHub Actions

---

# 🌟 About the Challenge

The **TerraWeek Challenge** was a 6-day hands-on learning journey focused on mastering **Terraform and Infrastructure as Code (IaC)** concepts through practical implementation.

Throughout the challenge, I learned:

* Terraform fundamentals
* Infrastructure as Code principles
* AWS resource provisioning
* State management
* Modules and reusable infrastructure
* Security scanning
* Terraform testing
* CI/CD automation
* Production best practices

---

# 📅 Day 01 – Introduction to Terraform & Infrastructure as Code

## Topics Covered

* What is Infrastructure as Code (IaC)?
* Why Terraform?
* Terraform Workflow
* Providers and Resources
* Terraform CLI Commands

## Hands-on Activities

✅ Installed Terraform

✅ Configured AWS CLI

✅ Created first AWS S3 bucket using Terraform

## Commands Learned

```bash
terraform init
terraform plan
terraform apply
terraform destroy
```

## Key Learnings

* Infrastructure can be managed as code.
* Terraform follows a declarative approach.
* Resources can be provisioned consistently and repeatedly.
* `terraform plan` is one of the most powerful commands for understanding infrastructure changes.

---

# 📅 Day 02 – Variables, Outputs & HCL

## Topics Covered

* HCL Syntax
* Variables
* Outputs
* Variable Validation
* Sensitive Variables
* Local Values

## Hands-on Activities

✅ Created reusable configurations using variables.

✅ Implemented variable validation.

✅ Used outputs to expose resource information.

## Example

```hcl
variable "environment" {
  validation {
    condition = contains(
      ["dev", "staging", "prod"],
      var.environment
    )

    error_message = "Invalid environment."
  }
}
```

## Key Learnings

* Avoid hardcoding values.
* Variables make infrastructure reusable.
* Validation helps prevent configuration mistakes.

---

# 📅 Day 03 – Providers, Resources & AWS Infrastructure

## Topics Covered

* Providers
* Resources
* Data Sources
* User Data
* Import Existing Resources

## Hands-on Activities

✅ Provisioned AWS resources.

✅ Created EC2 instances.

✅ Used user-data for server bootstrap.

✅ Learned resource importing.

## Key Learnings

* Terraform can manage both new and existing infrastructure.
* User-data is preferred over provisioners for bootstrapping.

---

# 📅 Day 04 – Terraform State & Backend

## Topics Covered

* Terraform State
* Remote State
* State Locking
* State Commands
* Backend Configuration

## Hands-on Activities

✅ Configured remote state using S3.

✅ Enabled native state locking.

✅ Explored:

```bash
terraform state list
terraform state show
terraform state pull
terraform state rm
```

## Key Learnings

* State is the source of truth for Terraform.
* Never commit `.tfstate` files to Git.
* Remote state is essential for team collaboration.
* State locking prevents concurrent modifications.

---

# 📅 Day 05 – Modules & Reusable Infrastructure

## Topics Covered

* Local Modules
* Registry Modules
* Module Inputs & Outputs
* Module Versioning

## Hands-on Activities

✅ Built reusable Terraform modules.

✅ Consumed official Terraform Registry modules.

## Key Learnings

* Modules reduce duplication.
* Modules improve maintainability.
* Production infrastructure should always be modular.

---

# 📅 Day 06 – Advanced Terraform & Capstone Project

## Topics Covered

* Terraform Workspaces
* Terraform Test Framework
* Security Scanning
* Cost Estimation
* CI/CD with GitHub Actions
* Terraform Best Practices

---

# 🏗️ Capstone Project

## AWS 2-Tier Infrastructure using Terraform

### GitHub Repository

🔗 **Repository:** https://github.com/Sakshi-Saxena/terraform-2tier-app

---

## Infrastructure Components

✅ Custom VPC

✅ Public & Private Subnets

✅ Internet Gateway

✅ NAT Gateway

✅ Auto Scaling Group

✅ EC2 Instances

✅ Security Groups

✅ Amazon S3 Bucket

---

## Implemented Features

### Terraform Workspaces

```bash
terraform workspace new staging
terraform workspace select staging
terraform workspace show
```

---

### Terraform Testing

```bash
terraform test
```

---

### Security Scanning

```bash
trivy config .
```

---

### Cost Estimation

```bash
infracost breakdown --path=tfplan
```

---

### CI/CD Pipelines

#### Terraform CI

* terraform fmt
* terraform validate
* terraform test
* trivy config
* terraform plan
* infracost breakdown

#### Terraform Apply

* Manual workflow dispatch

#### Terraform Destroy

* Manual workflow dispatch

---

## Key Learnings

* Production infrastructure needs automation.
* Security and cost should be part of the development lifecycle.
* CI/CD significantly improves reliability.
* Terraform testing increases confidence before deployments.

---

# 🏆 Overall Challenge Learnings

## Terraform

✅ Infrastructure as Code

✅ Providers & Resources

✅ Variables & Outputs

✅ Modules

✅ Workspaces

✅ State Management

✅ Testing

---

## AWS

✅ S3

✅ EC2

✅ VPC

✅ Security Groups

✅ Auto Scaling

✅ Networking

---

## DevOps Practices

✅ GitHub Actions

✅ Security Scanning

✅ Cost Optimization

✅ Automation

✅ Documentation

---

# 💡 Biggest Takeaways

* Infrastructure should be reproducible.
* Infrastructure should be secure.
* Infrastructure should be tested.
* Infrastructure should be cost-aware.
* Infrastructure should be automated.
* Infrastructure should be easy to maintain.

---


# 🙏 Acknowledgements

A huge thank you to **TrainWithShubham** and **Shubham Londhe** for organizing this incredible challenge and simplifying Terraform concepts with practical, hands-on learning.

This challenge not only improved my Terraform skills but also strengthened my understanding of building production-ready infrastructure using Infrastructure as Code.

---

# 📌 Connect With Me

🔗 LinkedIn: https://www.linkedin.com/in/sakshi-saxena11/

🔗 GitHub: https://github.com/Sakshi-Saxena

---

#TerraWeekChallenge #Terraform #AWS #DevOps #InfrastructureAsCode #CloudComputing #GitHubActions #Automation #LearningInPublic
