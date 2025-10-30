# ⚙️ Traefik Kubernetes Quick Start on Kind with Localhost Exposure

This repository demonstrates a reliable method for implementing the official Traefik Kubernetes Quick Start Guide using a local kind (Kubernetes in Docker) cluster and the official Traefik Helm chart.

The core objective is to configure the setup to allow direct access to the Traefik Dashboard on your host machine's localhost.

---

## 🚀 Step-by-Step Execution

Follow these commands in order to set up the cluster and deploy Traefik.

### Step 1: Create the Kind Cluster

Use the configuration file to create the cluster with the port mapping enabled.

```bash
kind create cluster --config kind-config.yaml
```

### Step 2: Deploy Traefik via Helm

Ensure you have added the Traefik repository before installing the chart with the custom values.

```bash
# Add the Traefik Helm repository (if not already added)
helm repo add traefik https://traefik.github.io/charts

# Update the local Helm chart repository cache
helm repo update

# Install Traefik using the custom NodePort values
helm install traefik traefik/traefik -f traefik-values.yaml
```

### Step 3: Verify Localhost Access

Wait a moment for the Traefik deployment to become ready. Once ready, the dashboard should be accessible directly via localhost on port 80.

```bash
curl localhost/dashboard/
```

Open your browser and navigate to http://localhost/dashboard/ to view the Traefik web interface.


