# ⚙️ Run Guide: Practical Kubernetes + DevOps Exam

This guide explains how to locally set up and execute the **Mid-Level Practical Exam** solutions using `minikube` or any standard local Kubernetes cluster (like Docker Desktop, kind, etc.).

## 📋 Prerequisites
- **Docker** installed and running
- **Minikube** installed (`minikube start`)
- **kubectl** CLI installed
- **Helm** installed (Optional, for managing Ingress)

---

## 🚀 Execution Steps (Locally)

### Step 1: Start Minikube & Enable Addons
Start your local cluster. We also need to enable the NGINX Ingress controller and Metrics Server for autoscaling.
```bash
minikube start --memory 4096 --cpus 4

# Enable necessary addons for the exam
minikube addons enable ingress
minikube addons enable metrics-server
```

### Step 2: Apply the Configurations
The exam files are numbered in logical order. Apply them sequentially to the cluster.

```bash
# 1. Creates the devops-exam namespace, the NGINX deployment, and the ClusterIP service
kubectl apply -f 01-deployment.yaml

# 2. Creates the ConfigMap and Opaque Secret for environment variables
kubectl apply -f 02-configmap-secret.yaml

# 3. Provisions the 1Gi PersistentVolume and the PersistentVolumeClaim
kubectl apply -f 03-storage.yaml

# 4. Sets up the Ingress rule for exam.local -> nginx-service
kubectl apply -f 04-ingress.yaml

# 5. Configures the Horizontal Pod Autoscaler (HPA)
kubectl apply -f 05-hpa.yaml

# 6. Sets up the ServiceAccount, Role, and RoleBinding (RBAC)
kubectl apply -f 06-rbac.yaml
```

### Step 3: Verify the Deployment
Let's make sure everything is running smoothly.

```bash
# Check all resources in the namespace
kubectl get all,ingress,pvc,secret,configmap -n devops-exam

# Verify the autoscaler is picking up metrics
kubectl get hpa -n devops-exam
```

### Step 4: Testing the Ingress Locally
To test the domain routing `exam.local`, you need to map it to the Minikube IP address.

```bash
# Get the Minikube IP
minikube ip
# Output might be something like: 192.168.49.2
```

Add the IP to your `/etc/hosts` file (requires sudo):
```bash
sudo nano /etc/hosts
# Add this line at the bottom:
# 192.168.49.2  exam.local
```

Now, try reaching the service via curl:
```bash
curl http://exam.local
# You should see the default NGINX welcome page!
```

---

## 🛠️ Simulating the Load for HPA (Section 6)
To see the Autoscaler in action, open a new terminal tab and run a busybox container to generate load against the service:

```bash
kubectl run -i --tty load-generator --rm --image=busybox -n devops-exam -- /bin/sh
# Once inside the shell, hit the nginx service repeatedly:
while true; do wget -q -O- http://nginx-service; done
```
*Wait a few minutes, then in your original terminal, run `kubectl get hpa -n devops-exam -w` to watch the replicas scale up.*

---

## 🔒 Testing the RBAC (Section 7)
Verify that the `exam-user` ServiceAccount has the correct restricted permissions.

```bash
# This should succeed:
kubectl auth can-i list pods -n devops-exam --as=system:serviceaccount:devops-exam:exam-user

# This should fail (Forbidden):
kubectl auth can-i delete pods -n devops-exam --as=system:serviceaccount:devops-exam:exam-user
# Also forbidden (cannot list secrets):
kubectl auth can-i get secrets -n devops-exam --as=system:serviceaccount:devops-exam:exam-user
```

---

## 🧹 Cleanup
When you are done testing, you can delete the entire namespace to clean up all resources created during this exam.

```bash
kubectl delete namespace devops-exam
minikube stop
```
