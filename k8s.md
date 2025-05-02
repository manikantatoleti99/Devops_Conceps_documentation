https://chatgpt.com/canvas/shared/6814506c5bc08191a235200ef09a573f

**DevOps Starter Guide: Kubernetes with kops on AWS (Free Tier-Friendly)**

---

## 🚀 Introduction to Kubernetes (K8s)

### What is Kubernetes?

Kubernetes (K8s) is an **open-source container orchestration platform** that automates:

- Deployment
- Scaling
- Management of containerized applications

### Why Kubernetes?

While Docker lets you run containers, Kubernetes helps manage them **at scale**, across many machines (nodes), ensuring:

- High availability
- Auto-healing
- Load balancing
- Rolling updates

### Problems with Docker Alone

- No built-in scheduling
- No self-healing or scaling
- Not ideal for production at scale

### kubectl

- A command-line tool to interact with Kubernetes API server
- Used to manage and troubleshoot clusters and workloads

### Kubernetes Architecture

- **Master Node**: API server, controller manager, scheduler, etcd
- **Worker Nodes**: kubelet, container runtime (e.g., Docker, containerd), kube-proxy
- **Pods**: Smallest deployable unit (contains one or more containers)

---

## 🧰 Kubernetes in Real Environments

### Lower Environments (Dev, Staging)

- Install K8s directly using tools like `minikube`, `kubeadm`, or `kind`

### Production Environments

- Use Managed K8s Distributions:
  - EKS (AWS)
  - GKE (Google)
  - AKS (Azure)

### kops (Kubernetes Operations)

- A CLI tool to create, update, delete, and manage production-grade Kubernetes clusters in AWS
- Automates cluster setup: EC2, VPC, IAM, etc.
- Works well for Dev/Test and small-scale production

---

## ✨ Free Tier Setup: Kubernetes on EC2 using kops

### 📅 Pre-requisites

1. AWS account (free-tier eligible)
2. IAM user with EC2, S3, Route53 permissions
3. Ubuntu 22.04 EC2 instance (t2.micro)
4. Install the following on EC2:
   - AWS CLI
   - kubectl
   - kops

### ⚙️ Install Commands on Ubuntu EC2

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Python (required for AWS CLI and future automation)
sudo apt install python3 python3-pip -y

# Install AWS CLI
sudo apt install awscli -y

# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client

# Install kops
curl -LO https://github.com/kubernetes/kops/releases/latest/download/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
kops version
```

### 🔑 AWS CLI Configuration

```bash
aws configure
```

- Provide IAM Access Key & Secret Key
- Region: us-east-1

### 📦 Create S3 Bucket for kops State Store

```bash
aws s3api create-bucket --bucket my-kops-state-ubuntu-demo --region us-east-1
aws s3api put-bucket-versioning \
  --bucket my-kops-state-ubuntu-demo \
  --versioning-configuration Status=Enabled

export KOPS_STATE_STORE=s3://my-kops-state-ubuntu-demo
```

Make it persistent:

```bash
echo 'export KOPS_STATE_STORE=s3://my-kops-state-ubuntu-demo' >> ~/.bashrc
source ~/.bashrc
```

### 📆 Create Kubernetes Cluster (Free Tier Friendly)

```bash
kops create cluster \
--name=k8s.free-tier.local \
--zones=us-east-1a \
--dns=none \
--node-count=1 \
--node-size=t2.micro \
--master-size=t2.micro \
--yes
```

### 🌟 Validate Cluster

```bash
kops validate cluster --name=k8s.free-tier.local
kubectl get nodes
```

### 🌌 Deploy a Sample App

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl get svc
```

### ✅ Access the App

```bash
curl http://<EC2_Public_IP>:<NodePort>
```

---

## ❌ Clean Up Resources (Avoid Charges)

```bash
kops delete cluster --name=k8s.free-tier.local --yes
```

---

## 🚀 Next Steps in DevOps Journey

- Write and deploy YAML files (Pods, Deployments, Services, Ingress)
- Use Helm for Kubernetes package management
- CI/CD with GitHub Actions, Jenkins, ArgoCD
- Learn EKS (Managed K8s by AWS)
- Automate with Terraform, Ansible
- Set up observability (Prometheus + Grafana)
- Manage logs (EFK Stack)

---

You are now on your way from Automation Tester to a **real-world DevOps Engineer**! Let this doc be your foundation — stay consistent, and you’ll master it step by step.

