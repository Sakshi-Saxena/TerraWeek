````markdown
# 📦 TerraWeek Day 05 – Modules: Reusable, Composable Infrastructure

> **Date:** 16 July 2026  
> **Challenge:** #TerraWeekChallenge  
> **Topic:** Terraform Modules, Registry Modules & Version Locking

---

# 📖 Overview

Day 05 was all about one of Terraform's most powerful concepts—**Modules**.

Instead of copying and pasting Terraform code repeatedly, I learned how to package infrastructure into reusable building blocks that can be shared across environments, teams, and projects.

I explored:

- Writing my own reusable module
- Consuming modules from the Terraform Registry
- Instantiating modules multiple times using `for_each`
- Publishing modules to GitHub
- Version locking for reproducible infrastructure

---

# 🎯 Learning Objectives

- Understand what Terraform modules are and why they matter.
- Learn the difference between root and child modules.
- Build reusable local modules with inputs and outputs.
- Consume Registry and Git-based modules.
- Implement module version locking.
- Use `for_each` with modules.

---

# 📝 Task 1: Modules – The Why

## What is a Module?

A module is a **container of Terraform configuration files** that work together to provision infrastructure.

Every Terraform configuration is a module.

---

## What is the Root Module?

The directory where you run:

```bash
terraform init
terraform plan
terraform apply
```

is called the **Root Module**.

---

## What is a Child Module?

A module called by another module.

Example:

```hcl
module "web_server" {
  source = "./modules/ec2_instance"
}
```

`./modules/ec2_instance` is the child module.

---

# Benefits of Modules

## ♻️ Reusability

Write infrastructure once and reuse it multiple times.

---

## 🎯 Consistency

Ensures every environment follows the same standards.

---

## 🔒 Encapsulation

Hides implementation details and exposes only required inputs and outputs.

---

## 📌 Versioning

Allows controlled upgrades and reproducible deployments.

---

## 🧪 Testing

Modules can be developed, tested, and improved independently.

---

# Files in a Well-Structured Module

```text
my-module/
├── main.tf
├── variables.tf
├── outputs.tf
└── README.md
```

---

## main.tf

Contains the infrastructure resources.

---

## variables.tf

Defines module inputs.

---

## outputs.tf

Exposes useful values.

---

## README.md

Provides documentation and usage examples.

---

# 📝 Task 2: Write Your Own Module

Created a reusable EC2 module:

```text
modules/
└── ec2_instance/
    ├── main.tf
    ├── variables.tf
    ├── outputs.tf
```

---

## What I Learned

✅ Modules should accept IDs and values as inputs.

✅ Shared lookups should be resolved in the root module.

✅ Modules become more reusable and efficient.

---

## Root Module Example

The root module:

- Resolved the AMI.
- Resolved subnet information.
- Resolved security groups.
- Passed values into the child module.

The child module:

- Created the EC2 instance.
- Returned outputs back to the root module.

---

## Commands Executed

```bash
terraform init
terraform plan
terraform apply
terraform destroy
```

---

# 📸 Screenshots

### Module Initialization

<img width="1600" height="456" alt="image" src="https://github.com/user-attachments/assets/a0244fe3-b846-43a7-a293-83f7bd02a52d" />


---

### Terraform Plan

<img width="1600" height="612" alt="image" src="https://github.com/user-attachments/assets/6fecba59-63a8-44cd-9805-d1b09941954c" />

---

### Terraform Apply

<img width="1600" height="612" alt="image" src="https://github.com/user-attachments/assets/d2c7e456-57a2-4dc4-bc59-9e0689882c75" />

<img width="1536" height="708" alt="image" src="https://github.com/user-attachments/assets/a971c43f-09c6-477e-805b-590ab91eb129" />


---

### Terraform Destroy

<img width="1536" height="708" alt="image" src="https://github.com/user-attachments/assets/94522fd7-c798-4235-a7b7-fbe9aa35550a" />

---

# 📝 Task 3: Modular Composition using for_each

Instantiated the same module multiple times:

- app
- worker
- cache

---

## Why use `for_each` with modules?

✅ Cleaner code.

✅ No duplication.

✅ Easier maintenance.

✅ Each module instance gets a stable identity.

---

## What I Learned

Terraform creates:

```text
module.servers["app"]
module.servers["worker"]
module.servers["cache"]
```

instead of managing resources through numeric indexes.

This makes infrastructure easier to scale and maintain.

---

# 📸 Screenshots

### Terraform Plan with Multiple Modules

<img width="1536" height="708" alt="image" src="https://github.com/user-attachments/assets/145ec65b-5b59-4936-a639-cf94e4f383fa" />


---

### Terraform Apply

<img width="1536" height="708" alt="image" src="https://github.com/user-attachments/assets/53b28e46-43d0-462b-8a8a-78387a49a035" />

---

### AWS Console


<img width="1536" height="708" alt="image" src="https://github.com/user-attachments/assets/3184a891-53d7-4fce-a81f-18275b3803b5" />

---

# 📝 Task 4: Consume a Registry Module + Version Locking

Consumed the official AWS VPC module from the Terraform Registry.

---

## Why use Registry Modules?

✅ Community maintained.

✅ Production tested.

✅ Reduces development time.

✅ Follows best practices.

---

## Why Version Locking Matters

Without version pinning:

- Module behavior may unexpectedly change.
- New releases can introduce breaking changes.

With version pinning:

✅ Reproducible builds.

✅ Predictable deployments.

✅ Safer upgrades.

---

## Understanding `~>` (Pessimistic Constraint)

### `~> 5.0`

Allows:

```text
5.0
5.1
5.2
5.9
```

Does NOT allow:

```text
6.0
```

---

### `~> 5.1.0`

Allows:

```text
5.1.1
5.1.2
5.1.9
```

Does NOT allow:

```text
5.2.0
```

---

# 📸 Screenshots

### Module Download During terraform init

<img width="1536" height="376" alt="image" src="https://github.com/user-attachments/assets/55205ecb-d638-49db-9c3f-84256ae634db" />


---

### Terraform Plan

<img width="1536" height="466" alt="image" src="https://github.com/user-attachments/assets/33ff9c81-30b1-48d6-908b-935de911a26b" />


---

### Terraform Output & apply

<img width="1536" height="562" alt="image" src="https://github.com/user-attachments/assets/234e6670-7e72-44fc-bcbc-1f20d7f00e57" />


---

### AWS Console

<img width="1536" height="681" alt="image" src="https://github.com/user-attachments/assets/f00f6bb7-84bf-4997-a363-e0b089e1b3ef" />


---

# 📝 Task 5: Ways to Lock Module Versions

---

## Registry Modules

```hcl
version = "~> 5.0"
```

Other examples:

```hcl
version = "= 5.1.2"
version = ">= 5.0"
version = "< 6.0"
```

---

## Git Tags

```hcl
source = "git::https://github.com/org/repo.git//path?ref=v1.2.0"

```

---

## Git Branch

```hcl
source = "git::https://github.com/org/repo.git//path?ref=main"
```

---

## Git Commit SHA (Immutable)

```hcl
source = "git::https://github.com/org/repo.git//path?ref=2a4c6de9f8b1..."
```

---

# Why Pin Versions?

✅ Reproducible infrastructure.

✅ Stable deployments.

✅ Controlled upgrades.

✅ No surprise breaking changes.

---

# 🍫 Bonus Learnings

## Added README and Validation to My Module

Implemented:

- Module documentation.
- Variable validation.
- Better error handling.

---

## Published My Module to GitHub

Steps:

1. Created a GitHub repository.
2. Pushed module code.
3. Created a Git tag.

```bash
git tag v1.0.0
git push origin v1.0.0
```

Consumed the module using:

```hcl
source = "git::https://github.com/<username>/<repo>.git?ref=v1.0.0"

<img width="1333" height="126" alt="image" src="https://github.com/user-attachments/assets/596b9d6f-870c-4498-b270-f96eec2ece21" />

```

---

## Explored Module Composition

Passed outputs from one module as inputs into another.

Example:

```text
VPC Module
     ↓
Subnet IDs
     ↓
EC2 Module
```

This allows building complex infrastructure from small reusable components.

---

# 🎯 Key Takeaways

- Understood the importance of Terraform Modules.
- Built and consumed local modules.
- Used modules with `for_each`.
- Consumed Registry Modules.
- Implemented Module Version Locking.
- Published and consumed modules from GitHub.
- Learned module composition patterns.

---

# 🚀 Conclusion

Day 05 showed how Terraform scales beyond small projects.

Modules make infrastructure:

✅ Reusable  
✅ Maintainable  
✅ Consistent  
✅ Versioned  
✅ Production Ready

> **Good infrastructure isn't written repeatedly—it is packaged, versioned, and reused through modules.**

---

## 🙏 Acknowledgements

A huge thank you to **TrainWithShubham** and **Shubham Londhe** for organizing the **#TerraWeekChallenge** and making Terraform concepts easy to understand through practical examples.

---

#Terraform #IaC #TerraformChallenge #TerraWeekChallenge #TerraformModules #AWS #CloudComputing #DevOps #InfrastructureAsCode #CloudEngineer #TrainWithShubham
````
