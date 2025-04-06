🎩📦 What is Helm?
Helm is the package manager for Kubernetes – like apt, yum, or brew but for K8s!
It allows you to define, install, and manage Kubernetes applications using charts.

🎯 Why Use Helm?
✅ Simplifies complex deployments
🔁 Makes applications reproducible
⚙️ Supports parameterization via values.yaml
🔄 Easy upgrades, rollbacks, and version control
📦 Share and reuse charts across teams or the community

📁 Helm Chart Structure
When you create a Helm chart, it generates this structure:

bash
Copy
Edit
mychart/
├── Chart.yaml         # Metadata about the chart
├── values.yaml        # Default config values
├── charts/            # Dependencies
├── templates/         # K8s resource templates (YAML files with Go templating)
│   ├── deployment.yaml
│   ├── service.yaml
│   └── _helpers.tpl   # Template helpers
🛠️ How to Create a Helm Chart
bash
Copy
Edit
helm create mychart
🔧 This generates all necessary files in a folder named mychart.

📝 Essential Files
Chart.yaml (Chart metadata)
yaml
Copy
Edit
apiVersion: v2
name: mychart
description: A Helm chart for Kubernetes
version: 0.1.0
appVersion: "1.0"
values.yaml (Default values for templates)
yaml
Copy
Edit
replicaCount: 2

image:
  repository: nginx
  tag: stable
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80
templates/deployment.yaml (Sample Deployment with templating)
yaml
Copy
Edit
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
🚀 Common Helm Commands
🔍 Search for Charts
bash
Copy
Edit
helm search hub nginx                  # Search Helm Hub
helm search repo bitnami/nginx        # Search added repo
📦 Add a Helm Repository
bash
Copy
Edit
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
📥 Install a Chart
bash
Copy
Edit
helm install my-nginx bitnami/nginx
⚙️ Install Your Local Chart
bash
Copy
Edit
helm install myapp ./mychart
📝 Customize with --set or -f values.yaml
bash
Copy
Edit
helm install myapp ./mychart --set replicaCount=3
helm install myapp ./mychart -f custom-values.yaml
🔄 Upgrade a Release
bash
Copy
Edit
helm upgrade myapp ./mychart
📃 View Helm Releases
bash
Copy
Edit
helm list
🔙 Rollback
bash
Copy
Edit
helm rollback myapp 1
❌ Uninstall a Release
bash
Copy
Edit
helm uninstall myapp
✅ Tips
Use helm template to preview rendered YAML without installing:

bash
Copy
Edit
helm template myapp ./mychart
Lint your chart for errors:

bash
Copy
Edit
helm lint ./mychart
