
## Basic (Module, Resource, Data)

In Terraform, **module**, **resource**, and **data** blocks are key concepts that serve different purposes in defining infrastructure as code. Here’s a breakdown of their differences and use cases:

### **1. Modules**
- **Definition**: A **module** is a container for multiple resources that are used together. It allows you to organize and reuse your infrastructure code across multiple configurations or projects. A module can be thought of as a "package" of Terraform configurations.
  
- **Use Case**: You use modules to encapsulate and reuse configurations. For example, if you need to deploy the same infrastructure (like a VPC or a set of Cloud Run services) in multiple environments (e.g., development, staging, production), you can define it once in a module and reuse it in multiple places.

- **Example**:
  ```hcl
  module "network" {
    source = "./modules/network"  # Local path to the module or remote URL
    vpc_id = var.vpc_id           # Input variables for the module
    cidr   = var.vpc_cidr
  }
  ```

- **Purpose**: To group and organize Terraform configurations into reusable and shareable components.

### **2. Resources**
- **Definition**: A **resource** represents a piece of infrastructure, such as a virtual machine, load balancer, or database. Resources are the basic building blocks in Terraform that describe the infrastructure components Terraform will manage.

- **Use Case**: Resources define the specific infrastructure you want to create, update, or destroy. For example, you would define a resource to create an AWS EC2 instance, an Azure storage account, or a Google Cloud Run service.

- **Example**:
  ```hcl
  resource "aws_instance" "web" {
    ami           = "ami-12345678"
    instance_type = "t2.micro"
  }
  ```

- **Purpose**: To define the actual cloud infrastructure that will be provisioned.

### **3. Data Sources (Data Blocks)**
- **Definition**: A **data** block allows you to fetch or read information from existing infrastructure that is **not** managed by your Terraform configuration. It doesn't create or manage resources but instead accesses information about them.

- **Use Case**: Use `data` when you need to reference existing infrastructure. For example, if a VPC is already created by another team or process, you can use a data block to read its details (such as the VPC ID) and reference it in your configuration.

- **Example**:
  ```hcl
  data "aws_vpc" "existing_vpc" {
    filter {
      name   = "tag:Name"
      values = ["my-vpc"]
    }
  }

  output "vpc_id" {
    value = data.aws_vpc.existing_vpc.id
  }
  ```

- **Purpose**: To fetch and use information about infrastructure components that are created outside of Terraform or by another Terraform configuration.

---

### **Summary of Differences**

| **Feature**       | **Module**                       | **Resource**                         | **Data Source**                    |
|-------------------|----------------------------------|--------------------------------------|------------------------------------|
| **Purpose**       | Reusable, encapsulated code      | Defines and manages infrastructure   | Fetches data from existing infrastructure |
| **Creates Infra** | No (but can define resources)    | Yes                                 | No                                 |
| **Use Case**      | Grouping and reusing code        | Provisioning cloud resources         | Accessing existing infrastructure   |
| **Example**       | Module for reusable components   | EC2 instance, Cloud Run service      | Get details about existing VPC, AMI |

### **When to Use Each**

- **Modules**: Use modules when you want to create reusable, repeatable, and shareable infrastructure components. Modules help you organize your configuration and avoid duplication.

- **Resources**: Use resources when you want to provision and manage new infrastructure components, such as compute instances, storage buckets, databases, or VPCs.

- **Data Sources**: Use data sources when you need to retrieve information about infrastructure that already exists but that Terraform does not manage (or that is managed in another Terraform configuration).

By understanding these core concepts, you can effectively structure your Terraform configurations to manage complex infrastructure at scale.


## GitLab Managed Terraform State


GitLab stores Terraform state in **Terraform State Backends**, which are typically configured in your `.gitlab-ci.yml` file or directly in your Terraform configuration. Here are the main ways to store Terraform state with GitLab:

### 1. **GitLab Managed Terraform State (via GitLab Managed Project)**:
GitLab has a built-in feature for managing Terraform state files within GitLab itself. This is stored as part of the project under the **Terraform State** section in the GitLab UI. To use this feature, you need to use GitLab CI/CD pipelines, and the state is automatically stored in GitLab.

**How to use GitLab Managed Terraform State**:

- In your `.gitlab-ci.yml`, use the `gitlab-terraform` Docker image or include the Terraform template.
  
  Example `.gitlab-ci.yml` file:

  ```yaml
  terraform:
    image: hashicorp/terraform:latest
    script:
      - terraform init
      - terraform apply -auto-approve
    environment:
      name: production
    artifacts:
      paths:
        - .terraform/
      when: always
    cache:
      key: ${CI_COMMIT_REF_SLUG}
      paths:
        - .terraform/
    dependencies: []
  ```

In this case, GitLab will store the Terraform state in its internal storage, which you can access in the GitLab UI:

- Navigate to your project → **Operations** → **Terraform**.

From here, you can download, view, or delete your Terraform state.

### 2. **External Terraform State Backends**:
You can configure Terraform to use an external backend for storing the state (which is recommended for larger projects or if you want to manage state independently). Some popular backends include:
- **AWS S3**
- **Google Cloud Storage (GCS)**
- **Azure Blob Storage**
- **HashiCorp Terraform Cloud**
- **Consul**
  
In these cases, the state is not stored within GitLab itself but on the external backend of your choice.

#### Example for using Google Cloud Storage (GCS):

In your `main.tf`, you would define the backend like so:

```hcl
terraform {
  backend "gcs" {
    bucket = "my-terraform-bucket"
    prefix = "terraform/state"
  }
}
```

#### Example for using AWS S3:

```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-bucket"
    key    = "terraform/state"
    region = "us-west-2"
  }
}
```

In both cases, GitLab will run the Terraform commands, but the state will be stored in the respective backend (GCS, S3, etc.).

### Summary:
- **GitLab Managed Terraform State**: Stored within GitLab and accessible via the GitLab UI.
- **External Backends**: You can configure Terraform to store its state in cloud storage (S3, GCS, etc.), independent of GitLab. This is done via the `backend` block in your `main.tf`.



## CLI
| Command                                                                                                                                                                                                                                                                                                                                                     | Description                                                     | Common Options                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- | ----------------------------------------------------- |
| `terraform init`                                                                                                                                                                                                                                                                                                                                            | Initializes a working directory, downloads providers            | `-upgrade`, `-backend-config=KEY=VALUE`               |
| `terraform plan`                                                                                                                                                                                                                                                                                                                                            | Creates an execution plan for changes                           | `-out=FILE`, `-var 'key=value'`, `-detailed-exitcode` |
| `terraform apply`                                                                                                                                                                                                                                                                                                                                           | Applies changes to match the desired state                      | `-auto-approve`, `-var-file=FILE`, `-refresh=false`   |
| `terraform destroy`                                                                                                                                                                                                                                                                                                                                         | Destroys all managed resources                                  | `-auto-approve`, `-var 'key=value'`                   |
| `terraform fmt`                                                                                                                                                                                                                                                                                                                                             | Formats configuration files to canonical style                  | `-check`, `-diff`, `-recursive`                       |
| `terraform validate`                                                                                                                                                                                                                                                                                                                                        | Validates configuration syntax and correctness                  | None significant                                      |
| `terraform state`                                                                                                                                                                                                                                                                                                                                           | Manipulates the state file (e.g., list, move, remove)           | Subcommands: `list`, `mv`, `rm`                       |
| `terraform workspace`                                                                                                                                                                                                                                                                                                                                       | Manages multiple workspaces (e.g., new, select, list)           | Subcommands: `new NAME`, `select NAME`, `list`        |
| `terraform output`                                                                                                                                                                                                                                                                                                                                          | Displays defined output values                                  | `-json`, `-raw`                                       |
| `terraform providers`                                                                                                                                                                                                                                                                                                                                       | Shows provider info or locks versions                           | Subcommand: `lock`                                    |
| `terraform refresh`                                                                                                                                                                                                                                                                                                                                         | Updates state to match real-world infrastructure                | `-var 'key=value'`                                    |
| `terraform taint`                                                                                                                                                                                                                                                                                                                                           | Marks a resource to be recreated (e.g., `aws_instance.example`) | None significant                                      |
| `terraform untaint`                                                                                                                                                                                                                                                                                                                                         | Removes tainted status (e.g., `aws_instance.example`)           | None significant                                      |
| `terraform version`                                                                                                                                                                                                                                                                                                                                         | Displays Terraform and provider versions                        | None significant                                      |
| `terraform graph`                                                                                                                                                                                                                                                                                                                                           | Generates a dependency graph in DOT format                      | `-type=plan`                                          |
| `terraform force-unlock`                                                                                                                                                                                                                                                                                                                                    | Remove lock                                                     |                                                       |
| See what will be destroyed (safe preview)<br>`terraform plan -destroy`<br><br># 2) Destroy everything Terraform knows about in this workspace<br>`terraform destroy -auto-approve`<br><br># 3) Re-initialize (pull modules/providers again)<br>`terraform init -upgrade`<br><br># 4) Rebuild with your current variables<br>`terraform apply -auto-approve` | Destroy infra                                                   |                                                       |
## Initital settings

1. Create tokemn for terraform user - Settings-Access Tokens create project access tokens.
	1. token name - terraform
	2. scope
	3. write_repository
2. export name and passowrid to local env for terraform debug
	export TF_HTTP_USERNAME="terraform"
	export TF_HTTP_PASSWORD="token"
3. main.tf
   provide a limnk to repo with terraforn scripts
   backend "http" {

address = "https://git.seller-1.ru/api/v4/projects/28/backend/state/rtk-terraform"

lock_address = "https://git.seller-1.ru/api/v4/projects/28/backend/state/rtk-terraform/lock"

lock_method = "POST"

unlock_address = "https://git.seller-1.ru/api/v4/projects/28/backend/state/rtk-terraform/lock"

unlock_method = "DELETE"

retry_wait_min = 5

}

required_version = ">= 1.3"

}
4. terraform init -reconfigure or terraform init


export GITLAB_ACCESS_TOKEN=<YOUR-ACCESS-TOKEN>
export TF_STATE_NAME=default
terraform init \
    -backend-config="address=http://git.seller-1.ru/api/v4/projects/31/terraform/state/$TF_STATE_NAME" \
    -backend-config="lock_address=http://git.seller-1.ru/api/v4/projects/31/terraform/state/$TF_STATE_NAME/lock" \
    -backend-config="unlock_address=http://git.seller-1.ru/api/v4/projects/31/terraform/state/$TF_STATE_NAME/lock" \
    -backend-config="username=rukonovalov" \
    -backend-config="password=$GITLAB_ACCESS_TOKEN" \
    -backend-config="lock_method=POST" \
    -backend-config="unlock_method=DELETE" \
    -backend-config="retry_wait_min=5"
    
    
    ## Commands
