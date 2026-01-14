# kustomize-capstone-project
kustomize capstone project

# Kustomize Capstone Project

## Project Title.

Deploying a Web Application on Kubernetes Using Kustomize and GitHub Actions

---

## Project Overview

This project demonstrates how to deploy a web application in a Kubernetes environment using **Kustomize** to manage multiple environments (development, staging, and production) and **GitHub Actions** to automate deployments through a CI/CD pipeline.

The focus of the project is on:

* Environment-specific configuration management
* Infrastructure as Code (IaC) principles
* CI/CD automation
* Secure configuration handling

---

## Objectives

* Deploy a containerized web application on Kubernetes
* Use Kustomize to manage dev, staging, and production configurations
* Automate deployments using GitHub Actions
* Manage ConfigMaps and Secrets securely
* Trigger deployments automatically on code changes

---

## Architecture Overview

**Key Components:**

* **Ubuntu**: Local development environment and CI runner host
* **Minikube**: Local Kubernetes cluster
* **Kubernetes**: Container orchestration platform
* **Kustomize**: Environment customization tool
* **GitHub Actions**: CI/CD automation
* **Docker**: Container runtime (Minikube driver)

---

## Project Structure

```
Kustomize-capstone/
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
├── overlays/
│   ├── dev/
│   ├── staging/
│   └── prod/
└── .github/workflows/
    └── deploy-minikube.yml
```

---

## Implementation Details

### Base Configuration

The `base/` directory contains shared Kubernetes resources:

* Deployment
* Service

These resources define common settings such as container image, ports, probes, and default resource limits.

---

### Environment Overlays

Each environment customizes the base configuration:

| Environment | Replicas | Purpose                   |
| ----------- | -------- | ------------------------- |
| Dev         | 1        | Development & testing     |
| Staging     | 2        | Pre-production validation |
| Production  | 3        | Live environment          |

Overlays customize:

* Replica count
* Resource limits
* Environment variables
* Namespace

---

### ConfigMaps and Secrets

* **ConfigMaps** store non-sensitive configuration data
* **Secrets** store sensitive values
* Kustomize generators are used to create environment-specific configurations
* Deployments consume them using `envFrom`

---

## CI/CD Pipeline (GitHub Actions)

A self-hosted GitHub Actions runner is used on the same Ubuntu host running Minikube.

### Pipeline Workflow

1. Triggered on push to `develop`, `staging`, or `main`
2. Checks out repository code
3. Ensures Minikube is running
4. Applies the appropriate Kustomize overlay
5. Verifies deployment rollout

---

## Testing

* Replica counts were modified in overlay configurations
* Changes were pushed to GitHub
* GitHub Actions automatically deployed updates
* Deployments were verified using `kubectl`

---

## Challenges Faced and Solutions

### 1. kubectl not installed

**Solution:** Installed kubectl using Snap on Ubuntu

### 2. Minikube CPU limitation

**Solution:** Adjusted Minikube CPU and memory settings

### 3. Kubernetes context not set

**Solution:** Started Minikube to generate kubeconfig automatically

### 4. Namespace not found errors

**Solution:** Created required namespaces before applying overlays

### 5. Deployment YAML strict decoding error

**Cause:** Incorrect `envFrom` indentation

**Solution:** Fixed YAML structure using `configMapRef` and `secretRef`

### 6. GitHub workflow push rejected

**Cause:** Missing `workflow` scope in Personal Access Token

**Solution:** Generated a new PAT with `repo` and `workflow` permissions

### 7. Git path errors

**Cause:** Commands run outside repository root

**Solution:** Always executed kubectl and kustomize commands from repo root

---

## Key Learnings

* Kustomize cleanly separates environment configuration
* Kubernetes configurations are OS-agnostic
* CI/CD runners must have access to the target cluster
* YAML indentation is critical in Kubernetes manifests
* Self-hosted runners work best with Minikube

---

## Future Improvements

* Deploy to a managed Kubernetes service (EKS, AKS, or GKE)
* Add Docker image build stage to the pipeline
* Use Sealed Secrets or External Secrets
* Add monitoring and observability tools

---

## Conclusion

This project demonstrates a practical, real-world approach to deploying and managing Kubernetes applications across multiple environments using Kustomize and CI/CD automation.

It highlights both technical implementation and troubleshooting skills essential for modern DevOps workflows.
