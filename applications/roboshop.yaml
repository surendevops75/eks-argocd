# Argo CD Roboshop Application Deployment Example

This manifest demonstrates how to deploy the complete:

```text
roboshop
```

application into Kubernetes using Argo CD.

Argo CD follows the:

```text
GitOps
```

deployment model where:

```text
Git Repository = Single Source of Truth
```

Instead of manually applying Kubernetes manifests using:

```bash
kubectl apply -f
```

Argo CD continuously synchronizes Kubernetes cluster state with the manifests stored in Git repository.

This manifest deploys all resources stored inside the:

```text
roboshop/
```

directory from the Git repository.

---

# Argo CD Application Manifest

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

  # Application name visible in Argo CD UI
  name: roboshop

  # Namespace where Argo CD is installed
  namespace: argocd

spec:

  # --------------------------------------------------------
  # ARGO CD PROJECT
  # --------------------------------------------------------

  # Logical grouping for applications
  project: default

  # --------------------------------------------------------
  # SOURCE CONFIGURATION
  # --------------------------------------------------------

  source:

    # Git repository containing Kubernetes manifests
    repoURL: https://github.com/daws-86s/eks-argocd.git

    # Git branch/tag/commit to synchronize
    targetRevision: main

    # Repository folder containing manifests
    path: roboshop

  # --------------------------------------------------------
  # DESTINATION CONFIGURATION
  # --------------------------------------------------------

  destination:

    # Current Kubernetes cluster
    server: https://kubernetes.default.svc

    # Namespace where Argo CD application exists
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

# Argo CD Application

```yaml
kind: Application
```

Core Argo CD resource used to:

* Deploy applications
* Synchronize Kubernetes manifests
* Manage GitOps workflows

This Application manages:

```text
Roboshop application resources
```

inside Kubernetes cluster.

---

# GitOps Model

Argo CD implements:

```text
GitOps
```

where:

```text
Git repository becomes deployment source
```

Deployment flow:

```text
Developer pushes changes to Git
                ↓
Argo CD detects changes
                ↓
Kubernetes cluster gets synchronized
```

---

# metadata.name

```yaml
name: roboshop
```

Application name displayed inside Argo CD UI.

Represents:

```text
Entire Roboshop platform
```

deployment.

---

# metadata.namespace

```yaml
namespace: argocd
```

Namespace where Argo CD is installed.

Application resource must exist here.

Usually:

```text
argocd
```

---

# project

```yaml
project: default
```

Argo CD project provides:

* Logical grouping
* RBAC controls
* Repository access rules
* Cluster restrictions

`default` project allows standard deployments.

---

# source Section

Defines:

```text
Where Kubernetes manifests come from
```

---

# repoURL

```yaml
repoURL:
```

Git repository containing:

* Kubernetes YAML manifests
* Helm charts
* Kustomize configurations

Argo CD continuously monitors this repository.

---

# targetRevision

```yaml
targetRevision: main
```

Defines Git reference to synchronize.

Can be:

* Branch
* Tag
* Commit SHA

Examples:

```text
main
develop
v1.0.0
```

---

# path

```yaml
path: roboshop
```

Folder inside repository containing Kubernetes manifests.

Example repository structure:

```text
eks-argocd/
 ├── roboshop/
 │    ├── catalogue.yaml
 │    ├── cart.yaml
 │    ├── user.yaml
 │    └── payment.yaml
```

Argo CD deploys all manifests inside:

```text
roboshop/
```

directory.

---

# destination

Defines deployment target.

---

# server

```yaml
server: https://kubernetes.default.svc
```

Represents:

```text
Current Kubernetes cluster
```

Used for in-cluster deployments.

---

# namespace

```yaml
namespace: argocd
```

Namespace where Argo CD application object exists.

Actual application manifests may deploy resources into different namespaces such as:

```text
roboshop
```

---

# syncPolicy

Controls synchronization behavior between:

```text
Git Repository
        ↓
Kubernetes Cluster
```

---

# automated

```yaml
automated:
```

Enables automatic synchronization.

Without this:

* Manual sync required

With this:

* Git changes automatically deployed

---

# prune

```yaml
prune: true
```

Behavior:

```text
Delete resource from Git
          ↓
Argo CD deletes resource from cluster
```

Keeps cluster aligned with Git repository.

---

# selfHeal

```yaml
selfHeal: true
```

Automatically restores manually modified resources.

Example:

```text
Engineer manually edits deployment
              ↓
Argo CD detects drift
              ↓
Restores original Git configuration
```

This prevents:

* Configuration drift
* Manual inconsistencies

---

# GitOps Deployment Workflow

```text
Developer updates manifests in Git
                 ↓
Argo CD detects repository changes
                 ↓
Compares desired vs actual cluster state
                 ↓
Synchronizes Kubernetes resources
                 ↓
Application gets deployed automatically
```

---

# Real DevOps Use Cases

# Microservices Deployment

Deploy multiple services:

* catalogue
* cart
* payment
* shipping
* user

using single GitOps application.

---

# Continuous Delivery

Automatic deployment whenever:

```text
Git repository changes
```

---

# Kubernetes Platform Automation

Used for:

* Environment deployments
* Application management
* Cluster bootstrapping

---

# Self-Healing Infrastructure

Automatically restores:

* Deleted resources
* Modified configurations
* Drifted deployments

---

# Benefits of Argo CD

* Automated deployments
* Git-based infrastructure management
* Drift detection
* Self-healing applications
* Easy rollback capability

---

# Why GitOps Is Important

Traditional deployment model:

```text
Engineer manually applies manifests
```

Problems:

* Hard to audit
* Drift occurs
* Manual errors increase

GitOps provides:

* Version control
* Auditability
* Repeatability
* Safer deployments

---

# Best Practice

Store:

```text
All Kubernetes manifests in Git
```

and avoid:

```text
Manual kubectl changes
```

to maintain GitOps consistency.

---

# Benefits of This Manifest

* Automated synchronization
* GitOps-based deployment
* Self-healing infrastructure
* Centralized application management
* Continuous delivery automation

---

# Why This Manifest Is Important

This manifest demonstrates foundational GitOps deployment concepts using:

* Argo CD
* Kubernetes

These patterns are heavily used in enterprise DevOps and cloud-native platforms.

---

# Example Repository Structure

```text
eks-argocd/
 ├── roboshop/
 │    ├── catalogue.yaml
 │    ├── cart.yaml
 │    ├── user.yaml
 │    ├── payment.yaml
 │    └── shipping.yaml
```

Argo CD deploys all manifests inside:

```text
roboshop/
```

folder automatically.

---

# How to Deploy

1. Install Argo CD in Kubernetes cluster
2. Save manifest as:

```text
roboshop-app.yaml
```

3. Apply manifest:

```bash
kubectl apply -f roboshop-app.yaml
```

4. Open Argo CD UI
5. Observe application synchronization
6. Verify workloads:

```bash
kubectl get pods -n roboshop
```

---
