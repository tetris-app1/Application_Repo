# 🎮 Application_Repo
   Tetris Kubernetes Deployment with ArgoCD

 🚀 Welcome to the Tetris Kubernetes Deployment repository!
    This project demonstrates how to deploy a full-stack Tetris application on Kubernetes using best practices such as Deployments, Services, ConfigMaps, Redis, and ArgoCD for GitOps-based continuous deployment.
    Any change pushed to this repository is automatically synchronized to the Kubernetes cluster using ArgoCD.

# 📌 Table of Contents

1. 🧠 Project Overview

2. 🧱 Architecture Components

3. 🗂️ Repository Structure

4. ☸️ Kubernetes Resources

   - Backend
   - Frontend
   - Redis

5- 🔁 Ansible Automation for ArgoCD Deployment

6- 🚀 Deployment Steps

7- 🔐 Important Notes


# 🧠 Project Overview

 This repository contains Kubernetes manifests for deploying:

- 🎨 **Frontend**: React-based Tetris UI

- 🧠 **Backend**: Node.js API for game logic

- 🗄️ **Redis**: In-memory datastore for game state

- 🔁 **Install ArgoCD via Helm:** The playbook adds the ArgoCD Helm repository, installs ArgoCD in the argocd namespace, and patches the argocd-server            service to LoadBalancer.

- 🔐 **Configure ArgoCD repository secrets securely:**
     - All sensitive data (GitHub tokens, etc.) are stored in HashiCorp Vault.
     - Ansible fetches the GitHub token from Vault at runtime.
     - A Kubernetes secret is created in ArgoCD with the repository credentials.

- 🗂️ **Create ArgoCD ApplicationSet:**

    - The Application.yaml defines an ApplicationSet that automatically discovers and syncs all directories in the Git repository (apps/*).

    - Sync policy is automated with prune and selfHeal enabled.

 The project follows a GitOps workflow, where Kubernetes state is fully managed through Git.



# 🧱 Architecture Components
```text
        User
         │
         ▼
        Frontend Service (NodePort / ClusterIP)
         │
         ▼
        Backend Service
         │
         ▼
        Redis
```

- Frontend communicates with Backend via internal Kubernetes Service

- Backend stores game state in Redis

- ArgoCD watches GitHub and syncs changes automatically

# 🗂️ Repository Structure

```text
            ├── k8s_files/
      │   ├── backend-deployment.yaml
      │   ├── backend-service.yaml
      │   ├── frontend-deployment.yaml
      │   ├── frontend-service.yaml
      │   ├── redis-deployment.yaml
      │   └── redis-service.yaml
      ├── roles/
      │   └── argocd/
      │       ├── tasks/main.yml
      │       └── files/
      │           ├── secrets.yaml
      │           └── Application.yaml

```
# ☸️ Kubernetes Resources

1- **🧠 Backend Deployment**

    🐳 Image:
    101561167685.dkr.ecr.us-east-1.amazonaws.com/tetris-backend:13
    
    📦 Replicas: 3
    
    🔌 Container Port: 4000
    
    ⚙️ Configuration:
    Loaded from backend-config ConfigMap
    
    ❤️ Readiness Probe:
    TCP check on port 4000


2- **🎨 Frontend Deployment**

    🐳 Image:
    101561167685.dkr.ecr.us-east-1.amazonaws.com/tetris-frontend:7
    
    📦 Replicas: 3
    
    🌐 Container Port: 80
    
    ⚙️ Configuration:
    Loaded from frontend-config ConfigMap
    
    ❤️ Readiness Probe:
    HTTP GET / on port 80

3- **🗄️ Redis Deployment**

    🐳 Image: redis:7.0
    
    📦 Replicas: 1
    
    🔌 Port: 6379
    
    ❤️ Readiness Probe:
    TCP check on port 6379


# 🔁 Ansible Automation for ArgoCD Deployment

1️⃣ Project Overview

This project automates the deployment and configuration of ArgoCD on a Kubernetes cluster using:

 - Ansible

 - Helm

 - Kubernetes

 - ApplicationSet (GitOps)

 - Ansible Vault (Secure GitHub authentication)

The automation performs:

 - Helm installation

 - ArgoCD deployment

 - Service exposure

 - Admin credential retrieval

 - Secure GitHub repository integration

 - ApplicationSet creation for GitOps workflow

2️⃣ Architecture Overview

Deployment Flow:

         Ansible
            ↓
         Helm
            ↓
         Kubernetes Cluster
            ↓
         ArgoCD
            ↓
         GitHub Repository
            ↓
         ApplicationSet → Auto Deployment

3️⃣ ArgoCD Deployment via Helm

Helm repository is added:

      kubernetes.core.helm_repository:
        name: argo
        repo_url: https://argoproj.github.io/argo-helm


Then ArgoCD is installed:

      kubernetes.core.helm:
        name: argocd
        chart_ref: argo/argo-cd
        release_namespace: argocd
        create_namespace: true

4️⃣ Exposing ArgoCD Service

The ArgoCD service is patched to LoadBalancer:

      kubectl patch svc argocd-server -n argocd \
      -p '{"spec": {"type": "LoadBalancer"}}'


Then the External IP is retrieved:

      kubectl get svc argocd-server -n argocd



5️⃣ Retrieving ArgoCD Admin Credentials

The initial admin password is extracted from Kubernetes Secret:

      kubectl -n argocd get secret argocd-initial-admin-secret \
      -o jsonpath="{.data.password}" | base64 --decode


6️⃣ Secure GitHub Authentication using Ansible Vault

To prevent exposing GitHub Personal Access Token (PAT), Ansible Vault is used.

Encrypted using:

      ansible-vault create vault.yml

Vault File (vault.yml)

      github_token: ghp_xxxxxxxxxxxxx



7️⃣ Git Repository Secret for ArgoCD

A Kubernetes Secret is created dynamically:
      
      apiVersion: v1
      kind: Secret
      metadata:
        name: app-repo
        namespace: argocd
        labels:
          argocd.argoproj.io/secret-type: repository
      type: Opaque
      stringData:
        url: https://github.com/tetris-app1/Application_Repo.git
        username: linamohamed93
        password: "{{ github_token }}"


This allows ArgoCD to securely authenticate if  GitHub repository is a private.


8️⃣ ApplicationSet Configuration (GitOps Automation)

The ApplicationSet automatically generates applications from the Git repository:
      
      apiVersion: argoproj.io/v1alpha1
      kind: ApplicationSet

Features:

   1- Git generator

   2- Automatic application creation

   3- Auto-sync enabled

   4- Self-healing

   5- Pruning enabled

      syncPolicy:
        automated:
          prune: true
          selfHeal: true


🔐 Security Considerations

- GitHub token stored encrypted

- No hardcoded credentials

- Secure GitOps workflow

- Minimal token permissions recommended


🎯 Final Result

After execution:

 - ArgoCD deployed automatically
 
 - External URL generated
 
 - Admin credentials retrieved
 
 - Git repository connected securely
 
 - Applications auto-deployed using GitOps
 
 - Self-healing and pruning enabled

# 🚀 Deployment Steps

**1️⃣ Clone the repository**
```sh
git clone https://github.com/tetris-app1/Application_Repo.git
cd Application_Repo
```

**2️⃣ (Optional) Manual Kubernetes Deployment**
```sh
kubectl apply -f k8s_files/
```

**3️⃣ Deploy using ArgoCD (Recommended)**
```sh
kubectl apply -f argocd/tetris-app.yaml
```

**4️⃣ Verify Deployment**
```sh
kubectl get pods
kubectl get svc
```

# 🔐 Important Notes

1- ⚠️ ECR Access
    Ensure your Kubernetes cluster can pull images from Amazon ECR:
      
   - Configure imagePullSecrets
   
   - Or use IAM Roles for Service Accounts (IRSA) on EKS


2- ⚠️ ConfigMaps
    Make sure these ConfigMaps exist before deployment:
    
   - backend-config

   - frontend-config

3- ⚠️ Repository URL
    If you fork or rename the repo, update:
```sh
        repoURL: <your-new-repo>
```
