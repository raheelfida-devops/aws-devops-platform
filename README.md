# AWS DevOps CI/CD Platform

> **raheelfida** | [GitHub](https://github.com/raheelfida-devops)

## Overview
Complete production-style DevOps platform: code commit → automated test → Docker build → security scan → ECR push → EC2 deployment → health check.

## Architecture

Developer Git Push
↓
GitHub Repository
↓
GitHub Actions CI/CD
├─ Checkout Code
├─ Install Dependencies
├─ Run Unit Tests
├─ Build Docker Image
├─ Security Scan (Trivy)
├─ Authenticate via OIDC
├─ Push to Amazon ECR
└─ Deploy to EC2 + Health Check
↓
Amazon ECR — Docker Image Storage
↓
EC2 Ubuntu Server
├─ Docker Runtime
├─ Nginx Reverse Proxy
├─ Node.js Application
└─ CloudWatch Monitoring
↓
CloudWatch — Logs & Metrics
plaintext

## Tech Stack
| Component | Technology |
|---|---|
| Application | Node.js 20 + Express |
| Containerization | Docker (non-root) |
| CI/CD | GitHub Actions |
| Infrastructure as Code | Terraform |
| Cloud Provider | AWS |
| AWS Services | VPC, EC2, ECR, IAM, CloudWatch |
| Auth Method | OIDC (no long-lived credentials) |
| Reverse Proxy | Nginx |

## Project Structure

```text
aws-devops-platform/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── app/
│   ├── src/
│   │   └── server.js
│   ├── tests/
│   │   └── health.test.js
│   ├── Dockerfile
│   ├── package-lock.json
│   └── package.json
├── terraform/
│   ├── ec2.tf
│   ├── ecr.tf
│   ├── outputs.tf
│   ├── provider.tf
│   ├── security.tf
│   ├── terraform.tfvars
│   ├── variables.tf
│   └── vpc.tf
├── .gitignore
└── README.md
```

## License
MIT
Save: Ctrl+O → Enter → Ctrl+X
File 3: LICENSE
bash
nano LICENSE
Paste:
plaintext
MIT License

Copyright (c) 2026 raheelfida


## 🔧 How It Works

### 1. Trigger
- Push any commit to `main` branch → pipeline starts automatically

### 2. Build Stage
- Check out code
- Authenticate with AWS using stored credentials
- Log in to Amazon ECR
- Build Docker image with commit SHA as version tag
- Push image to private ECR repository

### 3. Deploy Stage
- SSH into EC2 instance using encrypted private key
- Authenticate Docker to ECR
- Pull latest image
- Stop & remove previous container
- Start updated container with port mapping
- App live at public endpoint

### 4. Result
- ✅ Deployment complete — typically **under 45 seconds**
- 🌐 App accessible at: **http://<IP_ADDRESS>**

---

## 🛠️ Local Setup

```bash
# Clone repository
git clone https://github.com/raheelfida-devops/aws-devops-platform.git
cd aws-devops-platform

# Configure SSH key
chmod 600 ~/.ssh/aws-devops-key
ssh -i ~/.ssh/aws-devops-key ubuntu@<IP_ADDRESS>

# Run locally
cd app
docker build -t aws-devops-app .
docker run -p 3000:3000 aws-devops-app
🔐 GitHub Secrets Configuration
Repository → Settings → Secrets and variables → Actions
Table
Secret Name	Purpose
AWS_ACCESS_KEY_ID	AWS programmatic access key ID
AWS_SECRET_ACCESS_KEY	AWS programmatic secret key
EC2_SSH_PRIVATE_KEY	Full SSH private key block (including BEGIN/END lines)
EC2_HOST	Public IP address or DNS name of the deployment EC2 instance
📊 AWS Resources (us-east-1)
Table
Resource	Value
Account ID	449952321810
EC2 Instance ID	i-0ecd9ddd496164a98
Public IP	<IP_ADDRESS>
ECR Registry	449952321810.dkr.ecr.us-east-1.amazonaws.com
ECR Repository	aws-devops-platform-repo
SSH Key Name	aws-devops-platform-ssh-key
Instance Type	t3.micro
Platform	Ubuntu 22.04 LTS (Jammy)
✅ Status
Table
Feature	Status
Infrastructure provisioning	✅ Complete
Dockerfile & container build	✅ Verified
ECR repository & authentication	✅ Active
GitHub Actions pipeline	✅ Fully automated
EC2 SSH deployment	✅ Operational
Live endpoint accessibility	✅ http://<IP_ADDRESS>
Auto-deploy on code push	✅ Working
🚀 Usage
bash
# Make changes → commit → push → watch it deploy
git add .
git commit -m "feat: Add new feature"
git push

# Monitor progress
# → https://github.com/raheelfida-devops/aws-devops-platform/actions
📅 Timeline
Oct 2, 2026 — Initial deployment platform goes live ✅
Next — OIDC migration, testing gates, monitoring, IP restriction
👤 Author
raheelfida
GitHub: @raheelfida-devops
Project: aws-devops-platform
