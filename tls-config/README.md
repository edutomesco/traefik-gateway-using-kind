# ⚙️ Traefik Kubernetes Quick Start on Kind with Localhost HTTPS Exposure

This repository demonstrates a reliable method for implementing the official Traefik Kubernetes Quick Start Guide using a local kind (Kubernetes in Docker) cluster. This configuration specifically uses the NodePort approach combined with Kind's extraPortMapping to expose the necessary ports (80 and 443) to the host machine.

The core objective is to configure Traefik using the modern Kubernetes Gateway API and set up a local TLS certificate for secure HTTPS access to the Traefik Dashboard directly on your host machine's localhost (https://localhost/dashboard/), following the official Traefik guidelines for TLS setup.

---

## 🚀 Step-by-Step Execution

Follow these commands in order to set up the cluster and deploy Traefik.

### Step 0: Generate Self-Signed TLS Certificate

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key -out tls.crt \
  -subj "/CN=localhost"
```

### Step 0: Create Kubernetes TLS Secret

```bash
kubectl create secret tls traefik-dashboard-tls \
  --cert=tls.crt \
  --key=tls.key
```

### Step 1: Create the Kind Cluster

Use the configuration file to create the cluster with the port mapping enabled.

```bash
kind create cluster --config kind-config-tls.yaml
```

### Step 2: Deploy Traefik via Helm

Ensure you have added the Traefik repository before installing the chart with the custom values.

```bash
# Add the Traefik Helm repository (if not already added)
helm repo add traefik https://traefik.github.io/charts

# Update the local Helm chart repository cache
helm repo update

# Install Traefik using the custom NodePort values
helm install traefik traefik/traefik -f traefik-values-tls.yaml
```

### Step 3: Verify Localhost Access

Wait a moment for the Traefik deployment to become ready. Once ready, the dashboard should be accessible directly via localhost on port 80.

```bash
curl https://dashboard.docker.localhost/dashboard/
```

Open your browser and navigate to http://localhost/dashboard/ to view the Traefik web interface.


