### Detailed Guide with Code Examples and Hands-On Activities

---

### 1. Variables

**Introduction to Variables**

**What are Variables?**
- Variables in Terraform allow you to parameterize your configurations, making them more flexible and reusable.

**Types of Variables:**
- `string`
- `number`
- `bool`
- `list`
- `map`

**Sample Code and Explanation:**

1. **String Variable:**

   ```hcl
   # variables-string.tf
   variable "instance_type" {
     description = "Type of instance"
     type        = string
     default     = "t2.micro"
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = var.instance_type
   }
   ```

   - This configuration defines a string variable for the instance type and uses it in the EC2 instance resource.

2. **Number Variable:**

   ```hcl
   # variables-number.tf
   variable "instance_count" {
     description = "Number of instances"
     type        = number
     default     = 2
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     count         = var.instance_count
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

   - This configuration defines a number variable for the instance count and uses it in the EC2 instance resource.

3. **Boolean Variable:**

   ```hcl
   # variables-boolean.tf
   variable "create_instance" {
     description = "Boolean to create instance"
     type        = bool
     default     = true
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     count         = var.create_instance ? 1 : 0
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

   - This configuration defines a boolean variable to conditionally create an EC2 instance.

4. **List Variable:**

   ```hcl
   # variables-list.tf
   variable "instance_types" {
     description = "List of instance types"
     type        = list(string)
     default     = ["t2.micro", "t2.small"]
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     count         = length(var.instance_types)
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = element(var.instance_types, count.index)
   }
   ```

   - This configuration defines a list variable for instance types and uses it to create multiple EC2 instances.

5. **Map Variable:**

   ```hcl
   # variables-map.tf
   variable "instance_tags" {
     description = "Map of instance tags"
     type        = map(string)
     default     = {
       Name = "example-instance"
       Env  = "dev"
     }
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     tags = var.instance_tags
   }
   ```

   - This configuration defines a map variable for instance tags and uses it in the EC2 instance resource.

**Hands-on Activity:**

1. **Define and Use a String Variable:**
   - Create a new file `variables-string.tf`.
   - Add the code to define and use a string variable.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Define and Use a Number Variable:**
   - Create a new file `variables-number.tf`.
   - Add the code to define and use a number variable.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Define and Use a Boolean Variable:**
   - Create a new file `variables-boolean.tf`.
   - Add the code to define and use a boolean variable.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Define and Use a List Variable:**
   - Create a new file `variables-list.tf`.
   - Add the code to define and use a list variable.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Define and Use a Map Variable:**
   - Create a new file `variables-map.tf`.
   - Add the code to define and use a map variable.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 2. State Management

**Introduction to State Management**

**What is State Management?**
- Terraform state is used to map real-world resources to your configuration, keep track of metadata, and improve performance for large infrastructures.

**Why Manage State?**
- State allows Terraform to know what resources to add, update, or delete.

**Types of Backends:**
- Local
- Remote (e.g., S3, Azure Storage, GCS, etc.)

**Sample Code and Explanation:**

1. **Local State:**

   ```hcl
   # state-local.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

   - This configuration uses local state, which is stored in the `terraform.tfstate` file in the working directory.

2. **Remote State with AWS S3:**

   ```hcl
   # state-remote-s3.tf
   terraform {
     backend "s3" {
       bucket = "my-terraform-state"
       key    = "path/to/my/key"
       region = "us-west-2"
     }
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

   - This configuration uses remote state stored in an AWS S3 bucket.

3. **Remote State with Azure Storage:**

   ```hcl
   # state-remote-azure.tf
   terraform {
     backend "azurerm" {
       resource_group_name  = "myResourceGroup"
       storage_account_name = "myStorageAccount"
       container_name       = "tfstate"
       key                  = "terraform.tfstate"
     }
   }

   provider "azurerm" {
     features {}
   }

   resource "azurerm_resource_group" "example" {
     name     = "example-resources"
     location = "West US"
   }
   ```

   - This configuration uses remote state stored in Azure Storage.

4. **Remote State with Google Cloud Storage:**

   ```hcl
   # state-remote-gcs.tf
   terraform {
     backend "gcs" {
       bucket = "my-terraform-state"
       prefix = "terraform/state"
     }
   }

   provider "google" {
     project = "my-project-id"
     region  = "us-central1"
   }

   resource "google_compute_instance" "example" {
     name         = "example-instance"
     machine_type = "f1-micro"
     zone         = "us-central1-a"

     boot_disk {
       initialize_params {
         image = "debian-cloud/debian-9"
       }
     }

     network_interface {
       network = "default"
     }
   }
   ```

   - This configuration uses remote state stored in Google Cloud Storage.

5. **Remote State with Terraform Cloud:**

   ```hcl
   # state-remote-tfc.tf
   terraform {
     backend "remote" {
       hostname = "app.terraform.io"
       organization = "my-org"

       workspaces {
         name = "my-workspace"
       }
     }
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

   - This configuration uses remote state stored in Terraform Cloud.

**Hands-on Activity:**

1. **Use Local State:**
   - Create a new file `state-local.tf`.
   - Add the code to use local state.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use Remote State with AWS S3:**
   - Create a new file `state-remote-s3.tf`.
   - Add the code to use remote state with AWS S3.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Use Remote State with Azure Storage:**
   - Create a new file `state-remote-azure.tf`.
   - Add the code to use remote state with Azure Storage.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply


     ```

4. **Use Remote State with Google Cloud Storage:**
   - Create a new file `state-remote-gcs.tf`.
   - Add the code to use remote state with Google Cloud Storage.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Use Remote State with Terraform Cloud:**
   - Create a new file `state-remote-tfc.tf`.
   - Add the code to use remote state with Terraform Cloud.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 3. Modules

**Introduction to Modules**

**What are Modules?**
- Modules in Terraform are self-contained packages of Terraform configurations that are managed as a group.

**Why Use Modules?**
- Modules allow you to encapsulate and reuse configurations, making your infrastructure more modular and easier to manage.

**Creating a Module:**

1. **Create a Simple Module:**
   - Create a directory `modules/ec2-instance`.
   - Create a file `modules/ec2-instance/main.tf`.
   - Add the following code:

     ```hcl
     # modules/ec2-instance/main.tf
     variable "instance_name" {
       description = "Name of the instance"
       type        = string
       default     = "example-instance"
     }

     variable "instance_type" {
       description = "Type of instance"
       type        = string
       default     = "t2.micro"
     }

     provider "aws" {
       region = "us-west-2"
     }

     resource "aws_instance" "example" {
       ami           = "ami-0c55b159cbfafe1f0"
       instance_type = var.instance_type

       tags = {
         Name = var.instance_name
       }
     }
     ```

   - Create a file `modules/ec2-instance/outputs.tf`.
   - Add the following code:

     ```hcl
     # modules/ec2-instance/outputs.tf
     output "instance_id" {
       description = "ID of the instance"
       value       = aws_instance.example.id
     }
     ```

2. **Use the Module:**
   - Create a new file `main.tf`.
   - Add the following code:

     ```hcl
     # main.tf
     module "ec2_instance" {
       source        = "./modules/ec2-instance"
       instance_name = "my-instance"
       instance_type = "t2.small"
     }

     output "instance_id" {
       value = module.ec2_instance.instance_id
     }
     ```

**Sample Code and Explanation:**

1. **Module for EC2 Instance:**

   ```hcl
   # modules/ec2-instance/main.tf
   variable "instance_name" {
     description = "Name of the instance"
     type        = string
     default     = "example-instance"
   }

   variable "instance_type" {
     description = "Type of instance"
     type        = string
     default     = "t2.micro"
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = var.instance_type

     tags = {
       Name = var.instance_name
     }
   }
   ```

2. **Outputs for the Module:**

   ```hcl
   # modules/ec2-instance/outputs.tf
   output "instance_id" {
     description = "ID of the instance"
     value       = aws_instance.example.id
   }
   ```

3. **Using the Module:**

   ```hcl
   # main.tf
   module "ec2_instance" {
     source        = "./modules/ec2-instance"
     instance_name = "my-instance"
     instance_type = "t2.small"
   }

   output "instance_id" {
     value = module.ec2_instance.instance_id
   }
   ```

4. **Module for VPC:**

   ```hcl
   # modules/vpc/main.tf
   variable "vpc_cidr" {
     description = "CIDR block for the VPC"
     type        = string
     default     = "10.0.0.0/16"
   }

   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_vpc" "example" {
     cidr_block = var.vpc_cidr

     tags = {
       Name = "example-vpc"
     }
   }
   ```

5. **Using the VPC Module:**

   ```hcl
   # main.tf
   module "vpc" {
     source   = "./modules/vpc"
     vpc_cidr = "10.1.0.0/16"
   }
   ```

**Hands-on Activity:**

1. **Create a Simple Module:**
   - Create a directory `modules/ec2-instance`.
   - Create a file `modules/ec2-instance/main.tf` and add the code to create an EC2 instance.
   - Create a file `modules/ec2-instance/outputs.tf` and add the code to output the instance ID.
   - Use the module in a new file `main.tf` and apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use the Module for VPC:**
   - Create a directory `modules/vpc`.
   - Create a file `modules/vpc/main.tf` and add the code to create a VPC.
   - Use the module in a new file `main.tf` and apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 4. Data Sources

**Introduction to Data Sources**

**What are Data Sources?**
- Data sources in Terraform allow you to fetch data from external sources and use it in your configurations.

**Why Use Data Sources?**
- Data sources provide a way to query external data, making your configurations more dynamic and adaptable.

**Sample Code and Explanation:**

1. **AWS AMI Data Source:**

   ```hcl
   # data-source-ami.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_ami" "example" {
     most_recent = true

     filter {
       name   = "name"
       values = ["amzn2-ami-hvm-*-x86_64-gp2"]
     }

     filter {
       name   = "virtualization-type"
       values = ["hvm"]
     }

     owners = ["137112412989"]
   }

   resource "aws_instance" "example" {
     ami           = data.aws_ami.example.id
     instance_type = "t2.micro"
   }
   ```

   - This configuration uses a data source to fetch the latest Amazon Linux 2 AMI and uses it to create an EC2 instance.

2. **AWS VPC Data Source:**

   ```hcl
   # data-source-vpc.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_vpc" "example" {
     filter {
       name   = "tag:Name"
       values = ["my-vpc"]
     }
   }

   resource "aws_subnet" "example" {
     vpc_id            = data.aws_vpc.example.id
     cidr_block        = "10.0.1.0/24"
     availability_zone = "us-west-2a"
   }
   ```

   - This configuration uses a data source to fetch a VPC by tag and uses its ID to create a subnet.

3. **Azure Resource Group Data Source:**

   ```hcl
   # data-source-azure-rg.tf
   provider "azurerm" {
     features {}
   }

   data "azurerm_resource_group" "example" {
     name = "my-resource-group"
   }

   resource "azurerm_storage_account" "example" {
     name                     = "examplestorageacct"
     resource_group_name      = data.azurerm_resource_group.example.name
     location                 = data.azurerm_resource_group.example.location
     account_tier             = "Standard"
     account_replication_type = "LRS"
   }
   ```

   - This configuration uses a data source to fetch an Azure resource group and uses its name and location to create a storage account.

4. **Google Cloud Project Data Source:**

   ```hcl
   # data-source-gcp-project.tf
   provider "google" {
     project = "my-project-id"
     region  = "us-central1"
   }

   data "google_project" "example" {
     project_id = "my-project-id"
   }

   resource "google_compute_network" "example" {
     name       = "example-network"
     project    = data.google_project.example.project_id
     auto_create_subnetworks = true
   }
   ```

   - This configuration uses a data source to fetch a Google Cloud project and uses its ID to create a network.

5. **GitHub Repository Data Source:**

   ```hcl
   # data-source-github-repo.tf
   provider "github" {
     token = "my-github-token"
   }

   data "github_repository" "example" {
     full_name = "hashicorp/terraform"
   }

   output "repo_id" {
     value = data.github_repository.example.node_id
   }
   ```

   - This configuration uses a data source to fetch a GitHub repository and outputs its node ID.

**Hands-on Activity:**

1. **Use AWS AMI Data Source:**
   - Create a

 new file `data-source-ami.tf`.
   - Add the code to use the AWS AMI data source.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use AWS VPC Data Source:**
   - Create a new file `data-source-vpc.tf`.
   - Add the code to use the AWS VPC data source.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Use Azure Resource Group Data Source:**
   - Create a new file `data-source-azure-rg.tf`.
   - Add the code to use the Azure resource group data source.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Use Google Cloud Project Data Source:**
   - Create a new file `data-source-gcp-project.tf`.
   - Add the code to use the Google Cloud project data source.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Use GitHub Repository Data Source:**
   - Create a new file `data-source-github-repo.tf`.
   - Add the code to use the GitHub repository data source.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 5. Provisioners

**Introduction to Provisioners**

**What are Provisioners?**
- Provisioners in Terraform are used to execute scripts or commands on a local or remote machine as part of the resource creation or destruction process.

**Why Use Provisioners?**
- Provisioners are useful for bootstrapping instances, running configuration management tools, or performing other setup tasks.

**Sample Code and Explanation:**

1. **Local Exec Provisioner:**

   ```hcl
   # provisioner-local-exec.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     provisioner "local-exec" {
       command = "echo Instance ${self.id} is created"
     }
   }
   ```

   - This configuration uses a local exec provisioner to run a local command after the EC2 instance is created.

2. **Remote Exec Provisioner:**

   ```hcl
   # provisioner-remote-exec.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     connection {
       type     = "ssh"
       user     = "ec2-user"
       private_key = file("~/.ssh/id_rsa")
       host     = self.public_ip
     }

     provisioner "remote-exec" {
       inline = [
         "sudo yum install -y httpd",
         "sudo systemctl start httpd"
       ]
     }
   }
   ```

   - This configuration uses a remote exec provisioner to run commands on the EC2 instance after it is created.

3. **File Provisioner:**

   ```hcl
   # provisioner-file.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     connection {
       type     = "ssh"
       user     = "ec2-user"
       private_key = file("~/.ssh/id_rsa")
       host     = self.public_ip
     }

     provisioner "file" {
       source      = "index.html"
       destination = "/var/www/html/index.html"
     }
   }
   ```

   - This configuration uses a file provisioner to upload a local file to the EC2 instance after it is created.

4. **Chef Provisioner:**

   ```hcl
   # provisioner-chef.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     connection {
       type     = "ssh"
       user     = "ec2-user"
       private_key = file("~/.ssh/id_rsa")
       host     = self.public_ip
     }

     provisioner "chef" {
       node_name = "example-node"
       server_url = "https://chef-server.example.com/organizations/my-org"
       validation_client_name = "my-validator"
       validation_key = file("/path/to/my-validator.pem")

       run_list = ["recipe[my-cookbook::default]"]
     }
   }
   ```

   - This configuration uses a Chef provisioner to run a Chef recipe on the EC2 instance after it is created.

5. **Puppet Provisioner:**

   ```hcl
   # provisioner-puppet.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     connection {
       type     = "ssh"
       user     = "ec2-user"
       private_key = file("~/.ssh/id_rsa")
       host     = self.public_ip
     }

     provisioner "puppet" {
       manifests_path = "manifests"
       manifest_file  = "site.pp"
       server         = "puppet.example.com"
     }
   }
   ```

   - This configuration uses a Puppet provisioner to apply Puppet manifests on the EC2 instance after it is created.

**Hands-on Activity:**

1. **Use Local Exec Provisioner:**
   - Create a new file `provisioner-local-exec.tf`.
   - Add the code to use the local exec provisioner.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use Remote Exec Provisioner:**
   - Create a new file `provisioner-remote-exec.tf`.
   - Add the code to use the remote exec provisioner.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Use File Provisioner:**
   - Create a new file `provisioner-file.tf`.
   - Add the code to use the file provisioner.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Use Chef Provisioner:**
   - Create a new file `provisioner-chef.tf`.
   - Add the code to use the Chef provisioner.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Use Puppet Provisioner:**
   - Create a new file `provisioner-puppet.tf`.
   - Add the code to use the Puppet provisioner.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 6. Functions and Expressions

**Introduction to Functions and Expressions**

**What are Functions and Expressions?**
- Functions and expressions in Terraform are used to manipulate and transform data within your configurations.

**Commonly Used Functions:**
- `concat()`
- `element()`
- `join()`
- `length()`
- `lookup()`

**Sample Code and Explanation:**

1. **Concat Function:**

   ```hcl
   # functions-concat.tf
   variable "list1" {
     default = ["a", "b"]
   }

   variable "list2" {
     default = ["c", "d"]
   }

   output "concatenated_list" {
     value = concat(var.list1, var.list2)
   }
   ```

   - This configuration uses the `concat` function to concatenate two lists and outputs the result.

2. **Element Function:**

   ```hcl
   # functions-element.tf
   variable "list" {
     default = ["a", "b", "c"]
   }

   output "second_element" {
     value = element(var.list, 1)
   }
   ```

   - This configuration uses the `element` function to get the second element from a list and outputs the result.

3. **Join Function:**

   ```hcl
   # functions-join.tf
   variable "list" {
     default = ["a", "b", "c"]
   }

   output "joined_string" {
     value = join(",", var.list)
   }
   ```

   - This configuration uses the `join` function to join a list into a string with commas and outputs the result.

4. **Length Function:**

   ```hcl
   # functions-length.tf
   variable "list" {
     default = ["a", "b", "c"]
   }

   output "list_length" {
     value = length(var.list)
   }
   ```

   - This configuration uses the `length` function to get the length of a list and outputs the result.

5. **Lookup Function:**

   ```hcl
   # functions-lookup.tf
   variable "map" {
     default = {
       key1 = "value1"
       key2 = "value2"
     }
   }

   output "value_for_key1" {
     value = lookup(var.map, "key1", "default")


   }
   ```

   - This configuration uses the `lookup` function to get a value from a map by key and outputs the result.

**Hands-on Activity:**

1. **Use Concat Function:**
   - Create a new file `functions-concat.tf`.
   - Add the code to use the `concat` function.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use Element Function:**
   - Create a new file `functions-element.tf`.
   - Add the code to use the `element` function.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Use Join Function:**
   - Create a new file `functions-join.tf`.
   - Add the code to use the `join` function.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Use Length Function:**
   - Create a new file `functions-length.tf`.
   - Add the code to use the `length` function.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Use Lookup Function:**
   - Create a new file `functions-lookup.tf`.
   - Add the code to use the `lookup` function.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 7. Conditional Expressions

**Introduction to Conditional Expressions**

**What are Conditional Expressions?**
- Conditional expressions in Terraform allow you to make decisions within your configurations based on certain conditions.

**Why Use Conditional Expressions?**
- Conditional expressions enable you to create more dynamic and flexible configurations.

**Sample Code and Explanation:**

1. **Simple Conditional Expression:**

   ```hcl
   # conditional-simple.tf
   variable "env" {
     description = "Environment"
     default     = "dev"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = var.env == "prod" ? "t2.large" : "t2.micro"

     tags = {
       Name = var.env == "prod" ? "production-instance" : "development-instance"
     }
   }
   ```

   - This configuration uses conditional expressions to set the instance type and tags based on the environment.

2. **Complex Conditional Expression:**

   ```hcl
   # conditional-complex.tf
   variable "env" {
     description = "Environment"
     default     = "dev"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = var.env == "prod" ? "t2.large" : (var.env == "staging" ? "t2.medium" : "t2.micro")

     tags = {
       Name = var.env == "prod" ? "production-instance" : (var.env == "staging" ? "staging-instance" : "development-instance")
     }
   }
   ```

   - This configuration uses nested conditional expressions to handle multiple environments.

3. **Conditional with Count:**

   ```hcl
   # conditional-count.tf
   variable "create_instance" {
     description = "Create instance"
     default     = true
   }

   resource "aws_instance" "example" {
     count = var.create_instance ? 1 : 0

     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

   - This configuration uses a conditional expression with the `count` argument to conditionally create an instance.

**Hands-on Activity:**

1. **Use Simple Conditional Expression:**
   - Create a new file `conditional-simple.tf`.
   - Add the code to use a simple conditional expression.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use Complex Conditional Expression:**
   - Create a new file `conditional-complex.tf`.
   - Add the code to use a complex conditional expression.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Use Conditional with Count:**
   - Create a new file `conditional-count.tf`.
   - Add the code to use a conditional expression with the `count` argument.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 8. Loops

**Introduction to Loops**

**What are Loops?**
- Loops in Terraform allow you to iterate over lists, maps, and sets to create resources or perform other actions multiple times.

**Why Use Loops?**
- Loops enable you to create and manage multiple resources dynamically and efficiently.

**Sample Code and Explanation:**

1. **For Loop with Resources:**

   ```hcl
   # loops-for-resources.tf
   variable "instance_names" {
     description = "Names of the instances"
     type        = list(string)
     default     = ["instance1", "instance2", "instance3"]
   }

   resource "aws_instance" "example" {
     for_each = toset(var.instance_names)

     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     tags = {
       Name = each.value
     }
   }
   ```

   - This configuration uses a `for_each` loop to create multiple EC2 instances based on a list of names.

2. **For Loop with Outputs:**

   ```hcl
   # loops-for-outputs.tf
   variable "instance_names" {
     description = "Names of the instances"
     type        = list(string)
     default     = ["instance1", "instance2", "instance3"]
   }

   resource "aws_instance" "example" {
     for_each = toset(var.instance_names)

     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     tags = {
       Name = each.value
     }
   }

   output "instance_ids" {
     value = { for name, instance in aws_instance.example : name => instance.id }
   }
   ```

   - This configuration uses a `for` loop in the output block to create a map of instance names to instance IDs.

3. **Count Loop with Resources:**

   ```hcl
   # loops-count-resources.tf
   variable "instance_count" {
     description = "Number of instances"
     default     = 3
   }

   resource "aws_instance" "example" {
     count = var.instance_count

     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     tags = {
       Name = "instance-${count.index + 1}"
     }
   }
   ```

   - This configuration uses a `count` loop to create a specified number of EC2 instances.

**Hands-on Activity:**

1. **Use For Loop with Resources:**
   - Create a new file `loops-for-resources.tf`.
   - Add the code to use a `for_each` loop with resources.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use For Loop with Outputs:**
   - Create a new file `loops-for-outputs.tf`.
   - Add the code to use a `for` loop in the output block.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Use Count Loop with Resources:**
   - Create a new file `loops-count-resources.tf`.
   - Add the code to use a `count` loop with resources.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 9. Dynamic Blocks

**Introduction to Dynamic Blocks**

**What are Dynamic Blocks?**
- Dynamic blocks in Terraform allow you to generate nested blocks dynamically within a resource or module.

**Why Use Dynamic Blocks?**
- Dynamic blocks enable you to create more flexible and reusable configurations by dynamically generating nested blocks based on input variables.

**Sample Code and Explanation:**

1. **Dynamic Block with Security Groups:**

   ```hcl
   # dynamic-block-sg.tf
   variable "ingress_rules" {
     description = "Ingress rules for the security group"
     type = list(object({
       cidr_blocks = list(string)
       from_port   = number
       to_port     = number
       protocol    = string
     }))
     default = [
       {
         cidr_blocks = ["0.0.0.0/0"]
         from_port   = 80
         to_port     = 80
         protocol    = "tcp"
       },
       {
         cidr_blocks = ["0.0.0.0/0"]
         from_port   = 443
         to_port     = 443
         protocol    = "tcp"
       }
     ]
   }

   resource "aws_security_group" "example" {
     name        = "example-sg"
     description = "Example security group"

     dynamic "ingress" {
       for_each = var.ingress_rules
       content {
         cidr_blocks = ingress.value.cidr_blocks
         from_port   = ingress.value.from_port
         to_port     = ingress.value.to_port
         protocol    = ingress.value.protocol
       }
     }
   }
   ```

   - This configuration uses a dynamic block to create ingress rules for a

 security group based on a list of rules.

2. **Dynamic Block with Tags:**

   ```hcl
   # dynamic-block-tags.tf
   variable "tags" {
     description = "Tags for the resources"
     type = map(string)
     default = {
       Name = "example-instance"
       Environment = "dev"
     }
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     dynamic "tag" {
       for_each = var.tags
       content {
         key   = tag.key
         value = tag.value
       }
     }
   }
   ```

   - This configuration uses a dynamic block to create tags for an EC2 instance based on a map of tags.

3. **Dynamic Block with IAM Policies:**

   ```hcl
   # dynamic-block-iam.tf
   variable "policies" {
     description = "IAM policies for the role"
     type = list(string)
     default = ["arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess", "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"]
   }

   resource "aws_iam_role" "example" {
     name = "example-role"

     assume_role_policy = jsonencode({
       Version = "2012-10-17"
       Statement = [{
         Effect = "Allow"
         Principal = {
           Service = "ec2.amazonaws.com"
         }
         Action = "sts:AssumeRole"
       }]
     })
   }

   resource "aws_iam_role_policy_attachment" "example" {
     for_each = toset(var.policies)
     role     = aws_iam_role.example.name
     policy_arn = each.value
   }
   ```

   - This configuration uses a dynamic block to attach multiple IAM policies to a role based on a list of policy ARNs.

**Hands-on Activity:**

1. **Use Dynamic Block with Security Groups:**
   - Create a new file `dynamic-block-sg.tf`.
   - Add the code to use a dynamic block with security groups.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use Dynamic Block with Tags:**
   - Create a new file `dynamic-block-tags.tf`.
   - Add the code to use a dynamic block with tags.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Use Dynamic Block with IAM Policies:**
   - Create a new file `dynamic-block-iam.tf`.
   - Add the code to use a dynamic block with IAM policies.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 10. Modules

**Introduction to Modules**

**What are Modules?**
- Modules in Terraform are self-contained packages of Terraform configurations that are managed as a group.

**Why Use Modules?**
- Modules help you organize and reuse your Terraform code, making it easier to manage and maintain complex infrastructures.

**Sample Code and Explanation:**

1. **Simple Module:**

   ```hcl
   # modules/simple-instance/main.tf
   variable "instance_name" {
     description = "Name of the instance"
     type        = string
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     tags = {
       Name = var.instance_name
     }
   }

   # modules/simple-instance/variables.tf
   variable "instance_name" {
     description = "Name of the instance"
     type        = string
   }

   # modules/simple-instance/outputs.tf
   output "instance_id" {
     value = aws_instance.example.id
   }

   # root module
   module "example_instance" {
     source        = "./modules/simple-instance"
     instance_name = "example-instance"
   }
   ```

   - This example defines a simple module to create an EC2 instance and uses it in the root module.

2. **Complex Module:**

   ```hcl
   # modules/network/main.tf
   variable "vpc_cidr" {
     description = "CIDR block for the VPC"
     type        = string
   }

   resource "aws_vpc" "example" {
     cidr_block = var.vpc_cidr
   }

   resource "aws_subnet" "example" {
     vpc_id            = aws_vpc.example.id
     cidr_block        = cidrsubnet(var.vpc_cidr, 8, 1)
     availability_zone = "us-west-2a"
   }

   # modules/network/variables.tf
   variable "vpc_cidr" {
     description = "CIDR block for the VPC"
     type        = string
   }

   # modules/network/outputs.tf
   output "vpc_id" {
     value = aws_vpc.example.id
   }

   output "subnet_id" {
     value = aws_subnet.example.id
   }

   # root module
   module "example_network" {
     source   = "./modules/network"
     vpc_cidr = "10.0.0.0/16"
   }
   ```

   - This example defines a more complex module to create a VPC and subnet and uses it in the root module.

3. **Module with Multiple Resources:**

   ```hcl
   # modules/complete-infrastructure/main.tf
   variable "vpc_cidr" {
     description = "CIDR block for the VPC"
     type        = string
   }

   variable "instance_name" {
     description = "Name of the instance"
     type        = string
   }

   resource "aws_vpc" "example" {
     cidr_block = var.vpc_cidr
   }

   resource "aws_subnet" "example" {
     vpc_id            = aws_vpc.example.id
     cidr_block        = cidrsubnet(var.vpc_cidr, 8, 1)
     availability_zone = "us-west-2a"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
     subnet_id     = aws_subnet.example.id

     tags = {
       Name = var.instance_name
     }
   }

   # modules/complete-infrastructure/variables.tf
   variable "vpc_cidr" {
     description = "CIDR block for the VPC"
     type        = string
   }

   variable "instance_name" {
     description = "Name of the instance"
     type        = string
   }

   # modules/complete-infrastructure/outputs.tf
   output "vpc_id" {
     value = aws_vpc.example.id
   }

   output "subnet_id" {
     value = aws_subnet.example.id
   }

   output "instance_id" {
     value = aws_instance.example.id
   }

   # root module
   module "example_infrastructure" {
     source        = "./modules/complete-infrastructure"
     vpc_cidr      = "10.0.0.0/16"
     instance_name = "example-instance"
   }
   ```

   - This example defines a module to create a complete infrastructure with a VPC, subnet, and EC2 instance, and uses it in the root module.

**Hands-on Activity:**

1. **Create a Simple Module:**
   - Create a new directory `modules/simple-instance`.
   - Add the code to define the simple module.
   - Use the module in the root module.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Create a Complex Module:**
   - Create a new directory `modules/network`.
   - Add the code to define the complex module.
   - Use the module in the root module.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Create a Module with Multiple Resources:**
   - Create a new directory `modules/complete-infrastructure`.
   - Add the code to define the module with multiple resources.
   - Use the module in the root module.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 11. Best Practices

**Introduction to Best Practices**

**Why Follow Best Practices?**
- Following best practices helps ensure your Terraform configurations are reliable, maintainable, and scalable.

**Best Practices:**

1. **Use Modules:**
   - Organize your configurations into reusable modules.

2. **Use Version Control:**
   - Use version control to manage your Terraform configurations.

3. **Use Variables:**
   - Use variables to parameterize your configurations and avoid hardcoding values.

4. **Use State Locking:**
   - Enable state locking to prevent concurrent state modifications.

5. **Use Remote State:**
   - Use remote state to share the state file across team members.

6. **Use Workspaces:**
   - Use workspaces to manage multiple environments (e.g., dev, staging, prod).

7. **Use Resource Tags:**
   - Use tags to organize and manage your resources.

8. **Use Outputs:**
   - Use outputs to expose information about your resources.

9. **Use Provisioners Sparingly:**
   - Use provisioners only when necessary and prefer configuration management tools for complex provisioning.

10. **Test and Validate Configurations:**
    - Test and validate your configurations before applying them to production.

**Sample

 Code and Explanation:**

1. **Using Modules:**

   ```hcl
   module "example_instance" {
     source        = "./modules/simple-instance"
     instance_name = "example-instance"
   }
   ```

2. **Using Variables:**

   ```hcl
   variable "instance_type" {
     description = "Type of instance to use"
     type        = string
     default     = "t2.micro"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = var.instance_type

     tags = {
       Name = "example-instance"
     }
   }
   ```

3. **Using Remote State:**

   ```hcl
   terraform {
     backend "s3" {
       bucket = "my-terraform-state"
       key    = "path/to/my/state"
       region = "us-west-2"
     }
   }
   ```

4. **Using Workspaces:**

   ```sh
   terraform workspace new dev
   terraform workspace new staging
   terraform workspace new prod
   ```

5. **Using Resource Tags:**

   ```hcl
   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     tags = {
       Name        = "example-instance"
       Environment = "dev"
     }
   }
   ```

6. **Using Outputs:**

   ```hcl
   output "instance_id" {
     value = aws_instance.example.id
   }
   ```

**Hands-on Activity:**

1. **Use Modules:**
   - Organize your existing configurations into modules.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Use Variables:**
   - Replace hardcoded values in your configurations with variables.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Use Remote State:**
   - Configure remote state for your configurations.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Use Workspaces:**
   - Create and switch between workspaces for different environments.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Use Resource Tags:**
   - Add tags to your resources.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

6. **Use Outputs:**
   - Add output blocks to expose information about your resources.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 12. Project

**Introduction to the Project**

**What is the Project?**
- The project is a hands-on exercise that allows you to apply the concepts and skills you have learned in the training to create a real-world Terraform configuration.

**Project Overview:**
- You will create a Terraform configuration to deploy a complete infrastructure on AWS, including a VPC, subnets, security groups, EC2 instances, and an RDS database.

**Project Requirements:**

1. **Create a VPC:**
   - Create a VPC with a specified CIDR block.
   - Create public and private subnets in different availability zones.

2. **Create Security Groups:**
   - Create security groups for the EC2 instances and RDS database.
   - Define ingress and egress rules for the security groups.

3. **Create EC2 Instances:**
   - Create EC2 instances in the public subnets.
   - Use a dynamic block to assign security groups and tags to the instances.

4. **Create an RDS Database:**
   - Create an RDS database in the private subnets.
   - Use a dynamic block to assign security groups and tags to the database.

5. **Use Modules:**
   - Organize your configurations into reusable modules.

6. **Use Variables and Outputs:**
   - Use variables to parameterize your configurations.
   - Use outputs to expose information about your resources.

**Sample Code and Explanation:**

1. **Create a VPC:**

   ```hcl
   # modules/network/main.tf
   variable "vpc_cidr" {
     description = "CIDR block for the VPC"
     type        = string
   }

   resource "aws_vpc" "example" {
     cidr_block = var.vpc_cidr
   }

   resource "aws_subnet" "public" {
     vpc_id            = aws_vpc.example.id
     cidr_block        = cidrsubnet(var.vpc_cidr, 8, 1)
     availability_zone = "us-west-2a"
     map_public_ip_on_launch = true
   }

   resource "aws_subnet" "private" {
     vpc_id            = aws_vpc.example.id
     cidr_block        = cidrsubnet(var.vpc_cidr, 8, 2)
     availability_zone = "us-west-2b"
   }

   # modules/network/variables.tf
   variable "vpc_cidr" {
     description = "CIDR block for the VPC"
     type        = string
   }

   # modules/network/outputs.tf
   output "vpc_id" {
     value = aws_vpc.example.id
   }

   output "public_subnet_id" {
     value = aws_subnet.public.id
   }

   output "private_subnet_id" {
     value = aws_subnet.private.id
   }

   # root module
   module "network" {
     source   = "./modules/network"
     vpc_cidr = "10.0.0.0/16"
   }
   ```

2. **Create Security Groups:**

   ```hcl
   # modules/security/main.tf
   variable "ingress_rules" {
     description = "Ingress rules for the security group"
     type = list(object({
       cidr_blocks = list(string)
       from_port   = number
       to_port     = number
       protocol    = string
     }))
   }

   resource "aws_security_group" "example" {
     name        = "example-sg"
     description = "Example security group"

     dynamic "ingress" {
       for_each = var.ingress_rules
       content {
         cidr_blocks = ingress.value.cidr_blocks
         from_port   = ingress.value.from_port
         to_port     = ingress.value.to_port
         protocol    = ingress.value.protocol
       }
     }

     egress {
       cidr_blocks = ["0.0.0.0/0"]
       from_port   = 0
       to_port     = 0
       protocol    = "-1"
     }
   }

   # modules/security/variables.tf
   variable "ingress_rules" {
     description = "Ingress rules for the security group"
     type = list(object({
       cidr_blocks = list(string)
       from_port   = number
       to_port     = number
       protocol    = string
     }))
   }

   # modules/security/outputs.tf
   output "sg_id" {
     value = aws_security_group.example.id
   }

   # root module
   module "security" {
     source        = "./modules/security"
     ingress_rules = [
       {
         cidr_blocks = ["0.0.0.0/0"]
         from_port   = 80
         to_port     = 80
         protocol    = "tcp"
       },
       {
         cidr_blocks = ["0.0.0.0/0"]
         from_port   = 443
         to_port     = 443
         protocol    = "tcp"
       }
     ]
   }
   ```

3. **Create EC2 Instances:**

   ```hcl
   # modules/ec2/main.tf
   variable "instance_name" {
     description = "Name of the instance"
     type        = string
   }

   variable "subnet_id" {
     description = "ID of the subnet"
     type        = string
   }

   variable "sg_id" {
     description = "ID of the security group"
     type        = string
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
     subnet_id     = var.subnet_id

     tags = {
       Name = var.instance_name
     }

     vpc_security_group_ids = [var.sg_id]
   }

   # modules/ec2/variables.tf
   variable "instance_name" {
     description = "Name of the instance"
     type        = string
   }

   variable "subnet_id" {
     description = "ID of the subnet"
     type        = string
   }

   variable "sg_id" {
     description = "ID of the security group"
     type        = string
   }

   # modules/ec2/outputs.tf
   output "instance_id" {
     value = aws_instance.example.id
   }

   # root module
   module "ec2_instance" {
     source        = "./modules/ec2"
     instance_name = "example-instance"
     subnet_id     = module.network.public_subnet_id
     sg_id         = module.security.sg_id
   }
   ```

4. **Create an RDS Database:**

   ```hcl
   # modules/rds/main.tf
   variable "db_name" {
     description = "Name of the database"
     type        = string
   }

   variable "subnet_ids" {
     description = "IDs of

 the subnets"
     type        = list(string)
   }

   variable "sg_id" {
     description = "ID of the security group"
     type        = string
   }

   resource "aws_db_instance" "example" {
     identifier              = var.db_name
     engine                  = "mysql"
     instance_class          = "db.t2.micro"
     allocated_storage       = 20
     name                    = var.db_name
     username                = "admin"
     password                = "password"
     vpc_security_group_ids  = [var.sg_id]
     db_subnet_group_name    = aws_db_subnet_group.example.name
     skip_final_snapshot     = true
   }

   resource "aws_db_subnet_group" "example" {
     name       = "example-db-subnet-group"
     subnet_ids = var.subnet_ids
   }

   # modules/rds/variables.tf
   variable "db_name" {
     description = "Name of the database"
     type        = string
   }

   variable "subnet_ids" {
     description = "IDs of the subnets"
     type        = list(string)
   }

   variable "sg_id" {
     description = "ID of the security group"
     type        = string
   }

   # modules/rds/outputs.tf
   output "db_instance_id" {
     value = aws_db_instance.example.id
   }

   # root module
   module "rds" {
     source    = "./modules/rds"
     db_name   = "example-db"
     subnet_ids = [module.network.private_subnet_id]
     sg_id      = module.security.sg_id
   }
   ```

**Hands-on Activity:**

1. **Create a VPC:**
   - Create a new directory `modules/network`.
   - Add the code to define the VPC and subnets.
   - Use the module in the root module.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Create Security Groups:**
   - Create a new directory `modules/security`.
   - Add the code to define the security groups.
   - Use the module in the root module.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Create EC2 Instances:**
   - Create a new directory `modules/ec2`.
   - Add the code to define the EC2 instances.
   - Use the module in the root module.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Create an RDS Database:**
   - Create a new directory `modules/rds`.
   - Add the code to define the RDS database.
   - Use the module in the root module.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Use Variables and Outputs:**
   - Replace hardcoded values in your configurations with variables.
   - Add output blocks to expose information about your resources.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

### 13. Conclusion

**Wrap-up and Next Steps**

**Conclusion:**
- Congratulations on completing the Terraform training! You have learned how to use Terraform to manage and provision your infrastructure, and have gained hands-on experience with various Terraform concepts and features.

**Next Steps:**
- Continue exploring Terraform documentation and community resources.
- Practice by creating more complex configurations and projects.
- Consider obtaining a Terraform certification to validate your skills and knowledge.

---

### 14. Feedback

**We Value Your Feedback:**
- Please take a moment to provide feedback on the training. Your feedback is important to us and helps us improve future training sessions.

**Feedback Form:**
- [Link to Feedback Form]
