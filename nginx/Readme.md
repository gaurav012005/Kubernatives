# 🚀 NGINX on Kubernetes – Ultimate Beginner-Friendly Guide

Welcome to your **✨NGINX on Kubernetes✨** playground! This README is a complete cheat sheet for deploying and managing NGINX on a Kubernetes cluster using YAML templates, with clear explanations, comments, and all essential `kubectl` commands. Perfect for beginners! 🌱💡

---

## 📁 Repository Contents

| File                  | Description                            |
|-----------------------|----------------------------------------|
| `nginx-deployment.yaml` | Full Deployment template with labels, volumes, resource limits, etc. |
| `nginx-pod.yaml`        | Basic Pod manifest with custom command for NGINX |

---

## 📄 nginx-deployment.yaml – Full Deployment Manifest

This file defines a Deployment with 2 replicas of an NGINX container and attaches an ephemeral volume to serve content.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2  # 🔁 Number of pod replicas
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
          image: nginx:latest
          ports:
            - containerPort: 80  # 🌐 Port exposed by container
          resources:
            requests:
              memory: "64Mi"
              cpu: "250m"
            limits:
              memory: "128Mi"
              cpu: "500m"
          volumeMounts:
            - mountPath: "/usr/share/nginx/html"
              name: nginx-html
      volumes:
        - name: nginx-html
          emptyDir: {}  # ⚠️ Ephemeral - data lost on pod restart
```

### 📘 Key Concepts
- **Deployment** manages rolling updates and replica scaling.
- **Labels** help identify and group resources.
- **emptyDir** volume is useful for temporary data.
- **Resource requests/limits** ensure fair CPU & memory usage.

---

## 📄 nginx-pod.yaml – Simple Pod Manifest with Command

This basic Pod example demonstrates how to run NGINX with a custom entrypoint.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
      command: ["/bin/sh"]
      args: ["-c", "nginx -g 'daemon off;'"]
```

### 📘 Key Concepts
- **command/args** override the container’s default entrypoint.
- **nginx -g 'daemon off;'** keeps NGINX in the foreground so Kubernetes can monitor it.

---

## 🧪 Apply, Inspect, and Debug Resources

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-pod.yaml

kubectl get pods
kubectl describe pod nginx-pod
kubectl logs nginx-pod
```

Use `describe` and `logs` to troubleshoot any Pod issues.

---

## ⚙️ Scaling Deployments

You can scale deployments dynamically:

```bash
kubectl scale deployment nginx-deployment --replicas=5
kubectl edit deployment nginx-deployment
kubectl get deployment nginx-deployment
```

---

## 🔄 Rollout Management & Update Strategy

### ✅ Updating NGINX Version
```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.23
kubectl rollout status deployment nginx-deployment
kubectl rollout history deployment nginx-deployment
kubectl rollout undo deployment nginx-deployment
```

### 🧠 How It Works:
- Old Pods are terminated gradually
- New Pods are launched with the updated image
- Ensures zero downtime (if configured properly)

### 🎯 Optional Rollout Strategy
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

---

## 📌 Controller Types Explained

### 1. **Deployment**
- 🔁 Ensures desired number of pods
- 🚀 Supports rolling updates
- 🎯 Use for stateless applications

### 2. **ReplicaSet**
- ✅ Maintains a stable set of Pods
- 🔗 Usually managed by a Deployment

### 3. **StatefulSet**
- 🆔 Stable, ordered Pod names (e.g. nginx-0)
- 💾 Supports persistent storage per Pod
- 🕒 Ordered, graceful rolling updates
- 🎯 Ideal for databases, Kafka, etc.

---

## 🏷️ Labels & Selectors

```yaml
labels:
  app: nginx
  tier: frontend

selector:
  matchLabels:
    app: nginx
```

- **matchLabels** helps controllers select matching Pods.
- Useful for service discovery and resource grouping.

---

## 📦 ReplicaSet Template

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    app: nginx
spec:
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
          image: nginx:latest
          ports:
            - containerPort: 80
```

---

## 🧾 Jobs & CronJobs – Run Tasks Once or on Schedule

### 🧪 Job Template
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: hello-job
spec:
  template:
    spec:
      containers:
        - name: hello
          image: busybox
          command: ["echo", "Hello from job!"]
      restartPolicy: Never
```

### ⏰ CronJob Template
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello-cron
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: hello
              image: busybox
              command: ["echo", "Hello from cron!"]
          restartPolicy: OnFailure
```

```bash
kubectl get jobs
kubectl get cronjobs
```

---

## 💾 Persistent Volumes & Claims

### PersistentVolume (PV)
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

### PersistentVolumeClaim (PVC)
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

### Pod Using PVC
```yaml
spec:
  volumes:
    - name: storage
      persistentVolumeClaim:
        claimName: my-pvc
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: storage
```

```bash
kubectl get pv
kubectl get pvc
```

✅ When a PVC is "Bound", it's linked to a matching PV.

---

## 🌐 Ingress – External Traffic Routing

### Ingress Resource Template
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```

✅ Routes `http://example.com/` to the backend service named `my-service`.

```bash
kubectl get ingress
```

> ⚠️ Requires NGINX Ingress Controller to be installed in the cluster

---

