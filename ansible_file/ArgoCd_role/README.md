Role Name
=========

argocd

An Ansible role to install and configure ArgoCD on a Kubernetes cluster using Helm.  
The role also configures service exposure, retrieves access credentials, and deploys GitOps resources.

---

Requirements
------------

- Ansible 2.9+
- Kubernetes cluster running and accessible
- Valid kubeconfig file (example: /home/lina/.kube/config)
- Helm installed on the control machine
- kubectl installed on the control machine
- Internet access to download ArgoCD Helm chart


Role Variables
--------------
        kubeconfig_path--> Path to the kubeconfig file used to access the Kubernetes cluster.

What This Role Does
====================

1- Adds the official ArgoCD Helm repository.

2- Installs ArgoCD in the argocd namespace.

3- Patches argocd-server service to type LoadBalancer.

4- Retrieves the external IP of ArgoCD.

5- Retrieves the initial admin password.

6- Displays ArgoCD login information.

7- Applies repository secret configuration (secrets.yaml).

8- Deploys Application or ApplicationSet (Application.yaml).


Dependencies
------------

Dependencies

No external Galaxy role dependencies.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
       
        - hosts: localhost
          become: false
          roles:
            - role: argocd

Accessing ArgoCD
=================
After successful execution:

    URL: https://<LoadBalancer-IP>
    
    Username: admin
    
    Password: (Automatically retrieved from secret)

You can also retrieve the password manually:

    kubectl -n argocd get secret argocd-initial-admin-secret \
    -o jsonpath="{.data.password}" | base64 --decode

Files Required Inside Role
=========================
    roles/
      argocd/
        tasks/main.yml
        files/secrets.yaml
        files/Application.yaml
        README.md


secrets.yaml → Git repository credentials

Application.yaml → ArgoCD Application or ApplicationSet definition


License
-------

BSD

Author Information
------------------
Lina Mohamed
