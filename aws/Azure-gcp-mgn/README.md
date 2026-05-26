# GCP VM Migration Project Summary

(Based on Azure → GCP Migration using Migrate to Virtual Machines)


## 1. Project Overview

This project explains end-to-end VM migration from Microsoft Azure to Google Cloud Platform (GCP) using Google Cloud native service:

* **Migrate to Virtual Machines (M2VM)**

Goal:

* Securely migrate workloads with:

  * Minimal downtime
  * Minimal data loss
  * Faster migration
  * Easy automation

---

# 2. Migration High-Level Flow

```text
Assessment
   ↓
Planning
   ↓
Source Configuration
   ↓
GCP Configuration
   ↓
Replication
   ↓
Test Clone
   ↓
Cutover
   ↓
Validation
   ↓
Optimization & Cleanup
```

---

# 3. Migration Architecture

## Source Environment

* Azure Virtual Machines
* Azure Subscription
* Azure AD Application
* Azure APIs

## Destination Environment

* Google Cloud Platform
* Compute Engine VMs
* Migrate to Virtual Machines Service

## Communication

* Internet-based encrypted transfer
* TLS/SSL over Port 443
* No VPN required

---

# 4. Tools Used

| Tool                                      | Purpose              |
| ----------------------------------------- | -------------------- |
| Google Migrate to Virtual Machines (M2VM) | VM migration         |
| Google Cloud SDK (gcloud)                 | CLI setup            |
| Azure AD Application                      | Authentication       |
| IAM Roles                                 | Authorization        |
| Snapshot Replication                      | Data synchronization |

---

# 5. Migration Strategies (6 R’s)

| Strategy    | Description         |
| ----------- | ------------------- |
| Rehost      | Lift and Shift      |
| Replatform  | Small modernization |
| Rearchitect | Full redesign       |
| Repurchase  | SaaS replacement    |
| Retain      | Keep on-prem        |
| Retire      | Decommission        |

Most projects use:

* **Rehost (Lift & Shift)**

---

# 6. Supported Sources

## Supported

* VMware
* Azure
* AWS

## Planned

* Hyper-V
* Oracle Cloud
* IBM Cloud

---

# 7. Supported Operating Systems

## Windows

* Windows Server 2016
* Windows Server 2019
* Windows Server 2022

## Linux

* CentOS 7.9+
* RHEL 7.9+
* Ubuntu
* Rocky Linux

---

# 8. Step-by-Step Migration Process

# Step 1 — Assessment Phase

## Activities

* Identify applications
* Check dependencies
* Check OS compatibility
* Group migration batches
* Identify critical workloads

## Output

* Migration plan
* VM inventory
* Dependency mapping

---

# Step 2 — Prepare Google Cloud

## Tasks

* Create Host Project
* Create Target Project
* Enable APIs:

  * VM Migration API
  * Service Management API

## Install

* Google Cloud SDK

## Configure IAM

* VM Migration Admin
* Viewer roles

---

# Step 3 — Prepare Azure Environment

## Create Azure AD Application

Purpose:

* Authentication between GCP and Azure

## Generate

* Client Secret

## Assign Custom Role Permissions

Required permissions:

* Read VM configuration
* Create snapshots
* Delete snapshots
* Shutdown VMs

---

# Step 4 — Connect Azure to GCP

Provide:

* Subscription ID
* Tenant ID
* Client ID
* Client Secret

Result:

* Azure VMs discovered in GCP Migration Console

---

# Step 5 — Onboarding VMs

## Select VMs

Choose VMs to migrate.

## Register into M2VM

Migration service starts managing selected VMs.

---

# Step 6 — Replication Phase

## Initial Replication

* Full VM disk copy
* Snapshot-based transfer

## Delta Replication

* Incremental sync
* Every:

  * 15 minutes
  * 2 hours
  * Configurable

Benefits:

* Reduced downtime
* Faster cutover

---

# Step 7 — Configure Target VM

## Configure:

* GCP Project
* Region
* Zone
* Machine Type
* Network
* Firewall
* Disk Type
* Encryption
* Licensing

Licensing Options:

* BYOL
* PAYG

---

# Step 8 — Test Clone

Purpose:

* Validate migration safely

## Activities

* Create replica VM in GCP
* Validate:

  * Files
  * Applications
  * Browser bookmarks
  * Website access
  * Connectivity

No impact to source VM.

---

# Step 9 — Cutover

## Final Migration

Steps:

1. Shutdown source VM
2. Final incremental sync
3. Start VM in GCP
4. Validate applications

Goal:

* Minimal downtime

---

# Step 10 — Validation

## Validate:

* Application functionality
* Data integrity
* Website accessibility
* Network connectivity
* User access

---

# Step 11 — Finalization & Cleanup

## Cleanup Tasks

* Remove old Azure VMs
* Delete test VMs
* Remove unused snapshots
* Cost optimization

---

# 9. Security Best Practices

| Area            | Best Practice        |
| --------------- | -------------------- |
| Authentication  | Azure AD App         |
| Authorization   | Least Privilege IAM  |
| Encryption      | TLS/SSL              |
| Access Control  | Role-based           |
| Data Protection | Snapshot replication |

---

# 10. Advantages of M2VM

| Feature                      | Benefit             |
| ---------------------------- | ------------------- |
| No VPN                       | Simpler setup       |
| Encrypted Internet Migration | Secure              |
| Delta Replication            | Faster sync         |
| Test Clone                   | Safe validation     |
| Native Google Tool           | Lower cost          |
| Minimal Downtime             | Better availability |

---

# 11. Common Challenges

| Challenge             | Solution                |
| --------------------- | ----------------------- |
| Legacy OS             | Replatform or retain    |
| Firewall issues       | Proper rule setup       |
| Bandwidth limitations | Schedule replication    |
| Dependency issues     | Assessment phase        |
| Downtime concerns     | Test clone + delta sync |

---

# 12. Key Best Practices

* Always perform assessment first
* Use least privilege roles
* Test before cutover
* Monitor replication continuously
* Use naming/tagging standards
* Clean unused resources after migration
* Start with Lift & Shift approach

---


