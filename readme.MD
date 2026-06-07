# 🚀 EKS GitOps Deployment with Argo CD

![AWS](https://img.shields.io/badge/AWS-EKS-orange)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.30-blue)
![ArgoCD](https://img.shields.io/badge/GitOps-ArgoCD-red)
![GitOps](https://img.shields.io/badge/Deployment-GitOps-green)

## 📌 Overview

This repository contains Kubernetes manifests and Argo CD configurations for deploying applications on Amazon EKS using GitOps principles.

The project demonstrates how to manage Kubernetes resources declaratively using Argo CD's App of Apps pattern while organizing infrastructure and application components into reusable modules.

---

## 🏗️ Architecture

```text
Git Repository
      │
      ▼
   Argo CD
      │
      ▼
 App of Apps
      │
 ┌────┼────┐
 ▼    ▼    ▼
Namespaces
Storage
Applications
      │
      ▼
 Amazon EKS
      │
      ▼
 RoboShop Microservices
```

---

## 📂 Repository Structure

```text
.
├── applications/
├── namespaces/
├── roboshop/
├── storage-classes/
├── app-of-apps.yaml
└── app-of-apps.md
```

---

## 📁 Components

### applications/

Contains Argo CD Application manifests responsible for deploying workloads into the cluster.

**Purpose**
- Application lifecycle management
- GitOps synchronization
- Automated deployments

---

### namespaces/

Contains Kubernetes namespace definitions.

**Purpose**
- Logical isolation of workloads
- Environment segregation
- Resource organization

---

### storage-classes/

Contains Kubernetes StorageClass definitions.

**Purpose**
- Dynamic volume provisioning
- Persistent storage management
- EKS storage integration

---

### roboshop/

Contains deployment manifests for the RoboShop microservices application.

**Includes**
- Deployments
- Services
- Configurations
- Application resources

---

### app-of-apps.yaml

Main Argo CD App of Apps manifest.

**Purpose**
- Bootstrap Argo CD applications
- Centralized application management
- Automated resource discovery

---

## 🔄 Deployment Workflow

```text
Developer Pushes Changes
           │
           ▼
      Git Repository
           │
           ▼
        Argo CD
           │
           ▼
     Detects Changes
           │
           ▼
     Sync Kubernetes
           │
           ▼
      Amazon EKS
           │
           ▼
      Applications
```

---

## 🎯 Key Concepts Demonstrated

### GitOps

- Infrastructure as Code
- Declarative Deployments
- Version Controlled Infrastructure
- Automated Reconciliation

### Argo CD

- Continuous Delivery
- Application Synchronization
- Drift Detection
- Self-Healing Deployments

### Kubernetes

- Namespace Management
- Storage Provisioning
- Application Deployment
- Resource Organization

### Amazon EKS

- Managed Kubernetes
- Cloud-Native Deployment
- Scalable Infrastructure
- Production-Ready Clusters

---

## 🛠 Technologies Used

- Amazon EKS
- Kubernetes
- Argo CD
- GitOps
- YAML
- Storage Classes
- RoboShop Microservices

---

## 📚 Learning Objectives

This repository helps you understand:

- GitOps Fundamentals
- Argo CD Architecture
- App of Apps Pattern
- Kubernetes Resource Management
- Amazon EKS Deployments
- Continuous Delivery
- Infrastructure as Code
- Cloud-Native Application Deployment

---

## 👨‍💻 Ideal For

- DevOps Engineers
- Platform Engineers
- Cloud Engineers
- Kubernetes Administrators
- Site Reliability Engineers (SREs)
- GitOps Practitioners

---

## 🚀 Features

- GitOps-based deployments
- Argo CD App of Apps pattern
- Namespace automation
- Storage class management
- EKS integration
- Declarative infrastructure
- Automated synchronization
- Scalable application management

---

## ⭐ Support

If you find this repository useful:

- Star the repository ⭐
- Fork the project 🍴
- Share it with fellow DevOps Engineers 🚀

---

## 📖 Topics

EKS • Kubernetes • Argo CD • GitOps • DevOps • AWS • Cloud Native • App of Apps • Continuous Delivery • Infrastructure as Code