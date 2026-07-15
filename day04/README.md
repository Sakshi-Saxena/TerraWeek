# 🗄️ TerraWeek Day 04 – Terraform State & Remote Backends (Native Locking)

> **Date:** 15 July 2026  
> **Challenge:** #TerraWeekChallenge  
> **Topic:** Terraform State Management, Remote Backends & Native S3 Locking

---

# 📖 Overview

Day 04 was all about understanding the **heart of Terraform – the State File**.

I learned why Terraform needs state, why it is considered sensitive, how to manipulate it safely using `terraform state` commands, and how to store state remotely in an **S3 backend with native locking** using `use_lockfile = true`.

I also explored **Terraform Import Blocks**, allowing existing infrastructure to be brought under Terraform management without recreating resources.

---

# 🎯 Learning Objectives

- Understand what Terraform State is and why it exists.
- Learn why state files are sensitive.
- Explore Terraform state commands.
- Configure a remote backend using Amazon S3.
- Enable native state locking using `use_lockfile`.
- Import existing infrastructure into Terraform state.

---

# 📝 Task 1: Why State Matters

## What is `terraform.tfstate`?

The `terraform.tfstate` file is Terraform's **source of truth**.

It stores:

✅ Resource IDs

✅ Resource attributes

✅ Resource dependencies

✅ Metadata about managed infrastructure

✅ Outputs

Terraform uses this file to determine:

- What infrastructure already exists.
- What needs to be created.
- What needs to be updated.
- What needs to be destroyed.

---

## Why should you never edit the state file manually?

❌ Can corrupt the state.

❌ May cause Terraform to lose track of resources.

❌ Can lead to accidental resource recreation or deletion.

❌ Can introduce inconsistencies between real infrastructure and Terraform state.

Terraform state should always be modified through:

- `terraform apply`
- `terraform import`
- `terraform state` commands

---

## Why should you never commit state to Git?

State files may contain:

- Resource IDs
- Public IPs
- Database endpoints
- Credentials
- Secrets
- Sensitive outputs

Committing state files to Git poses a significant security risk.

---

## What is State Drift?

State drift occurs when infrastructure is changed **outside Terraform**.

Example:

```text
Terraform created an EC2 instance.
↓
User changes its configuration manually in AWS Console.
↓
Terraform state becomes outdated.
```

---

## How do `terraform plan` and `terraform refresh` help?

### terraform plan

Detects differences between:

- Terraform configuration
- Terraform state
- Real infrastructure

---

### terraform refresh

Updates the state file to match the current infrastructure.

---

## Why is State Sensitive?

Terraform state can store:

✅ Passwords

✅ API Keys

✅ Database Credentials

✅ Sensitive Outputs

For this reason:

- Use remote backends.
- Enable encryption.
- Restrict access permissions.

---

# 📝 Task 2: Exploring Terraform State Commands

Practiced the following commands:

---

## List Managed Resources

```bash
terraform state list
```

Shows all resources managed by Terraform.

---

## Inspect a Resource

```bash
terraform state show <resource_address>
```

Displays all attributes of a resource stored in state.

---

## Rename a Resource in State

```bash
terraform state mv <src> <dest>
```

Moves or renames resources within the state without recreating infrastructure.

---

## Stop Managing a Resource

```bash
terraform state rm <resource_address>
```

Removes the resource from state but does not delete the actual infrastructure.

---

## Human-Readable State

```bash
terraform show
```

Displays the current state in a readable format.

---

## When would these commands be useful?

✅ Refactoring resource names.

✅ Migrating resources between modules.

✅ Recovering from mistakes.

✅ Importing existing resources.

✅ Removing resources from Terraform management.

---

# 📸 Screenshots

> Add screenshot here.

<img width="1600" height="722" alt="image" src="https://github.com/user-attachments/assets/21a039f8-080f-42bb-a15d-569bb33a7899" />


<img width="1600" height="421" alt="image" src="https://github.com/user-attachments/assets/6b9d9d45-cf6e-49d5-b9b5-24eafcbe801c" />


# 📝 Task 3: Bootstrap Backend Infrastructure

Created backend infrastructure using local state.

Resources Created:

✅ S3 Bucket

✅ Bucket Encryption

✅ Bucket Versioning

The bucket was created first because the backend must exist before Terraform can use it.

---

## Commands Executed

```bash
cd backend_infra
terraform init
terraform apply
```

---

## Why is bootstrapping required?

Terraform cannot store state in a bucket that does not yet exist.

The backend infrastructure must be created first.

---

# 📸 Screenshots

### Backend Infrastructure Apply

<img width="1600" height="345" alt="image" src="https://github.com/user-attachments/assets/048120a6-b224-49cd-89d6-ac5049181541" />

<img width="1600" height="761" alt="image" src="https://github.com/user-attachments/assets/66f31f59-77e5-4c26-bc29-3594625765ee" />


<img width="1600" height="761" alt="image" src="https://github.com/user-attachments/assets/9d334137-f7ea-45fd-b467-80783b59f750" />



---

### S3 Bucket in AWS Console

<img width="1030" height="374" alt="image" src="https://github.com/user-attachments/assets/9de1c041-a725-45c7-a3d2-f4c485efda22" />


---

# 📝 Task 4: Configure Remote Backend with Native Locking

Configured Terraform backend:

```hcl
terraform {
  backend "s3" {
    bucket       = "your-bucket-name"
    key          = "day04/terraform.tfstate"
    region       = "us-east-1"
    encrypt      = true
    use_lockfile = true
  }
}
```

---

## What does `use_lockfile = true` do?

Creates a temporary:

```text
terraform.tfstate.tflock
```

file in S3 while Terraform operations are running.

This prevents multiple users from modifying the state simultaneously.

---

## Why is Native S3 Locking Important?

✅ Prevents concurrent state modifications.

✅ Eliminates the need for DynamoDB locking.

✅ Simpler architecture.

✅ Recommended for Terraform 1.11+.

---

## Migrated Local State to Remote State

Commands:

```bash
cd backend_demo
terraform init
terraform apply
```

Terraform automatically migrated the local state to Amazon S3.

---

## Verification

Verified:

✅ `terraform.tfstate` uploaded to S3.

✅ `.tflock` file appeared during apply and disappeared after completion.

---

# 📸 Screenshots

### Backend Configuration

<img width="1600" height="761" alt="image" src="https://github.com/user-attachments/assets/df58bbe5-7155-4411-9b23-7023853e8604" />

<img width="1600" height="557" alt="image" src="https://github.com/user-attachments/assets/33dcde13-57c7-42ce-afde-60b58636dabf" />


---

### terraform.tfstate in S3

<img width="1600" height="397" alt="image" src="https://github.com/user-attachments/assets/80ed9860-e1d8-4805-b106-2af86b40a0e8" />


---

# 📝 Task 5: Import an Existing Resource

Created an S3 bucket manually using the AWS Console and imported it into Terraform management.

---

## Import Block

```hcl
import {
  to = aws_s3_bucket.imported
  id = "sakshi-import-demo-2026"
}
```

---

## Resource Block

```hcl
resource "aws_s3_bucket" "imported" {
}
```

---

## Commands Executed

```bash
terraform init
terraform plan -generate-config-out=generated.tf
terraform apply
```

---

## What did I learn?

Terraform can:

✅ Discover existing infrastructure.

✅ Import it into state.

✅ Generate configuration automatically.

✅ Manage previously manual resources.

---

# 📸 Screenshots

---

### Import Plan


<img width="1747" height="764" alt="image" src="https://github.com/user-attachments/assets/c52b0772-3f6f-41e5-932d-a44946066772" />

---


# 🍫 Bonus Learnings

## 🌐 Compare Remote Backends

Terraform supports multiple remote backends for storing state files.

### Amazon S3
- Create an S3 bucket.
- Configure backend:

```hcl
terraform {
  backend "s3" {
    bucket = "my-state-bucket"
    key    = "terraform.tfstate"
    region = "us-east-1"
  }
}
```

---

### HCP Terraform (Terraform Cloud)
- Create an organization and workspace in Terraform Cloud.
- Configure:

```hcl
terraform {
  cloud {
    organization = "my-org"

    workspaces {
      name = "my-workspace"
    }
  }
}
```

---

### Azure Storage Backend
- Create a Storage Account and Container.
- Configure:

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-demo"
    storage_account_name = "mystorageaccount"
    container_name       = "tfstate"
    key                  = "terraform.tfstate"
  }
}
```

---

### Google Cloud Storage (GCS)
- Create a GCS bucket.
- Configure:

```hcl
terraform {
  backend "gcs" {
    bucket = "terraform-state-bucket"
    prefix = "terraform/state"
  }
}
```

---

## 🔄 S3 Bucket Versioning & State Recovery

### Enable Versioning

```hcl
resource "aws_s3_bucket_versioning" "state" {
  bucket = aws_s3_bucket.state.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

### Recover a Previous State Version

1. Go to **AWS Console → S3 → Bucket → Show Versions**.
2. Locate an older version of `terraform.tfstate`.
3. Download or restore that version if needed.

### Why is this useful?

✅ Recover from accidental deletions.

✅ Restore corrupted state files.

✅ Maintain state history.

---

## 🔀 Moved Blocks

Used to rename or move resources without destroying and recreating them.

### Example

Before:

```hcl
resource "random_pet" "demo" {
  length = 3
}
```

After:

```hcl
resource "random_pet" "pet_name" {
  length = 3
}
```

Add:

```hcl
moved {
  from = random_pet.demo
  to   = random_pet.pet_name
}
```

Run:

```bash
terraform plan
terraform apply
```

Terraform updates the state without recreating the resource.

---

## 🗑️ Removed Blocks

Stop managing a resource without deleting the actual infrastructure.

### Example

```hcl
removed {
  from = aws_s3_bucket.logs

  lifecycle {
    destroy = false
  }
}
```

Run:

```bash
terraform apply
```

Terraform removes the resource from state but leaves it running in AWS.

---

## ✅ Check Blocks

Used to create continuous assertions and validations.

### Example

```hcl
check "bucket_versioning" {
  assert {
    condition     = aws_s3_bucket_versioning.state.versioning_configuration[0].status == "Enabled"
    error_message = "Bucket versioning must be enabled."
  }
}
```

Run:

```bash
terraform plan
terraform apply
```

Terraform verifies the condition and throws an error if it fails.

### Why use Check Blocks?

✅ Enforce best practices.

✅ Validate infrastructure health.

✅ Prevent configuration mistakes.

✅ Add guardrails to deployments.

---

# 🎯 Key Takeaways

- Understood the importance of Terraform State.
- Learned why state files are sensitive.
- Practiced state manipulation commands.
- Configured a remote backend using S3.
- Enabled native S3 state locking.
- Imported existing resources into Terraform.
- Learned how Terraform safely manages infrastructure state.

---

# 🚀 Conclusion

Day 04 was a deep dive into one of the most critical concepts in Terraform—**State Management**.

Understanding state and remote backends is essential for working with Terraform in real-world production environments and teams.

> **Infrastructure can be defined as code, but Terraform State is what allows that code to understand and manage reality.**

---

## 🙏 Acknowledgements

A huge thank you to **TrainWithShubham** and **Shubham Londhe** for organizing the **#TerraWeekChallenge** and making Terraform concepts easy to understand through practical examples.

---

#Terraform #IaC #TerraformChallenge #TerraWeekChallenge #TerraformState #AWS #CloudComputing #DevOps #InfrastructureAsCode #S3 #CloudEngineer #TrainWithShubham
