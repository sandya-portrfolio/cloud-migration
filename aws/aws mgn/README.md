Project Overview

This project demonstrates end-to-end lift-and-shift migration of on-premises Linux and Windows virtual machines to AWS using AWS Application Migration Service (MGN) and Cloud Migration Factory (CMF). The migration was executed in multiple migration waves with continuous replication, testing, cutover validation, DNS updates, and post-migration verification while ensuring minimal downtime and secure migration practices.

Migration Architecture: 
Source Environment:
- VMware / On-Premises Datacenter
- Linux Servers
- Windows Servers
- Application + Database Servers

Target Environment:
- AWS EC2
- Private Subnets
- AWS MGN Replication Servers
- Conversion Servers
- CloudWatch Monitoring

1. AWS Application Migration Service (MGN)
AWS Application Migration Service is primarily designed for lift-and-shift migrations but also supports re-platforming. It facilitates migration of on-premises systems to the AWS cloud.
The service supports agent-based migration for individual servers or agentless migration via VMware vCenter or Hyper-V environments.

2. Migration Process Overview
The migration involves four major steps: agent installation, replication setup, testing, and cutover execution.
Continuous data replication is supported until cutover is performed, allowing for minimal downtime.
Testing can be done for system acceptance or user acceptance before final cutover.

3. Source Environments and Compatibility
 MGN supports migrations from physical servers, VMware vCenter, Hyper-V, and third-party clouds such as Azure or GCP.
Uses standard ports: TCP 443 (SSL) for authentication and TCP 1500 for data replication.
Replication occurs at the disk level, with data fully encrypted during transfer.

4.AWS Infrastructure Components in Migration
 Replication instances handle data transfer and staging volumes in AWS public subnets.
Conversion instances create EC2 instances from replicated volumes automatically.
The tool manages replication and conversion instances dynamically based on workload.

5.Practical Demonstration Setup
Demonstration involves migrating four on-premises Linux and Windows servers in waves, grouping applications logically for phased migration.
Systems include Wordpress with MariaDB (NoSQL) and Office Business server with PostgreSQL (relational database).
Target AWS setup uses private subnets for migrated applications and public subnets for replication and conversion components.

6. Initialization and IAM Role Creation
 Before migration, MGN service must be initialized in the AWS region (example used: Oregon).
Initialization automatically creates multiple IAM roles (~7 initially, increased to 20 in some cases) to handle various migration tasks with least privilege principles.
Roles cover replication, conversion, monitoring, and other migration lifecycle activities.

7. Agent Installation and Access Management
 Agents must be installed on source Linux/Windows machines unless using vCenter agentless migration.
Migration user with limited IAM permissions and access keys is created for agent authentication and replication.
Agent setup includes specifying AWS region and replication options (e.g., full disk replication).

8. Replication Template and Launch Template Configuration
Replication templates define parameters such as instance size, subnet placement, encryption, and network throttling to protect production workloads during migration.
Launch templates specify how EC2 instances will be created post-migration, including instance types, IP addressing (retain or new), storage options, and licensing.

9. Migration Lifecycle States
Migration progresses through six lifecycle states:
Not Ready (initial sync incomplete)
Ready for Testing (initial replication complete)
Testing In Progress (test instances launched in isolated VPC/subnet)
Ready for Cutover (testing successful)
Cutover In Progress (final migration)
Cutover Complete (migration finalized)
Testing involves launching instances in isolated environments to avoid impacting production.
Application Grouping and Wave Planning
 Systems can be logically grouped by application for better management.
AWS MGN supports wave planning, allowing phased migration scheduling and tracking within the console.
Facilitates managing thousands of servers in large-scale migrations.

10. Test Instance Launch and Validation
 After initial sync, test instances are launched in AWS for functional validation by application teams.
Test instances are temporary, free for 90 days, and terminated after validation to avoid additional costs.

11. Cutover Execution and Finalization
 Once testing is complete and accepted, systems are marked ready for cutover.
Test instances are terminated, and final cutover EC2 instances are launched using the previously created snapshots.
Cutover involves stopping source servers, updating DNS records, and switching production traffic to AWS instances.
After cutover, replication stops and migration resources are cleaned up.
Systems on AWS are verified for operational status and connectivity.

12. Additional Features and Best Practices
 Supports wide range of Windows and Linux OS versions (mostly x86 architecture).
Emphasizes least privilege access with dedicated service accounts and key rotation policies.
Network throttling options reduce impact on production networks during replication.
Migration Factory concept integrates people, process, and tools for large-scale migration success.

13. Monitoring and Troubleshooting
CloudTrail integration provides detailed logs and events for auditing and troubleshooting migration activities.
Monitoring replication progress, snapshot creation, and instance launch statuses accessible via the MGN console.

14. commands
    # AWS MGN Agent Installation (Linux)

## Step 1: Download AWS Replication Installer

```bash
wget -O ./aws-replication-installer-init.py https://aws-application-migration-service-us-east-1.s3.amazonaws.com/latest/linux/aws-replication-installer-init.py
```

---

## Step 2: Run Installer

```bash
sudo python aws-replication-installer-init.py
```

---

## Step 3: Provide AWS Details

Installer prompts for:
```text
AWS Region
Access Key ID
Secret Access Key
```

---

## Step 4: Start Replication

After successful authentication:
- MGN agent gets installed
- Source server registers with AWS MGN
- Continuous disk replication starts

---

## Step 5: Verify Replication

AWS Console:
```text
AWS MGN → Source Servers
```

Expected Status:
```text
Initial Sync Started
```

Then:
```text
Ready for Testing
```

---

# Replication Workflow

```text
On-Prem Server
      ↓
MGN Agent Installed
      ↓
Continuous Disk Replication
      ↓
AWS Staging Area (EBS)
      ↓
Conversion Server
      ↓
EC2 Test Instance
      ↓
Production Cutover
```

Summary of Migration Steps: 
1. Check migration pre-requisites and validate source server readiness  
2. Install AWS MGN replication agents on source servers  
3. Push DNS/update launch scripts to all servers  
4. Verify replication status and synchronization health  
5. Validate launch templates (instance type, subnet, security groups, tags)  
6. Launch test instances in AWS  
7. Verify test instance health and connectivity  
8. Mark servers ready for cutover migration  
9. Shutdown source/on-premises servers during cutover window  
10. Launch cutover instances in AWS EC2  
11. Verify cutover instance status and application services  
12. Perform application validation and smoke testing  
13. Finalize migration cutover and business sign-off  
14. Disconnect source systems and replication from AWS  
15. Proceed with next migration wave/server batch

End-to-End process:
# AWS MGN Migration – End-to-End Migration Process

## Project Overview

This project demonstrates end-to-end lift-and-shift migration of on-premises Linux and Windows virtual machines to AWS using AWS Application Migration Service (MGN) and Cloud Migration Factory (CMF). The migration was executed in multiple migration waves with continuous replication, testing, cutover validation, DNS updates, and post-migration verification while ensuring minimal downtime and secure migration practices.

---

# Migration Architecture

Source Environment:
- VMware / On-Premises Datacenter
- Linux Servers
- Windows Servers
- Application + Database Servers

Target Environment:
- AWS EC2
- Private Subnets
- AWS MGN Replication Servers
- Conversion Servers
- CloudWatch Monitoring

---

# Migration Flow

## Step 1: Validate Migration Prerequisites

Validated:
- Supported operating systems
- Network connectivity
- Firewall rules
- VPN/Direct Connect availability
- Disk space and bandwidth
- AWS IAM permissions

Required Ports:
- TCP 443 → Authentication
- TCP 1500 → Data replication

---

# Step 2: Initialize AWS MGN Service

Initialize AWS Application Migration Service in target AWS region.

During initialization:
- IAM roles created automatically
- Replication templates created
- Launch templates initialized

AWS Services Used:
- AWS MGN
- IAM
- EC2
- EBS
- CloudWatch

---

# Step 3: Create IAM User for Migration

Created dedicated IAM user with least privilege access.

Policy Used:
```text
AWSApplicationMigrationAgentPolicy
```

Purpose:
- Agent installation
- Replication authentication
- Secure migration access

---

# Step 4: Download and Install MGN Agent

Installed AWS MGN replication agents on source servers.

Linux:
```bash
wget https://aws-application-migration-service-agent-installer
```

Windows:
- Downloaded installer package
- Installed replication agent manually

---

# Step 5: Configure Replication

Configured:
- Replication subnet
- Security groups
- Replication servers
- Staging area
- Bandwidth throttling
- EBS disk settings

Replication Process:
- Disk-level continuous replication
- Encrypted data transfer
- Snapshot creation
- Incremental synchronization

---

# Step 6: Verify Replication Status

Validated:
- Agent connectivity
- Replication health
- Synchronization percentage
- Snapshot creation
- CloudWatch events

Migration States:
1. Not Ready
2. Ready for Testing
3. Test in Progress
4. Ready for Cutover
5. Cutover in Progress
6. Cutover Complete

---

# Step 7: Create Migration Waves

Grouped servers into migration waves.

Example:
- Wave 1 → Application + Database Servers
- Wave 2 → Remaining workloads

Purpose:
- Controlled migration execution
- Reduced operational risk
- Easier rollback management

---

# Step 8: Configure Launch Templates

Configured EC2 launch templates:
- Instance types
- Subnets
- Security groups
- IAM roles
- Private IP retention
- EBS configuration

Configured:
- Target private subnets
- Production security groups
- DNS settings

---

# Step 9: Launch Test Instances

Launched test instances in AWS for validation.

Validated:
- Server boot status
- Application availability
- Database connectivity
- Network reachability
- Service dependencies

Performed:
- UAT testing
- Integration testing
- Smoke testing

---

# Step 10: Mark Servers Ready for Cutover

After successful testing:
- Marked workloads ready for cutover
- Terminated temporary test instances
- Prepared production migration window

---

# Step 11: Execute Cutover Migration

Performed final production migration.

Activities:
- Shutdown source servers
- Trigger cutover launch
- Launch production EC2 instances
- Verify replication completion

AWS MGN automatically:
- Created conversion servers
- Attached replicated volumes
- Booted production instances

---

# Step 12: Validate Cutover Environment

Validated:
- EC2 instance health
- Application accessibility
- Database services
- Port connectivity
- Service startup
- Performance validation

Checked:
- 2/2 EC2 status checks
- Application URLs
- DNS resolution

---

# Step 13: Update DNS Records

Updated DNS records to redirect traffic to AWS environment.

Linux DNS:
```bash
Bind DNS Updates
```

Windows DNS:
```powershell
ipconfig /registerdns
```

Validated:
- A records
- PTR records
- Name resolution

---

# Step 14: Finalize Cutover

Finalized migration in AWS MGN.

Actions:
- Stopped replication
- Disconnected source systems
- Removed temporary replication resources
- Archived migrated servers

---

# Step 15: Post-Migration Activities

Performed:
- Monitoring setup
- CloudWatch alarms
- Backup validation
- Security verification
- Documentation updates
- Knowledge transfer

---

# Troubleshooting Activities

Handled:
- Replication lag
- Agent installation failures
- Launch template issues
- Security group connectivity
- Boot failures
- DNS propagation issues

---

# AWS Services Used

- AWS Application Migration Service (MGN)
- Cloud Migration Factory (CMF)
- Amazon EC2
- Amazon EBS
- Amazon VPC
- AWS IAM
- AWS Systems Manager
- Amazon CloudWatch
- Route53 / DNS
- CloudTrail

---


