# Kubernetes Deployment, ConfigMap and Service Example

This manifest demonstrates how to deploy an application in Kubernetes using:

* ConfigMap
* Deployment
* Service

These are core Kubernetes resources used in almost every production Kubernetes environment.

This example deploys the:

```text
catalogue
```

backend application inside the:

```text
roboshop
```

namespace.

The workflow includes:

* Application configuration management
* Pod deployment
* Health checks
* Internal service exposure

This is a common microservices deployment pattern used in DevOps and cloud-native platforms.

---

# Kubernetes Manifest

```yaml
# --------------------------------------------------------
# CONFIGMAP
# --------------------------------------------------------

apiVersion: v1
kind: ConfigMap

metadata:
  name: catalogue
  namespace: roboshop

# Key-value configuration data
data:

  # Enable MongoDB integration
  MONGO: "true"

  # MongoDB connection URL
  MONGO_URL: "mongodb://mongodb:27017/catalogue"

---

# --------------------------------------------------------
# DEPLOYMENT
# --------------------------------------------------------

apiVersion: apps/v1
kind: Deployment

metadata:

  # Deployment name
  name: catalogue

  namespace: roboshop

  # Labels for Deployment and ReplicaSet
  labels:
    project: roboshop
    component: catalogue
    tier: backend

spec:

  # Number of pod replicas
  replicas: 1

  # --------------------------------------------------------
  # LABEL SELECTOR
  # --------------------------------------------------------

  selector:

    # Select matching pods
    matchLabels:
      project: roboshop
      component: catalogue
      tier: backend

  # --------------------------------------------------------
  # POD TEMPLATE
  # --------------------------------------------------------

  template:

    metadata:

      # Pod labels
      labels:
        project: roboshop
        component: catalogue
        tier: backend

    spec:

      containers:

      # Container definition
      - name: catalogue

        # Docker image
        image: joindevops/catalogue:v1

        # Always pull latest image version
        imagePullPolicy: Always

        # --------------------------------------------------------
        # LOAD ENV VARIABLES FROM CONFIGMAP
        # --------------------------------------------------------

        envFrom:
        - configMapRef:
            name: catalogue

        # --------------------------------------------------------
        # CONTAINER PORT
        # --------------------------------------------------------

        ports:
        - name: liveness-port
          containerPort: 8080

        # --------------------------------------------------------
        # READINESS PROBE
        # --------------------------------------------------------

        readinessProbe:

          httpGet:

            # Health endpoint
            path: /health

            # Port to check
            port: liveness-port

          # Failure threshold
          failureThreshold: 1

          # Probe interval
          periodSeconds: 15

        # --------------------------------------------------------
        # LIVENESS PROBE
        # --------------------------------------------------------

        livenessProbe:

          httpGet:

            path: /health
            port: liveness-port

          failureThreshold: 1
          periodSeconds: 15

---

# --------------------------------------------------------
# SERVICE
# --------------------------------------------------------

apiVersion: v1
kind: Service

metadata:

  # Service name
  name: catalogue

  namespace: roboshop

spec:

  # --------------------------------------------------------
  # POD SELECTOR
  # --------------------------------------------------------

  selector:
    project: roboshop
    component: catalogue
    tier: backend

  # --------------------------------------------------------
  # SERVICE PORTS
  # --------------------------------------------------------

  ports:

    - protocol: TCP

      # Service port
      port: 8080

      # Container port
      targetPort: 8080
```

---

# Important Concepts

# ConfigMap

```yaml
kind: ConfigMap
```

A ConfigMap stores:

* Environment variables
* Application configuration
* External settings

without hardcoding them inside container images.

---

# Why ConfigMap Is Important

Without ConfigMap:

* Configuration gets embedded inside image
* Environment changes require image rebuild

With ConfigMap:

```text
Update ConfigMap
      ↓
Restart Pod
      ↓
New configuration applied
```

---

# envFrom

```yaml
envFrom:
```

Loads all ConfigMap values as environment variables.

Example:

```yaml
MONGO=true
MONGO_URL=mongodb://...
```

becomes available inside container.

---

# Deployment

```yaml
kind: Deployment
```

Deployment manages:

* Pod creation
* Scaling
* Updates
* Self-healing

Deployment automatically creates:

```text
ReplicaSet
      ↓
Pods
```

---

# replicas

```yaml
replicas: 1
```

Defines number of running pod instances.

Example:

```text
1 replica → 1 pod
3 replicas → 3 pods
```

Used for:

* High availability
* Load balancing
* Scalability

---

# Labels

```yaml
labels:
```

Labels are metadata attached to Kubernetes objects.

Used for:

* Pod selection
* Service discovery
* Organization

Example:

```yaml
project: roboshop
component: catalogue
tier: backend
```

---

# selector

```yaml
selector:
```

Defines which pods belong to Deployment or Service.

Kubernetes matches:

```text
Selector Labels
        =
Pod Labels
```

---

# Container Image

```yaml
image: joindevops/catalogue:v1
```

Defines Docker image to run.

Format:

```text
repository/image:tag
```

Example:

```text
joindevops/catalogue:v1
```

---

# imagePullPolicy

```yaml
imagePullPolicy: Always
```

Behavior:

```text
Always pull latest image from registry
```

Useful for:

* Development
* Continuous deployments

Other values:

* `IfNotPresent`
* `Never`

---

# Ports

```yaml
containerPort: 8080
```

Defines application listening port inside container.

---

# Readiness Probe

```yaml
readinessProbe:
```

Checks whether pod is:

```text
Ready to receive traffic
```

If readiness fails:

* Pod stays running
* Service does NOT send traffic

---

# Liveness Probe

```yaml
livenessProbe:
```

Checks whether application is:

```text
Alive and healthy
```

If liveness fails:

* Kubernetes restarts container automatically

---

# /health Endpoint

```yaml
path: /health
```

Health check API endpoint exposed by application.

Common response:

```text
200 OK
```

---

# failureThreshold

```yaml
failureThreshold: 1
```

After:

```text
1 failed check
```

probe marks pod unhealthy.

---

# periodSeconds

```yaml
periodSeconds: 15
```

Probe executes every:

```text
15 seconds
```

---

# Service

```yaml
kind: Service
```

Provides stable network access to pods.

Without Service:

* Pod IP changes frequently
* Applications cannot reliably communicate

Service provides:

```text
Stable DNS + Stable Virtual IP
```

---

# targetPort

```yaml
targetPort: 8080
```

Container port receiving traffic.

---

# Service Flow

```text
Client
   ↓
Service
   ↓
Pod
   ↓
Container Port 8080
```

---

# Kubernetes Deployment Workflow

```text
Apply Manifest
       ↓
ConfigMap Created
       ↓
Deployment Created
       ↓
ReplicaSet Created
       ↓
Pod Created
       ↓
Container Started
       ↓
Health Checks Run
       ↓
Service Exposes Application
```

---

# Real DevOps Use Cases

# ConfigMap

Used for:

* Database URLs
* Feature flags
* Environment configuration

---

# Deployment

Used for:

* Rolling updates
* Self-healing applications
* Scaling microservices

---

# Probes

Used for:

* Application health monitoring
* Auto-recovery
* Zero-downtime deployments

---

# Service

Used for:

* Internal service communication
* Stable networking
* Load balancing

---

# Best Practice

Store:

* Passwords
* Tokens
* Secrets

inside:

```text
Secret
```

NOT inside:

```text
ConfigMap
```

because ConfigMaps are not encrypted.

---

# Benefits of This Manifest

* Decoupled configuration
* Self-healing deployment
* Automatic health monitoring
* Stable networking
* Kubernetes-native application deployment

---

# Why This Manifest Is Important

This manifest demonstrates foundational Kubernetes concepts using:

* Kubernetes
* Deployments
* Services
* ConfigMaps
* Health probes

These are core building blocks used in enterprise cloud-native DevOps platforms.

---

# How to Deploy

1. Save manifest as:

```text
catalogue.yaml
```

2. Apply manifest:

```bash
kubectl apply -f catalogue.yaml
```

3. Verify resources:

```bash
kubectl get pods -n roboshop
kubectl get svc -n roboshop
kubectl get configmap -n roboshop
```

4. Check pod health and service connectivity

---
