# EC2-Runner
# AWS EC2 Self-Hosted GitHub Runner

This repository contains the Terraform configuration used to provision an **EC2 instance** that acts as a **self-hosted GitHub Actions runner**.  
The runner serves as the execution environment for deploying an Amazon EKS cluster and running CI/CD jobs for a containerised Spotify-clone application.

---

## 📌 Project Overview
This part of the project focuses on:

- Deploying an **EC2 instance** using Terraform.
- Installing essential DevOps tools via `install_tools.sh`:
  - Docker
  - AWS CLI
  - kubectl
  - Terraform
  - Trivy
  - SonarQube (Docker container)
- Registering the EC2 instance as a **self-hosted GitHub runner**.
- Preparing the environment so it can deploy the EKS cluster from the second repository.

A self-hosted runner is used instead of GitHub-hosted runners to support:
- Custom installations (e.g., security tools such as **Falco**)
- Realistic production-style infrastructure
- Possible compliance or runtime-monitoring requirements

---

## 🏗️ Infrastructure Components (Terraform)

### **Key Terraform files**
- **provider.tf** – Configures AWS provider (region: `eu-west-2`).
- **backend.tf** – Stores Terraform state in an S3 bucket (user must set bucket name).
- **get_AMI.tf** – Dynamically fetches latest Amazon Linux 2 AMI.
- **main.tf** – Creates:
  - EC2 instance (t2.large)
  - IAM role + policy
  - User data script to install tools
- **deploy.yaml** – GitHub Actions workflow to deploy this infrastructure.

---

## 🚀 Deployment Workflow
1. Fork repository and update:
   - `backend.tf` → set your S3 bucket name.
   - `main.tf` → set your EC2 key pair.
2. Add AWS credentials to repository secrets.
3. Run **workflow_dispatch** manually from GitHub Actions:
   - `terraform init`
   - `terraform plan`
   - `terraform apply`
4. SSH into the EC2 instance and verify installed tools.
5. Configure the instance as a **self-hosted runner** for the second repo.

---

## ✔️ Result
After deployment, the EC2 instance runs as a fully configured GitHub Actions runner capable of:

- Deploying an EKS cluster
- Executing CI/CD pipelines
- Running SonarQube, Docker builds, vulnerability scans, etc.

This EC2 runner forms the **foundation of the entire EKS deployment workflow**.
