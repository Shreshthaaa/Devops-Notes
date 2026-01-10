# Terraform Essentials: Infrastructure as Code

Terraform is a powerful tool used to manage infrastructure safely and efficiently. It transforms manual setup into automated code.


## 1. Understanding Terraform and IaC

Terraform is an **Infrastructure as Code (IaC)** tool that uses a **declarative** approach. You define the final state you want, and Terraform handles the steps to get there.

### The Benefits of IaC

* **Speed:** Automation is faster than manual clicking.
* **Consistency:** Eliminates "works on my machine" issues by using the same code for every environment.
* **Version Control:** You can store your infrastructure code in Git, allowing you to track changes and roll back if needed.


## 2. Terraform Architecture

Terraform relies on a few core components to function:

* **Providers:** Plugins that talk to external APIs (AWS, Azure, Docker).
* **Resources:** The actual components you want to build (EC2 instances, S3 buckets).
* **State:** A sensitive file (`terraform.tfstate`) that acts as Terraform's memory, mapping your code to the real-world resources it created.


## 3. The Execution Workflow

Terraform follows a strict three-step process to ensure safety.

1. **Init:** `terraform init` prepares your directory and downloads the necessary providers.
2. **Plan:** `terraform plan` shows exactly what Terraform will do without actually making changes. It is a "dry run."
3. **Apply:** `terraform apply` executes the plan and builds the infrastructure.


## 4. HCL: Syntax and Structure

Terraform uses **HashiCorp Configuration Language (HCL)**. It is organized into blocks and arguments.

### Resource Block Example

```hcl
resource "local_file" "demo" {
  filename = "/tmp/demo.txt"
  content  = "Learning Terraform"
}

```

* `local_file`: The resource type (Provider + Object).
* `demo`: The internal name you use to reference this block.
* `filename/content`: Arguments that define the resource properties.


## 5. Variables and Outputs

To make your code reusable and dynamic, use variables and outputs.

* **Variables:** Inputs for your code. Instead of hardcoding a filename, use `var.filename`.
* **Outputs:** Displays specific information after the build is finished, such as a website URL or an IP address.

```hcl
output "container_id" {
  value = docker_container.nginx.id
}

```


## 6. Common Commands Cheat Sheet

* `terraform fmt`: Automatically cleans up and indents your code.
* `terraform validate`: Checks if your code is logically correct before planning.
* `terraform show`: Shows the current state of your managed infrastructure.
* `terraform destroy`: Safely removes all infrastructure managed by the current configuration.


## Summary of Data Types

| Type | Example |
| --- | --- |
| **String** | `"us-east-1"` |
| **Number** | `80` |
| **List** | `["web1", "web2"]` |
| **Map** | `{ env = "dev", team = "finance" }` |