## To Launch, Create an EC2 Server Instance and proceed with below ##

## STAGE 1 
# 1. Create an EKS Cluster using this command:

In this project I learnt how to install ArgoCD on Kubernetes, deploy an application, and rollback to a previous version using the Web UI.

ArgoCD is a powerful GitOps continuous delivery tool for Kubernetes, and in this step-by-step project ,i will cover:
- Installing ArgoCD on a Kubernetes cluster
- Deploying an application using ArgoCD
- Performing a rollback to a previous version (CLI & Web UI methods)

## CREATE EC2 INSTANCE

1. # INSTALL EKSCTL

##copy all in this stage and paste

# install eksctl

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

eksctl version


2. # INSTALL AWSCLI 

# install aws cli
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" 
sudo apt install unzip 
unzip awscliv2.zip 
sudo ./aws/install

#check the version

aws --version


# Configure AWS

aws configure

3. # INSTALL KUBECTL

## copy all in this stage and paste

#give permissions to kubectl

#1. Download the latest stable kubectl binary
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

#2. Make the binary executable
chmod +x kubectl

#3. Move it to a directory in your PATH
sudo mv kubectl /usr/local/bin/

#4. Verify installation (client version only)
kubectl version --client

4. # Create EKS cluster

  eksctl create cluster \
  --name eks-cluster-100 \
  --version 1.29 \
  --region eu-north-1 \
  --nodegroup-name ng-1 \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3


# Get EKS Cluster service
eksctl get cluster --name eks-cluster-100 --region eu-north-1

 # Update eks kubeconfig once k8s cluster is installed successfully
aws eks update-kubeconfig --name eks-cluster-100

# .......................................................




1. Generate a Valid APP_KEY (IMPORTANT)

LibreNMS will not start without a valid APP_KEY.

# Run this once:

docker run --rm librenms/librenms php artisan key:generate --show

# Then update:

APP_KEY: "base64:PASTE_GENERATED_KEY"

2.  Deploy via Argo CD
kubectl apply -f argocd/librenms-app.yaml


# Then: Open Argo CD UI

- Sync librenms

- Watch pods go Healthy

3. Verify Deployment

kubectl get pods -n librenms
kubectl get ingress -n librenms

# Expected pods:

- librenms

- librenms-mariadb

- librenms-rrdcached

- librenms-cron

4. First Login

- URL: http://librenms.example.com

- Default user created during setup

- Complete web installer if prompted


5. Production Hardening (Recommended)

- External MariaDB (RDS / Cloud SQL)

- Enable TLS (cert-manager)

- Use SealedSecrets / External Secrets

- Increase RRD storage (50–100Gi)


## ✅ Result

You now have:

✔ Fully GitOps-managed LibreNMS

✔ Auto-healing via Argo CD

✔ Persistent storage

✔ Scalable & production-ready setup