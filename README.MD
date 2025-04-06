
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
