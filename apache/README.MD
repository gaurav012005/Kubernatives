
# ⚖️📈 Kubernetes Autoscaling

Autoscaling automatically adjusts your application's resources based on real-time demand — saving 💰 and boosting ⚡️ performance.

---

## 🔄 **Horizontal Pod Autoscaler (HPA)**  
📌 Scales the number of pods based on CPU, memory, or custom metrics.

🔹 **Example YAML:**
```
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

🔹 **Commands:**
```bash
kubectl autoscale deployment nginx --cpu-percent=50 --min=2 --max=5
kubectl get hpa
```

---

## 📦 **Vertical Pod Autoscaler (VPA)**  
📌 Adjusts CPU/memory *requests and limits* vertically over time.  
🚨 Requires VPA controller (not enabled by default).

🔹 **Example YAML:**
```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: vpa-demo
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind: Deployment
    name: demo-app
  updatePolicy:
    updateMode: "Auto"
```

🔹 **Install VPA:**
```bash
kubectl apply -f https://github.com/kubernetes/autoscaler/releases/latest/download/vertical-pod-autoscaler.yaml
```

---

## ⚡🕹️ **KEDA – Kubernetes Event-Driven Autoscaler**  
📌 Scales pods based on **external events** like Kafka, Prometheus, Azure Queue, etc.

🔹 **Example YAML:**
```yaml
apiVersion: keda.sh/v1alpha1
kind: ScaledObject
metadata:
  name: keda-scaler
spec:
  scaleTargetRef:
    name: my-worker
  triggers:
  - type: cpu
    metadata:
      type: Utilization
      value: "50"
```

🔹 **Install KEDA:**
```bash
kubectl apply -f https://github.com/kedacore/keda/releases/latest/download/keda.yaml
```

---

## 📏 **Metrics Server (Required for HPA)**  
Collects resource usage metrics (CPU/Memory) for autoscaling.

🔹 **Install Metrics Server:**
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

🔹 **Commands:**
```bash
kubectl top pod       # View pod usage
kubectl top node      # View node usage
```

---

# 🧲📌 Node Affinity

📌 **Node Affinity** ensures pods are scheduled to nodes matching specific labels.

🔹 **Example YAML:**
```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: disktype
          operator: In
          values:
          - ssd
```

✅ This schedules pods only on nodes labeled with `disktype=ssd`.

🔹 **Label a node:**
```bash
kubectl label nodes <node-name> disktype=ssd
```

---

# 🔐🛡️ Role-Based Access Control (RBAC)

RBAC helps you **control access** in Kubernetes — who can do what, where.

---

## 👩‍💼 What is a User?
➡️ A user is an external identity (e.g., developer, admin) managed outside the cluster.

## 🧑‍💼 What is a Service Account?
➡️ A service account is used **by Pods** to access the Kubernetes API.

---

## 📄 Role + RoleBinding YAML for a Service Account
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
  namespace: dev
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: dev
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: dev
subjects:
- kind: ServiceAccount
  name: my-service-account
  namespace: dev
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

## 🧰 Useful RBAC Commands
```bash
# Create a service account
kubectl create serviceaccount my-sa -n dev

# View all service accounts in a namespace
kubectl get serviceaccounts -n dev

# Describe a RoleBinding
kubectl describe rolebinding read-pods -n dev
```

---

Let me know if you want this grouped into a downloadable `.md` file or added to your existing repo structure! 😊
