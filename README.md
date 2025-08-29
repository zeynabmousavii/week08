# Week 08 - Continuous Delivery (CD) to AKS with GitHub Actions

This example demonstrates a robust Continuous Delivery (CD) pipeline using GitHub Actions to deploy your e-commerce microservices and frontend application to an Azure Kubernetes Service (AKS) cluster. Building upon Week 07's Continuous Integration (CI), this setup automates the final step of getting your tested Docker images from Azure Container Registry (ACR) onto your Kubernetes cluster.

## 🚀 Purpose

The primary goals of this example are to illustrate:

- **Automated Deployment:** Deploying containerized applications (backend microservices and frontend) to AKS.
- **Two-Phase CD:** Using separate GitHub Actions workflows for backend and frontend deployment to handle dynamic external IP addresses of LoadBalancer services.
- **Dynamic Configuration:** How to inject dynamically obtained backend service IPs into the frontend's JavaScript configuration during the CD process.
- **GitHub Actions for CD:** Leveraging GitHub's native CI/CD platform for orchestrating deployments.

## 🛠️ Prerequisites

Before you begin, ensure you have the following:

2.  **Azure Kubernetes Service (AKS):** An existing AKS cluster. You will need its **name** and **resource group name**.
3.  **Azure Container Registry (ACR):** An existing ACR where your Docker images are pushed by the CI pipeline. You will need its **name**.
4.  **GitHub Repository Secrets:**
    - In your GitHub repository, go to **Settings** > **Secrets and variables** > **Actions**.
    - Click **New repository secret** for each:
      - `AZURE_AKS_CREDENTIALS`: Paste the **entire JSON output** from the AKS Deployment Service Principal.
      - `AZURE_CREDENTIALS`: If you used a separate SP for ACR push in CI, paste its **entire JSON output** here. (Often, `AZURE_AKS_CREDENTIALS` can also be used for ACR login if it has sufficient permissions, simplifying secrets).

## 📝 Configuration Files

### 1. Verify Kubernetes Manifests (`week08/k8s/*.yaml`)

- **Images**: All Deployment resources must reference to your ACR with proper image name and tags.

```yaml
image: <YOUR_ACR_NAME>.azurecr.io/<image_name>:<image_tag>
```

### 2. Update Backend Pipeline (`.github/workflows/backend-cd.yml`) & Frontend Pipeline (`.github/workflows/frontend-cd.yml`)

Ensure you replace all placeholders value to actual values.
