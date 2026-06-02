# Kubernetes StorageClass Example

This manifest demonstrates how to create a `StorageClass` in Kubernetes for dynamic storage provisioning using Amazon Web Services EBS volumes.

A `StorageClass` defines:

* Which storage provider to use
* How volumes should be provisioned
* Volume lifecycle behavior
* Binding strategy

This configuration uses the:

```text
AWS EBS CSI Driver
```

to dynamically create EBS volumes for Kubernetes workloads.

StorageClasses are commonly used in DevOps and Kubernetes environments for:

* Databases
* Stateful applications
* Persistent storage
* Dynamic PVC provisioning

---

# StorageClass Manifest

```yaml
# --------------------------------------------------------
# STORAGE CLASS API VERSION
# --------------------------------------------------------

apiVersion: storage.k8s.io/v1

# --------------------------------------------------------
# RESOURCE TYPE
# --------------------------------------------------------

# Kubernetes StorageClass resource
kind: StorageClass

metadata:

  # StorageClass name
  name: roboshop-ebs

# --------------------------------------------------------
# RECLAIM POLICY
# --------------------------------------------------------

# Controls what happens to storage
# after PVC is deleted
reclaimPolicy: Retain

# --------------------------------------------------------
# STORAGE PROVISIONER
# --------------------------------------------------------

# AWS EBS CSI Driver
# Dynamically provisions EBS volumes
provisioner: ebs.csi.aws.com

# --------------------------------------------------------
# VOLUME BINDING MODE
# --------------------------------------------------------

# Volume creation waits until pod scheduling
volumeBindingMode: WaitForFirstConsumer
```

---

# Important Concepts

# StorageClass

```yaml
kind: StorageClass
```

A `StorageClass` defines:

```text
How Kubernetes should create storage volumes
```

It acts like a storage template for PersistentVolumeClaims (PVCs).

---

# Why StorageClass Is Important

Without StorageClass:

* Storage must be created manually
* PersistentVolumes (PVs) need manual management

With StorageClass:

```text
PVC Created
      ↓
Kubernetes automatically provisions storage
```

This is called:

```text
Dynamic Provisioning
```

---

# metadata.name

```yaml
name: roboshop-ebs
```

StorageClass identifier used inside PVCs.

Example:

```yaml
storageClassName: roboshop-ebs
```

---

# provisioner

```yaml
provisioner: ebs.csi.aws.com
```

Defines storage backend provider.

This uses:

```text
AWS Elastic Block Store (EBS)
```

through:

```text
CSI Driver
```

---

# CSI Driver

CSI stands for:

```text
Container Storage Interface
```

CSI drivers allow Kubernetes to communicate with external storage systems.

Examples:

* AWS EBS
* EFS
* Azure Disk
* GCP Persistent Disk

---

# AWS EBS

EBS = Elastic Block Store.

Provides:

* Persistent block storage
* SSD/HDD volumes
* Durable storage for EC2 instances

Commonly used for:

* Databases
* Stateful applications
* Kubernetes persistent volumes

---

# reclaimPolicy

```yaml
reclaimPolicy: Retain
```

Defines what happens when:

```text
PersistentVolumeClaim (PVC) gets deleted
```

---

# Retain Policy

```yaml
Retain
```

Behavior:

* Underlying EBS volume is NOT deleted
* Data remains محفوظ (preserved)

Flow:

```text
Delete PVC
      ↓
PV released
      ↓
EBS volume remains
```

Useful for:

* Databases
* Important application data
* Disaster recovery

---

# Other Reclaim Policies

# Delete

```yaml
reclaimPolicy: Delete
```

Behavior:

```text
Delete PVC
      ↓
Delete PV
      ↓
Delete EBS volume
```

Common for temporary workloads.

---

# Recycle (Deprecated)

Older cleanup mechanism.

Rarely used now.

---

# volumeBindingMode

```yaml
volumeBindingMode: WaitForFirstConsumer
```

Controls when storage volume gets created and bound.

---

# WaitForFirstConsumer

Behavior:

```text
PVC Created
      ↓
Kubernetes waits
      ↓
Pod gets scheduled
      ↓
Volume created in correct AZ
```

This is important in cloud environments like AWS.

---

# Why WaitForFirstConsumer Is Important

AWS EBS volumes are:

```text
Availability Zone (AZ) specific
```

If volume gets created before pod scheduling:

* Pod may schedule in different AZ
* Volume attachment fails

`WaitForFirstConsumer` prevents this issue.

---

# Example Flow

Without `WaitForFirstConsumer`:

```text
Volume created in us-east-1a
        ↓
Pod scheduled in us-east-1b
        ↓
Pod cannot attach volume
```

With `WaitForFirstConsumer`:

```text
Pod scheduled first
        ↓
Volume created in same AZ
        ↓
Pod attaches volume successfully
```

---

# Dynamic Provisioning Workflow

```text
Application creates PVC
           ↓
PVC references StorageClass
           ↓
Kubernetes calls AWS EBS CSI Driver
           ↓
New EBS volume created automatically
           ↓
PV generated automatically
           ↓
Pod mounts volume
```

---

# Real DevOps Use Cases

# Databases

Used for:

* MySQL
* PostgreSQL
* MongoDB
* Redis persistence

---

# Stateful Applications

Applications requiring persistent storage:

* Jenkins
* Elasticsearch
* Kafka
* Monitoring stacks

---

# Kubernetes StatefulSets

StorageClasses are commonly used with:

```text
StatefulSet
```

resources.

---

# Backup and Recovery

Using:

```yaml
reclaimPolicy: Retain
```

to preserve data even after workload deletion.

---

# Benefits of StorageClass

* Dynamic storage provisioning
* Reduced manual operations
* Better scalability
* Cloud-native storage management
* Automated volume lifecycle

---

# Best Practice

For production workloads:

* Use `WaitForFirstConsumer`
* Use CSI drivers
* Choose reclaim policy carefully

Example:

* `Retain` for databases
* `Delete` for temporary workloads

---

# Why This Manifest Is Important

This manifest demonstrates core Kubernetes storage concepts using:

* Kubernetes
* Amazon Web Services
* AWS EBS CSI Driver
* Dynamic persistent storage provisioning

These concepts are foundational for running stateful workloads in Kubernetes.

---

# How to Deploy

1. Ensure AWS EBS CSI Driver is installed
2. Save manifest as:

```text
storageclass.yaml
```

3. Apply manifest:

```bash
kubectl apply -f storageclass.yaml
```

4. Verify StorageClass:

```bash
kubectl get sc
```

5. Create PVC using:

```yaml
storageClassName: roboshop-ebs
```

---
