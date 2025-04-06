📜🔧 Custom Resource Definitions (CRDs)
🔍 What is a CRD?
A CRD extends Kubernetes by letting you define your own custom resources. It allows users to manage custom application logic like native K8s objects! 🤖

🎯 Why Use CRDs?
Extend Kubernetes without modifying its core

Define your own kind: (e.g., MyApp, Database, etc.)

Used by Operators (👀 see below!)

📄 Sample YAML – Custom Resource Definition
yaml
Copy
Edit
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
After applying this, you can create a custom object like:

yaml
Copy
Edit
apiVersion: stable.example.com/v1
kind: CronTab
metadata:
  name: my-cronjob
spec:
  schedule: "* * * * *"
  image: busybox
🤖 Operators
🔍 What is an Operator?
An Operator is a controller built using CRDs to automate the lifecycle of a complex application (like a database) on Kubernetes.

🚀 Why Use Operators?
Automates install/upgrade/backup/restore of apps

Encapsulates human knowledge into automation

Manages stateful or complex workloads

Example: MongoDB Operator, Prometheus Operator, Elasticsearch Operator

🔌📡 Kubernetes API
🧠 What is the Kubernetes API?
The Kubernetes API Server is the central communication hub. Everything—kubectl, Helm, controllers—talks to the cluster via this API.

⚙️ Useful Commands
bash
Copy
Edit
kubectl api-resources          # List all available resource types
kubectl get --raw /apis        # List all API groups
kubectl explain deployment     # Show API schema for an object
🛠️🐞 Debugging & Troubleshooting
🔍 Logs
bash
Copy
Edit
kubectl logs <pod-name>                  # View logs
kubectl logs <pod-name> -c <container>  # View logs from a specific container
kubectl logs <pod-name> --previous      # Show logs from crashed containers
🔧 Troubleshooting Commands
bash
Copy
Edit
kubectl describe pod <pod-name>         # Show detailed info and events
kubectl get events --sort-by=.metadata.creationTimestamp  # View cluster events
kubectl exec -it <pod-name> -- bash     # Access container shell
kubectl port-forward pod/<pod-name> 8080:80  # Forward port for debugging
🎩📦 Helm – The Kubernetes Package Manager
📦 What is Helm?
Helm is like apt/yum/homebrew for Kubernetes!
It lets you package, share, and deploy applications using reusable templates called charts.

🎯 Why Use Helm?
Automate app installs and upgrades 🚀

Parameterize deployments 🧩

Rollbacks & history 🕰️

Great for CI/CD pipelines ⚙️

📄 Helm Basics
bash
Copy
Edit
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo nginx
helm install my-nginx bitnami/nginx
helm list                        # List releases
helm upgrade my-nginx bitnami/nginx --set replicaCount=3
helm uninstall my-nginx
📁 Chart Template Structure
pgsql
Copy
Edit
mychart/
  Chart.yaml        # Info about the chart
  values.yaml       # Default configuration values
  templates/
    deployment.yaml # K8s resources as templates
    service.yaml
