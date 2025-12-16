## To Launch, Create an EC2 Server Instance and proceed with below ##

<img width="1737" height="741" alt="Image" src="https://github.com/user-attachments/assets/23bf66cb-65ef-4a1e-86af-ed8450d0df8c" />

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
  --name eks-cluster-101 \
  --version 1.29 \
  --region eu-north-1 \
  --nodegroup-name ng-2 \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3


# Get EKS Cluster service
eksctl get cluster --name eks-cluster-101 --region eu-north-1

 # Update eks kubeconfig once k8s cluster is installed successfully
aws eks update-kubeconfig --name eks-cluster-101


# INSTALL DOCKER ON THE EC2 SERVER
sudo apt update
sudo apt install -y docker.io

sudo systemctl status docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
newgrp docker
groups
ls -l /var/run/docker.sock

# correct output should be same below
srw-rw---- 1 root docker ...

# .......................................................

# STAGE 2


# create argocd namespace


1. kubectl create namespace argocd

# install argocd from argocd repository 

2. kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

<img width="1846" height="492" alt="Image" src="https://github.com/user-attachments/assets/851f3845-6ace-4bb1-9bb4-7e073fb73a25" />


# kubectl get pods -n argocd
kubectl get pods -n argocd

<img width="1894" height="418" alt="Image" src="https://github.com/user-attachments/assets/b7643c38-225e-4e75-939d-7518f90df65e" />

# then get argocd service
kubectl get svc -n argocd

<img width="2156" height="772" alt="Image" src="https://github.com/user-attachments/assets/cd52302a-129f-4973-9c98-30ed162960ce" />

# then get argocd from the webbrowser by edit the service of LoadBalancer
kubectl edit svc argocd-server -n argocd

# change it to clusterIP to LoadBalancer

# then run

kubectl get svc -n argocd

<img width="2170" height="788" alt="Image" src="https://github.com/user-attachments/assets/12007782-cbeb-42c9-ba9d-d0237da67fff" />

# copy the loadbalancer endpoints and take to the browser
a3b8eedf3068e47138671446a49e2078-2029049580.eu-west-2.elb.amazonaws.com

# on the browser

<img width="2276" height="1556" alt="Image" src="https://github.com/user-attachments/assets/5bc79605-f1b6-4fc0-ab8d-bee945c25024" />

# argocd GUI

<img width="2144" height="1540" alt="Image" src="https://github.com/user-attachments/assets/f96c4278-f341-4368-8854-93886a8d3682" />

# username is 
admin
# get argocd password

- Get ArgoCD default password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
