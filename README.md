

Step-By-Step guide to learning automation (IaC) using Terraform

Challenge: Automate the provisioning of an S3 bucket and an EC2 instance in AWS 
 
Step 1: Install Terraform on your laptop

On Windows: https://youtu.be/4yvlR4_5ZF0


Step 2: On VS Code, create a folder, switch to folder and create the terraform files
```
mkdir terraform-project

cd terraform-project

touch main.tf variable.tf
```








Step 4: Use AWS Hashicorp Documentation to build your .tf files 




terraform init
terraform plan 
terraform apply 

terraform destroy -auto-approve


terraform destroy 
terraform validate 



In main.tf file: 

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
