# 🚀 Complete DevOps CI/CD Pipeline with GitOps

This project demonstrates a comprehensive DevOps CI/CD pipeline implementation using modern tools and practices including Infrastructure as Code (IaC), Continuous Integration/Continuous Deployment (CI/CD), GitOps, and Kubernetes orchestration.

## 📋 Project Overview

This project implements a complete DevOps pipeline for a Reddit Clone application with the following components:

- **Infrastructure as Code (IaC)**: Terraform for AWS infrastructure provisioning
- **CI/CD Pipeline**: Jenkins with automated builds and deployments
- **Code Quality**: SonarQube integration for static code analysis
- **Container Security**: Trivy for vulnerability scanning
- **Container Registry**: Docker Hub for image storage
- **Kubernetes Orchestration**: AWS EKS cluster
- **Monitoring**: Prometheus and Grafana for observability
- **GitOps**: ArgoCD for automated deployments
- **Notifications**: Email alerts for pipeline events

## 🏗️ Architecture

<img width="1440" alt="ReditStruc" src="https://github.com/user-attachments/assets/3f3f51ae-4b3d-43fe-96b0-57c4f0451205" />

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   GitHub Repo   │───▶│   Jenkins CI    │───▶│  Docker Hub     │
│   (Source Code) │    │   (Pipeline)    │    │  (Registry)     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       ▼                       ▼
         │              ┌─────────────────┐    ┌─────────────────┐
         │              │   SonarQube     │    │     ArgoCD      │
         │              │  (Code Quality) │    │   (GitOps)      │
         │              └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Email Alerts   │    │     Trivy       │    │   AWS EKS       │
│  (Notifications)│    │  (Security)     │    │  (Kubernetes)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                       │
                                                       ▼
                                              ┌─────────────────┐
                                              │ Prometheus +    │
                                              │ Grafana         │
                                              │ (Monitoring)    │
                                              └─────────────────┘
```

## 🛠️ Technologies Used

- **Infrastructure**: AWS EC2, AWS EKS, Terraform
- **CI/CD**: Jenkins, Docker, Docker Hub
- **Code Quality**: SonarQube
- **Security**: Trivy
- **Container Orchestration**: Kubernetes (EKS)
- **GitOps**: ArgoCD
- **Monitoring**: Prometheus, Grafana
- **Notifications**: Email (SMTP)
- **Version Control**: GitHub

## 📁 Project Structure

```
23_June_2025_Project/
├── Redit-Clone-Deployment/
│   └── Jenkins-SonarQube-VM/
│       ├── main.tf                 # Terraform EC2 configuration
│       ├── provider.tf             # AWS provider configuration
│       ├── install.sh              # Installation script
│       └── terraform.tfstate       # Terraform state files
├── screenshots/                    # Project documentation images
└── README.md                       # This file
```

## 🚀 Implementation Steps

### 1. Infrastructure Setup with Terraform

**EC2 Instance Configuration:**
- **Instance Type**: t2.large
- **AMI**: Ubuntu (ami-0287a05f0ef0e9d9a)
- **Region**: ap-south-1
- **Storage**: 40GB EBS volume
- **Security Groups**: Custom SG with ports 22, 80, 443, 8080, 9000, 3000


**Terraform Configuration:**
```hcl
resource "aws_instance" "web" {
  ami                    = "ami-0287a05f0ef0e9d9a"
  instance_type          = "t2.large"
  key_name               = "Linux-VM-Key7"
  vpc_security_group_ids = [aws_security_group.Jenkins-VM-SG.id]
  user_data              = templatefile("./install.sh", {})
  
  tags = {
    Name = "Jenkins-SonarQube"
  }
}
```

### 2. Jenkins Configuration

Jenkins is automatically installed and configured on the EC2 instance with:
- Java 17 (Temurin)
- Jenkins LTS
- Docker integration
- Pipeline capabilities


### 3. SonarQube Integration

SonarQube is deployed as a Docker container for code quality analysis:
- **Port**: 9000
- **Image**: sonarqube:lts-community
- **Integration**: Jenkins plugin

![SonarQube Analysis](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/SonarQube-Analysis.png?raw=true)

### 4. CI/CD Pipeline Implementation

**Pipeline Components:**
- Source code checkout from GitHub
- Code quality analysis with SonarQube
- Security scanning with Trivy
- Docker image building
- Image push to Docker Hub
- Email notifications

![Success Build Docker Image](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Success-Build-DockerImage.png?raw=true)
![Docker Image at DockerHub](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/DockerImage-At-DockerHub.png?raw=true)
![Success Build Up to TrivyFS](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Success-Build-Upto-TrivyFS.png?raw=true)

### 5. Email Notification System

Configured email notifications for:
- Build success/failure
- Pipeline completion
- Security scan results

![Email Send Success](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Email-Send-Success.png?raw=true)
![Email Send Success2](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Email-Send-Success2.png?raw=true)
![Email Post Action Build Success](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Email-Post-Action-Build-Success.png?raw=true)
![Received Email of All Result](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Recieved-Email-Of-All-Result.png?raw=true)
![Received Email of All Result2](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Recieved-Email-Of-All-Result2.png?raw=true)

### 6. AWS EKS Cluster Setup

Created Kubernetes cluster for application deployment:
- **Cluster Type**: EKS (Elastic Kubernetes Service)
- **Region**: AWS
- **Purpose**: Application orchestration

![AWS EKS Created](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/AWS-EKS-Created.png?raw=true)

### 7. Monitoring Stack (Prometheus + Grafana)

Deployed monitoring solution using Helm:
- **Prometheus**: Metrics collection
- **Grafana**: Visualization and dashboards
- **Helm Charts**: Kubernetes package management

![Grafana Monitoring](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Grafana-Monitoring.png?raw=true)
![Grafana Monitoring2](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Grafana-Monitoring2.png?raw=true)
![Grafana Monitoring3](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Grafana-Monitoring3.png?raw=true)
![Grafana Monitoring4](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Grafana-Monitoring4.png?raw=true)
![Grafana Monitoring5](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Grafana-Monitoring5.png?raw=true)

### 8. ArgoCD GitOps Implementation

**ArgoCD Setup:**
- Installed on Kubernetes cluster
- Connected to GitHub repository
- Automated deployment pipeline

![ArgoCD Installation](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Screenshot%202025-06-23%20at%2011.19.28%20AM.png?raw=true)
![Repo ArgoCD Connected](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/Repo-ArgoCD-Connected.png?raw=true)
![EKS Cluster Added to ArgoCD](https://github.com/Anshuman-git-code/Redit-Clone-Deployment/blob/main/screenshots/EKS-Cluster-Added-To-ArgoCD.png?raw=true)

### 9. Automated Deployment Pipeline

**GitOps Workflow:**
- Code changes trigger GitHub webhook
- Jenkins builds and tests application
- ArgoCD automatically deploys to EKS
- Continuous monitoring and feedback

![Automated-Deployment-ChangesAtRepo-AutomateBuild-AtJenkinsJob](https://github.com/user-attachments/assets/5571995c-457e-4d92-bd13-4b211b6106f7)

![Screenshot 2025-06-23 at 11 19 28 AM](https://github.com/user-attachments/assets/6159ccf9-dbc1-46ef-8a21-e856e01be32c)


## 🔧 Installation and Setup

### Prerequisites
- AWS CLI configured
- Terraform installed
- Docker installed
- kubectl configured
- Helm installed

### Quick Start

1. **Clone the repository:**
```bash
git clone <repository-url>
cd 23_June_2025_Project
```

2. **Deploy Infrastructure:**
```bash
cd Redit-Clone-Deployment/Jenkins-SonarQube-VM
terraform init
terraform plan
terraform apply
```

3. **Access Services:**
- Jenkins: `http://<EC2-IP>:8080`
- SonarQube: `http://<EC2-IP>:9000`
- ArgoCD: `http://<EKS-IP>:80`

## 📊 Key Features

### ✅ Infrastructure as Code
- Complete AWS infrastructure defined in Terraform
- Reproducible and version-controlled deployments
- Automated provisioning and teardown

### ✅ Continuous Integration
- Automated code quality checks
- Security vulnerability scanning
- Docker image building and publishing
- Comprehensive testing pipeline

### ✅ GitOps Deployment
- Declarative deployment manifests
- Automated sync with Git repository
- Rollback capabilities
- Multi-environment support

### ✅ Monitoring and Observability
- Real-time metrics collection
- Custom Grafana dashboards
- Alert management
- Performance monitoring

### ✅ Security
- Container vulnerability scanning
- Code quality gates
- Secure image registry
- RBAC implementation

## 🎯 Benefits

1. **Automation**: Reduced manual intervention in deployment process
2. **Reliability**: Consistent and reproducible deployments
3. **Security**: Integrated security scanning and quality gates
4. **Scalability**: Kubernetes-based orchestration
5. **Observability**: Comprehensive monitoring and alerting
6. **Compliance**: Audit trail and version control

## 📈 Monitoring Dashboards

The project includes comprehensive monitoring with:
- **Application Metrics**: Response times, error rates, throughput
- **Infrastructure Metrics**: CPU, memory, disk usage
- **Container Metrics**: Pod health, resource utilization
- **Custom Dashboards**: Business-specific KPIs

## 🔄 CI/CD Pipeline Flow

```
1. Developer pushes code to GitHub
2. GitHub webhook triggers Jenkins build
3. Jenkins runs SonarQube analysis
4. Trivy scans for vulnerabilities
5. Docker image is built and pushed
6. Email notification sent
7. ArgoCD detects changes and deploys
8. Application is live on EKS
9. Monitoring dashboards updated
```

## 🛡️ Security Features

- **Container Scanning**: Trivy vulnerability assessment
- **Code Quality**: SonarQube static analysis
- **Network Security**: AWS Security Groups
- **Access Control**: RBAC in Kubernetes
- **Secret Management**: Kubernetes secrets

## 📞 Support and Maintenance

- **Monitoring**: 24/7 application and infrastructure monitoring
- **Alerts**: Automated email notifications for critical events
- **Backup**: Automated backup strategies
- **Documentation**: Comprehensive setup and maintenance guides

## 🎉 Conclusion

This project demonstrates a production-ready DevOps pipeline that combines modern tools and practices to deliver reliable, secure, and scalable applications. The implementation showcases best practices in:

- Infrastructure as Code
- Continuous Integration/Continuous Deployment
- GitOps methodology
- Container orchestration
- Monitoring and observability
- Security automation

The pipeline ensures fast, reliable, and secure software delivery while maintaining high code quality and operational excellence.

---

**Project Status**: ✅ Complete  
**Last Updated**: June 23, 2025  
**Technology Stack**: AWS, Terraform, Jenkins, Docker, Kubernetes, ArgoCD, Prometheus, Grafana 
