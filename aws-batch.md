# AWS Batch

## Overview
**AWS Batch** is a fully managed service for running **batch computing workloads** at any scale. It is designed for **long-running, compute-intensive jobs** that require scheduling, queuing, and dependency management.

AWS Batch is **not intended for lightweight or cron-style jobs**.

---

## Key Characteristics
- Supports **long-running batch jobs**
- Provides **job queues** and **job dependency management**
- Automatically provisions and scales compute resources
- Requires **compute environments** to be configured and managed
- Higher **startup and initialization time** compared to event-driven services

---

## Compute Model
- AWS Batch **does not run jobs directly on EC2**
- Jobs are containerized and scheduled on:
  - Amazon ECS (EC2 or Fargate)
  - Amazon EKS
- Compute resources can be:
  - EC2 On-Demand
  - EC2 Spot
  - AWS Fargate

---

## Integrations
- Amazon ECS / Amazon EKS
- Amazon S3 (input/output data)
- AWS IAM (job permissions)
- Amazon SageMaker (ML batch inference or training jobs)

---

## When to Use AWS Batch
- Scientific simulations
- Media transcoding
- Financial risk modeling
- Machine learning batch inference
- Large-scale data processing jobs

---

## When NOT to Use AWS Batch
- Simple cron jobs
- Lightweight or short-lived container tasks
- Event-driven workflows (use EventBridge or Lambda instead)

---

## Exam Tip
> Choose **AWS Batch** when the problem mentions **queues, dependencies, long-running compute jobs, or batch processing at scale**.
