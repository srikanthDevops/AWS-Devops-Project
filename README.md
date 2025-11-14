# AWS-Devops-Project
Deploy a containerized Spring Boot application on AWS using EKS, Terraform, Jenkins CI/CD, Prometheus/Grafana monitoring, and ensure zero-downtime deployments.
## ✅ Overview
This project demonstrates a real-time AWS DevOps pipeline using Terraform, Docker, Kubernetes (EKS), Jenkins, and Prometheus/Grafana.

---

## ✅ Prerequisites
- AWS CLI configured
- Terraform installed
- Docker installed
- Jenkins on EC2 (`t3.medium`, 30GB storage)
- kubectl & Helm installed
- IAM roles for EKS, EC2, and Jenkins

---

## ✅ Deployment Steps

### 1. Provision Infrastructure
```bash
cd terraform
terraform init
terraform apply -auto-approve
```

### 2. Build Spring Boot App
```bash
cd app
mvn clean package
```

### 3. Build & Push Docker Image
```bash
aws ecr create-repository --repository-name myapp --region <region-id>
aws ecr get-login-password --region <region-id> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region-id>.amazonaws.com
docker build -t myapp .
docker tag myapp:latest <account_id>.dkr.ecr.us-east-1.amazonaws.com/myapp:latest
docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/myapp:latest
```

### 4. Deploy to EKS
Update image and account-id in `k8s/deployment.yaml` and apply:
```bash
kubectl apply -f k8s/deployment.yaml
```

### 5. Configure Jenkins Pipeline
- Use `Jenkinsfile` for pipeline stages.
- Stages: Checkout → Build → Docker Push → Deploy to EKS.

### 6. Monitoring Setup
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack
```
Access Grafana:
```bash
kubectl port-forward svc/prometheus-grafana 3000:80
```

### 7. Zero-Downtime Deployment
- Kubernetes RollingUpdate strategy.
- AWS CodeDeploy Blue/Green for EC2 apps.

### 8. Performance Optimization
- Enable Cluster Autoscaler.
- Use ElastiCache.
- Enable S3 Transfer Acceleration.

---

## ✅ Repository Structure
```
aws-devops-project/
├── terraform/main.tf
├── app/Dockerfile
├── app/pom.xml
├── app/src/main/java/com/example/demo/DemoApplication.java
├── k8s/deployment.yaml
├── Jenkinsfile
└── README.md
```
