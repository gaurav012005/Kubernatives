
# 🚀 Kubernetes All Commands.

Welcome to **Kubernetes in One Shot** – a comprehensive, command-rich guide to mastering Kubernetes fast ⚡. Whether you're a beginner or brushing up your skills, this README has you covered – one command, one concept at a time.

---

## 🧠 Core Concepts

### 🏰 Monolithic vs 🧩 Microservices
| Type | Description |
|------|-------------|
| 🏰 **Monolithic** | Single codebase, tightly coupled, hard to scale/update |
| 🧩 **Microservices** | Small, independent services, easy to scale/debug |

---

### 🧱 Kubernetes Architecture
- 🧠 **Control Plane** – API Server, Scheduler, etcd, Controller Manager
- 🖥️ **Worker Node** – Kubelet, Kube Proxy, Pods
- 📡 Use `kubectl cluster-info` to view cluster components

---

## ⚙️ Cluster Setup

### 🧪 Local with Kind
```bash
kind create cluster --name=tws-cluster --config=config.yml
kubectl config use-context kind-tws-cluster
```

### ☁️ AWS EC2 or Self-Hosted
```bash
kubeadm init --pod-network-cidr=10.244.0.0/16
kubectl apply -f kube-flannel.yml
```

---

## 🔧 Kubectl Basics & Pods

```bash
kubectl get nodes
kubectl run nginx --image=nginx -n nginx
kubectl describe pod nginx -n nginx
```

---

## 📂 Namespaces, Labels, Selectors, Annotations

```bash
kubectl create namespace monitoring
kubectl label namespace monitoring team=devops
kubectl describe namespace monitoring
```

---

## ⚒️ Workloads

### ✅ Deployments
```bash
kubectl apply -f deployment.yml
kubectl scale deployment nginx-deployment --replicas=3 -n nginx
```

### 🛠️ StatefulSets
```bash
kubectl apply -f statefulset.yml
kubectl describe statefulset mysql -n database
```

### ♻️ DaemonSets
```bash
kubectl apply -f daemonset.yml
```

### 📦 ReplicaSets
```bash
kubectl apply -f replicaset.yml
```

### ⏱️ Jobs & CronJobs
```bash
kubectl apply -f job.yml
kubectl apply -f cronjob.yml
```

---

## 🌐 Networking

### 🔗 Services
```bash
kubectl apply -f service.yml
kubectl describe svc nginx-service -n nginx
```

### 🚪 Ingress
```bash
kubectl apply -f ingress.yml
```

### 🛡️ Network Policies
```bash
kubectl apply -f networkpolicy.yml
```

---

## 🗄️ Storage

### 📌 PV & PVC
```bash
kubectl apply -f persistentVolume.yml
kubectl apply -f persistentVolumeClaim.yml
```

### 📦 Storage Classes
```bash
kubectl get storageclass
```

---

## 🧩 ConfigMaps & Secrets

### 🛠️ ConfigMap
```bash
kubectl create configmap app-config --from-file=config.properties
```

### 🔐 Secrets
```bash
kubectl create secret generic db-credentials \
--from-literal=username=admin --from-literal=password=admin123
```

---

## 📈 Scaling & Scheduling

### 🔄 Horizontal Pod Autoscaler
```bash
kubectl autoscale deployment nginx --cpu-percent=50 --min=1 --max=10 -n nginx
```

### 📐 Node Affinity & Taints
```bash
kubectl taint nodes node1 key=value:NoSchedule
kubectl apply -f node-affinity.yml
```

### 🧮 Resource Quotas & Limits
```bash
kubectl apply -f resourcequota.yml
kubectl describe quota my-quota -n dev
```

---

## 🔐 Cluster Admin

### 🔑 RBAC
```bash
kubectl apply -f role.yml
kubectl apply -f rolebinding.yml
```

### 🧬 Custom Resources (CRDs)
```bash
kubectl apply -f crd.yml
kubectl get crd
```

---

## 📊 Monitoring & Logging

### 📏 Metrics Server
```bash
kubectl apply -f metrics-server.yml
kubectl top node
```

### 📡 Prometheus & Grafana
```bash
helm install prometheus-stack prometheus-community/kube-prometheus-stack --namespace monitoring
kubectl port-forward svc/prometheus-stack-grafana 3000:80 -n monitoring --address=0.0.0.0
```

---

## 🧪 Advanced Features

### ⛵ Helm Charts
```bash
helm create my-chart
helm install my-app my-chart -n my-namespace --create-namespace
```

### 🧼 Init & Sidecar Containers
```bash
kubectl apply -f init-container.yml
kubectl apply -f sidecar-container.yml
```

---

## 🔐 Security

```bash
kubectl apply -f podsecuritypolicy.yml
kubectl apply -f secrets-encryption.yml
```

---

## ☁️ Cloud-Native Kubernetes

### 🟦 EKS (AWS)
```bash
eksctl create cluster --name my-cluster
```

### 📏 Cluster Autoscaler
```bash
kubectl apply -f cluster-autoscaler.yml
```

---

## 🛠️ Debugging & Troubleshooting

```bash
kubectl logs pod-name -n namespace
kubectl describe pod pod-name -n namespace
kubectl exec -it pod-name -n namespace -- bash
```

---

## 📁 Projects & Real-World Use Cases

### 💬 **Microservices** – Full Stack ChatApp  
Build & deploy a microservice-based chat app with Ingress, HPA, PVCs.

### 🔁 **CI/CD** – Wanderlust Mega Project  
Learn Kubernetes CI/CD pipelines using Jenkins & ArgoCD.

### 📈 **Monitoring** – K8s Voting App  
Monitor app metrics with Prometheus, Grafana, Node Exporter.

### 🧰 **Starter Kit** – Kubestarter  
Get started with YAMLs, scripts, and Helm for fast deployments.  
📦 [GitHub Repo](https://github.com/your/kubestarter)

---

## 📌 Cleanup
```bash
kubectl delete -f deployment.yml
kubectl delete namespace dev
```

---

## 💡 Pro Tip: Manage Contexts
```bash
kubectl config get-contexts
kubectl config use-context kind-tws-cluster
kubectl config rename-context long-context-name my-cluster
```

---

## 🧭 Project: https://github.com/gaurav012005/full-stack_chatApp



