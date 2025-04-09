
- ✅ CRDs  
- 🤖 Operators  
- 🔌 Kubernetes API  
- 🛠️ Debugging  
- 🎩 Helm (with chart structure)

---

```markdown
# 📜🔧 Custom Resource Definitions (CRDs)

### 🔍 What is a CRD?
A **CRD (Custom Resource Definition)** lets you define your **own resource types** in Kubernetes.  
Think of it as creating your own K8s objects like `MyApp`, `Database`, etc. 🤖

---

### 🎯 Why Use CRDs?
- ✅ Extend Kubernetes **without modifying the core**
- 🛠️ Build custom APIs for your apps
- 🤝 Used by Operators to manage complex apps

---

### 📄 Sample CRD Definition
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: crontabs.stable.example.com
spec:
  group: stable.example.com
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                schedule:
                  type: string
                image:
                  type: string
  scope: Namespaced
  names:
    plural: crontabs
    singular: crontab
    kind: CronTab
    shortNames:
      - ct
```

---

### 📄 Sample Custom Resource (Using the CRD)
```yaml
apiVersion: stable.example.com/v1
kind: CronTab
metadata:
  name: my-cronjob
spec:
  schedule: "* * * * *"
  image: busybox
```

---

# 🤖 Operators

### 🔍 What is an Operator?
An **Operator** is a Kubernetes controller built using CRDs. It automates complex app lifecycles like:

- 🚀 Install & Upgrade  
- ♻️ Backup & Restore  
- 🔄 Scaling & Self-Healing

---

### 📦 Examples
- MongoDB Operator  
- Prometheus Operator  
- Elasticsearch Operator

> Operators encapsulate human operational knowledge into code! 🤓

---

# 🔌📡 Kubernetes API

### 🧠 What is the Kubernetes API?
The **Kubernetes API Server** is the central hub. Everything in K8s — from `kubectl` to controllers — communicates through it.

---

### ⚙️ Useful API Commands
```bash
kubectl api-resources             # List all available resource types
kubectl get --raw /apis           # Show all API groups
kubectl explain deployment        # Show schema & fields of resources
```

---

# 🛠️🐞 Debugging & Troubleshooting

### 🔍 Logs
```bash
kubectl logs <pod-name>                   # View logs
kubectl logs <pod-name> -c <container>    # View specific container logs
kubectl logs --previous                   # Logs from crashed pods
```

---

### 🔧 Debugging Commands
```bash
kubectl describe pod <pod-name>           # Events, conditions, IPs, etc.
kubectl get events --sort-by=.metadata.creationTimestamp  # Timeline of events
kubectl exec -it <pod-name> -- bash       # Shell access inside pod
kubectl port-forward pod/<pod-name> 8080:80  # Forward pod port to localhost
```

---

# 🎩📦 Helm – The Kubernetes Package Manager

### 📦 What is Helm?
**Helm** is like `apt`, `yum`, or `brew` — but for Kubernetes!  
It lets you **install, upgrade, and manage applications** as packages called *charts* 🎯

---

### 🎯 Why Use Helm?
- 🚀 Install apps easily
- 🧩 Use variables for flexibility
- 🕰️ Rollbacks & history
- ⚙️ Great for CI/CD workflows

---

### 📄 Helm Basics
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo nginx

helm install my-nginx bitnami/nginx
helm list                                # List Helm releases
helm upgrade my-nginx bitnami/nginx --set replicaCount=3
helm uninstall my-nginx
```

---

### 📁 Chart Template Structure
```
mychart/
├── Chart.yaml        # Info about the chart (name, version, etc.)
├── values.yaml       # Default values (used in templates)
└── templates/        # Actual K8s resources
    ├── deployment.yaml
    └── service.yaml
```

💡 Charts are reusable, versionable, and great for customizing deployments.

---


Happy charting! 🎉
```
