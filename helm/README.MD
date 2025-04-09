
# 🎩📦 Helm – The Kubernetes Package Manager

### 🔍 What is Helm?
Helm is the **package manager for Kubernetes** – like `apt`, `yum`, or `brew`, but for K8s!  
It allows you to **define, install, and manage applications** using pre-configured templates called **charts**.

---

### 🎯 Why Use Helm?
✅ Simplifies complex deployments  
🔁 Makes applications reproducible  
⚙️ Parameterize configs via `values.yaml`  
🔄 Easy **upgrades**, **rollbacks**, and **version control**  
📦 Share and reuse charts across teams or the community  

---

# 📁 Helm Chart Structure

When you create a chart, Helm generates this directory structure:

```bash
mychart/
├── Chart.yaml        # Metadata about the chart
├── values.yaml       # Default config values
├── charts/           # Dependencies (optional sub-charts)
├── templates/        # K8s YAML templates with Go templating
│   ├── deployment.yaml
│   ├── service.yaml
│   └── _helpers.tpl  # Custom reusable templates
```

---

# 🛠️ How to Create a Helm Chart

```bash
helm create mychart
```

🔧 This creates a boilerplate chart folder named `mychart`.

---

# 📝 Essential Files

### `Chart.yaml` (Chart Metadata)
```yaml
apiVersion: v2
name: mychart
description: A Helm chart for Kubernetes
version: 0.1.0
appVersion: "1.0"
```

### `values.yaml` (Default Values for Templates)
```yaml
replicaCount: 2

image:
  repository: nginx
  tag: stable
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80
```

### `templates/deployment.yaml` (Deployment Template)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Chart.Name }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Chart.Name }}
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: {{ .Values.service.port }}
```

---

# 🚀 Common Helm Commands

### 🔍 Search for Charts
```bash
helm search hub nginx             # Search Helm Hub
helm search repo bitnami/nginx   # Search in added repos
```

### 📦 Add a Helm Repository
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

### 📥 Install a Chart
```bash
helm install my-nginx bitnami/nginx
```

### ⚙️ Install Your Local Chart
```bash
helm install myapp ./mychart
```

### 📝 Customize with `--set` or `-f`
```bash
helm install myapp ./mychart --set replicaCount=3
helm install myapp ./mychart -f custom-values.yaml
```

### 🔄 Upgrade a Release
```bash
helm upgrade myapp ./mychart
```

### 📃 View Helm Releases
```bash
helm list
```

### 🔙 Rollback a Release
```bash
helm rollback myapp 1
```

### ❌ Uninstall a Release
```bash
helm uninstall myapp
```

---

# ✅ Pro Tips

👀 Preview your templates without installing:
```bash
helm template myapp ./mychart
```

🧪 Lint your chart for errors:
```bash
helm lint ./mychart
```

---

