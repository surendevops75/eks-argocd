# Argo CD Namespace Management Application Example

This manifest demonstrates how to manage Kubernetes namespaces using Argo CD in a GitOps workflow.

Instead of manually creating namespaces using:

```bash
kubectl create namespace
```

the namespaces are managed declaratively through Git repositories.

This approach ensures:

* Version-controlled namespaces
* Automated synchronization
* Consistent cluster configuration
* GitOps-based infrastructure management

This is a very common enterprise DevOps practice in Kubernetes environments.

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

  # Application name
  name: namespace

  # Namespace where Argo CD is installed
  namespace: argocd

spec:

  # --------------------------------------------------------
  # ARGO CD PROJECT
  # --------------------------------------------------------

  # Logical grouping of applications
  project: default

  # --------------------------------------------------------
  # SOURCE CONFIGURATION
  # --------------------------------------------------------

  source:

    # Git repository containing namespace manifests
    repoURL: https://github.com/daws-86s/eks-argocd.git

    # Git branch/tag/commit
    targetRevision: main

    # Repository folder containing namespace YAMLs
    path: namespaces

  # --------------------------------------------------------
  # DESTINATION CONFIGURATION
  # --------------------------------------------------------

  destination:

    # Current Kubernetes cluster
    server: https://kubernetes.default.svc

    # Namespace where Argo CD manages application
    namespace: argocd

  # --------------------------------------------------------
  # SYNCHRONIZATION POLICY
  # --------------------------------------------------------

  syncPolicy:

    automated:

      # Remove deleted resources automatically
      prune: true

      # Restore drifted resources automatically
      selfHeal: true
```

---

# Important Concepts

# Argo CD Application

```yaml
kind: Application
```

Core Argo CD resource used for:

* Deploying Kubernetes manifests
* Synchronizing Git repositories
* Managing Kubernetes resources

This application specifically manages:

```text
Namespaces
```

inside Kubernetes cluster.

---

# GitOps Model

Argo CD follows:

```text
GitOps
```

where:

```text
Git Repository
      =
Single Source of Truth
```

Cluster state is continuously synchronized with Git repository.

---

# Namespace Management Using GitOps

Instead of manually creating namespaces:

```bash
kubectl create ns roboshop
```

namespaces are stored as YAML manifests in Git repository.

Example:

```text
namespaces/
 ├── roboshop.yaml
 ├── monitoring.yaml
 └── ingress.yaml
```

Argo CD automatically applies them.

---

# metadata.name

```yaml
name: namespace
```

Application name visible inside Argo CD UI.

This application manages:

```text
Namespace resources
```

---

# metadata.namespace

```yaml
namespace: argocd
```

Application resource must exist inside namespace where Argo CD is installed.

Usually:

```text
argocd
```

---

# project

```yaml
project: default
```

Argo CD projects provide:

* Application grouping
* RBAC control
* Repository restrictions
* Cluster access policies

`default` project allows standard deployments.

---

# source Section

Defines:

```text
Where manifests come from
```

---

# repoURL

```yaml
repoURL:
```

Git repository containing Kubernetes manifests.

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
path: namespaces
```

Folder inside repository containing namespace manifests.

Example repository structure:

```text
eks-argocd/
 ├── namespaces/
 │    ├── roboshop.yaml
 │    ├── monitoring.yaml
 │    └── ingress.yaml
```

Argo CD reads all YAMLs from:

```text
namespaces/
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

Used for in-cluster deployment.

---

# namespace

```yaml
namespace: argocd
```

Namespace where Argo CD application itself exists.

This does NOT mean all namespaces are created inside:

```text
argocd
```

The actual namespace manifests define their own namespaces.

---

# syncPolicy

Controls synchronization behavior.

---

# automated

```yaml
automated:
```

Enables automatic synchronization.

Without this:

* Manual sync required

With this:

* Git changes automatically applied

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
Engineer manually edits namespace
              ↓
Argo CD detects drift
              ↓
Restores Git configuration
```

This prevents:

* Configuration drift
* Manual inconsistencies

---

# GitOps Workflow

```text
Developer updates namespace YAML in Git
                  ↓
Argo CD detects repository changes
                  ↓
Compares desired vs actual cluster state
                  ↓
Applies namespace changes automatically
                  ↓
Cluster stays aligned with Git
```

---

# Real DevOps Use Cases

# Cluster Bootstrapping

Automatically create:

* Application namespaces
* Monitoring namespaces
* Logging namespaces
* Security namespaces

when cluster starts.

---

# Environment Separation

Namespaces used for:

```text
dev
qa
uat
prod
```

environment isolation.

---

# Multi-Team Kubernetes Platforms

Different namespaces for:

* Frontend team
* Backend team
* Platform team
* Monitoring team

---

# GitOps Namespace Management

Benefits:

* Version control
* Auditability
* Easy rollback
* Centralized management

---

# Benefits of Namespace GitOps

* Automated namespace creation
* Consistent cluster setup
* Reduced manual operations
* Better platform standardization
* Git-based infrastructure management

---

# Best Practice

Manage:

```text
Namespaces as Code
```

instead of manually creating them.

This ensures:

* Repeatability
* Auditability
* Consistency
* Disaster recovery capability

---

# Why This Manifest Is Important

This manifest demonstrates foundational GitOps infrastructure management concepts using:

* Argo CD
* Kubernetes

These practices are widely used in enterprise Kubernetes DevOps platforms.

---

# Example Namespace Manifest

Inside:

```text
namespaces/
```

directory you may have:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: roboshop
```

Argo CD automatically applies it.

---

# How to Deploy

1. Install Argo CD in Kubernetes cluster
2. Save manifest as:

```text
namespace-app.yaml
```

3. Apply manifest:

```bash
kubectl apply -f namespace-app.yaml
```

4. Open Argo CD UI
5. Observe namespace application synchronization
6. Verify namespaces:

```bash
kubectl get ns
```

---
