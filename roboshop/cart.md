# Kubernetes Cart Service Deployment Example

This manifest demonstrates how to deploy the `cart` microservice in Kubernetes using:

* ConfigMap
* Deployment
* Service
* Health Probes

The cart service is part of the:

```text
roboshop
```

microservices application and communicates with:

* Redis
* Catalogue service

This is a common cloud-native microservices deployment pattern used in modern DevOps platforms.

The manifest demonstrates:

* Application configuration management
* Inter-service communication
* Container deployment
* Kubernetes networking
* Health monitoring

---

# Kubernetes Manifest

```yaml
# --------------------------------------------------------
# CONFIGMAP
# --------------------------------------------------------

apiVersion: v1
kind: ConfigMap

metadata:

  # ConfigMap name
  name: cart

  # Kubernetes namespace
  namespace: roboshop

# Application configuration
data:

  # Redis hostname
  REDIS_HOST: "redis"

  # Catalogue service hostname
  CATALOGUE_HOST: "catalogue"

  # Catalogue service port
  CATALOGUE_PORT: "8080"

---

# --------------------------------------------------------
# DEPLOYMENT
# --------------------------------------------------------

apiVersion: apps/v1
kind: Deployment

metadata:

  # Deployment name
  name: cart

  namespace: roboshop

  # Labels for Deployment/ReplicaSet
  labels:
    project: roboshop
    component: cart
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
      component: cart
      tier: backend

  # --------------------------------------------------------
  # POD TEMPLATE
  # --------------------------------------------------------

  template:

    metadata:

      # Pod labels
      labels:
        project: roboshop
        component: cart
        tier: backend

    spec:

      containers:

      # Container definition
      - name: cart

        # Docker image
        image: joindevops/cart:v1

        # Always pull latest image
        imagePullPolicy: Always

        # --------------------------------------------------------
        # LOAD CONFIGMAP VARIABLES
        # --------------------------------------------------------

        envFrom:
        - configMapRef:
            name: cart

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

            # Port name
            port: liveness-port

          # Failed attempts before marking unready
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
  name: cart

  namespace: roboshop

spec:

  # --------------------------------------------------------
  # POD SELECTOR
  # --------------------------------------------------------

  selector:
    project: roboshop
    component: cart
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

ConfigMap stores:

* Environment variables
* External configuration
* Application settings

without rebuilding container images.

---

# Application Configuration

This ConfigMap provides:

```yaml
REDIS_HOST: "redis"
CATALOGUE_HOST: "catalogue"
CATALOGUE_PORT: "8080"
```

These values allow cart service to communicate with:

* Redis service
* Catalogue service

---

# Inter-Service Communication

Inside Kubernetes:

```text
cart
  ↓
catalogue
  ↓
redis
```

Services communicate using:

```text
Kubernetes Service DNS
```

Example:

```text
http://catalogue:8080
```

---

# Deployment

```yaml
kind: Deployment
```

Deployment manages:

* Pod creation
* Scaling
* Rolling updates
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

Defines number of pod instances.

Examples:

```text
1 replica → 1 pod
3 replicas → 3 pods
```

Used for:

* High availability
* Horizontal scaling

---

# Labels

```yaml
labels:
```

Labels are metadata used for:

* Pod selection
* Service discovery
* Resource organization

Example:

```yaml
project: roboshop
component: cart
tier: backend
```

---

# selector

```yaml
selector:
```

Matches pods using labels.

Kubernetes connects:

```text
Service Selector
        ↓
Matching Pods
```

---

# Container Image

```yaml
image: joindevops/cart:v1
```

Defines Docker image used by container.

Format:

```text
repository/image:tag
```

Example:

```text
joindevops/cart:v1
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

* Continuous deployments
* Development environments

---

# envFrom

```yaml
envFrom:
```

Loads all ConfigMap values as environment variables.

Application receives:

```text
REDIS_HOST
CATALOGUE_HOST
CATALOGUE_PORT
```

inside container automatically.

---

# Container Port

```yaml
containerPort: 8080
```

Defines application listening port inside container.

---

# Readiness Probe

```yaml
readinessProbe:
```

Checks whether application is:

```text
Ready to receive traffic
```

If readiness fails:

* Pod stays running
* Service stops routing traffic

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

Typical response:

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
1 failed health check
```

pod becomes unhealthy.

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

Provides stable networking for pods.

Without Service:

* Pod IPs change frequently
* Applications cannot reliably communicate

Service provides:

```text
Stable DNS + Stable Virtual IP
```

---

# Service Networking Flow

```text
Client
   ↓
cart Service
   ↓
cart Pod
   ↓
Container Port 8080
```

---

# targetPort

```yaml
targetPort: 8080
```

Port exposed inside container.

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

* Service URLs
* Database connections
* Feature flags
* Runtime configuration

---

# Deployment

Used for:

* Rolling updates
* Self-healing applications
* Scaling microservices

---

# Health Probes

Used for:

* Auto-recovery
* Zero-downtime deployments
* Kubernetes health monitoring

---

# Service

Used for:

* Internal communication
* Service discovery
* Load balancing

---

# Microservices Communication

Cart service communicates with:

* Redis cache
* Catalogue backend

through Kubernetes internal networking.

---

# Best Practice

Store:

* Passwords
* API tokens
* Database secrets

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
* Cloud-native microservices deployment

---

# Why This Manifest Is Important

This manifest demonstrates foundational Kubernetes microservices concepts using:

* Kubernetes
* Deployments
* Services
* ConfigMaps
* Health probes

These are core building blocks used in enterprise cloud-native DevOps environments.

---

# How to Deploy

1. Save manifest as:

```text
cart.yaml
```

2. Apply manifest:

```bash
kubectl apply -f cart.yaml
```

3. Verify resources:

```bash
kubectl get pods -n roboshop
kubectl get svc -n roboshop
kubectl get configmap -n roboshop
```

4. Test service connectivity and health endpoint

---
