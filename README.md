📦 What is Kubernetes?
Kubernetes (aka K8s) is an open-source platform for managing containerized applications. 🚀 It automates deployment, scaling, and operations of app containers across clusters of hosts.

🔧 Developed by Google, now maintained by the Cloud Native Computing Foundation (CNCF).

Key Features:

⚙️ Automated Deployment & Scaling

📦 Container Orchestration

🔁 Rolling Updates & Rollbacks

🔍 Self-Healing (Auto-restart, Auto-replace)

🌐 Service Discovery & Load Balancing

🧱 Monolithic vs 🧩 Microservices
🏰 Monolithic Architecture
"All-in-one" giant app

🧱 Single codebase

🔗 Tightly coupled components

😰 Hard to scale or update parts individually

⚠️ One bug can bring down the whole app

👎 Example: Updating the payment module means redeploying the entire app.

🧩 Microservices Architecture
"Divide & conquer" approach

🧬 Each service = small, independent unit

🚀 Can be deployed and scaled individually

🛠 Easier to maintain, test, and debug

💬 Communicates via APIs (often REST or gRPC)

👍 Example: Just want to scale the login service? No problem!

🤖 Why Use Kubernetes?
Kubernetes makes working with microservices easier and smarter! 💡

✅ Automation – Let Kubernetes handle the heavy lifting
✅ Scalability – Scale up/down based on demand 📈📉
✅ High Availability – Keeps your app running, even if parts fail 💪
✅ Portability – Works on any cloud ☁️ or on-prem 🖥
✅ Speed – Ship features faster with CI/CD pipelines 🚀
✅ Resource Efficiency – Optimizes your server usage 💸

🧠 Kubernetes Architecture ![68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f76322f726573697a653a6669743a313430302f312a30537564786575356d51794e336168693146563439412e706e67](https://github.com/user-attachments/assets/a3e8a610-89a9-4e69-a1e8-e62c63d7da33)

🔹 A Kubernetes Cluster consists of two main components:

🧠 Control Plane (Master)
Manages the whole cluster 🧭
Includes:

🧵 API Server – Front door of the cluster; talks to users via kubectl

🧠 etcd – Stores the config/state (like a brain 🧬)

🧑‍🏫 Scheduler – Decides where to run pods (like a matchmaker 💘)

🛠️ Controller Manager – Watches the cluster & makes decisions

🖥️ Worker Nodes
Where your app runs 💻
Each node has:

🔌 Kubelet – Talks to the control plane and manages pods

🔄 Service Proxy – Handles networking, load balancing

📦 Pods – Smallest deployable unit (runs your containers 🐳)

🧍‍♂️ Users interact via kubectl, which talks to the API Server, which coordinates the rest.

🧩 Kubernetes Cluster Types
🧪 Local Dev (Kind / kubectl)

For learning, testing, and development

Lightweight and runs locally (e.g. Docker-based)

Tools: kind, minikube

☁️ Cloud Managed (EKS / AKS / GKE)

Fully managed by cloud providers

Easy to scale, integrate with other cloud services

Examples:

🔷 EKS – AWS

🟩 AKS – Azure

🔴 GKE – Google Cloud

🛠️ Self-Managed (kubeadm)

You install and manage everything

Great for on-prem or custom infrastructure

Tool: kubeadm (official installer)
for installation refer scripts: folder pereristics

⚙️ Kubernetes Cluster Setup Commands
🧪 Kind (Kubernetes IN Docker)
Run a cluster locally using Docker 🐳

bash
Copy
Edit
# 🛠️ Install kind (Linux/macOS)
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind && mv ./kind /usr/local/bin/kind

# 🚀 Create a new cluster
kind create cluster

# 📜 View cluster info
kubectl cluster-info

# ❌ Delete cluster
kind delete cluster
📦 kubectl (Kubernetes CLI)
Your magic wand to control the cluster 🪄

bash
Copy
Edit
# 🛠️ Install kubectl (Linux/macOS)
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && mv kubectl /usr/local/bin/

# 🤝 Connect to cluster (context already set for kind or kubeadm)
kubectl get nodes

# 🔍 Check all pods in all namespaces
kubectl get pods -A

# 📤 Apply YAML config
kubectl apply -f your-config.yaml

# 📜 Describe a resource
kubectl describe pod <pod-name>

# 🧼 Delete a resource
kubectl delete -f your-config.yaml
🛠️ kubeadm (Self-hosted Cluster Setup)
For setting up your own cluster on bare metal or VMs 🖥️

bash
Copy
Edit
# 🧱 Install kubeadm, kubelet & kubectl
apt update && apt install -y kubeadm kubelet kubectl
apt-mark hold kubeadm kubelet kubectl

# 🚀 Initialize the control plane (on master node)
kubeadm init --pod-network-cidr=10.244.0.0/16

# 📋 Setup kubeconfig for kubectl
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config

# 🌐 Install CNI network plugin (e.g., Flannel)
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

# 🤝 Join worker node (run the token command from master output)
kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>

🔄 Switching Cluster Contexts (kubectl)
When you have multiple clusters (local, cloud, or self-hosted), use kubectl context to switch between them 🧙‍♂️

📜 View all contexts
bash
Copy
Edit
kubectl config get-contexts
🧭 Check current context
bash
Copy
Edit
kubectl config current-context
🔁 Switch to a specific cluster context
bash
Copy
Edit
kubectl config use-context <context-name>
🔹 Example Contexts:
Cluster Type	Context Name	Notes
🧪 Kind	kind-kind	Created automatically by kind create cluster
🛠️ Kubeadm	kubernetes-admin@<hostname>	Set during kubeadm init
☁️ Cloud (EKS)	arn:aws:eks:...	Set by aws eks update-kubeconfig
☁️ Cloud (GKE)	gke_project_zone_cluster	Set by gcloud container clusters get-credentials
☁️ Cloud (AKS)	aks-resource-group-cluster	Set by az aks get-credentials
🧼 Bonus: Rename a context for ease
bash
Copy
Edit
kubectl config rename-context long-context-name my-eks
Now you can just:

bash
Copy
Edit
kubectl config use-context my-eks

🚀 Kubernetes Core Concepts & Commands
This section covers Namespace, Pod, Service, and Deployment – the most fundamental Kubernetes resources – with creation steps and essential commands.

🧱 1. What is a Namespace?
A Namespace is a logical partition within a Kubernetes cluster used to isolate and organize resources (like environments: dev, test, prod).

✅ Create a Namespace
bash
Copy
Edit
kubectl create namespace dev
Or using YAML:

yaml
Copy
Edit
# namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
bash
Copy
Edit
kubectl apply -f namespace.yaml
📂 List & Access Namespaces
bash
Copy
Edit
kubectl get namespaces
kubectl get all -n dev         # View all resources in 'dev'
kubectl config set-context --current --namespace=dev   # Set default namespace
📦 2. What is a Pod?
A Pod is the smallest deployable unit in Kubernetes. It can hold one or more containers.

✅ Create a Pod
yaml
Copy
Edit
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: dev
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
bash
Copy
Edit
kubectl apply -f pod.yaml
📂 Pod Commands
bash
Copy
Edit
kubectl get pods -n dev
kubectl describe pod nginx-pod -n dev
kubectl logs nginx-pod -n dev
kubectl exec -it nginx-pod -n dev -- /bin/bash
🌐 3. What is a Service?
A Service exposes your Pod(s) on a network. It enables communication within the cluster or externally.

Types: ClusterIP (default), NodePort, LoadBalancer, ExternalName

✅ Create a Service
yaml
Copy
Edit
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: dev
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
Make sure your Pod has the label app: nginx in its metadata.

bash
Copy
Edit
kubectl apply -f service.yaml
📂 Service Commands
bash
Copy
Edit
kubectl get services -n dev
kubectl describe service nginx-service -n dev
🚀 4. What is a Deployment?
A Deployment manages replica Pods and enables rolling updates, rollbacks, and scaling.

✅ Create a Deployment
yaml
Copy
Edit
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
bash
Copy
Edit
kubectl apply -f deployment.yaml
📂 Deployment Commands
bash
Copy
Edit
kubectl get deployments -n dev
kubectl describe deployment nginx-deployment -n dev
kubectl rollout status deployment nginx-deployment -n dev
kubectl scale deployment nginx-deployment --replicas=3 -n dev
kubectl rollout undo deployment nginx-deployment -n dev
📌 Clean Up Resources
bash
Copy
Edit
kubectl delete -f pod.yaml
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
kubectl delete namespace dev


under mysql /=statefulset  template ,
configmap template,secrets tenplate ahy use this concepts
