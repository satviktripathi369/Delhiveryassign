# Deploy a Containerized Web Application with CI/CD and Monitoring

This project demonstrates deploying a containerized WordPress web application on Kubernetes using AWS infrastructure, automated CI/CD pipelines, and integrated monitoring using Prometheus and Grafana.

## Objective

Set up and deploy a WordPress application using:
- **Docker** for containerization
- **Kubernetes** for orchestration
- **GitHub Actions** for CI/CD automation
- **Prometheus and Grafana** for monitoring

## Key Features
- **Infrastructure Setup**: Kubernetes cluster provisioned on AWS using `kops`.
- **Automated CI/CD Pipeline**: Validates Kubernetes manifests and deploys them to the cluster.
- **Monitoring and Observability**: Prometheus and Grafana integration for real-time metrics visualization.

---

## Tools and Technologies Used

- **Containerization**: Docker
- **Orchestration**: Kubernetes (`kops`, `kubectl`)
- **CI/CD**: GitHub Actions
- **Infrastructure as Code (IaC)**: AWS services
- **Monitoring**: Prometheus and Grafana
- **Cloud**: AWS (Free Tier)

---

## Workflow Overview

### CI/CD Pipeline
The pipeline includes two jobs:
1. **Validation (CI)**:
   - Validates Kubernetes YAML manifests using `kubeval`.
   - Ensures compliance with schema rules.
2. **Deployment (CD)**:
   - Deploys the validated Kubernetes manifests to the AWS Kubernetes cluster.
   - Verifies the application deployment and its services.

### Monitoring
Prometheus scrapes metrics from Kubernetes pods, and Grafana visualizes them using predefined dashboards.

---

## Setup and Deployment

### 1. Launch an EC2 Instance
1. Launch an Ubuntu instance on AWS (t2.micro).
2. Install necessary tools (`kops`, `kubectl`, AWS CLI).
3. Set up SSH access and security groups for Kubernetes communication.

### 2. Create S3 Bucket and Hosted Zones
- **S3 Bucket**: Stores the Kubernetes cluster's state managed by `kops`.
- **Route53 Hosted Zone**: Enables domain-based DNS for the Kubernetes API.

### 3. Create Kubernetes Cluster
- Use `kops` to define and create a Kubernetes cluster:
  ```bash
  kops create cluster --name=<cluster-name> --state=s3://<bucket-name> ...
  kops update cluster --name=<cluster-name> --state=s3://<bucket-name> --yes
