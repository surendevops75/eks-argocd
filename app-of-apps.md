# Argo CD App of Apps Pattern Example

This manifest demonstrates the **App of Apps** pattern in Argo CD.

The App of Apps pattern is a popular GitOps architecture used in Kubernetes environments to manage multiple applications from a single parent Argo CD application.

Instead of deploying every application manually, a parent application manages child applications stored inside a Git repository.

This approach is commonly used in enterprise DevOps environments for:

* Platform bootstrapping
* Cluster onboarding
* Multi-application management
* GitOps standardization
* Environment management

---

# Argo CD App of Apps Manifest

```yaml
# --------------------------------------------------------
# API VERSION
# --------------------------------------------------------

# Argo CD API group and version
apiVersion: argoproj.io/v1alpha1

# --------------------------------------------------------
# RESOURCE TYPE
# --------------------------------------------------------

# Argo CD Application resource
kind: Application

metadata:

  # Parent application name
  name: platform-root

  # Namespace where Argo CD is installed
  namespace: argocd

  # --------------------------------------------------------
  # FINALIZERS
  # --------------------------------------------------------

  # Ensures child resources are deleted properly
  # before deleting parent application
  finalizers:
    - resources-finalizer.argocd.argoproj.io

spec:

  # --------------------------------------------------------
  # ARGO CD PROJECT
  # --------------------------------------------------------

  project: default

  # --------------------------------------------------------
  # SOURCE CONFIGURATION
  # --------------------------------------------------------

  source:

    # Git repository containing child applications
    repoURL: https://github.com/daws-86s/eks-argocd.git

    # Git branch/tag/commit
    targetRevision: main

    # Path containing child Application manifests
    path: applications

  # --------------------------------------------------------
  # DESTINATION CONFIGURATION
  # --------------------------------------------------------

  destination:

    # Current Kubernetes cluster
    server: https://kubernetes.default.svc

    # Namespace where child applications are created
    namespace: argocd

  # --------------------------------------------------------
  # SYNCHRONIZATION POLICY
  # --------------------------------------------------------

  syncPolicy:

    automated:

      # Automatically remove deleted resources
      prune: true

      # Automatically restore drifted resources
      selfHeal: true
```

---

# Important Concepts

# App of Apps Pattern

The App of Apps pattern means:

```text
One Parent Application
          ↓
Manages Multiple Child Applications
```

Instead of manually creating:

```text
catalogue-app
user-app
cart-app
payment-app
```

the parent application automatically deploys and manages them.

---

# Why App of Apps Is Useful

Without App of Apps:

* Many Application manifests must be managed manually
* Cluster bootstrapping becomes difficult
* GitOps scaling becomes harder

App of Apps helps:

* Centralize management
* Standardize deployments
* Simplify onboarding
* Scale Kubernetes platforms

---

# kind: Application

```yaml
kind: Application
```

This is a standard Argo CD Application resource.

In App of Apps architecture:

* Parent application manages child applications
* Child applications manage Kubernetes workloads

---

# metadata.name

```yaml
name: platform-root
```

Represents:

```text
Root Parent Application
```

This application becomes the entry point for platform deployment.

---

# finalizers

```yaml
finalizers:
  - resources-finalizer.argocd.argoproj.io
```

Finalizers prevent Kubernetes resource deletion until cleanup is completed.

---

# Why Finalizers Are Important

Without finalizers:

* Parent application may get deleted
* Child resources may remain orphaned

With finalizers:

```text
Delete Parent App
        ↓
Delete Child Applications
        ↓
Delete Kubernetes Resources
        ↓
Delete Parent App
```

This ensures proper cleanup.

---

# source Section

Defines where child application manifests are stored.

---

# repoURL

```yaml
repoURL:
```

Git repository containing:

* Child Argo CD Applications
* Kubernetes manifests
* Helm charts

Argo CD continuously monitors this repository.

---

# targetRevision

```yaml
targetRevision: main
```

Defines Git branch/tag/commit to synchronize.

Examples:

```text
main
develop
release-v1
```

---

# path

```yaml
path: applications
```

Folder inside repository containing child application manifests.

Example repository structure:

```text
eks-argocd/
 ├── applications/
 │    ├── catalogue.yaml
 │    ├── user.yaml
 │    ├── cart.yaml
 │    └── payment.yaml
```

---

# destination

```yaml
destination:
```

Defines where applications are deployed.

---

# server

```yaml
server: https://kubernetes.default.svc
```

Represents:

```text
Current Kubernetes cluster
```

Used for in-cluster deployment.

---

# namespace

```yaml
namespace: argocd
```

Namespace where child Argo CD applications are created.

---

# syncPolicy

Controls synchronization behavior.

---

# automated

```yaml
automated:
```

Enables automatic synchronization between:

```text
Git Repository
        ↓
Kubernetes Cluster
```

Without manual approval.

---

# prune

```yaml
prune: true
```

Behavior:

* If child application is removed from Git
* Argo CD also removes it from cluster

Example:

```text
Delete payment.yaml from Git
            ↓
Argo CD removes payment application
```

---

# selfHeal

```yaml
selfHeal: true
```

Automatically restores drifted resources.

Example:

```text
Manual kubectl change
         ↓
Argo CD detects drift
         ↓
Restores original Git configuration
```

---

# App of Apps Workflow

```text
Deploy platform-root Application
                ↓
Argo CD reads applications/ directory
                ↓
Child Applications created
                ↓
Child Applications deploy workloads
                ↓
Entire platform becomes operational
```

---

# Real DevOps Use Cases

# Cluster Bootstrapping

Automatically deploy:

* Monitoring stack
* Ingress controllers
* Applications
* Security tools

when cluster starts.

---

# Multi-Application Management

Manage:

```text
catalogue
user
payment
shipping
cart
```

applications from one root app.

---

# Enterprise GitOps Platforms

Used for:

* Platform engineering
* Multi-team Kubernetes management
* Standardized deployments

---

# Environment Management

Different root applications for:

```text
dev-root
qa-root
prod-root
```

---

# Benefits of App of Apps

* Centralized application management
* Easier Kubernetes onboarding
* Reduced operational complexity
* GitOps standardization
* Better scalability

---

# Why This Pattern Is Important

The App of Apps pattern is one of the most important GitOps architectures used with:

* Argo CD
* Kubernetes

It enables:

* Platform automation
* Large-scale Kubernetes management
* Enterprise GitOps workflows

---

# Best Practice

Store:

```text
All child applications
```

inside Git repository and manage them through:

```text
One Root Application
```

instead of manually creating applications one by one.

---

# Example Repository Structure

```text
eks-argocd/
 ├── applications/
 │    ├── catalogue.yaml
 │    ├── user.yaml
 │    ├── cart.yaml
 │    ├── payment.yaml
 │    └── shipping.yaml
```

Parent app reads all manifests from:

```text
applications/
```

directory.

---

# How to Deploy

1. Install Argo CD in Kubernetes cluster
2. Save manifest as:

```text
platform-root.yaml
```

3. Apply manifest:

```bash
kubectl apply -f platform-root.yaml
```

4. Open Argo CD UI
5. Observe child applications getting created automatically
6. Verify workloads inside Kubernetes cluster

---
