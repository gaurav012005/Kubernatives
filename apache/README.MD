⚖️📈 Kubernetes Autoscaling
Autoscaling helps your cluster automatically adjust resources based on load 🚀

🔄 Horizontal Pod Autoscaler (HPA)
📌 Scales the number of pods based on CPU/memory or custom metrics.

yaml
Copy
Edit
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
📦 Vertical Pod Autoscaler (VPA)
📌 Adjusts resource requests/limits (CPU, memory) of Pods vertically.

🚨 Needs a VPA controller (not enabled by default)

yaml
Copy
Edit
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: vpa-demo
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       Deployment
    name:       demo-app
  updatePolicy:
    updateMode: "Auto"
⚡🕹️ KEDA (Kubernetes Event-Driven Autoscaler)
📌 Scales pods based on external events (like Kafka, Azure Queue, Prometheus, etc.)

yaml
Copy
Edit
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
✅ Install KEDA: kubectl apply -f https://github.com/kedacore/keda/releases/latest/download/keda.yaml

📏 Metrics Server (Required for HPA)
Metrics Server collects pod resource usage for autoscaling.

bash
Copy
Edit
kubectl top pod       # View pod usage
kubectl top node      # View node usage
🔧 Install it:

bash
Copy
Edit
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
🧲📌 What is Node Affinity?
📌 Node affinity lets you schedule pods to specific nodes based on labels.

📄 YAML – Node Affinity Example
yaml
Copy
Edit
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: disktype
          operator: In
          values:
          - ssd
🎯 Only schedule Pods on nodes labeled with disktype=ssd.

🔐🛡️ Role-Based Access Control (RBAC)
RBAC controls who can do what in your cluster — 🧍‍♂️ Users, 🧰 ServiceAccounts, and Roles.

🧑‍💼 What is a Service Account?
A ServiceAccount is used by Pods to interact with the Kubernetes API.

👩‍💼 What is a User?
Users are typically external (admins, devs) authenticated outside the cluster.

📄 YAML – Role + RoleBinding for ServiceAccount
yaml
Copy
Edit
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
  namespace: dev
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
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
🧰 Useful RBAC Commands
bash
Copy
Edit
kubectl create serviceaccount my-sa -n dev
kubectl get serviceaccounts -n dev
kubectl describe rolebinding read-pods -n dev
