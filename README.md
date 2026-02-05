# Application_Repo
Application Repo for K8S files

# Welcome to the Tetris Kubernetes Deployment repository! 🎮

This project demonstrates how to deploy a full-stack Tetris application on Kubernetes using Deployments, Services, and Redis. It also integrates ArgoCD for automated continuous deployment, ensuring that any updates pushed to the repository are automatically applied to the cluster.

# The project includes:

1- Frontend: React-based UI served via a Kubernetes Deployment.

2- Backend: Node.js API served via a Kubernetes Deployment.

3- Redis: In-memory database for managing game state.

4- ArgoCD: Continuous deployment configuration to automate updates.

# Repository Structure
prep>``` text .
├── 📁 k8s_files
│   ├── 📄 backend-deployment.yaml
│   ├── 📄 backend-service.yaml
│   ├── 📄 frontend-deployment.yaml
│   ├── 📄 frontend-service.yaml
│   ├── 📄 redis-deployment.yaml
│   └── 📄 redis-service.yaml
└── 📁 argocd
    └── 📄 tetris-app.yaml ``` </pre




# Kubernetes Deployments
# Backend

Image: 101561167685.dkr.ecr.us-east-1.amazonaws.com/tetris-backend:13

Port: 4000

Replicas: 3

Configuration: Loaded from backend-config ConfigMap

Readiness Probe: TCP check on port 4000

# Frontend

Image: 101561167685.dkr.ecr.us-east-1.amazonaws.com/tetris-frontend:7

Port: 80

Replicas: 3

Configuration: Loaded from frontend-config ConfigMap

Readiness Probe: HTTP GET / on port 80

# Redis

Image: redis:7.0

Port: 6379

Replicas: 1

Readiness Probe: TCP check on port 6379

# ArgoCD Application

The argocd/tetris-app.yaml defines an ArgoCD Application:

Repository URL: https://github.com/tetris-app1/Application_Repo

Path: k8s_files

Target Revision: HEAD

Namespace: default

Sync Policy: Automated with prune and selfHeal enabled

This ensures automatic synchronization of any updates pushed to the k8s_files folder in the repository.
