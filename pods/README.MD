🐳🆚 Init Container vs Sidecar Container
⚡ TL;DR:
Feature	🛫 Init Container	🤝 Sidecar Container
Purpose	Run before main container	Run alongside main container
Lifecycle	Runs once, then exits	Runs continuously with main app
Use Case	Setup, wait-for, pre-tasks	Logging, proxies, syncing, helpers
Restart	Re-run if it fails	Restarts with the pod
🛫 Init Containers
🔍 What is it?
An Init Container is a special container that runs before the main application starts. It’s used for setup tasks like waiting for services or initializing configs. 🧼

🧠 Why Use It?
Wait for a DB to become available 📡

Pull data or configs before app starts 📦

Perform security checks 🔐

📄 YAML Template – Init Container
yaml
Copy
Edit
apiVersion: v1
kind: Pod
metadata:
  name: init-demo
spec:
  containers:
  - name: app-container
    image: busybox
    command: ['sh', '-c', 'echo 🚀 Main container started! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'echo ⏳ Waiting for service... && sleep 10']
📝 The main container won’t start until the init-myservice finishes.

🤝 Sidecar Containers
🔍 What is it?
A Sidecar Container runs alongside the main container, sharing the same Pod and resources. It’s often used to extend or support the main container.

🧠 Why Use It?
Logging agents 📝

Proxy servers 🌐

Backup/sync processes ☁️

Service mesh sidecars (like Istio Envoy) 🔄

📄 YAML Template – Sidecar Container
yaml
Copy
Edit
apiVersion: v1
kind: Pod
metadata:
  name: sidecar-demo
spec:
  containers:
  - name: app-container
    image: busybox
    command: ['sh', '-c', 'echo 🚀 App is running! && sleep 3600']
  - name: sidecar-logger
    image: busybox
    command: ['sh', '-c', 'while true; do echo 📚 Logging app...; sleep 5; done']
🧩 Both containers run at the same time and can share volumes and network.

🔗 Combine Both – Init + Sidecar
yaml
Copy
Edit
apiVersion: v1
kind: Pod
metadata:
  name: combo-demo
spec:
  volumes:
  - name: shared-data
    emptyDir: {}

  initContainers:
  - name: init-downloader
    image: busybox
    command: ['sh', '-c', 'echo ⏬ Downloading files... && sleep 5']
    volumeMounts:
    - name: shared-data
      mountPath: /data

  containers:
  - name: main-app
    image: busybox
    command: ['sh', '-c', 'echo 🚀 App started && sleep 3600']
    volumeMounts:
    - name: shared-data
      mountPath: /data

  - name: sidecar-helper
    image: busybox
    command: ['sh', '-c', 'while true; do echo 📦 Watching data...; sleep 5; done']
    volumeMounts:
    - name: shared-data
      mountPath: /data
🧠 Here:

init-downloader runs first

main-app and sidecar-helper run together

They share data via emptyDir volume
