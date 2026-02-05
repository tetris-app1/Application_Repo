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
.
├── k8s_files/
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   ├── redis-deployment.yaml
│   └── redis-service.yaml
└── argocd/
    └── tetris-app.yaml
