# 🚀 GitOps Deployment of Spring Boot Application on AWS EKS using Helm & ArgoCD

![AWS](https://img.shields.io/badge/AWS-EKS-orange?style=for-the-badge&logo=amazonaws)
![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.33-blue?style=for-the-badge&logo=kubernetes)
![Helm](https://img.shields.io/badge/Helm-v3-0F1689?style=for-the-badge&logo=helm)
![ArgoCD](https://img.shields.io/badge/ArgoCD-GitOps-EF7B4D?style=for-the-badge&logo=argo)
![Docker](https://img.shields.io/badge/Docker-Container-blue?style=for-the-badge&logo=docker)
![GitHub](https://img.shields.io/badge/GitHub-Repository-black?style=for-the-badge&logo=github)

---

# 📖 Project Overview

This project demonstrates a **production-style GitOps deployment** of a **Spring Boot application** on **Amazon Elastic Kubernetes Service (EKS)** using **AWS Fargate**, **Helm**, and **ArgoCD**.

Instead of deploying applications manually with Kubernetes manifests, this project follows the **GitOps methodology**, where the desired state of the application is stored in a Git repository. ArgoCD continuously monitors the repository and automatically synchronizes any changes to the Kubernetes cluster.

The application is packaged as a **Helm Chart**, making deployments reusable, configurable, and easier to manage across environments.

To simplify cluster management, the application runs on **AWS Fargate**, eliminating the need to provision or manage worker nodes. The application is exposed through an **AWS LoadBalancer Service**, allowing external access to the Spring Boot REST API.

This project showcases modern cloud-native deployment practices widely adopted in production DevOps environments.

---

# 🎯 Project Objectives

The primary objectives of this project are:

- Deploy a Spring Boot application on Amazon EKS
- Use AWS Fargate for serverless Kubernetes workloads
- Package the application using Helm Charts
- Implement GitOps deployment with ArgoCD
- Automate application synchronization from GitHub
- Expose the application using an AWS LoadBalancer
- Demonstrate Kubernetes deployment best practices
- Gain hands-on experience with production-ready GitOps workflows

---

# ⭐ Key Features

- GitOps-based deployment workflow
- Amazon EKS cluster using AWS Fargate
- Helm Chart packaging
- ArgoCD continuous synchronization
- Declarative Kubernetes deployments
- Docker image hosted on Docker Hub
- Automatic reconciliation of cluster state
- Kubernetes Deployment and Service resources
- AWS LoadBalancer integration
- Version-controlled infrastructure and application configuration

---

# 🛠️ Technology Stack

| Category | Technology |
|-----------|------------|
| Cloud Provider | AWS |
| Container Platform | Docker |
| Container Registry | Docker Hub |
| Container Orchestration | Amazon EKS |
| Compute | AWS Fargate |
| Package Manager | Helm |
| GitOps Tool | ArgoCD |
| Version Control | Git & GitHub |
| Application | Spring Boot |
| Language | Java |
| Kubernetes Resources | Deployment, Service |
| Networking | AWS Elastic Load Balancer |

---

# 🏗️ Solution Architecture

> **Architecture Diagram**

```
📂 screenshots/architecture-diagram.png
```

After uploading screenshots, replace the placeholder with:

```markdown
<p align="center">
<img src="screenshots/architecture-diagram.png" width="100%">
</p>
```

---

# 🔄 GitOps Deployment Workflow

```text
                +----------------------+
                |     Developer        |
                +----------+-----------+
                           |
                     Push Helm Chart
                           |
                           ▼
                +----------------------+
                |      GitHub Repo     |
                +----------+-----------+
                           |
                  Watches Repository
                           |
                           ▼
                +----------------------+
                |       ArgoCD         |
                |  Continuous Sync     |
                +----------+-----------+
                           |
                     Deploy Helm Chart
                           |
                           ▼
                +----------------------+
                |      Amazon EKS      |
                |     AWS Fargate      |
                +----------+-----------+
                           |
                 Kubernetes Service
                           |
                           ▼
                AWS LoadBalancer Service
                           |
                           ▼
                 Spring Boot Application
```

---

# 📌 Why GitOps?

GitOps provides a modern approach to Kubernetes deployments by treating Git as the **single source of truth**.

Instead of manually applying Kubernetes manifests, all deployment configurations are stored in Git. ArgoCD continuously monitors the repository and ensures the Kubernetes cluster always matches the desired state.

### Benefits of GitOps

- Declarative deployments
- Version-controlled infrastructure
- Automated synchronization
- Easy rollback using Git history
- Improved deployment consistency
- Better collaboration across teams
- Reduced manual operational effort
- Enhanced auditability and traceability

---

# 📚 What You'll Learn

By completing this project, you will gain practical experience with:

- Amazon EKS
- AWS Fargate
- Kubernetes Deployments
- Kubernetes Services
- Helm Charts
- ArgoCD
- GitOps workflows
- Docker image deployment
- LoadBalancer Services
- Kubernetes application lifecycle management
- Production deployment practices

---
# 📂 Repository Structure

```text
springboot-gitops/
│
├── springboot-chart/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│       ├── deployment.yaml
│       ├── service.yaml
│       ├── _helpers.tpl
│       └── NOTES.txt
│
├── screenshots/
│   ├── architecture-diagram.png
│   ├── eks-cluster.png
│   ├── pods.png
│   ├── services.png
│   ├── loadbalancer.png
│   ├── argocd-dashboard.png
│   ├── github-repository.png
│   └── application.png
│
├── README.md
└── LICENSE
```

---

# ⚙️ Prerequisites

Before deploying this project, ensure the following tools are installed and configured.

| Tool | Purpose |
|------|----------|
| AWS CLI | Access AWS services |
| kubectl | Kubernetes command-line tool |
| eksctl | Create and manage EKS clusters |
| Helm | Package and deploy Kubernetes applications |
| Docker | Build and manage container images |
| Git | Version control |
| ArgoCD CLI *(Optional)* | Manage ArgoCD from the terminal |

---

# ☁️ AWS Services Used

This project utilizes the following AWS services:

- Amazon Elastic Kubernetes Service (EKS)
- AWS Fargate
- Elastic Load Balancer (ELB)
- IAM
- Amazon VPC
- Security Groups
- CloudFormation (created automatically by eksctl)

---

# 🚀 Deployment Steps

## Step 1 – Create Amazon EKS Cluster

Create an EKS cluster using **eksctl**.

```bash
eksctl create cluster \
--name springboot-eks-cluster \
--region us-east-1 \
--fargate
```

Verify the cluster:

```bash
kubectl get nodes
```

> **Note:** Since this project uses AWS Fargate, worker nodes are managed by AWS and pods run on serverless compute.

---

## Step 2 – Configure kubectl

Update the kubeconfig file.

```bash
aws eks update-kubeconfig \
--region us-east-1 \
--name springboot-eks-cluster
```

Verify the connection:

```bash
kubectl cluster-info
```

---

## Step 3 – Create Namespace

```bash
kubectl create namespace springboot
```

Verify:

```bash
kubectl get namespaces
```

---

## Step 4 – Install ArgoCD

Create the ArgoCD namespace.

```bash
kubectl create namespace argocd
```

Install ArgoCD.

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verify installation.

```bash
kubectl get pods -n argocd
```

---

## Step 5 – Access ArgoCD UI

Port-forward the ArgoCD server.

```bash
kubectl port-forward svc/argocd-server \
-n argocd 8080:443
```

Open your browser.

```text
https://localhost:8080
```

Retrieve the initial admin password.

```bash
kubectl \
-n argocd \
get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
```

---

## Step 6 – Clone Repository

```bash
git clone https://github.com/Prameet-26/springboot-gitops.git

cd springboot-gitops
```

---

## Step 7 – Helm Chart Structure

The application is packaged as a reusable Helm Chart.

```
springboot-chart/
│
├── Chart.yaml
├── values.yaml
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    ├── NOTES.txt
    └── _helpers.tpl
```

The Helm chart manages:

- Deployment
- Service
- Labels
- Image configuration
- Replica configuration
- Service type
- Container port

---

## Step 8 – Create ArgoCD Application

Inside the ArgoCD UI:

**Application Name**

```
springboot-gitops
```

**Repository URL**

```
https://github.com/Prameet-26/springboot-gitops.git
```

**Path**

```
springboot-chart
```

**Cluster**

```
https://kubernetes.default.svc
```

**Namespace**

```
default
```

Enable:

- ✅ Automatic Sync
- ✅ Self Heal
- ✅ Prune Resources

Click **Create**.

---

## Step 9 – Automatic Deployment

Once the application is created:

- ArgoCD detects the Helm Chart
- Generates Kubernetes manifests
- Deploys the application
- Continuously monitors GitHub
- Automatically synchronizes changes

This is the core GitOps workflow, where Git remains the single source of truth for deployments.

---

# 🔍 Verification Commands

Verify Pods

```bash
kubectl get pods
```

Verify Deployments

```bash
kubectl get deployments
```

Verify Services

```bash
kubectl get svc
```

Verify LoadBalancer

```bash
kubectl get svc springboot-app
```

Describe Deployment

```bash
kubectl describe deployment springboot-app
```

View Pod Logs

```bash
kubectl logs <pod-name>
```

Describe Service

```bash
kubectl describe svc springboot-app
```

Check Application URL

```text
http://<LOADBALANCER-DNS>/hello
```

A successful response confirms that the application has been deployed and exposed through the AWS LoadBalancer.

---
# 📦 Helm Chart Overview

This project uses **Helm**, the package manager for Kubernetes, to simplify application deployment and management.

Instead of manually writing and applying multiple Kubernetes YAML manifests, Helm packages all Kubernetes resources into a reusable chart.

## Helm Chart Components

| File | Description |
|------|-------------|
| `Chart.yaml` | Contains chart metadata such as name, version, and description |
| `values.yaml` | Stores configurable parameters including image, replicas, ports, and service type |
| `templates/deployment.yaml` | Creates the Kubernetes Deployment |
| `templates/service.yaml` | Creates the Kubernetes Service |
| `_helpers.tpl` | Reusable template functions |
| `NOTES.txt` | Displays deployment notes after installation |

### Benefits of Using Helm

- Reusable deployments
- Easy version management
- Environment-specific configurations
- Simplified application upgrades
- Consistent deployments
- Reduced YAML duplication

---

# 🔄 ArgoCD GitOps Synchronization

ArgoCD continuously monitors the GitHub repository for changes.

Whenever changes are pushed to the repository:

1. ArgoCD detects the Git commit.
2. Compares the desired state with the cluster.
3. Identifies configuration drift.
4. Automatically synchronizes the cluster.
5. Deploys updated resources.
6. Reports application health and sync status.

This eliminates manual deployment steps and ensures the Kubernetes cluster always reflects the latest version stored in Git.

---

# 📸 Project Screenshots

> Replace the placeholders below after uploading your screenshots.

---

## Architecture Diagram

```text
screenshots/architecture-diagram.png
```

```markdown
![Architecture](screenshots/architecture-diagram.png)
```

---

## Amazon EKS Cluster

```markdown
![EKS Cluster](screenshots/eks-cluster.png)
```

---

## Kubernetes Pods

```markdown
![Pods](screenshots/pods.png)
```

---

## Kubernetes Services

```markdown
![Services](screenshots/services.png)
```

---

## AWS LoadBalancer

```markdown
![LoadBalancer](screenshots/loadbalancer.png)
```

---

## ArgoCD Dashboard

```markdown
![ArgoCD](screenshots/argocd-dashboard.png)
```

---

## GitHub Repository

```markdown
![GitHub](screenshots/github-repository.png)
```

---

## Running Spring Boot Application

```markdown
![Application](screenshots/application.png)
```

---

# ✅ Deployment Verification

After the deployment, the following components were successfully verified:

| Component | Status |
|-----------|--------|
| Amazon EKS Cluster | ✅ Running |
| AWS Fargate Profile | ✅ Running |
| Kubernetes Deployment | ✅ Created |
| Kubernetes Pods | ✅ Running |
| Kubernetes Service | ✅ Running |
| AWS LoadBalancer | ✅ Provisioned |
| ArgoCD Application | ✅ Synced |
| Helm Chart | ✅ Successfully Deployed |
| Spring Boot Application | ✅ Accessible |

---

# 🧪 Testing the Application

Retrieve the external LoadBalancer endpoint:

```bash
kubectl get svc
```

Example:

```text
EXTERNAL-IP:
a3c0b9c2104e2444197281c98d3b3164-915139049.us-east-1.elb.amazonaws.com
```

Test the application:

```bash
curl http://<LOADBALANCER-DNS>/hello
```

Expected Output:

```text
Hello from Spring Boot running on AWS EKS!
```

---

# ⚠️ Challenges Faced

During the implementation of this project, several practical issues were encountered.

## 1. Fargate Pods Remaining Pending

### Problem

Pods were not scheduled after deployment.

### Cause

The namespace did not match the configured Fargate profile.

### Solution

Created the correct namespace and ensured workloads matched the Fargate profile selectors.

---

## 2. Unable to Connect to the EKS Cluster

### Problem

`kubectl` could not communicate with the cluster.

### Cause

The kubeconfig file was outdated.

### Solution

Updated the kubeconfig:

```bash
aws eks update-kubeconfig \
--region us-east-1 \
--name springboot-eks-cluster
```

---

## 3. LoadBalancer Took Time to Provision

### Problem

The external endpoint remained in the `Pending` state.

### Cause

AWS required additional time to create the Elastic Load Balancer.

### Solution

Waited for provisioning and monitored the service status:

```bash
kubectl get svc -w
```

---

## 4. Application Endpoint Not Responding Initially

### Problem

The application was deployed but the endpoint was not accessible.

### Cause

The application path and service configuration required verification.

### Solution

Reviewed the deployment, service configuration, and container logs using:

```bash
kubectl logs <pod-name>
kubectl describe svc springboot-app
```

After validating the configuration, the application became accessible through the AWS LoadBalancer.

---

## 5. GitOps Synchronization

### Problem

Configuration changes were not immediately reflected in the cluster.

### Cause

The repository path or synchronization settings needed verification.

### Solution

Confirmed the repository URL, Helm chart path, target namespace, and automatic synchronization settings within ArgoCD.

---

# 🛠️ Troubleshooting Commands

```bash
kubectl get pods

kubectl get svc

kubectl get deployment

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl describe svc springboot-app

kubectl get events

kubectl get all

kubectl get applications -n argocd

kubectl get pods -n argocd
```

---

# 💡 Key Learnings

This project provided hands-on experience with:

- GitOps deployment principles
- Amazon EKS architecture
- AWS Fargate serverless compute
- Kubernetes Deployments and Services
- Helm chart creation and customization
- ArgoCD continuous delivery
- Kubernetes troubleshooting
- AWS LoadBalancer integration
- Declarative infrastructure management
- Production-style application deployment

- # 🚀 Best Practices Followed

Throughout this project, several DevOps and Kubernetes best practices were implemented to align with real-world production workflows.

- Followed the GitOps methodology with Git as the single source of truth.
- Packaged the application using Helm for reusable and maintainable deployments.
- Used AWS Fargate to eliminate worker node management.
- Maintained a declarative deployment approach.
- Kept application source code and deployment manifests separated.
- Used Kubernetes namespaces for better resource isolation.
- Leveraged ArgoCD for continuous synchronization and automated deployments.
- Verified deployments using Kubernetes health checks and application logs.
- Managed all application configuration through version control.

---

# 📈 Future Enhancements

This project can be extended with additional production-grade capabilities:

- Implement GitHub Actions for CI before GitOps deployment.
- Integrate Amazon ECR for container image storage.
- Configure HTTPS using AWS ACM and an Application Load Balancer.
- Add Prometheus and Grafana for monitoring and observability.
- Implement Horizontal Pod Autoscaler (HPA).
- Configure ExternalDNS for automatic DNS management.
- Introduce Blue-Green or Canary deployment strategies using Argo Rollouts.
- Store sensitive configuration using AWS Secrets Manager or Kubernetes Secrets.
- Add policy enforcement with Kyverno or OPA Gatekeeper.
- Implement centralized logging using the EFK or Loki stack.

---

# 🧹 Cleanup Commands

Delete the ArgoCD application:

```bash
kubectl delete application springboot-gitops -n argocd
```

Delete the Spring Boot namespace (if applicable):

```bash
kubectl delete namespace springboot
```

Delete the ArgoCD namespace:

```bash
kubectl delete namespace argocd
```

Delete the EKS cluster:

```bash
eksctl delete cluster \
--name springboot-eks-cluster \
--region us-east-1
```

---

# 📚 Useful Commands

## Kubernetes

```bash
kubectl get pods

kubectl get svc

kubectl get deployments

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl get namespaces

kubectl get all

kubectl get events
```

## Helm

```bash
helm lint springboot-chart

helm template springboot-chart

helm install springboot springboot-chart

helm upgrade springboot springboot-chart

helm uninstall springboot
```

## ArgoCD

```bash
kubectl get pods -n argocd

kubectl get applications -n argocd

kubectl port-forward svc/argocd-server \
-n argocd 8080:443
```

## AWS CLI

```bash
aws eks update-kubeconfig \
--region us-east-1 \
--name springboot-eks-cluster

aws eks describe-cluster \
--name springboot-eks-cluster \
--region us-east-1
```

---

# 🏆 Skills Demonstrated

This project demonstrates practical experience with:

### Cloud
- Amazon Web Services (AWS)
- Amazon EKS
- AWS Fargate
- Elastic Load Balancer (ELB)

### Containers & Orchestration
- Docker
- Kubernetes
- Helm

### DevOps
- GitOps
- ArgoCD
- Git & GitHub

### Application
- Spring Boot
- Java

### Deployment
- Declarative Infrastructure
- Continuous Deployment (CD)
- Kubernetes Resource Management

---

# 🎯 Learning Outcomes

After completing this project, I gained practical experience in:

- Designing and deploying applications on Amazon EKS.
- Running Kubernetes workloads on AWS Fargate.
- Packaging applications using Helm Charts.
- Implementing GitOps workflows with ArgoCD.
- Managing Kubernetes resources using declarative configurations.
- Exposing applications through AWS LoadBalancer Services.
- Troubleshooting Kubernetes deployment issues.
- Understanding production deployment strategies and GitOps best practices.

---

# 🤝 Contributing

Contributions, suggestions, and improvements are welcome.

If you have ideas to enhance this project, feel free to fork the repository, create a feature branch, and submit a pull request.

---

# ⭐ Support

If you found this project helpful, consider giving the repository a **Star ⭐**.

It helps others discover the project and motivates continued learning and sharing.

---

# 👨‍💻 Author

**Prameet Kumar**

DevOps | AWS Cloud | Kubernetes | Terraform | Docker | Jenkins | GitOps

GitHub: https://github.com/Prameet-26

---

# 📜 License

This project is licensed under the **MIT License**.

See the `LICENSE` file for more details.

---

# 🎉 Conclusion

This project demonstrates a complete GitOps deployment workflow for a Spring Boot application on AWS EKS using AWS Fargate, Helm, and ArgoCD.

By adopting GitOps principles, the deployment process becomes automated, consistent, and version-controlled. ArgoCD continuously monitors the Git repository and ensures that the Kubernetes cluster remains synchronized with the desired application state.

Beyond deploying an application, this project provided hands-on experience with Kubernetes operations, Helm packaging, AWS networking, serverless Kubernetes using Fargate, and troubleshooting real deployment challenges. These are valuable skills for building and operating cloud-native applications in production environments.

This repository reflects a practical implementation of modern DevOps practices and serves as a solid foundation for more advanced GitOps and Kubernetes projects.

---

## 🌟 Thank You for Visiting!

If this repository helps you learn GitOps, Kubernetes, or AWS EKS, please consider leaving a ⭐ on the repository.

Happy Learning! 🚀
