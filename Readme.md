# Argocd Deployments 
- Application Manifest
- Helm Chart
- Kustomize

This repository contains ArgoCD application definitions for deploying applications and services to a Kubernetes cluster.  

Each application is defined in a separate YAML file and can be deployed using ArgoCD for continuous delivery.

> ✨ Purpose : Prevents direct changes to the cluster and ensures that all deployments are managed through GitOps practices. (Single source of truth)

## 🧠 Key Concepts 

- **GitOps**: Git as the single source of truth
- **Declarative Infrastructure**: YAML manifests define system state
- **Continuous Reconciliation**: Auto-sync between Git & cluster
- **Pull-based Deployment**: Cluster pulls changes instead of CI pushing

## Method 01: Deploy an application using a manifest file

![ArgoCD Application GUI](images/Argo%20image%201.png)
![ArgoCD Application Sync](images/Argo%20image%202.png)

## Method 02: Deploy an application using a Helm chart

![ArgoCD Application GUI](images/Argo%20image%203.png)
![ArgoCD Application GUI](images/Argo%20image%204.png)
-
Verifying the ArgoCD deployment (Helm) in the cluster
![ArgoCD Application GUI](images/Argo%20image%208.png)


## Method 03: Deploy an application using Kustomize

![ArgoCD Application GUI](images/Argo%20image%205.png)
![ArgoCD Application GUI](images/Argo%20image%206.png)
-
Verifying the ArgoCD deployment (Kustomized) in the cluster
![ArgoCD Application GUI](images/Argo%20image%207.png)

## Install the latest stable version of ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Forward ports

```bash
kubectl get services -n argocd
kubectl port-forward service/argocd-server -n argocd 8080:443
```

## Get credentials

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## Install ArgoCD CLI and log in

```bash
brew install argocd
kubectl port-forward svc/argocd-server -n argocd 8080:443
argocd login 127.0.0.1:8080
```

## Create an application using ArgoCD CLI

```bash
argocd app create webapp-kustom-prod \
    --repo https://github.com/devopsjourney1/argo-examples.git \
    --path kustom-webapp/overlays/prod \
    --dest-server https://kubernetes.default.svc \
    --dest-namespace prod
```

## ArgoCD command cheat sheet

```bash
argocd app create <appname>     # Create a new Argo CD application
argocd app list                 # List all applications
argocd app logs <appname>       # Show application logs
argocd app get <appname>        # Show application details
argocd app diff <appname>       # Compare live state with Git source
argocd app sync <appname>       # Sync application with source repository
argocd app history <appname>    # Show deployment history
argocd app rollback <appname>   # Roll back to a previous version
argocd app set <appname>        # Update application configuration
argocd app delete <appname>     # Delete an application
```

My Notes on this :
https://www.notion.so/myblogsite-shalindraperera/ArgoCD-3462e548f396806fbf6dfa6a40625d42?source=copy_link