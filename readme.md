
markdownCopy code# Deploying AWS Infrastructure and EC2 Instance with NGINX using Terraform

This project automates the creation of a complete **AWS network infrastructure** and launches an **Amazon EC2 instance** running **NGINX** on **Amazon Linux 2**.  
Terraform handles every step — from creating the **VPC**, **subnets**, and **route tables** to provisioning the **EC2 instance** and configuring **NGINX** via `user_data`.

---

## 🧩 Overview

When you apply this Terraform configuration, it will:

1. Create a **VPC** with a configurable CIDR block.
2. Set up both **public** and **private subnets**.
3. Create an **Internet Gateway** and **route tables** for internet connectivity.
4. Configure **Security Groups** to allow SSH (22) and HTTP (80).
5. Launch an **EC2 instance** in the public subnet.
6. Use **user data** to:
   - Update system packages  
   - Install and start **NGINX**  
   - Serve a simple HTML page

Once deployed, you can access your instance’s **public IP** in a web browser to see:

> “Hello from NGINX on Amazon Linux 2!”

---

## 🛠️ Prerequisites

Before running this Terraform setup, ensure that you have:

* An active **AWS Account**
* **Terraform** installed → [Download Terraform](https://developer.hashicorp.com/terraform/downloads)
* **AWS CLI** configured with credentials:

  ```bash
  aws configure
Proper IAM permissions for managing VPC, EC2, and networking components

📁 File Structure
bashCopy codeterraform-aws-nginx/
│
├── backend.tf           # Terraform backend configuration (S3)
├── main.tf              # Main Terraform resources (VPC, EC2, Subnets, Routes)
├── provider.tf          # AWS provider and version configuration
├── variables.tf         # Variable definitions
├── dev.tfvars           # Development environment variables
├── prod.tfvars          # Production environment variables
└── README.md            # Project documentation (this file)

⚙️ Terraform Backend
The backend.tf file configures Terraform to store its state remotely in Amazon S3.
Make sure your backend bucket exists before initializing Terraform.

🧾 User Data Script (for EC2 Instance)
This script runs automatically on instance startup to install and configure NGINX:
bashCopy code#!/bin/bash
yum update -y
yum install nginx -y
systemctl enable nginx
systemctl start nginx
This ensures NGINX starts on boot and serves the default welcome page.

🚀 Terraform Workflow
Run these commands from your project directory:
bashCopy code# Initialize Terraform and backend
terraform init

# Validate the configuration
terraform validate

# View the execution plan
terraform plan -var-file="dev.tfvars"

# Apply and create infrastructure
terraform apply -var-file="dev.tfvars"

# Destroy all resources when finished
terraform destroy -var-file="dev.tfvars"
You can switch environments by replacing dev.tfvars with prod.tfvars.

🌐 Accessing Your EC2 Instance
Once the deployment is complete, Terraform will output the public IP address of your EC2 instance.
Open it in your browser:
cppCopy codehttp://<public_ip>
You should see:
Hello from NGINX on Amazon Linux 2!

🧹 Cleanup
To prevent unnecessary AWS charges, destroy all created resources when you’re done:
bashCopy codeterraform destroy -var-file="dev.tfvars"

🪄 Bonus Tip
You can customize the HTML welcome message by modifying the user_data script inside main.tf.
For example:
bashCopy code<h1>Hello from Ranjeeth’s Terraform-powered EC2 Server 🚀</h1>

🧠 References
Terraform AWS Provider Documentation
Terraform CLI Reference
NGINX Official Documentation