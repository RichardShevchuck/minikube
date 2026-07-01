# Kubernetes Practice

Two independent Kubernetes setups for learning and experimentation.

## Structure

```
k8s/                      # Local Minikube practice
k8s-for-eks-with-ecr/     # AWS EKS + ECR practice
```

---

## k8s — Minikube

Basic Kubernetes concepts: Deployment, Service, ConfigMap, Namespace.

**Stack:** Minikube, nginx

**What it deploys:**
- Custom namespace `k8s`
- nginx Deployment — 3 replicas
- ConfigMap — custom HTML page mounted into nginx at `/usr/share/nginx/html`
- Service — exposes nginx within the cluster

**Files:**
```
k8s/
├── namespace.yml
├── configmap.yml
├── deployment.yml
└── service.yml
```

**Run locally:**

```bash
minikube start
kubectl apply -f k8s/
kubectl get pods -n k8s
```

---

## k8s-for-eks-with-ecr — AWS EKS + ECR

Practice with a real EKS cluster pulling a Docker image from a private ECR repository.

**Stack:** AWS EKS, AWS ECR, kubectl

**What it deploys:**
- Custom namespace `k8s-eks`
- Deployment — 4 replicas of a custom Docker image from ECR (`eu-central-1`)
- Service — LoadBalancer type (provisions an AWS ELB)

**Files:**
```
k8s-for-eks-with-ecr/
├── namespace.yml
├── deployment.yml
└── service.yml
```

**Deploy to EKS:**

```bash
aws eks update-kubeconfig --region eu-central-1 --name <cluster-name>
kubectl apply -f k8s-for-eks-with-ecr/
kubectl get svc -n k8s-eks   # get LoadBalancer external IP
```
