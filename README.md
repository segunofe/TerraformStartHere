

**Step-By-Step guide to learning automation (IaC) using Terraform**

**Challenge**: Automate the provisioning of 10 infrastructures in AWS - an S3 bucket and 9 EC2 instances
 
**Step 1**: Install Terraform on your laptop

On Windows: https://youtu.be/4yvlR4_5ZF0


**Step 2**: On VS Code, create a folder, switch to folder and create the terraform files
```
mkdir terraform-project

cd terraform-project

touch main.tf variable.tf
```

**Step 4**: Use AWS Hashicorp Documentation to build your .tf files.

main.tf

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  region = "us-west-2"
}

# Pull the latest uuntu AMI for creation of EC2 instance
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }

  owners = ["099720109477"] # Canonical
}


# Provision 10 EC2 instance with dynamic indexing for the name
resource "aws_instance" "demo-server" {
  count         = 9
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"

  tags = {
    Name = "Taiwo-server-${count.index + 1}"
  }
}




# Generate a random suffix for unique bucket name
resource "random_string" "bucket_suffix" {
  length  = 11
  lower   = true
  upper   = false
  special = false
}

resource "aws_s3_bucket" "bucket-demo" {
  bucket = "demobucket-${random_string.bucket_suffix.result}"

}


```

**Step 5**: Run terraform commands

```
terraform init

terraform plan

terraform apply 
```

**Other commands**

```
terraform destroy
terraform destroy -auto-approve
terraform validate 
```


In the above main.tf file: 

"required_provider" block tells terraform what provider (aws, azure, gcp) plugin to download and which version to use.

A plugin is a small piece of code that teaches Terraform how to talk to AWS.

"provider" block configures how Terraform should connect with AWS - Terraform create the resources in the region 


terraform.lock.hcl → locks exact provider versions so there is no accidental upgrade and every team member uses the same provider version. 

terraform.tfstate → Stores the current real-world state of your infrastructure.

Terraform uses it to understand what exists in AWS so it knows what to change.


terraform.tfstate.backup → automatically creates a backup of your previous state file.

https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket

To undo "Ctrl + u" after "vi" 

On your terminal in VS Code, switch to Git from Powershell if familiar with Git Bash
