### 07beta. Terraform with AWS

#### Introduction to AWS S3

**Overview:**
Amazon S3 (Simple Storage Service) is a scalable object storage service used for storing and retrieving any amount of data from anywhere on the web. 
S3 is commonly used for backups, content storage, and data archiving.

**Key Concepts:**
1. **Buckets:**
   - Containers for storing objects (files and metadata).
   - Unique name within AWS region and globally unique.

2. **Objects:**
   - Files stored in buckets.
   - Can include metadata and permissions.

3. **Storage Classes:**
   - Different pricing tiers based on access frequency (e.g., Standard, Standard-IA, Glacier).

4. **Versioning:**
   - Maintains multiple versions of objects to recover from unintended overwrites or deletions.

5. **Access Control:**
   - Managed using IAM policies, bucket policies, and ACLs (Access Control Lists).

#### S3 with Terraform

**Overview:**
Terraform can manage S3 buckets and objects, enabling infrastructure as code for storage solutions.

**Key Concepts:**
1. **aws_s3_bucket:**
   - Manages an S3 bucket.

2. **aws_s3_bucket_object:**
   - Manages an object within an S3 bucket.

3. **Bucket Policies:**
   - JSON documents defining access control.

**Example Configuration:**

**File: `main.tf`**
```hcl
provider "aws" {
  region = "us-east-1"
}

# Create an S3 bucket
resource "aws_s3_bucket" "example" {
  bucket = "terraform-example-bucket"
  acl    = "private"

  tags = {
    Name        = "Terraform Example Bucket"
    Environment = "Dev"
  }
}

# Enable versioning for the bucket
resource "aws_s3_bucket_versioning" "example" {
  bucket = aws_s3_bucket.example.bucket
  versioning_configuration {
    status = "Enabled"
  }
}

# Upload an object to the bucket
resource "aws_s3_bucket_object" "example" {
  bucket = aws_s3_bucket.example.bucket
  key    = "example.txt"
  source = "example.txt"
  acl    = "private"
}
```

**Steps to Execute:**
1. **Initialize Terraform:**
   ```sh
   terraform init
   ```

2. **Validate the Configuration:**
   ```sh
   terraform validate
   ```

3. **Create an Execution Plan:**
   ```sh
   terraform plan
   ```

4. **Apply the Configuration:**
   ```sh
   terraform apply -auto-approve
   ```

5. **Verify the Created Resources:**
   - Check the AWS S3 console to see the created bucket and uploaded object.

#### Introduction to DynamoDB

**Overview:**
Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. 
It's often used for applications requiring consistent, single-digit millisecond latency at any scale.

**Key Concepts:**
1. **Tables:**
   - Collections of items (similar to rows in a relational database).

2. **Items:**
   - Individual records in a table (similar to rows in a relational database).

3. **Attributes:**
   - Data elements within an item (similar to columns in a relational database).

4. **Primary Key:**
   - Uniquely identifies each item in a table.
   - Consists of Partition Key and optionally a Sort Key.

5. **Provisioned Throughput:**
   - Specifies the read and write capacity units for the table.

#### Demo DynamoDB

**Steps:**
1. **Create a DynamoDB Table:**
   - Go to the DynamoDB console.
   - Click "Create table".
   - Provide table name, primary key, and provisioned throughput.

2. **Add Items to the Table:**
   - Click on the created table.
   - Go to the "Items" tab.
   - Click "Create item" and add attribute values.

#### DynamoDB with Terraform

**Overview:**
Terraform can manage DynamoDB tables, items, and indexes.

**Key Concepts:**
1. **aws_dynamodb_table:**
   - Manages a DynamoDB table.

2. **aws_dynamodb_table_item:**
   - Manages items within a DynamoDB table.

**Example Configuration:**

**File: `main.tf`**
```hcl
provider "aws" {
  region = "us-east-1"
}

# Create a DynamoDB table
resource "aws_dynamodb_table" "example" {
  name         = "terraform-example-table"
  billing_mode = "PROVISIONED"

  hash_key = "ID"

  attribute {
    name = "ID"
    type = "S"
  }

  read_capacity  = 5
  write_capacity = 5

  tags = {
    Name        = "Terraform Example Table"
    Environment = "Dev"
  }
}

# Add an item to the table
resource "aws_dynamodb_table_item" "example" {
  table_name = aws_dynamodb_table.example.name
  hash_key   = "ID"
  item       = <<ITEM
{
  "ID": {"S": "123"},
  "Name": {"S": "Example Item"},
  "Value": {"N": "100"}
}
ITEM
}
```

**Steps to Execute:**
1. **Initialize Terraform:**
   ```sh
   terraform init
   ```

2. **Validate the Configuration:**
   ```sh
   terraform validate
   ```

3. **Create an Execution Plan:**
   ```sh
   terraform plan
   ```

4. **Apply the Configuration:**
   ```sh
   terraform apply -auto-approve
   ```

5. **Verify the Created Resources:**
   - Check the DynamoDB console to see the created table and item.

### Summary

This detailed guide covers the essentials of using Terraform with AWS S3 and DynamoDB. 
It includes theoretical explanations, practical steps, and comprehensive code samples to help you understand and apply these concepts effectively. 
If you have any further questions or need additional examples, feel free to ask!
