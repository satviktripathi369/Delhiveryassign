
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
  ```

### 4. CI/CD Pipeline
- **Validation**:
  - Fetches and validates Kubernetes manifests in the repository.
- **Deployment**:
  - Configures `kubectl` with `kubeconfig` (stored in GitHub Secrets).
  - Applies the validated manifests to the Kubernetes cluster.

### 5. Monitoring Setup
1. Deploy Prometheus and Grafana as Kubernetes deployments.
2. Expose Grafana using a LoadBalancer service for external access.
3. Add Prometheus as a Grafana data source and import dashboards for visualization.

---

## File Structure
```
.
├── manifests/
│   ├── deployment.yml        # WordPress deployment
│   ├── service.yml           # WordPress service
│   ├── grafana.yml           # Grafana deployment and service
│   ├── prometheus.yml        # Prometheus configuration
│   ├── configmap.yml         # Custom ConfigMap for WordPress
│   ├── mysql-deployment.yml  # MySQL deployment
├── .github/workflows/
│   ├── ci-cd.yml             # CI/CD pipeline definition
├── README.md
```

---

## How to Access the Application
1. Retrieve the external IP of the WordPress LoadBalancer service:
   ```bash
   kubectl get svc wordpress
   ```
2. Visit the IP address in your browser.

---

## Monitoring Dashboard
1. Access Grafana via the exposed LoadBalancer service.
2. Default credentials:
   - Username: `admin`
   - Password: `admin`
3. Add Prometheus as a data source and import a predefined dashboard.

---



## License
This project is licensed under the MIT License.
