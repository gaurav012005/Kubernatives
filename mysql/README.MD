📦✨ Kubernetes Essentials: StatefulSets, ConfigMaps, Secrets & More!
📦✨ What is a StatefulSet?
A StatefulSet is used to manage stateful applications. Unlike Deployments, each Pod gets a unique and stable identity (hostname, network, and storage) across restarts. Perfect for apps like:

🛢️ Databases (e.g., MySQL, MongoDB)

🔁 Queues and brokers

🧠 Anything that needs stable storage and ordered updates

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
A ConfigMap lets you store non-sensitive config as key-value pairs 🧩. Super helpful for:

🌱 Environment variables

📄 Config files

🧵 CLI arguments

📄 Sample YAML Template for ConfigMap
'''

apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  LOG_LEVEL: "debug"
'''

👉 Use in Pod:

'''
spec:
  containers:
  - name: myapp
    image: myapp:latest
    envFrom:
    - configMapRef:
        name: app-config
'''

🔐🗝️ What is a Secret?
A Secret stores sensitive data (passwords, tokens) safely using base64 encoding 🔐

Great for:

🗃️ DB passwords

🔏 TLS certificates

📲 API keys

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
'''

👉 Use in Pod:

'''
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
🎯 ResourceQuota restricts total usage (CPU, memory, pods) within a namespace.
🧱 LimitRange defines per-Pod/Container min and max resource usage.
Helps prevent one app from hogging all resources! 💪

📄 ResourceQuota YAML
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

📄 LimitRange YAML
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
Probes monitor container health and status:

✅ Readiness: Ready to serve traffic?

❤️ Liveness: Still running or needs restart?

🚀 Startup: Finished booting up?
Helps avoid downtime and improve reliability 🚦

📄 All Probes in Action
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
🧩 Add these under your container's spec in Pod or Deployment!

⚠️🚫 What are Taints and Tolerations?
Taints allow nodes to repel Pods by default 🧼
Tolerations let Pods opt in to land on such nodes 🤝
Used to isolate workloads, reserve resources, or protect sensitive nodes 🛡️
Prevents scheduling mistakes and improves node usage 🧠
Useful for dedicated nodes, GPUs, or special hardware setups ⚙️


kubectl taint nodes node1 key=value:NoSchedule
⛔ This means: “Don't schedule here unless tolerated!”

📄 Toleration YAML for a Pod
'''
tolerations:
- key: "key"
  operator: "Equal"
  value: "value"
  effect: "NoSchedule"
'''
🧩 Add this under spec: in your Pod or Deployment.
