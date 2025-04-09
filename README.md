# 📦✨ Kubernetes Essentials: StatefulSets, ConfigMaps, Secrets & More! 📦✨

## 🧠 What is Kubernetes?
Kubernetes (aka K8s) is an open-source platform for managing containerized applications. 🚀 It automates deployment, scaling, and operations of app containers across clusters of hosts.

🔧 Developed by Google, now maintained by the Cloud Native Computing Foundation (CNCF).

### 🔑 Key Features:
- ⚙️ Automated Deployment & Scaling
- 📦 Container Orchestration
- 🔁 Rolling Updates & Rollbacks
- 🔍 Self-Healing (Auto-restart, Auto-replace)
- 🌐 Service Discovery & Load Balancing

---

## 🧱 Monolithic vs 🧩 Microservices

### 🏰 Monolithic Architecture
- 🧱 Single codebase
- 🔗 Tightly coupled components
- 😰 Hard to scale or update individually

**Example:** Updating one module (like payment) means redeploying the entire app.

### 🧩 Microservices Architecture
- 🧬 Each service is an independent unit
- 🚀 Deployed and scaled individually
- 💬 Communicates via APIs (REST/gRPC)

**Example:** Scale login service independently without affecting others.

---

## 🤖 Why Use Kubernetes?
- ✅ Automation – Kubernetes handles the heavy lifting
- ✅ Scalability – Scale apps dynamically
- ✅ High Availability – Keeps apps running during failures
- ✅ Portability – Cloud or on-prem ready
- ✅ Speed – CI/CD ready for fast deployments
- ✅ Efficiency – Resource-optimized usage

---

## 🧠 Kubernetes Architecture

### 🧠 Control Plane (Master)
- 🧵 **API Server** – Gateway to cluster
- 🧠 **etcd** – Configuration/state storage
- 🧑‍🏫 **Scheduler** – Places Pods
- 🛠️ **Controller Manager** – Monitors and reacts

### 🖥️ Worker Nodes
- 🔌 **Kubelet** – Pod manager
- 🔄 **Service Proxy (kube-proxy)** – Networking and load balancing
- 📦 **Pods** – Run containers

---

## 🔧 Kubernetes Cluster Types

### 🧪 Local Dev (Kind, Minikube)
- Lightweight, for local development

### ☁️ Cloud Managed
- **EKS (AWS)**
- **AKS (Azure)**
- **GKE (Google Cloud)**

### 🛠️ Self-Managed
- On-prem or custom infra via `kubeadm`

---

## 🛠️ Cluster Setup (Kind)
```bash
# Install Kind
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind && mv ./kind /usr/local/bin/kind

# Create Cluster
kind create cluster

# View Info
kubectl cluster-info

# Delete Cluster
kind delete cluster
```

## 🛠️ Cluster Setup (kubeadm)
```bash
# Install tools
apt update && apt install -y kubeadm kubelet kubectl
apt-mark hold kubeadm kubelet kubectl

# Initialize Master
kubeadm init --pod-network-cidr=10.244.0.0/16

# Configure kubectl
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config

# Add network plugin (Flannel)
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

---

## 📦 Stateful Application with MySQL (StatefulSet + ConfigMap + Secret)

### 📄 Secret YAML (MySQL root password)
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
  namespace: dev
type: Opaque
data:
  mysql-root-password: c2VjdXJlcGFzcw==  # 'securepass'
```

### 📄 ConfigMap YAML (MySQL settings)
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-config
  namespace: dev
data:
  MYSQL_DATABASE: appdb
  MYSQL_USER: user
  MYSQL_PASSWORD: pass123
```

### 📄 StatefulSet YAML (MySQL)
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  namespace: dev
spec:
  serviceName: "mysql"
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: mysql-root-password
        - name: MYSQL_DATABASE
          valueFrom:
            configMapKeyRef:
              name: mysql-config
              key: MYSQL_DATABASE
        - name: MYSQL_USER
          valueFrom:
            configMapKeyRef:
              name: mysql-config
              key: MYSQL_USER
        - name: MYSQL_PASSWORD
          valueFrom:
            configMapKeyRef:
              name: mysql-config
              key: MYSQL_PASSWORD
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-persistent-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

---

## ✅ Apply Resources in Order
```bash
kubectl create namespace dev
kubectl apply -f secret.yaml
kubectl apply -f configmap.yaml
kubectl apply -f mysql-statefulset.yaml
```

---

## 📊 Bonus Concepts

### 🧱 Resource Quota YAML
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 2Gi
    limits.cpu: "8"
    limits.memory: 4Gi
```

### 🧱 Limit Range YAML
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-cpu-limits
  namespace: dev
spec:
  limits:
  - default:
      cpu: 500m
      memory: 256Mi
    defaultRequest:
      cpu: 250m
      memory: 128Mi
    type: Container
```

### 🩺 Probes Example (Liveness, Readiness, Startup)
```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5

startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

### ⚠️ Taints & Tolerations
```bash
kubectl taint nodes node1 key=value:NoSchedule
```

```yaml
# Pod toleration
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```

---

