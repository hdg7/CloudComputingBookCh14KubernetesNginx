# Nginx on Kubernetes with Kind

This guide explains how to deploy an **Nginx web server** on a Kubernetes cluster created with **Kind**.

---

## **Prerequisites**
- **Docker** installed and running.
- **Kind** installed ([Kind documentation](https://kind.sigs.k8s.io/)).
- **kubectl** installed and configured.
- Kubernetes manifest for Nginx (e.g., `nginx.yaml`).

---

## **Steps to Deploy Nginx on Kubernetes**

### **1. Create a Kind Cluster**
Use your custom Kind configuration file to create a cluster and wait for it to be ready:

```bash
sudo kind create cluster --name mycluster --config kind.config.nginx.yaml --wait 5m
```

- `--name mycluster`: Names the cluster `mycluster`.
- `--config kind.config.nginx.yaml`: Uses your configuration file.
- `--wait 5m`: Waits up to 5 minutes for the cluster to be ready.

Verify the cluster:

```bash
sudo kind get clusters
```

---

### **2. Pull the Nginx Image**
Download the official Nginx image locally:

```bash
sudo docker pull nginx
```

---

### **3. Load the Nginx Image into the Kind Cluster**
Make the image available to all nodes in the Kind cluster:

```bash
sudo kind load docker-image nginx --name mycluster
```

---

### **4. Apply the Nginx Deployment and Service**
Deploy Nginx using your Kubernetes manifest:

```bash
sudo kubectl apply -f nginx.yaml
```

This manifest should define:
- A **Deployment** for the Nginx pods.
- A **Service** exposing Nginx on a port (e.g., 8080).

---

### **5. Check Pods Status**
Ensure the Nginx pods are running:

```bash
sudo kubectl get pods
```

---

### **6. Access Nginx**
If your Service exposes Nginx on `localhost:8080`, test it with:

```bash
curl localhost:8080
```

You should see the default Nginx welcome page HTML.

---

### **7. Delete the Cluster**
When finished, delete the Kind cluster:

```bash
sudo kind delete cluster --name mycluster
```

---

## **Tips**
- To update Nginx configuration, edit `nginx.yaml` and reapply:
  ```bash
  kubectl apply -f nginx.yaml
  ```
- To check logs of an Nginx pod:
  ```bash
  kubectl logs <nginx-pod-name>
  ```
- To expose Nginx externally, consider using a NodePort or Ingress.

---
