


### Day 2: Providers, Resources, and Loops in Terraform

---

#### 2. Providers

**Introduction to Providers**

**What are Providers?**
- Providers in Terraform are plugins that allow you to interact with various APIs and cloud services. They are responsible for understanding the API interactions and exposing resources.

**Why Use Providers?**
- To manage infrastructure across different platforms and services.

**Common Providers:**
- AWS
- Azure
- Google Cloud Platform
- Kubernetes
- GitHub

**Sample Code and Explanation:**

1. **AWS Provider Configuration:**

   ```hcl
   # provider-aws.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

   - This configuration sets up the AWS provider and creates an EC2 instance.

2. **Azure Provider Configuration:**

   ```hcl
   # provider-azure.tf
   provider "azurerm" {
     features {}
   }

   resource "azurerm_resource_group" "example" {
     name     = "example-resources"
     location = "West US"
   }

   resource "azurerm_virtual_network" "example" {
     name                = "example-network"
     address_space       = ["10.0.0.0/16"]
     location            = azurerm_resource_group.example.location
     resource_group_name = azurerm_resource_group.example.name
   }
   ```

   - This configuration sets up the Azure provider and creates a resource group and a virtual network.

3. **Google Cloud Provider Configuration:**

   ```hcl
   # provider-gcp.tf
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

   - This configuration sets up the Google Cloud provider and creates a compute instance.

4. **Kubernetes Provider Configuration:**

   ```hcl
   # provider-k8s.tf
   provider "kubernetes" {
     config_path = "~/.kube/config"
   }

   resource "kubernetes_namespace" "example" {
     metadata {
       name = "example-namespace"
     }
   }
   ```

   - This configuration sets up the Kubernetes provider and creates a namespace.

5. **GitHub Provider Configuration:**

   ```hcl
   # provider-github.tf
   provider "github" {
     token = "my-github-token"
   }

   resource "github_repository" "example" {
     name        = "example-repo"
     description = "Example repository"
     visibility  = "public"
   }
   ```

   - This configuration sets up the GitHub provider and creates a repository.

**Hands-on Activity:**

1. **Set Up AWS Provider:**
   - Create a new file `provider-aws.tf`.
   - Add the code to set up the AWS provider and create an EC2 instance.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Set Up Azure Provider:**
   - Create a new file `provider-azure.tf`.
   - Add the code to set up the Azure provider and create a resource group and virtual network.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Set Up Google Cloud Provider:**
   - Create a new file `provider-gcp.tf`.
   - Add the code to set up the Google Cloud provider and create a compute instance.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Set Up Kubernetes Provider:**
   - Create a new file `provider-k8s.tf`.
   - Add the code to set up the Kubernetes provider and create a namespace.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Set Up GitHub Provider:**
   - Create a new file `provider-github.tf`.
   - Add the code to set up the GitHub provider and create a repository.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

#### 3. Resources

**Introduction to Resources**

**What are Resources?**
- Resources are the fundamental building blocks in Terraform that define infrastructure objects.

**Commonly Used Resources:**
- AWS EC2 Instances
- Azure Virtual Machines
- Google Cloud Storage Buckets
- Kubernetes Deployments
- GitHub Repositories

**Sample Code and Explanation:**

1. **AWS EC2 Instance:**

   ```hcl
   # resource-aws-instance.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

   - This configuration creates an EC2 instance in AWS.

2. **Azure Virtual Machine:**

   ```hcl
   # resource-azure-vm.tf
   provider "azurerm" {
     features {}
   }

   resource "azurerm_resource_group" "example" {
     name     = "example-resources"
     location = "West US"
   }

   resource "azurerm_virtual_network" "example" {
     name                = "example-network"
     address_space       = ["10.0.0.0/16"]
     location            = azurerm_resource_group.example.location
     resource_group_name = azurerm_resource_group.example.name
   }

   resource "azurerm_network_interface" "example" {
     name                = "example-nic"
     location            = azurerm_resource_group.example.location
     resource_group_name = azurerm_resource_group.example.name

     ip_configuration {
       name                          = "internal"
       subnet_id                     = azurerm_subnet.example.id
       private_ip_address_allocation = "Dynamic"
     }
   }

   resource "azurerm_linux_virtual_machine" "example" {
     name                = "example-vm"
     resource_group_name = azurerm_resource_group.example.name
     location            = azurerm_resource_group.example.location
     size                = "Standard_F2"
     admin_username      = "adminuser"
     network_interface_ids = [
       azurerm_network_interface.example.id,
     ]

     os_disk {
       caching              = "ReadWrite"
       storage_account_type = "Standard_LRS"
     }

     source_image_reference {
       publisher = "Canonical"
       offer     = "UbuntuServer"
       sku       = "18.04-LTS"
       version   = "latest"
     }
   }
   ```

   - This configuration creates a resource group, virtual network, and a Linux virtual machine in Azure.

3. **Google Cloud Storage Bucket:**

   ```hcl
   # resource-gcp-bucket.tf
   provider "google" {
     project = "my-project-id"
     region  = "us-central1"
   }

   resource "google_storage_bucket" "example" {
     name     = "my-example-bucket"
     location = "US"
   }
   ```

   - This configuration creates a storage bucket in Google Cloud.

4. **Kubernetes Deployment:**

   ```hcl
   # resource-k8s-deployment.tf
   provider "kubernetes" {
     config_path = "~/.kube/config"
   }

   resource "kubernetes_deployment" "example" {
     metadata {
       name = "example-deployment"
     }

     spec {
       replicas = 2

       selector {
         match_labels = {
           App = "example"
         }
       }

       template {
         metadata {
           labels = {
             App = "example"
           }
         }

         spec {
           container {
             image = "nginx:1.14.2"
             name  = "example"

             port {
               container_port = 80
             }
           }
         }
       }
     }
   }
   ```

   - This configuration creates a deployment in Kubernetes.

5. **GitHub Repository:**

   ```hcl
   # resource-github-repo.tf
   provider "github" {
     token = "my-github-token"
   }

   resource "github_repository" "example" {
     name        = "example-repo"
     description = "Example repository"
     visibility  = "public"
   }
   ```

   - This configuration creates a repository in GitHub.

**Hands-on Activity:**

1. **Create an AWS EC2 Instance:**
   - Create a new file `resource-aws-instance.tf`.
   - Add the code to create an EC2 instance.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Create an Azure Virtual Machine:**
   - Create a new file `resource-azure-vm.tf`.
   - Add the code to create a resource group, virtual network, and a Linux virtual machine.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Create a Google Cloud Storage Bucket:**
   - Create a new file `resource-gcp-bucket.tf`.
   - Add the

 code to create a storage bucket.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Create a Kubernetes Deployment:**
   - Create a new file `resource-k8s-deployment.tf`.
   - Add the code to create a deployment.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Create a GitHub Repository:**
   - Create a new file `resource-github-repo.tf`.
   - Add the code to create a repository.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

#### Loops in Terraform

**Introduction to Loops**

**Why Use Loops?**
- To simplify the creation of multiple similar resources without redundant code.

**Types of Loops:**
- `count`: Creates multiple instances of a resource.
- `for_each`: Iterates over a map or set.
- `for`: Generates lists and maps dynamically.

**Sample Code and Explanation:**

1. **Using `count` to Create Multiple Instances:**

   ```hcl
   # loops-count.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     count         = 3
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     tags = {
       Name = "example-instance-${count.index}"
     }
   }
   ```

   - This configuration creates three EC2 instances using the `count` parameter.

2. **Using `for_each` to Iterate Over a Map:**

   ```hcl
   # loops-for-each.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     for_each = {
       instance1 = "t2.micro"
       instance2 = "t2.small"
       instance3 = "t2.medium"
     }

     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = each.value

     tags = {
       Name = "example-instance-${each.key}"
     }
   }
   ```

   - This configuration creates EC2 instances with different instance types using `for_each`.

3. **Using `for` to Generate a List:**

   ```hcl
   # loops-for-list.tf
   variable "instance_types" {
     description = "List of instance types"
     type        = list(string)
     default     = ["t2.micro", "t2.small", "t2.medium"]
   }

   locals {
     instance_tags = [for type in var.instance_types : "${type}-instance"]
   }

   output "instance_tags" {
     value = local.instance_tags
   }
   ```

   - This configuration generates a list of instance tags using `for`.

4. **Using `for_each` with a Set:**

   ```hcl
   # loops-for-each-set.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     for_each = toset(["t2.micro", "t2.small", "t2.medium"])

     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = each.value

     tags = {
       Name = "example-instance-${each.value}"
     }
   }
   ```

   - This configuration creates EC2 instances with different instance types using `for_each` with a set.

5. **Using `count` with Conditionals:**

   ```hcl
   # loops-count-conditionals.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     count         = var.create_instances ? 3 : 0
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     tags = {
       Name = "example-instance-${count.index}"
     }
   }

   variable "create_instances" {
     description = "Boolean to create instances"
     type        = bool
     default     = true
   }
   ```

   - This configuration conditionally creates three EC2 instances based on a variable.

**Hands-on Activity:**

1. **Create Multiple Instances with `count`:**
   - Create a new file `loops-count.tf`.
   - Add the code to create multiple EC2 instances using `count`.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Iterate Over a Map with `for_each`:**
   - Create a new file `loops-for-each.tf`.
   - Add the code to create EC2 instances with different instance types using `for_each`.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

3. **Generate a List with `for`:**
   - Create a new file `loops-for-list.tf`.
   - Add the code to generate a list of instance tags using `for`.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

4. **Iterate Over a Set with `for_each`:**
   - Create a new file `loops-for-each-set.tf`.
   - Add the code to create EC2 instances with different instance types using `for_each` with a set.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

5. **Use `count` with Conditionals:**
   - Create a new file `loops-count-conditionals.tf`.
   - Add the code to conditionally create EC2 instances based on a variable.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

---

Detailed plan for Day 2, you will gain a comprehensive understanding of Providers, Resources, and Loops in Terraform, along with hands-on experience in implementing these concepts.




Detailed breakdown for Day 3, covering Data Sources, Provisioners, and Functions and Expressions, with step-by-step instructions and multiple code examples.

### Day 3: Data Sources, Provisioners, Functions and Expressions

#### 7. Data Sources

**Introduction to Data Sources**
- **What are Data Sources?**
  - Data sources allow you to fetch data from providers (e.g., AWS, Azure) that you can use to configure your resources.
  
- **Why Use Data Sources?**
  - To dynamically fetch information (e.g., the latest AMI ID, existing resources).
  - To avoid hardcoding values in your configurations.

**Sample Code and Explanation:**

1. **Fetching the Latest AMI ID:**
   ```hcl
   # data-source-ami.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_ami" "latest" {
     most_recent = true
     owners      = ["self"]

     filter {
       name   = "name"
       values = ["my-ami-*"]
     }
   }

   resource "aws_instance" "example" {
     ami           = data.aws_ami.latest.id
     instance_type = "t2.micro"
   }
   ```

   - This configuration fetches the latest AMI ID and uses it to launch an EC2 instance.

2. **Fetching VPC Information:**
   ```hcl
   # data-source-vpc.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_vpc" "default" {
     default = true
   }

   resource "aws_subnet" "example" {
     vpc_id            = data.aws_vpc.default.id
     cidr_block        = "10.0.1.0/24"
     availability_zone = "us-west-2a"
   }
   ```

   - This configuration fetches the default VPC ID and uses it to create a subnet.

**Hands-on Activity:**
1. **Add a Data Source to Fetch the Latest AMI:**
   - Create a new file `data-source-ami.tf`.
   - Add the code to fetch the latest AMI and use it in an EC2 instance.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Add a Data Source to Fetch VPC Information:**
   - Create a new file `data-source-vpc.tf`.
   - Add the code to fetch the default VPC ID and use it in a subnet resource.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

#### 8. Provisioners

**Introduction to Provisioners**
- **What are Provisioners?**
  - Provisioners are used to execute scripts on a local or remote machine as part of the resource creation or destruction process.
  
- **Types of Provisioners:**
  - `local-exec`: Executes a command locally.
  - `remote-exec`: Executes a command on a remote resource.

**Sample Code and Explanation:**

1. **Using `local-exec` Provisioner:**
   ```hcl
   # provisioner-local-exec.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     provisioner "local-exec" {
       command = "echo ${self.public_ip} > ip_address.txt"
     }
   }
   ```

   - This configuration uses `local-exec` to write the public IP address of the instance to a local file.

2. **Using `remote-exec` Provisioner:**
   ```hcl
   # provisioner-remote-exec.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     connection {
       type        = "ssh"
       user        = "ec2-user"
       private_key = file("~/.ssh/id_rsa")
       host        = self.public_ip
     }

     provisioner "remote-exec" {
       inline = [
         "sudo apt-get update",
         "sudo apt-get install -y nginx",
       ]
     }
   }
   ```

   - This configuration uses `remote-exec` to install NGINX on the EC2 instance.

**Hands-on Activity:**
1. **Add a `local-exec` Provisioner:**
   - Create a new file `provisioner-local-exec.tf`.
   - Add the code to use `local-exec` provisioner.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

2. **Add a `remote-exec` Provisioner:**
   - Create a new file `provisioner-remote-exec.tf`.
   - Add the code to use `remote-exec` provisioner.
   - Ensure you have an SSH key set up and accessible.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

#### 9. Functions and Expressions

**Introduction to Functions and Expressions**
- **What are Functions and Expressions?**
  - Functions and expressions in Terraform allow you to perform operations on your data.
  - Commonly used functions include: `length`, `file`, `lookup`, `concat`, `element`.

**Sample Code and Explanation:**

1. **Using the `length` Function:**
   ```hcl
   # functions-length.tf
   variable "instance_types" {
     description = "List of instance types"
     type        = list(string)
     default     = ["t2.micro", "t2.small", "t2.medium"]
   }

   output "instance_type_count" {
     value = length(var.instance_types)
   }
   ```

   - This configuration calculates the length of a list variable.

2. **Using the `file` Function:**
   ```hcl
   # functions-file.tf
   output "file_content" {
     value = file("example.txt")
   }
   ```

   - This configuration reads the content of a local file and outputs it.

3. **Using the `lookup` Function:**
   ```hcl
   # functions-lookup.tf
   variable "instance_tags" {
     description = "Map of instance tags"
     type        = map(string)
     default     = {
       Name = "example-instance"
       Env  = "dev"
     }
   }

   output "instance_name" {
     value = lookup(var.instance_tags, "Name", "default-name")
   }
   ```

   - This configuration looks up a value in a map variable.

4. **Using the `concat` Function:**
   ```hcl
   # functions-concat.tf
   variable "list1" {
     description = "First list"
     type        = list(string)
     default     = ["a", "b"]
   }

   variable "list2" {
     description = "Second list"
     type        = list(string)
     default     = ["c", "d"]
   }

   output "combined_list" {
     value = concat(var.list1, var.list2)
   }
   ```

   - This configuration concatenates two lists.

5. **Using the `element` Function:**
   ```hcl
   # functions-element.tf
   variable "instance_types" {
     description = "List of instance types"
     type        = list(string)
     default     = ["t2.micro", "t2.small", "t2.medium"]
   }

   output "second_instance_type" {
     value = element(var.instance_types, 1)
   }
   ```

   - This configuration retrieves an element from a list by index.

**Hands-on Activity:**
1. **Use the `length` Function:**
   - Create a new file `functions-length.tf`.
   - Add the code to use the `length` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

2. **Use the `file` Function:**
   - Create a new file `functions-file.tf`.
   - Create a local file `example.txt` with some content.
   - Add the code to use the `file` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

3. **Use the `lookup` Function:**
   - Create a new file `functions-lookup.tf`.
   - Add the code to use the `lookup` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

4. **Use the `concat` Function:**
   - Create a new file `functions-concat.tf`.
   - Add the code to use the `concat` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

5. **Use the `element` Function:**
   - Create a new file `functions-element.tf`.
   - Add the code to use the `element` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

detailed plan for Day 3, you will gain a deep understanding of Data Sources, Provisioners, and Functions and Expressions in Terraform, along with hands-on experience in implementing these concepts.

--------------
Great! Here’s a comprehensive and detailed breakdown for Day 3, covering Data Sources, Provisioners, and Functions and Expressions with step-by-step instructions and code examples.

### Day 3: Data Sources, Provisioners, Functions, and Expressions

---

#### 7. Data Sources

**Introduction to Data Sources**

**What are Data Sources?**
- Data sources allow you to fetch data from providers (e.g., AWS, Azure) which can be used to configure your resources dynamically.
  
**Why Use Data Sources?**
- To dynamically fetch information like the latest AMI ID or existing resources.
- To avoid hardcoding values in your configurations.

**Sample Code and Explanation:**

1. **Fetching the Latest AMI ID:**

   ```hcl
   # data-source-ami.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_ami" "latest" {
     most_recent = true
     owners      = ["self"]

     filter {
       name   = "name"
       values = ["my-ami-*"]
     }
   }

   resource "aws_instance" "example" {
     ami           = data.aws_ami.latest.id
     instance_type = "t2.micro"
   }
   ```

   - This configuration fetches the latest AMI ID and uses it to launch an EC2 instance.

2. **Fetching VPC Information:**

   ```hcl
   # data-source-vpc.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_vpc" "default" {
     default = true
   }

   resource "aws_subnet" "example" {
     vpc_id            = data.aws_vpc.default.id
     cidr_block        = "10.0.1.0/24"
     availability_zone = "us-west-2a"
   }
   ```

   - This configuration fetches the default VPC ID and uses it to create a subnet.

3. **Fetching AWS S3 Bucket Information:**

   ```hcl
   # data-source-s3.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_s3_bucket" "example" {
     bucket = "my-bucket"
   }

   output "s3_bucket_arn" {
     value = data.aws_s3_bucket.example.arn
   }
   ```

   - This configuration fetches information about an S3 bucket and outputs its ARN.

4. **Fetching AWS IAM User Information:**

   ```hcl
   # data-source-iam-user.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_iam_user" "example" {
     user_name = "my-user"
   }

   output "iam_user_arn" {
     value = data.aws_iam_user.example.arn
   }
   ```

   - This configuration fetches information about an IAM user and outputs its ARN.

5. **Fetching AWS Availability Zones:**

   ```hcl
   # data-source-availability-zones.tf
   provider "aws" {
     region = "us-west-2"
   }

   data "aws_availability_zones" "available" {}

   output "availability_zones" {
     value = data.aws_availability_zones.available.names
   }
   ```

   - This configuration fetches a list of available availability zones and outputs their names.

**Hands-on Activity:**

1. **Add a Data Source to Fetch the Latest AMI:**
   - Create a new file `data-source-ami.tf`.
   - Add the code to fetch the latest AMI and use it in an EC2 instance.
   - Apply the configuration:
     ```sh
     terraform init
     terraform apply
     ```

2. **Add a Data Source to Fetch VPC Information:**
   - Create a new file `data-source-vpc.tf`.
   - Add the code to fetch the default VPC ID and use it in a subnet resource.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

3. **Add a Data Source to Fetch S3 Bucket Information:**
   - Create a new file `data-source-s3.tf`.
   - Add the code to fetch S3 bucket information and output its ARN.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

4. **Add a Data Source to Fetch IAM User Information:**
   - Create a new file `data-source-iam-user.tf`.
   - Add the code to fetch IAM user information and output its ARN.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

5. **Add a Data Source to Fetch Availability Zones:**
   - Create a new file `data-source-availability-zones.tf`.
   - Add the code to fetch availability zones and output their names.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

---

#### 8. Provisioners

**Introduction to Provisioners**

**What are Provisioners?**
- Provisioners are used to execute scripts on a local or remote machine as part of the resource creation or destruction process.

**Types of Provisioners:**
- `local-exec`: Executes a command locally.
- `remote-exec`: Executes a command on a remote resource.

**Sample Code and Explanation:**

1. **Using `local-exec` Provisioner:**

   ```hcl
   # provisioner-local-exec.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     provisioner "local-exec" {
       command = "echo ${self.public_ip} > ip_address.txt"
     }
   }
   ```

   - This configuration uses `local-exec` to write the public IP address of the instance to a local file.

2. **Using `remote-exec` Provisioner:**

   ```hcl
   # provisioner-remote-exec.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     connection {
       type        = "ssh"
       user        = "ec2-user"
       private_key = file("~/.ssh/id_rsa")
       host        = self.public_ip
     }

     provisioner "remote-exec" {
       inline = [
         "sudo apt-get update",
         "sudo apt-get install -y nginx",
       ]
     }
   }
   ```

   - This configuration uses `remote-exec` to install NGINX on the EC2 instance.

3. **Using `file` Provisioner:**

   ```hcl
   # provisioner-file.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     connection {
       type        = "ssh"
       user        = "ec2-user"
       private_key = file("~/.ssh/id_rsa")
       host        = self.public_ip
     }

     provisioner "file" {
       source      = "index.html"
       destination = "/tmp/index.html"
     }
   }
   ```

   - This configuration uses `file` provisioner to upload a file to the EC2 instance.

4. **Combining `file` and `remote-exec` Provisioners:**

   ```hcl
   # provisioner-file-remote-exec.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"

     connection {
       type        = "ssh"
       user        = "ec2-user"
       private_key = file("~/.ssh/id_rsa")
       host        = self.public_ip
     }

     provisioner "file" {
       source      = "index.html"
       destination = "/tmp/index.html"
     }

     provisioner "remote-exec" {
       inline = [
         "sudo mv /tmp/index.html /var/www/html/index.html",
         "sudo systemctl restart nginx",
       ]
     }
   }
   ```

   - This configuration uploads a file to the EC2 instance and then moves it to the web server directory and restarts NGINX.

5. **Using `null_resource` with Provisioners:**

   ```hcl
   # provisioner-null-resource.tf
   provider "aws" {
     region = "us-west-2"
   }

   resource "null_resource" "example" {
     provisioner "local-exec" {
       command = "echo 'Provisioning completed' > provisioning.log"
     }
   }
   ```

   - This configuration uses a `null_resource` to execute a local command without creating any real infrastructure.

**Hands-on Activity:**

1. **Add a `local-exec` Provisioner:**
   - Create a new file `provisioner-local-exec.tf`.
   - Add the code to use `local-exec` provisioner.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

2. **Add a `remote-exec` Provisioner:**
   - Create a new file `provisioner-remote-exec

.tf`.
   - Add the code to use `remote-exec` provisioner.
   - Ensure you have an SSH key set up and accessible.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

3. **Add a `file` Provisioner:**
   - Create a new file `provisioner-file.tf`.
   - Create a local file `index.html` with some content.
   - Add the code to use `file` provisioner.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

4. **Combine `file` and `remote-exec` Provisioners:**
   - Create a new file `provisioner-file-remote-exec.tf`.
   - Add the code to use both `file` and `remote-exec` provisioners.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

5. **Use `null_resource` with Provisioners:**
   - Create a new file `provisioner-null-resource.tf`.
   - Add the code to use `null_resource` with a provisioner.
   - Apply the configuration:
     ```sh
     terraform apply
     ```

---

#### 9. Functions and Expressions

**Introduction to Functions and Expressions**

**What are Functions and Expressions?**
- Functions and expressions in Terraform allow you to perform operations on your data.

**Commonly Used Functions:**
- `length`: Returns the length of a list or string.
- `file`: Reads the contents of a file.
- `lookup`: Looks up a value in a map.
- `concat`: Concatenates multiple lists.
- `element`: Retrieves an element from a list by index.

**Sample Code and Explanation:**

1. **Using the `length` Function:**

   ```hcl
   # functions-length.tf
   variable "instance_types" {
     description = "List of instance types"
     type        = list(string)
     default     = ["t2.micro", "t2.small", "t2.medium"]
   }

   output "instance_type_count" {
     value = length(var.instance_types)
   }
   ```

   - This configuration calculates the length of a list variable.

2. **Using the `file` Function:**

   ```hcl
   # functions-file.tf
   output "file_content" {
     value = file("example.txt")
   }
   ```

   - This configuration reads the content of a local file and outputs it.

3. **Using the `lookup` Function:**

   ```hcl
   # functions-lookup.tf
   variable "instance_tags" {
     description = "Map of instance tags"
     type        = map(string)
     default     = {
       Name = "example-instance"
       Env  = "dev"
     }
   }

   output "instance_name" {
     value = lookup(var.instance_tags, "Name", "default-name")
   }
   ```

   - This configuration looks up a value in a map variable.

4. **Using the `concat` Function:**

   ```hcl
   # functions-concat.tf
   variable "list1" {
     description = "First list"
     type        = list(string)
     default     = ["a", "b"]
   }

   variable "list2" {
     description = "Second list"
     type        = list(string)
     default     = ["c", "d"]
   }

   output "combined_list" {
     value = concat(var.list1, var.list2)
   }
   ```

   - This configuration concatenates two lists.

5. **Using the `element` Function:**

   ```hcl
   # functions-element.tf
   variable "instance_types" {
     description = "List of instance types"
     type        = list(string)
     default     = ["t2.micro", "t2.small", "t2.medium"]
   }

   output "second_instance_type" {
     value = element(var.instance_types, 1)
   }
   ```

   - This configuration retrieves an element from a list by index.

**Hands-on Activity:**

1. **Use the `length` Function:**
   - Create a new file `functions-length.tf`.
   - Add the code to use the `length` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

2. **Use the `file` Function:**
   - Create a new file `functions-file.tf`.
   - Create a local file `example.txt` with some content.
   - Add the code to use the `file` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

3. **Use the `lookup` Function:**
   - Create a new file `functions-lookup.tf`.
   - Add the code to use the `lookup` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

4. **Use the `concat` Function:**
   - Create a new file `functions-concat.tf`.
   - Add the code to use the `concat` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

5. **Use the `element` Function:**
   - Create a new file `functions-element.tf`.
   - Add the code to use the `element` function.
   - Apply the configuration and observe the output:
     ```sh
     terraform apply
     ```

---

Detailed plan for Day 3, you will gain a deep understanding of Data Sources, Provisioners, and Functions and Expressions in Terraform, along with hands-on experience in implementing these concepts.

--------------



--------------

Detailed breakdown for Day 4 and Day 5, with sample codes and steps.

### Day 4: Terraform Cloud and Enterprise, Workspaces

#### 10. Terraform Cloud and Enterprise

**Introduction to Terraform Cloud**
- **Terraform Cloud Features:**
  - Remote Terraform execution
  - VCS integration
  - Private module registry
  - Collaboration and governance

**Setup and Configuration**
- **Step-by-Step Setup:**
  1. **Create a Terraform Cloud Account:**
     - Visit the [Terraform Cloud website](https://app.terraform.io/signup) and sign up for an account.
  2. **Create an Organization:**
     - Navigate to the "Organizations" section and create a new organization.
  3. **Create a Workspace:**
     - Inside your organization, create a new workspace. Connect this workspace to your version control system (VCS) repository containing your Terraform configuration files.
  4. **Configure Terraform Cloud in Local Environment:**
     - Install the Terraform CLI.
     - Set up Terraform Cloud credentials locally:
       ```sh
       terraform login
       ```
     - Initialize your configuration to use Terraform Cloud as the backend:
       ```hcl
       # main.tf
       terraform {
         backend "remote" {
           organization = "your-organization"

           workspaces {
             name = "your-workspace"
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
     - Initialize the configuration:
       ```sh
       terraform init
       ```

**Hands-on Activity:**
1. **Set Up a Terraform Cloud Workspace:**
   - Follow the steps above to create an account, organization, and workspace.
2. **Push Configuration to VCS:**
   - Ensure your configuration files are committed and pushed to the connected VCS repository.
3. **Trigger a Run in Terraform Cloud:**
   - Navigate to your workspace in Terraform Cloud.
   - Trigger a run by clicking the "Queue plan" button.
   - Review and apply the plan.

#### 11. Workspaces

**Introduction to Workspaces**
- **Concept of Workspaces:**
  - Workspaces allow you to manage multiple environments (e.g., dev, staging, prod) with the same configuration.
  - Each workspace has its own state.

**Commands:**
- List all workspaces:
  ```sh
  terraform workspace list
  ```
- Create a new workspace:
  ```sh
  terraform workspace new dev
  ```
- Select a workspace:
  ```sh
  terraform workspace select dev
  ```

**Sample Configuration:**
```hcl
# main.tf
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

**Hands-on Activity:**
1. **Create Workspaces:**
   - Create new workspaces for different environments:
     ```sh
     terraform workspace new dev
     terraform workspace new prod
     ```
2. **Switch Between Workspaces:**
   - Switch to the `dev` workspace:
     ```sh
     terraform workspace select dev
     ```
   - Apply the configuration:
     ```sh
     terraform apply
     ```
   - Switch to the `prod` workspace:
     ```sh
     terraform workspace select prod
     ```
   - Apply the configuration:
     ```sh
     terraform apply
     ```

### Day 5: Security and Best Practices

#### 12. Security and Best Practices

**Security in Terraform**
- **Managing Sensitive Data:**
  - Use environment variables to store sensitive data.
  - Use secrets management tools (e.g., AWS Secrets Manager, HashiCorp Vault).

**Sample Configuration:**
```hcl
# variables.tf
variable "db_password" {
  description = "The password for the database"
  type        = string
  sensitive   = true
}

# main.tf
provider "aws" {
  region = "us-west-2"
}

resource "aws_db_instance" "example" {
  identifier          = "example-db"
  instance_class      = "db.t2.micro"
  allocated_storage   = 10
  engine              = "mysql"
  engine_version      = "5.7"
  username            = "admin"
  password            = var.db_password
  skip_final_snapshot = true
}
```

- **Storing Sensitive Data Securely:**
  ```sh
  export TF_VAR_db_password="your-secret-password"
  terraform apply
  ```

**Best Practices**
- **Version Control:**
  - Store your Terraform configurations in a version control system (e.g., Git).
- **Use Modules:**
  - Organize and reuse code by using modules.
- **Keep State Secure:**
  - Store state files in a remote backend with locking (e.g., S3 with DynamoDB for locking).

**Sample Module:**
```hcl
# modules/instance/main.tf
resource "aws_instance" "example" {
  ami           = var.ami
  instance_type = var.instance_type
}

# modules/instance/variables.tf
variable "ami" {
  description = "The AMI to use for the instance"
}

variable "instance_type" {
  description = "Type of instance"
}

# main.tf
module "instance" {
  source        = "./modules/instance"
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

**Hands-on Activity:**
1. **Refactor Configuration to Use Modules:**
   - Move the instance resource to a module as shown above.
   - Update `main.tf` to use the module.
2. **Implement Secure Storage for Sensitive Data:**
   - Store sensitive data in environment variables or use a secrets management tool.
   - Refactor your configuration to read sensitive data securely.

By following this detailed plan, your team should have a comprehensive understanding of Terraform Cloud and Enterprise, workspaces, and security best practices, along with hands-on experience in implementing these concepts.
