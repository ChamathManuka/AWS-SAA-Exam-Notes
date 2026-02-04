# Amazon EC2 – Consolidated Notes

## EC2 Pricing Models

### On-Demand Instances
- Pay per second (Linux) or per hour (Windows)
- No long-term commitment
- Best for short-term, unpredictable workloads

### Reserved Instances (Up to ~75% discount)
- 1-year or 3-year commitment
- Best for steady-state workloads
- **Scopes**
  - **Regional RI**: Capacity discount across the region
  - **Zonal RI**: Capacity reservation in a specific AZ

### Spot Instances (Up to ~90% discount)
- Uses unused EC2 capacity
- Can be interrupted with 2-minute notice
- Best for fault-tolerant, batch, or stateless workloads

---

## EC2 Capacity & Reservations

### On-Demand Capacity Reservation
- Guarantees capacity in a **specific Availability Zone**
- No 1-year or 3-year commitment
- Can be created and released anytime
- Billed at **On-Demand price**
- Used for short-term events or predictable spikes

### EC2 Capacity Manager
- Aggregates:
  - On-Demand
  - Spot
  - Capacity Reservations
- Provides insights to optimize EC2 usage across accounts and organizations

---

## High Availability & Multi-AZ Design

### Minimum Instances with Multi-AZ
- If minimum **4 instances** are required during failure:
  - Use **3 AZs**
  - Deploy **2 instances per AZ**
- If one AZ fails → remaining AZs still meet minimum capacity

---

## Amazon Machine Image (AMI)

### AMI Basics
- Software blueprint for EC2
- Includes:
  - OS
  - Installed software
  - Configuration
  - EBS snapshots
  - Block device mappings
- AMIs are **region-specific**
- Copying an AMI to another region creates a new snapshot in that region

### Golden AMI
- Pre-configured production-ready AMI
- Includes:
  - OS patches
  - Application binaries
  - Runtime dependencies
  - Monitoring & logging agents
  - Security hardening
- Benefits:
  - Faster startup
  - Consistent environments
  - Less user data complexity

### AMI Copying & Sharing
- Can copy:
  - Encrypted ↔ encrypted
  - Unencrypted → encrypted
- **Encrypted AMIs cannot become unencrypted**
- Sharing an AMI requires:
  - `launchPermission`
  - KMS key permissions (if encrypted)
- Shared AMIs appear under **Shared With Me**

---

## EC2 User Data

- Used for:
  - Bootstrapping
  - Configuration
  - Script execution
- Supports:
  - Shell scripts
  - Cloud-init directives
- Runs as **root user**
- Executes **only on first boot**
- Does **not** rerun on stop/start or reboot
- Runs after instance boots (does not affect boot time directly)
- Complex scripts can delay instance readiness

---

## Launch Templates & Auto Scaling

### Launch Template
- Defines how EC2 instances are launched
- Includes:
  - AMI
  - Instance type
  - Key pair
  - Security groups
  - User data
  - IAM role
- Versioned and reusable
- Replaces **Launch Configurations** (which are immutable & deprecated)

### Tenancy Behavior
- Launch template tenancy overrides VPC tenancy
- If **either** is set to `dedicated`, instance runs as dedicated

---

## Elastic IP (EIP)

- Static public IPv4 address
- Used to mask instance failure
- Can be remapped to another instance quickly

### EIP vs ALB
- ALB costs money even for single instance
- EIP is cheaper for single-instance workloads

### Automation
- User data & IAM role needed **only** if instance reassigns EIP itself
- Otherwise, EIP can be attached externally via Console / CLI / IaC

---

## Dedicated Instances vs Dedicated Hosts

### Dedicated Instances
- Hardware dedicated to a single AWS account
- No control over host placement
- No host affinity
- Limited BYOL support

### Dedicated Hosts
- Full visibility & control over physical host
- Required for server-bound licenses
- Supports host affinity

> No performance or security difference between the two

---

## Instance Health & Recovery

### Impaired Instance
- Underlying hardware failure
- OS is healthy
- AWS migrates instance to new host
- RAM data is lost
- Instance store data is lost if migration occurs

### CloudWatch Auto Recovery
- Works **only for system status check failures**
- Requires EBS-backed instances
- Not supported for instance status check failures
- Cannot recover terminated instances

---

## Placement Groups

### Cluster Placement Group
- Instances placed close together
- Low latency, high throughput
- Same rack

### Spread Placement Group
- Instances placed on separate hardware
- Maximum fault tolerance
- Different racks, power, networking

### Partition Placement Group
- Instances divided into partitions
- Isolation between partitions (not individual instances)

---

## Spot Fleet & EC2 Fleet

- Launch and manage large fleets of EC2 instances
- Supports:
  - Multiple instance types
  - Multiple AZs
- Spot Fleet:
  - Attempts to maintain target capacity
  - Cannot guarantee availability
  - Replaces interrupted instances when possible

---

## EC2 Monitoring (CloudWatch)

### Default Metrics
- Published every **5 minutes**

### Detailed Monitoring
- 1-minute granularity
- Better visibility for alarms & scaling

### Alarm Actions
- Stop
- Terminate
- Reboot
- Recover

---

## EC2 Connectivity

### EC2 Instance Connect
- Temporary SSH access using one-time public keys
- Requires:
  - Public IP
  - Port 22 open
- Audited via CloudTrail
- No static SSH keys

### EC2 Instance Connect Endpoint
- Used for private subnet instances
- No public IP required

### AWS Systems Manager Session Manager
- No SSH or Bastion needed
- Secure, auditable access
- Requires:
  - SSM Agent
  - IAM role
- Best practice for private instances

---

## Source / Destination Check

- Enabled by default
- Ensures instance sends/receives only its own traffic
- Must be **disabled** for:
  - NAT instances
  - Routing appliances

---

## EC2 Instance Metadata (IMDS)

- Accessible via `169.254.169.254`
- Provides:
  - Instance identity
  - Networking details
  - IAM role credentials
- Uses **IMDSv2** (session-based, more secure)

---
