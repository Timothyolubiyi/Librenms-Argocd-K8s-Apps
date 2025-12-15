# Librenms-Argocd-K8s-Apps
This is a tested, functional approach to deploy LibreNMS on Kubernetes using Argo CD, following GitOps best practices. This setup uses the official LibreNMS Helm chart, which is the most stable and production-ready option.

✅ Architecture Assumptions

Kubernetes cluster (EKS / AKS / GKE / k3s / on-prem)

Argo CD already installed and working

Ingress controller available (NGINX recommended)

Persistent storage (default StorageClass exists)

Namespace: librenms


1️⃣ Git Repository Structure (Required)
librenms-argocd/
├── argocd/
│   └── librenms-app.yaml
└── helm/
    └── values.yaml