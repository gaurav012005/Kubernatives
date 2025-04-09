
# 📦✨ Kubernetes Essentials: StatefulSets, ConfigMaps, Secrets & More!

---

## 🛠️📦 What is a StatefulSet?

A **StatefulSet** manages **stateful applications**. Unlike Deployments, each Pod gets a **stable identity** (name, storage, network) across restarts.

Great for:
- 🛢️ Databases (MySQL, MongoDB)
- 🔁 Queues/Brokers
- 🧠 Apps needing stable storage or ordered updates

### 📄 Sample YAML – StatefulSet

```yaml
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
```

---

## 🧩 What is a ConfigMap?

A **ConfigMap** stores **non-sensitive configuration data** as key-value pairs. Useful for:

🌱 Environment variables  
📄 Config files  
🧵 CLI arguments  

### 📄 Sample YAML – ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  LOG_LEVEL: "debug"
```

👉 **Use in a Pod:**

```yaml
spec:
  containers:
    - name: myapp
      image: myapp:latest
      envFrom:
        - configMapRef:
            name: app-config
```

---

## 🔐🗝️ What is a Secret?

A **Secret** stores **sensitive data** like passwords and tokens, safely encoded with base64.

Used for:
- 🗃️ DB passwords
- 🔏 TLS certificates
- 📲 API keys

### 📄 Sample YAML – Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=        # admin
  password: c2VjcmV0MTIz    # secret123
```

👉 **Use in a Pod:**

```yaml
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
```

---

## 📊💡 Resource Quotas & Limits

🎯 **ResourceQuota**: Limits total usage of resources (CPU, memory, pods) per namespace  
🧱 **LimitRange**: Sets per-Pod or per-Container min/max values  

### 📄 ResourceQuota YAML

```yaml
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
```

### 📄 LimitRange YAML

```yaml
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
```

---

## 🩺🔍 What are Probes?

**Probes monitor container health** for reliability and auto-healing.

✅ **Readiness Probe** → Ready to serve traffic?  
❤️ **Liveness Probe** → Still alive or needs restart?  
🚀 **Startup Probe** → Finished booting?  

### 📄 All Probes in Action

```yaml
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
```

🧩 Add these under your container’s `spec:` in a Pod or Deployment!

---

## ⚠️🚫 Taints & Tolerations

**Taints** prevent Pods from scheduling on a node unless they "tolerate" it.  
**Tolerations** allow specific Pods to run on tainted nodes.

Used to:
- 🛡️ Reserve nodes for special workloads
- 🚧 Prevent mis-scheduling
- 💡 Isolate GPU or high-memory nodes

### 🔧 Taint a Node

```bash
kubectl taint nodes node1 key=value:NoSchedule
```

⛔ “Don’t schedule anything here unless tolerated!”

### 📄 Toleration YAML for a Pod

```yaml
tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```

🧩 Add under `spec:` in your Pod or Deployment.

---

