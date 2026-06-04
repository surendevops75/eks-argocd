# Kubernetes Namespace Example

This manifest demonstrates how to create a Namespace in Kubernetes.

A Namespace is a logical partition inside a Kubernetes cluster used to organize and isolate resources.

Namespaces help:
- Separate applications
- Organize environments
- Apply access controls
- Manage resource quotas
- Avoid naming conflicts

In this example, a namespace named `roboshop` is created for the Roboshop application.

---

# Namespace Manifest

```yaml
# Core Kubernetes API version
apiVersion: v1

# Kubernetes Namespace resource
kind: Namespace

metadata:

  # Namespace name
  name: roboshop

  # Labels attached to namespace
  labels:

    # Environment label
    environment: development

    # Project label
    project: roboshop
```

---

# Important Concepts

## Namespace

```yaml
kind: Namespace
```

A Namespace provides logical separation inside a Kubernetes cluster.

Example:

```text
Kubernetes Cluster
│
├── default
├── kube-system
├── monitoring
├── roboshop
└── expense
```

Each namespace can contain:
- Pods
- Deployments
- Services
- ConfigMaps
- Secrets
- Ingresses

---

## Why Namespaces Are Important

Without namespaces:

```text
All resources exist together
```

Problems:
- Naming conflicts
- Difficult management
- Poor isolation

With namespaces:

```text
roboshop namespace
      ↓
Contains catalogue, cart, user, payment services

expense namespace
      ↓
Contains expense application resources
```

This improves organization and security.

---

## Namespace Name

```yaml
name: roboshop
```

Creates a namespace called:

```text
roboshop
```

Resources can later be deployed into this namespace:

```yaml
metadata:
  namespace: roboshop
```

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: catalogue
  namespace: roboshop
```

---

## Labels

```yaml
labels:
```

Labels are metadata attached to Kubernetes resources.

They help with:
- Organization
- Filtering
- Automation
- Resource grouping
- Governance

---

## Environment Label

```yaml
environment: development
```

Identifies which environment the namespace belongs to.

Common values:

```text
development
qa
uat
staging
production
```

Useful for:
- Monitoring
- Cost tracking
- Security policies
- Resource reporting

---

## Project Label

```yaml
project: roboshop
```

Identifies the application or project.

Useful when multiple applications share the same cluster.

Example:

```text
project=roboshop
project=expense
project=monitoring
```

---

## Viewing Namespace Details

List namespaces:

```bash
kubectl get ns
```

View namespace labels:

```bash
kubectl get ns roboshop --show-labels
```

Describe namespace:

```bash
kubectl describe ns roboshop
```

---

## Real DevOps Use Cases

### Application Isolation

Separate applications into different namespaces:

```text
roboshop
expense
monitoring
logging
```

### Environment Separation

Use different namespaces for environments:

```text
roboshop-dev
roboshop-qa
roboshop-prod
```

### Access Control

Using Kubernetes RBAC:

```text
Developers
   ↓
Access only roboshop namespace

Platform Team
   ↓
Access entire cluster
```

### Resource Quotas

Apply CPU, memory, and storage limits per namespace.

---

## Namespace Deployment Flow

```text
Create Namespace
        ↓
Deploy Applications
        ↓
Create Services
        ↓
Create ConfigMaps & Secrets
        ↓
Manage Resources Independently
```

---

## Benefits of Namespaces

- Better resource organization
- Environment isolation
- Simplified access control
- Support for resource quotas
- Reduced naming conflicts
- Easier cluster management

---

## Best Practice

Avoid deploying all workloads into:

```text
default
```

Instead, create dedicated namespaces for:
- Applications
- Teams
- Environments

This improves scalability and maintainability.

---

## Why This Manifest Is Important

Namespaces are one of the most fundamental concepts in Kubernetes.

They provide:
- Logical isolation
- Multi-team support
- Better governance
- Enterprise-grade cluster organization

Almost every production Kubernetes environment relies heavily on namespaces.

---

## How to Deploy

Save as:

```text
roboshop-namespace.yaml
```

Apply the manifest:

```bash
kubectl apply -f roboshop-namespace.yaml
```

Verify creation:

```bash
kubectl get ns
```

Expected output:

```text
NAME       STATUS   AGE
roboshop   Active
```