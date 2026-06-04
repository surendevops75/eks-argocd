# Argo CD Storage Classes Application Example

This manifest demonstrates how to manage Kubernetes StorageClasses using Argo CD and the GitOps methodology.

Instead of manually creating StorageClasses using:

```bash
kubectl apply -f storageclass.yaml
```

the StorageClass definitions are stored in Git and automatically synchronized to the Kubernetes cluster by Argo CD.

This approach provides:

* Version-controlled storage configuration
* Automated synchronization
* Infrastructure as Code (IaC)
* Consistent cluster storage setup

This is a common pattern used in enterprise DevOps platforms running on Kubernetes.

---

# Argo CD Application Manifest

```yaml
# --------------------------------------------------------
# API VERSION
# --------------------------------------------------------

# Argo CD API version
apiVersion: argoproj.io/v1alpha1

# --------------------------------------------------------
# RESOURCE TYPE
# --------------------------------------------------------

# Argo CD Application resource
kind: Application

metadata:

  # Application name shown in Argo CD UI
  name: storage-classes

  # Namespace where Argo CD is installed
  namespace: argocd

spec:

  # --------------------------------------------------------
  # ARGO CD PROJECT
  # --------------------------------------------------------

  project: default

  # --------------------------------------------------------
  # SOURCE CONFIGURATION
  # --------------------------------------------------------

  source:

    # Git repository containing StorageClass manifests
    repoURL: https://github.com/daws-86s/eks-argocd.git

    # Branch/tag/commit to synchronize
    targetRevision: main

    # Folder containing StorageClass YAML files
    path: storage-classes

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

      # Delete resources removed from Git
      prune: true

      # Restore manually modified resources
      selfHeal: true
```

---

# Important Concepts

# StorageClass Management Through GitOps

StorageClasses define:

```text
How Kubernetes provisions persistent storage
```

Examples:

* AWS EBS StorageClass
* AWS EFS StorageClass
* NFS StorageClass
* Azure Disk StorageClass

Instead of managing them manually, GitOps stores them inside Git repositories.

---

# Why Manage StorageClasses with Argo CD?

Traditional approach:

```text
Administrator creates StorageClass manually
```

Problems:

* Difficult to audit
* No version history
* Changes may be forgotten
* Configuration drift occurs

GitOps approach:

```text
StorageClass YAML
        ↓
Stored in Git
        ↓
Argo CD Synchronizes
        ↓
Cluster Updated Automatically
```

---

# Application Name

```yaml
name: storage-classes
```

This application manages:

```text
All StorageClass resources
```

stored inside the repository.

---

# Source Repository

```yaml
repoURL:
```

Points to repository containing storage-related manifests.

Example structure:

```text
eks-argocd/
 ├── storage-classes/
 │    ├── ebs-sc.yaml
 │    ├── efs-sc.yaml
 │    └── gp3-sc.yaml
```

---

# targetRevision

```yaml
targetRevision: main
```

Defines Git branch to monitor.

Can also use:

* Tags
* Commit SHAs

Examples:

```text
main
release-v1
v2.0.0
```

---

# path

```yaml
path: storage-classes
```

Argo CD reads all manifests from:

```text
storage-classes/
```

directory.

Every YAML file inside this folder gets synchronized.

---

# Destination Cluster

```yaml
server: https://kubernetes.default.svc
```

Represents:

```text
Current Kubernetes Cluster
```

where StorageClasses will be created.

---

# Why Namespace Is Still Required?

StorageClass is a:

```text
Cluster-scoped resource
```

and does not belong to any namespace.

However, the Argo CD Application itself exists inside:

```yaml
namespace: argocd
```

---

# Automated Sync

```yaml
automated:
```

Enables automatic synchronization.

Workflow:

```text
Git Change
     ↓
Argo CD Detects
     ↓
Cluster Updated Automatically
```

No manual sync required.

---

# prune

```yaml
prune: true
```

Behavior:

```text
Delete StorageClass YAML from Git
                ↓
Argo CD removes it from cluster
```

Keeps Git and cluster aligned.

---

# selfHeal

```yaml
selfHeal: true
```

Automatically fixes configuration drift.

Example:

```text
Admin manually modifies StorageClass
                 ↓
Argo CD detects drift
                 ↓
Original Git version restored
```

---

# GitOps Workflow

```text
Developer updates StorageClass YAML
                  ↓
Pushes changes to Git
                  ↓
Argo CD detects changes
                  ↓
Compares desired state with cluster
                  ↓
Applies StorageClass updates
```

---

# Real DevOps Use Cases

# AWS EBS StorageClass

Used for:

* Databases
* Stateful applications
* Persistent block storage

Example:

```text
PostgreSQL
MongoDB
MySQL
```

---

# AWS EFS StorageClass

Used for:

```text
Shared file storage
Multi-pod access
ReadWriteMany workloads
```

Examples:

* Jenkins
* Shared uploads
* Content management systems

---

# Cluster Standardization

Ensures every Kubernetes cluster receives:

```text
Same StorageClass configuration
```

across:

* Dev
* QA
* UAT
* Production

---

# Platform Engineering

StorageClasses become part of:

```text
Infrastructure as Code
```

instead of manual administration.

---

# Example Repository Structure

```text
eks-argocd/
 ├── storage-classes/
 │
 ├── roboshop-ebs.yaml
 │
 ├── roboshop-efs.yaml
 │
 └── gp3-storageclass.yaml
```

Argo CD automatically synchronizes all files inside this folder.

---

# Benefits of GitOps Storage Management

* Version-controlled storage configuration
* Automated deployment
* Easy rollback
* Drift detection
* Cluster standardization
* Reduced manual operations

---

# Best Practice

Manage all cluster-level resources through Git:

```text
Namespaces
StorageClasses
Ingress Controllers
Cert Managers
Monitoring Stack
```

instead of creating them manually.

---

# Why This Manifest Is Important

This manifest demonstrates GitOps-based infrastructure management using:

* Argo CD
* Kubernetes

It enables automated and version-controlled management of Kubernetes storage infrastructure.

---

# How to Deploy

1. Save manifest as:

```text
storage-classes-app.yaml
```

2. Apply it:

```bash
kubectl apply -f storage-classes-app.yaml
```

3. Open Argo CD UI

4. Verify application status

5. Verify StorageClasses:

```bash
kubectl get storageclass
```

6. Confirm EBS/EFS StorageClasses are synchronized successfully

---
