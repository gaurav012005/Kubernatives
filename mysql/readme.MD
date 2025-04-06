
📦✨ What is a StatefulSet?
A StatefulSet is used to manage stateful applications. Unlike Deployments, each Pod in a StatefulSet has a unique identity (name + network + storage), which is stable across restarts.

🔍 Why use StatefulSet?
For stateful apps like databases (🛢️), queues, or any app that needs:

Stable hostname 🏷️

Stable persistent storage 💾

Ordered deployment, scaling, and deletion 🔢

📄 Sample YAML Template for StatefulSet
'''
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 3
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
        image: nginx:1.21
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
'''
🛠️📦 What is a ConfigMap?
A ConfigMap is used to store non-sensitive configuration data in key-value pairs. Great for passing config to apps without hardcoding!

🎯 Use Cases:
Environment variables 🌱

Config files 📄

Command-line arguments 🧩

📄 Sample YAML Template for ConfigMap
'''
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  LOG_LEVEL: "debug"
👉 How to use it in a Pod:
spec:
  containers:
  - name: myapp
    image: myapp:latest
    envFrom:
    - configMapRef:
        name: app-config
'''
🔐🗝️ What is a Secret?
A Secret is used to store sensitive data (like passwords, API keys, tokens) in base64 encoded format 🔐

🧰 Use Cases:
Database passwords 🗃️

TLS certificates 🔏

API tokens 📲

📄 Sample YAML Template for Secret
'''
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=       # admin
  password: c2VjcmV0MTIz   # secret123
👉 How to use it in a Pod:
spec:
  containers:
  - name: app
    image: myapp:latest
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: username
    - name: DB_PASS
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
'''
📊💡 What are Resource Quotas & Limits?
🎯 ResourceQuota:
Controls how much resource a namespace can use overall (CPU, memory, pods, etc.)

🧱 Limits:
Define the maximum and minimum amount of CPU/Memory per Pod/Container

📄 Sample YAML Template for ResourceQuota
'''
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
'''
📄 Sample YAML Template for Limits
'''
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
'''
🩺🔍 What are Probes?
Probes help check the health of your containers and decide whether to restart them, serve traffic, or wait until ready.

✅ Readiness Probe – Is the app ready to serve traffic?
❤️ Liveness Probe – Is the app still alive, or should it be restarted?
🚀 Startup Probe – Has the app finished starting up?
📄 YAML Example with All Probes
'''
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
'''
🔧 These can be added under a container: in your Pod or Deployment spec.

⚠️🚫 What are Taints and Tolerations?
🧼 Taint:
Marks a node as not suitable for general workloads unless a Pod has a matching toleration.

🤝 Toleration:
A Pod's way of saying "I'm okay to be scheduled on this tainted node."



kubectl taint nodes node1 key=value:NoSchedule
⛔ This tells Kubernetes: "Don’t schedule pods on node1 unless they tolerate this taint."

📄 Sample YAML – Add Toleration to Pod
'''
tolerations:
- key: "key"
  operator: "Equal"
  value: "value"
  effect: "NoSchedule"
'''
🧩 Add this under spec: of your Pod/Deployment to tolerate the taint.

