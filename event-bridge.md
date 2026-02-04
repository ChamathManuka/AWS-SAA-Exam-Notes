# Amazon EventBridge

## Overview
**Amazon EventBridge** is a serverless event bus service used to build **event-driven architectures**. It is recommended when applications need to **react to events from AWS services or third-party SaaS providers**.

EventBridge is the **only AWS event service with native integration to third-party SaaS applications**.

---

## Key Features
- Automatically ingests events from **90+ AWS services**
- Native integration with **third-party SaaS partners**
- No infrastructure or resource provisioning required by developers
- Uses a **standardized JSON-based event structure**
- Supports **fine-grained event filtering** using rules applied across the entire event body
- Enables loose coupling between event producers and consumers

---

## Event Targets
Amazon EventBridge can route events to **15+ AWS service targets**, including:
- AWS Lambda
- Amazon SQS
- Amazon SNS
- Amazon Kinesis Data Streams
- Amazon Kinesis Data Firehose
- AWS Step Functions

---

## Performance Characteristics
- **Typical latency**: ~0.5 seconds
- **Throughput**: Limited by default (service limits apply)
- Throughput limits can be **increased via AWS Support request**

---

## When to Use Amazon EventBridge
- Event-driven architectures
- SaaS application integrations
- Decoupled microservices
- Centralized event routing across AWS services
- Near real-time event processing

---

## Amazon EventBridge Scheduler

### Overview
**Amazon EventBridge Scheduler** is a fully managed, serverless scheduling service that allows you to run **cron-based or rate-based jobs at scale** without managing scheduling infrastructure.

### Key Benefits
- Supports **high-scale scheduling**
- Serverless and fully managed
- No need for custom cron servers or polling logic
- Integrates seamlessly with EventBridge targets

---

## Common Exam Tip
> Use **Amazon EventBridge** when:
> - Third-party SaaS event integration is required  
> - Events must be routed to multiple AWS services  
> - You need event filtering based on event payload content
