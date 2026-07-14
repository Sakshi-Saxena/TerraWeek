# ☁️ TerraWeek Day 03 – Providers, Resources & Your First Cloud Infrastructure

> **Date:** 14 July 2026  
> **Challenge:** #TerraWeekChallenge  
> **Topic:** Providers, Resources, Data Sources & AWS Infrastructure Provisioning

---

# 📖 Overview

Day 03 was my first step into provisioning **real cloud infrastructure** using Terraform and AWS.

I learned how Terraform interacts with cloud providers, the difference between resources and data sources, and how to provision a complete networking stack consisting of:

- VPC
- Public Subnet
- Internet Gateway
- Route Table
- Security Group
- EC2 Instance

I also explored Terraform's powerful **meta-arguments** like `count`, `for_each`, `depends_on`, and `lifecycle`.

---

# 🎯 Learning Objectives

- Configure Terraform providers and provider version pinning.
- Understand resources and data sources.
- Provision real cloud infrastructure on AWS.
- Learn and implement Terraform meta-arguments.
- Safely update and destroy infrastructure.

---

# 📝 Task 1: Providers & Version Pinning

## Terraform Block

```hcl
terraform {
  required_version = ">= 1.13"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}
```

---

## Why Version Pinning Matters

✅ Prevents unexpected breaking changes.

✅ Ensures consistent behavior across different environments.

✅ Keeps CI/CD pipelines reproducible.

✅ Makes collaboration easier because everyone uses the same provider version.

---

## What does `~>` mean?

The `~>` operator is called the **pessimistic version constraint**.

Example:

```hcl
version = "~> 6.0"
```

means:

- Allow:

```text
6.0
6.1
6.2
6.5
```

- Do NOT allow:

```text
7.0
```

This gives us bug fixes and minor updates while avoiding potentially breaking major version upgrades.

---

## Bonus: Provider Aliases

Terraform allows configuring multiple providers.

Example:

```hcl
provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "west"
  region = "us-west-2"
}
```

### When would we use this?

- Multi-region deployments
- Disaster recovery
- Cross-region backups
- Global applications

---

# 📝 Task 2: Resources vs Data Sources

## Resources

Resources create and manage infrastructure.

Example:

```hcl
resource "aws_instance" "web" {
  ...
}
```

Examples:

- EC2 Instances
- S3 Buckets
- VPCs
- Security Groups

---

## Data Sources

Data sources only read existing information.

Example:

```hcl
data "aws_ami" "al2023" {
  ...
}
```

Examples:

- Existing AMIs
- Availability Zones
- Existing VPCs
- Existing Route Tables

---

## Difference Between Them

| Resource | Data Source |
|-----------|--------------|
| Creates infrastructure | Reads existing information |
| Managed by Terraform | Not managed by Terraform |
| Can be modified by Terraform | Read-only |
| Stored in state | Stored as reference data |

---

# 📝 Task 3: Provision a Cloud Stack

## Infrastructure Created

### Networking

✅ VPC

✅ Public Subnet

✅ Internet Gateway

✅ Route Table

✅ Route Table Association

---

### Security

✅ Security Group

- SSH Port (22)
- HTTP Port (80)

---

### Compute

✅ EC2 Instance

- Latest Amazon Linux 2023 AMI
- Free-tier instance type
- User Data to install Nginx

---

## Architecture

```text
Internet
    │
    ▼
[Internet Gateway]
        │
        ▼
[Route Table]
        │
        ▼
[Public Subnet]
        │
        ▼
[Security Group]
        │
        ▼
[EC2 Instance]
```

---

## Commands Executed

### Initialize

```bash
terraform init
```

### Validate

```bash
terraform validate
```

### Plan

```bash
terraform plan
```

### Apply

```bash
terraform apply
```

### View State

```bash
terraform state list
```

---

## 📸 Screenshots

### Terraform Plan

> Add screenshot here.

---

### Terraform Apply

> Add screenshot here.

---

### Terraform State List

> Add screenshot here.

---

### Running EC2 Instance

> Add screenshot here.

---

### Nginx Welcome Page

> Add screenshot here.

---

# 📝 Task 4: Meta-Arguments in Action

---

## count

Creates multiple identical resources.

Example:

```hcl
resource "aws_instance" "web" {
  count = 2
}
```

### When to use?

- Identical resources
- Simple scaling

---

## for_each

Creates resources using unique keys.

Example:

```hcl
for_each = toset(["dev", "test", "prod"])
```

### When to use?

- Resources with stable identities
- Named resources
- Easier updates and deletions

---

## depends_on

Creates explicit dependencies.

Example:

```hcl
depends_on = [
  aws_internet_gateway.igw
]
```

### Why?

Ensures resources are created in the correct order.

---

## lifecycle

Controls how Terraform manages resources.

### create_before_destroy

```hcl
lifecycle {
  create_before_destroy = true
}
```

Creates replacement resources before destroying old ones.

---

### prevent_destroy

```hcl
lifecycle {
  prevent_destroy = true
}
```

Prevents accidental deletion.

---

### ignore_changes

```hcl
lifecycle {
  ignore_changes = [
    tags["LastModified"]
  ]
}
```

Ignores specific attribute changes.

---

# 🧠 count vs for_each

## count

Use when resources are:

- Identical
- Interchangeable
- Indexed numerically

---

## for_each

Use when resources:

- Have unique names
- Need stable identities
- Should not be reindexed when one is removed

---

# 📝 Task 5: Update & Destroy

## Changes Performed

- Modified resource tags.
- Explored changing instance attributes.
- Observed the Terraform execution plan.

---

## What I Learned

### In-place Update

Terraform modifies the existing resource.

Example:

```text
~ update in-place
```

---

### Resource Replacement

Terraform destroys and recreates the resource.

Example:

```text
-/+ destroy and create replacement
```

---

## Cleanup

Always destroy infrastructure after completing the exercise:

```bash
terraform destroy
```

This helps avoid unexpected AWS charges.

---

## 📸 Screenshots

### Update Plan Diff

> Add screenshot here.

---

### Terraform Destroy

> Add screenshot here.

---

# 🍫 Bonus Learnings

✅ Installed Nginx using EC2 User Data.

✅ Served a custom webpage from EC2.

✅ Explored Terraform dependency graphs.

✅ Learned about provider aliases and multi-region deployments.

---

# 🎯 Key Takeaways

- Configured and authenticated AWS Provider.
- Understood Provider Version Pinning.
- Learned the difference between Resources and Data Sources.
- Provisioned my first complete AWS networking stack.
- Used Terraform meta-arguments.
- Learned the difference between in-place updates and resource replacements.
- Safely destroyed infrastructure after testing.

---

# 🚀 Conclusion

Day 03 was my first experience building real cloud infrastructure with Terraform.

Understanding providers, resources, networking components, and meta-arguments makes Infrastructure as Code significantly more powerful and production-ready.

> **Infrastructure isn't just about creating resources—it's about defining relationships, dependencies, and managing change safely through code.**

---

## 🙏 Acknowledgements

A huge thank you to **TrainWithShubham** and **Shubham Londhe** for organizing the **#TerraWeekChallenge** and making Terraform concepts easy to understand.

---

#Terraform #IaC #TerraformChallenge #TerraWeekChallenge #AWS #CloudComputing #DevOps #InfrastructureAsCode #CloudEngineer #TrainWithShubham