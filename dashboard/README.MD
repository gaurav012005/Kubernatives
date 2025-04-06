📊✨ Kubernetes Dashboard
🔍 What is it?
The Kubernetes Dashboard is a web-based UI that lets you manage and monitor your cluster visually. It’s like a control panel for your K8s cluster! 😎

🎯 Why Use the Dashboard?
📦 View and manage workloads: Pods, Deployments, ReplicaSets, etc.

⚙️ Monitor cluster health, nodes, and namespaces

🛠️ Create/update/delete resources directly from the UI

🔐 Manage RBAC, secrets, configMaps

🚨 See logs, metrics, and events easily

🚀📦 Installing Kubernetes Dashboard
🔧 Apply the official dashboard manifest:

bash
Copy
Edit
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
📦 This installs:

Dashboard UI

Metrics Scraper

Dashboard service

🛡️🌐 Cluster-Level Access (Admin Role)
By default, dashboard access is limited. Let’s create an admin service account with full access. 🔐

📄 YAML – ClusterRoleBinding for Full Access
yaml
Copy
Edit
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
🚀 Apply the YAML:

bash
Copy
Edit
kubectl apply -f dashboard-admin.yaml
🔑 Get Login Token
bash
Copy
Edit
kubectl -n kubernetes-dashboard create token admin-user
Copy the token and use it to log in to the Dashboard UI.

🌍 Access the Dashboard
bash
Copy
Edit
kubectl proxy
Then open:
📫 http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

🛑 Note: Dashboard is only accessible via kubectl proxy unless you expose it via Ingress or LoadBalancer (not recommended in production without authentication).

