# 🧩 TerraWeek Day 02 – HCL Deep Dive: Variables, Types & Expressions

> **Date:** 13 July 2026  
> **Challenge:** #TerraWeekChallenge  
> **Topic:** Mastering HCL (HashiCorp Configuration Language)

---

# 📖 Overview

Day 02 was all about understanding the **language behind Terraform**—**HCL (HashiCorp Configuration Language)**.

Instead of just provisioning infrastructure, I learned how to write **flexible, reusable, and dynamic Terraform configurations** using variables, expressions, locals, outputs, and built-in functions.

---

# 🎯 Learning Objectives

- Understand HCL syntax, blocks, arguments, and expressions.
- Work with different Terraform variable types.
- Use variable validation and sensitive variables.
- Learn how `locals` and `outputs` simplify configurations.
- Explore built-in Terraform functions.
- Understand variable precedence.
- Build a real project using the Docker provider.

---

# 📝 Task 1: Mastering HCL Syntax

## ✅ What I Learned

### Terraform Block Anatomy

```hcl
block_type "label_one" "label_two" {
  argument = value
}
```

Example:

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "sakshi-terraform-bucket"
}
```

### Difference Between a Block and an Argument

#### Block
A block defines a new configuration object.

Example:

```hcl
resource "aws_instance" "web" {
}
```

#### Argument
An argument assigns a value to a property.

Example:

```hcl
instance_type = "t2.micro"
```

---

### Expressions Learned

#### String Interpolation

```hcl
"myapp-${var.environment}"
```

#### References

```hcl
aws_instance.web.id
```

#### Operators

```hcl
5 + 5
var.environment == "prod"
true && false
```

---

# 📝 Task 2: Variables, Types & Validation

## ✅ Variable Types Explored

### Primitive Types

- `string`
- `number`
- `bool`

Example:

```hcl
variable "environment" {
  type = string
}
```

---

### Collection Types

- `list(string)`
- `map(string)`
- `set(string)`

Examples:

```hcl
variable "availability_zones" {
  type = list(string)
}

variable "tags" {
  type = map(string)
}

variable "allowed_ports" {
  type = set(string)
}
```

---

### Structural Types

- `object({...})`
- `tuple([...])`

Example:

```hcl
variable "ec2_config" {
  type = object({
    instance_type = string
    volume_size   = number
  })
}
```

---

## ✅ Variable Validation

```hcl
validation {
  condition     = contains(["dev", "staging", "prod"], var.environment)
  error_message = "environment must be one of: dev, staging, prod."
}
```

### What I Learned

- Validation prevents invalid inputs.
- Improves reliability and consistency.
- Helps enforce standards in Terraform projects.

---

## ✅ Sensitive Variables

```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

### What I Learned

- Sensitive values are hidden from Terraform output.
- Useful for passwords, API keys, and secrets.

---

# 📝 Task 3: Locals, Outputs & Functions

## Locals

Used `locals` to create reusable computed values.

Example:

```hcl
locals {
  name_prefix = "tws-dev"
}
```

### Benefits

- Avoid repetition.
- Improve readability.
- Centralize commonly used values.

---

## Outputs

Outputs expose useful information after deployment.

Example:

```hcl
output "container_name" {
  value = docker_container.web.name
}
```

---

## Built-in Functions Explored

### `upper()`

```hcl
upper("terraweek")
```

Output:

```text
TERRAWEEK
```

---

### `join()`

```hcl
join("-", ["tws", "terraweek", "2026"])
```

Output:

```text
tws-terraweek-2026
```

---

### `merge()`

```hcl
merge({a=1}, {b=2})
```

Output:

```text
{
  a = 1
  b = 2
}
```
---

### Other Functions Explored

- `lookup()`
- `length()`
- `format()`

---

## Using Terraform Console

```bash
terraform console
```

Examples:

```bash
> upper("terraweek")
> join("-", ["tws", "terraform", "2026"])
> length(["a", "b", "c"])
```


<img width="1600" height="232" alt="image" src="https://github.com/user-attachments/assets/490e21a3-562a-4079-a8f1-c029651ea4fd" />

---

# 📝 Task 4: Build Something Real

## Project

Used the **Docker Provider** to:

✅ Pull an Nginx Docker image.

✅ Create and run an Nginx container.

✅ Configure everything using Terraform variables.

---

## Commands Executed

### Initialize

```bash
terraform init
```

### Plan

```bash
terraform plan \
-var 'container_name=tws-web' \
-var 'external_port=8080'
```

### Apply

```bash
terraform apply \
-var 'container_name=tws-web' \
-var 'external_port=8080'
```

### Check Outputs

```bash
terraform output
```

### Destroy Resources

```bash
terraform destroy \
-var 'container_name=tws-web' \
-var 'external_port=8080'
```

---

## Using `terraform.tfvars`

Instead of:

```bash
-var 'container_name=tws-web'
```

I also learned that variables can be stored in:

```text
terraform.tfvars
```
###Outputs:

<img width="1600" height="746" alt="image" src="https://github.com/user-attachments/assets/8fb16918-e5c5-4cb5-b81f-9ed1f5fef7ef" />

<img width="1600" height="819" alt="image" src="https://github.com/user-attachments/assets/8098896a-f4ab-46fa-ac15-40c4a63dbf13" />

<img width="1600" height="616" alt="image" src="https://github.com/user-attachments/assets/05c76c3c-defa-4de4-909f-80a65e412163" />

<img width="1600" height="774" alt="image" src="https://github.com/user-attachments/assets/d0889278-cfaa-413e-af41-6e45244e7c54" />


which makes commands cleaner and easier to manage.

---

# 📊 Terraform Variable Precedence

Terraform loads variable values in the following order:

```text
-var / -var-file
        ↓
*.auto.tfvars
        ↓
terraform.tfvars
        ↓
TF_VAR_ environment variables
        ↓
default values
```

The highest priority value wins.

---

# 🎯 Key Takeaways

- Learned the fundamentals of HCL syntax.
- Understood blocks, arguments, and expressions.
- Explored all major Terraform variable types.
- Implemented validation and sensitive variables.
- Used locals and outputs to simplify configurations.
- Practiced Terraform built-in functions.
- Understood variable precedence.
- Provisioned a Docker container entirely using Terraform variables.

---

# 🚀 Conclusion

Day 02 was a deep dive into the language that powers Terraform. Understanding HCL, variables, and expressions makes Terraform configurations more modular, reusable, and production-ready.

The biggest takeaway from today:

> **Infrastructure becomes truly powerful when it is parameterized, reusable, and easy to maintain.**

---

## 🙏 Acknowledgements

A huge thank you to **TrainWithShubham** and **Shubham Londhe** for organizing the **#TerraWeekChallenge** and making Terraform concepts easy to understand.

---

#Terraform #IaC #TerraformChallenge #TerraWeekChallenge #AWS #DevOps #CloudComputing #Docker #InfrastructureAsCode #TrainWithShubham
