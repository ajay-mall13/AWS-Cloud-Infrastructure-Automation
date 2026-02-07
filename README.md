# Automated AWS Infrastructure Deployment using Terraform

![AWS Infrastructure Diagram](AWS%20Terraform%20Infra.jpg)

## 📋 Overview

This project automates the deployment of a complete AWS infrastructure using Terraform. It provisions a highly available web application environment with VPC, public subnets across multiple availability zones, EC2 instances running Apache web servers, and an Application Load Balancer for traffic distribution.

## 🏗️ Infrastructure Components

The Terraform configuration creates the following AWS resources:

- **VPC (Virtual Private Cloud)**: Custom network with CIDR block `10.0.0.0/16`
- **Internet Gateway**: Enables internet connectivity for the VPC
- **Public Subnets**: Two subnets in different availability zones for high availability
  - Subnet 1: `10.0.0.0/24` (us-east-1a)
  - Subnet 2: `10.0.1.0/24` (us-east-1b)
- **Route Table**: Configured with routes to the Internet Gateway
- **Security Group**: Controls inbound/outbound traffic for EC2 instances and ALB
- **EC2 Instances**: Two Ubuntu-based web servers with Apache installed
- **Application Load Balancer (ALB)**: Distributes traffic across EC2 instances
- **Target Group**: Manages EC2 instances for the load balancer
- **S3 Bucket**: Storage for static assets (optional)

## 📁 Project Structure

```
.
├── main.tf           # Main Terraform configuration (infrastructure resources)
├── provider.tf       # AWS provider configuration
├── variable.tf       # Variable definitions
├── userdata1.sh      # Bootstrap script for EC2 instances
├── AWS Terraform Infra.jpg  # Architecture diagram
└── README.md         # Project documentation
```

## 🔧 Prerequisites

Before you begin, ensure you have the following installed:

- [Terraform](https://www.terraform.io/downloads.html) (v1.0 or later)
- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate credentials
- An AWS account with appropriate permissions to create resources

## ⚙️ Configuration

### AWS Provider Setup

The project is configured to use AWS provider version `5.11.0` and deploys resources in the `us-east-1` region by default.

```hcl
provider "aws" {
  region = "us-east-1"
}
```

### Variables

- **cidr**: VPC CIDR block (default: `10.0.0.0/16`)

You can customize variables by creating a `terraform.tfvars` file:

```hcl
cidr = "10.0.0.0/16"
```

## 🚀 Deployment Instructions

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd "Automated AWS Infrastructure Deployment using Terraform"
```

### Step 2: Initialize Terraform

Initialize the Terraform working directory and download required providers:

```bash
terraform init
```

### Step 3: Review the Execution Plan

Preview the resources that will be created:

```bash
terraform plan
```

### Step 4: Deploy the Infrastructure

Apply the Terraform configuration to create the AWS resources:

```bash
terraform apply
```

Type `yes` when prompted to confirm the deployment.

### Step 5: Access Your Application

After successful deployment, Terraform will output the DNS name of the Application Load Balancer. Access your web application using:

```
http://<alb-dns-name>
```

## 🌐 Web Application

The EC2 instances are automatically configured with Apache web server through the `userdata1.sh` script. The deployed application displays:

- A welcome page with animated text
- Instance ID for identifying which server is responding
- Custom portfolio content

## 🔐 Security

The security group is configured to allow:

- **Inbound Traffic**:
  - HTTP (port 80) from anywhere
  - SSH (port 22) for administrative access
- **Outbound Traffic**:
  - All traffic allowed

> **⚠️ Warning**: For production environments, restrict SSH access to specific IP addresses and consider using a bastion host.

## 📊 Resource Details

### EC2 Instances

- **AMI**: Ubuntu (latest)
- **Instance Type**: t2.micro (Free tier eligible)
- **Count**: 2 instances
- **Auto-assigned Public IP**: Enabled
- **User Data**: Automated Apache installation and configuration

### Application Load Balancer

- **Type**: Application Load Balancer
- **Scheme**: Internet-facing
- **Health Check**: HTTP on port 80, path `/`
- **Target Type**: Instance

## 🧹 Cleanup

To destroy all created resources and avoid ongoing charges:

```bash
terraform destroy
```

Type `yes` when prompted to confirm the destruction.

## 📝 Notes

- The infrastructure is designed for high availability with resources spread across multiple availability zones
- All resources are tagged appropriately for easy identification
- The setup uses AWS Free Tier eligible resources where possible
- Ensure your AWS credentials have sufficient permissions to create VPC, EC2, ALB, and S3 resources

## 🛠️ Customization

You can customize the infrastructure by modifying:

- **Instance Type**: Change the EC2 instance type in `main.tf`
- **Region**: Update the region in `provider.tf`
- **Subnet CIDR**: Modify subnet configurations in `main.tf`
- **Web Content**: Edit `userdata1.sh` to customize the web application

## 📚 Additional Resources

- [Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/)
- [AWS Application Load Balancer Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

This project is open source and available for educational purposes.

---

**Author**: Ajay Mall
**Last Updated**: February 2026
