# Kubernetes Namespace Example

This manifest demonstrates how to create a Namespace in Kubernetes.

A Namespace is a logical partition inside a Kubernetes cluster used to organize and isolate resources.

Namespaces help:

* Separate applications
* Organize environments
* Apply access controls
* Manage resource quotas
* Avoid naming conflicts

In this example, a namespace named:

```text
expense
```

is created for the Expense application.

---

# Namespace Manifest

```yaml
# --------------------------------------------------------
# API VERSION
# --------------------------------------------------------

# Core Kubernetes API version
apiVersion: v1

# --------------------------------------------------------
# RESOURCE TYPE
# --------------------------------------------------------

# Kubernetes Namespace resource
kind: Namespace

metadata:

  # Namespace name
  name: expense

  # Labels attached to namespace
  labels:

    # Environment label
    environment: development

    # Project label
    project: expense
```

---

# Important Concepts

# Namespace

```yaml
kind: Namespace
```

A Namespace provides logical separation inside a Kubernetes cluster.

Think of it as a folder inside the cluster where resources are grouped together.

Example:

```text
Kubernetes Cluster
│
├── default
├── kube-system
├── monitoring
├── expense
└── roboshop
```

Each namespace can contain:

* Pods
* Deployments
* Services
* ConfigMaps
* Secrets

---

# Why Namespaces Are Important

Without namespaces:

```text
All resources exist together
```

Problems:

* Naming conflicts
* Difficult management
* Poor isolation

With namespaces:

```text
expense namespace
      ↓
Contains only expense resources

roboshop namespace
      ↓
Contains only roboshop resources
```

This improves organization and security.

---

# metadata.name

```yaml
name: expense
```

Defines namespace name.

Resources can be deployed into this namespace using:

```yaml
metadata:
  namespace: expense
```

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  namespace: expense
```

---

# Labels

```yaml
labels:
```

Labels are key-value metadata attached to Kubernetes resources.

Used for:

* Filtering
* Organization
* Resource selection
* Automation

---

# Environment Label

```yaml
environment: development
```

Identifies environment type.

Common values:

```text
development
qa
uat
staging
production
```

Useful for:

* Monitoring
* Cost allocation
* Policy enforcement

---

# Project Label

```yaml
project: expense
```

Identifies application or project ownership.

Useful for:

* Resource grouping
* Reporting
* Governance

Example:

```text
project=expense
project=roboshop
project=monitoring
```

---

# Viewing Namespace Labels

You can verify labels using:

```bash
kubectl get ns expense --show-labels
```

or

```bash
kubectl describe ns expense
```

---

# Namespace Isolation

Namespaces provide logical isolation.

Example:

```text
Namespace: expense
 ├── backend
 ├── frontend
 └── mysql

Namespace: roboshop
 ├── catalogue
 ├── cart
 └── user
```

Resources remain organized and separated.

---

# Common Built-in Namespaces

Kubernetes creates some namespaces automatically:

| Namespace       | Purpose                      |
| --------------- | ---------------------------- |
| default         | Default workload namespace   |
| kube-system     | Kubernetes system components |
| kube-public     | Public cluster information   |
| kube-node-lease | Node heartbeat information   |

---

# Real DevOps Use Cases

# Environment Separation

Create separate namespaces:

```text
expense-dev
expense-qa
expense-prod
```

for environment isolation.

---

# Team Isolation

Different teams can have:

```text
frontend-team
backend-team
platform-team
```

namespaces.

---

# Resource Quotas

Apply limits per namespace:

```text
CPU
Memory
Storage
```

to prevent resource abuse.

---

# RBAC Access Control

Grant permissions at namespace level.

Example:

```text
Developers
   ↓
Access only expense namespace

Platform Team
   ↓
Access all namespaces
```

---

# Namespace Workflow

```text
Create Namespace
        ↓
Deploy Applications
        ↓
Create Services
        ↓
Create ConfigMaps
        ↓
Manage Resources Separately
```

---

# Benefits of Namespaces

* Resource organization
* Environment isolation
* Easier management
* RBAC integration
* Resource quota support
* Better governance

---

# Best Practice

Use namespaces for:

```text
Different applications
Different teams
Different environments
```

instead of deploying everything into:

```text
default
```

namespace.

---

# Why This Manifest Is Important

This manifest demonstrates one of the most fundamental concepts in:

* Kubernetes

Namespaces are the foundation for:

* Multi-tenant clusters
* Environment separation
* Security boundaries
* Enterprise Kubernetes management

---

# How to Deploy

1. Save file as:

```text
namespace.yaml
```

2. Apply manifest:

```bash
kubectl apply -f namespace.yaml
```

3. Verify namespace:

```bash
kubectl get ns
```

4. Check labels:

```bash
kubectl describe ns expense
```

Expected output:

```text
NAME      STATUS   AGE
expense   Active
```

---
