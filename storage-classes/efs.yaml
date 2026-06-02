# Kubernetes EFS StorageClass Example

This manifest demonstrates how to create a `StorageClass` in Kubernetes using Amazon Web Services EFS (Elastic File System).

Unlike EBS volumes, which are block storage and usually attached to a single node, EFS provides:

```text
Shared Network File Storage
```

This means multiple pods across multiple nodes can access the same storage simultaneously.

This StorageClass uses the:

```text
AWS EFS CSI Driver
```

to dynamically provision EFS Access Points for Kubernetes workloads.

EFS StorageClasses are commonly used for:

* Shared application storage
* Jenkins shared workspace
* Media storage
* Shared uploads
* Multi-pod persistent storage

---

# StorageClass Manifest

```yaml
# --------------------------------------------------------
# RESOURCE TYPE
# --------------------------------------------------------

kind: StorageClass

# --------------------------------------------------------
# API VERSION
# --------------------------------------------------------

apiVersion: storage.k8s.io/v1

metadata:

  # StorageClass name
  name: roboshop-efs

# --------------------------------------------------------
# STORAGE PROVISIONER
# --------------------------------------------------------

# AWS EFS CSI Driver
provisioner: efs.csi.aws.com

# --------------------------------------------------------
# DRIVER PARAMETERS
# --------------------------------------------------------

parameters:

  # Dynamic Access Point provisioning mode
  provisioningMode: efs-ap

  # Existing AWS EFS FileSystem ID
  fileSystemId: fs-02ba4dd076173e1a8

  # Directory permissions
  directoryPerms: "700"

  # Base path inside EFS
  # Optional parameter
  basePath: "/roboshop"
```

---

# Important Concepts

# StorageClass

```yaml
kind: StorageClass
```

Defines how Kubernetes dynamically provisions persistent storage.

Applications use:

```yaml
storageClassName:
```

inside PVCs to request storage automatically.

---

# AWS EFS

EFS stands for:

```text
Elastic File System
```

It is:

* Managed NFS storage
* Shared file system
* Network-based storage

Provided by Amazon Web Services.

---

# EFS vs EBS

| Feature           | EBS           | EFS            |
| ----------------- | ------------- | -------------- |
| Storage Type      | Block Storage | File Storage   |
| Multi-node access | ❌ Usually No  | ✅ Yes          |
| Shared storage    | ❌             | ✅              |
| AZ limitation     | Single AZ     | Multi-AZ       |
| Common Use        | Databases     | Shared storage |

---

# provisioner

```yaml
provisioner: efs.csi.aws.com
```

Defines storage provider.

Uses:

```text
AWS EFS CSI Driver
```

to communicate with AWS EFS service.

---

# CSI Driver

CSI stands for:

```text
Container Storage Interface
```

CSI drivers allow Kubernetes to interact with external storage providers.

Examples:

* AWS EBS
* AWS EFS
* Azure Disk
* Google Persistent Disk

---

# parameters

```yaml
parameters:
```

Provides driver-specific configuration.

These parameters are interpreted by:

```text
EFS CSI Driver
```

---

# provisioningMode

```yaml
provisioningMode: efs-ap
```

`efs-ap` means:

```text
EFS Access Point provisioning
```

Kubernetes automatically creates:

* Access Points
* Directories
* Mount configurations

for PVCs.

---

# EFS Access Point

An EFS Access Point provides:

* Application-specific entry path
* Access isolation
* Permission management

Useful for:

* Multi-tenant workloads
* Shared EFS environments
* Secure storage separation

---

# fileSystemId

```yaml
fileSystemId: fs-02ba4dd076173e1a8
```

Specifies existing EFS filesystem.

This filesystem must already exist in AWS.

Example:

```text
fs-xxxxxxxxxxxxxxxxx
```

---

# directoryPerms

```yaml
directoryPerms: "700"
```

Defines Linux permissions for created directories.

Permission breakdown:

```text
7 → read + write + execute
0 → no permissions
0 → no permissions
```

Result:

```text
Owner → full access
Others → no access
```

---

# basePath

```yaml
basePath: "/roboshop"
```

Optional parameter.

Defines root directory inside EFS where PVC directories are created.

Example generated path:

```text
/roboshop/pvc-12345
```

Useful for:

* Organizing storage
* Environment separation
* Application grouping

---

# Dynamic Provisioning Workflow

```text
Application creates PVC
           ↓
PVC references StorageClass
           ↓
EFS CSI Driver creates Access Point
           ↓
Directory created inside EFS
           ↓
PV created automatically
           ↓
Pod mounts shared storage
```

---

# Why EFS Is Important

EBS limitations:

* Usually attached to one node
* Not ideal for shared storage

EFS advantages:

* Shared across nodes
* Multi-pod access
* Multi-AZ support
* Managed file system

---

# Real DevOps Use Cases

# Shared Jenkins Workspace

Multiple Jenkins agents access same files.

---

# Shared Upload Storage

Applications storing:

* Images
* Documents
* Videos

---

# Kubernetes Shared Volumes

Multiple pods accessing same persistent data.

---

# Monitoring and Logging

Shared storage for:

* Logs
* Metrics
* Reports

---

# Multi-Replica Applications

Applications needing:

```text
ReadWriteMany (RWX)
```

access mode.

---

# Access Modes

EFS commonly supports:

```text
ReadWriteMany (RWX)
```

Meaning:

```text
Multiple pods can read/write simultaneously
```

EBS usually supports:

```text
ReadWriteOnce (RWO)
```

---

# Benefits of EFS StorageClass

* Shared persistent storage
* Multi-node access
* Dynamic provisioning
* Fully managed storage
* High availability
* Multi-AZ support

---

# Best Practice

Use EFS for:

* Shared workloads
* Multi-replica applications
* File-based storage

Use EBS for:

* Databases
* High-performance block storage

---

# Why This Manifest Is Important

This manifest demonstrates advanced Kubernetes persistent storage concepts using:

* Kubernetes
* Amazon Web Services
* AWS EFS CSI Driver
* Shared network storage

These concepts are heavily used in enterprise Kubernetes platforms.

---

# How to Deploy

1. Create AWS EFS filesystem
2. Install AWS EFS CSI Driver
3. Save manifest as:

```text
efs-storageclass.yaml
```

4. Apply manifest:

```bash
kubectl apply -f efs-storageclass.yaml
```

5. Verify StorageClass:

```bash
kubectl get sc
```

6. Create PVC using:

```yaml
storageClassName: roboshop-efs
```

---
