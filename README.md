# Kubernetes Practice

Two independent Kubernetes setups for hands-on learning — one for local Minikube and one for AWS EKS with ECR.

## Structure

```
k8s/                      # Local Minikube practice
k8s-for-eks-with-ecr/     # AWS EKS + ECR practice
```

---

## k8s — Minikube

Basic Kubernetes concepts: Namespace, Deployment, Service, ConfigMap with volume mount.

**What it deploys:**
- `nginx` — 3 replicas
- ConfigMap — custom HTML page mounted into nginx at `/usr/share/nginx/html`
- ClusterIP Service

```
k8s/
├── namespace.yml
├── configmap.yml     # Custom HTML served by nginx
├── deployment.yml    # 3 replicas, mounts ConfigMap as volume
└── service.yml
```

**Run locally:**

```bash
minikube start
kubectl apply -f k8s/
kubectl get pods -n k8s
minikube service -n k8s nginx-service
```

---

## k8s-for-eks-with-ecr — AWS EKS + ECR

Practice with a real EKS cluster pulling a private Docker image from ECR.

**What it deploys:**
- 4 replicas of a custom image from ECR (`eu-central-1`)
- LoadBalancer Service (provisions an AWS ELB)

```
k8s-for-eks-with-ecr/
├── namespace.yml
├── deployment.yml    # 4 replicas, image from private ECR
└── service.yml       # LoadBalancer type
```

**Deploy to EKS:**

```bash
# Authenticate to ECR
aws ecr get-login-password --region eu-central-1 | \
  docker login --username AWS --password-stdin <account-id>.dkr.ecr.eu-central-1.amazonaws.com

# Configure kubectl
aws eks update-kubeconfig --region eu-central-1 --name <cluster-name>

# Deploy
kubectl apply -f k8s-for-eks-with-ecr/

# Get LoadBalancer hostname
kubectl get svc -n k8s-eks
```

## Key Concepts

- **ConfigMap as volume** — inject config files into pods without rebuilding images
- **ECR image pull from EKS** — node IAM role grants ECR read access automatically
- **LoadBalancer Service** — EKS provisions an AWS ELB automatically via cloud-controller-manager
