# Amazon CloudFront (Exam-Oriented Notes)

## What Amazon CloudFront Is
Amazon CloudFront is a **global Content Delivery Network (CDN)** that accelerates delivery of **static and dynamic content** by caching data at **edge locations close to users**.

It improves:
- Performance (low latency)
- Security
- Availability
- Scalability

CloudFront operates at **Layer 7 (HTTP/HTTPS)**.

---

## How CloudFront Works
CloudFront:
- Receives user requests at the **nearest edge location**
- Serves cached content if available
- Otherwise fetches content from the **origin**
- Caches content based on **cache-control headers and behaviors**

---

## Supported Origins
CloudFront can retrieve content from:
- Amazon S3 (static objects)
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)
- Amazon EC2
- On-premises servers
- Third-party/custom origins

---

## Protocol & Traffic Support
CloudFront supports:
- HTTP / HTTPS
- RTMP (legacy media use cases)

CloudFront **does NOT support**:
- UDP-based traffic (gaming)
- MQTT (IoT)
- VoIP protocols

For **UDP or non-HTTP traffic**, use **AWS Global Accelerator**, not CloudFront.

---

## CloudFront vs Global Accelerator (Exam Tip)
- **CloudFront** → HTTP-based workloads (web, APIs, video streaming)
- **Global Accelerator** → TCP/UDP workloads, gaming, IoT

Although Global Accelerator supports UDP, **AWS video streaming is HTTP-based over TCP**, so **CloudFront is the correct service** for live and on-demand streaming.

---

## CloudFront with Load Balancers & VPC

### Internet-Facing Load Balancers
CloudFront can front:
- Internet-facing ALB
- Internet-facing NLB

These have **public DNS names** and accept traffic from both **CloudFront and internet clients**.

### VPC Origins (Private Load Balancers)
CloudFront can connect to **internal NLBs in private subnets** using **VPC origins**.

VPC origins allow CloudFront to:
- Access private resources
- Keep origins off the public internet
- Improve security posture

---

## Caching Behavior & Dynamic Content
CloudFront uses:
- Standard HTTP cache-control headers
- Cache behaviors with URL path patterns

Examples:
/images/*
/static/*
/api/*


CloudFront is **NOT ideal** for:
- Highly dynamic, uncacheable content
- Real-time bidirectional applications

---

## Custom Domain for S3 with Route 53 & CloudFront
A custom domain (e.g., `uploads.example.com`) allows users to access S3 content via a **branded HTTPS URL**.

### Architecture
- **Route 53** → DNS maps domain to CloudFront
- **CloudFront**:
  - Terminates TLS
  - Uses an ACM certificate
  - Fetches content from S3
- **S3 remains private**

### Important Exam Rule
> CloudFront can use **ACM public certificates only from `us-east-1`**

Certificates created in any other region **cannot** be attached to CloudFront.

---

## CloudFront with S3 Static Website Hosting
S3 static website endpoints support:
- GET
- HEAD

They **do NOT support**:
- PUT
- POST

➡️ **Not suitable for user uploads**

---

## HTTPS, TLS & Origin Encryption
CloudFront supports:
- HTTPS from client → edge
- Optional HTTPS from edge → origin

### Two Models
1. TLS terminates at the edge (data travels inside AWS network)
2. TLS re-encryption to origin (fully encrypted end-to-end)

---

## CloudFront Field-Level Encryption
Field-level encryption encrypts **specific sensitive fields** at the edge.

Key points:
- Encryption happens close to the user
- Data remains encrypted through the stack
- Uses customer-managed public/private key pairs
- Does **NOT** use AWS KMS
- Encrypt up to **10 fields per request**
- Must specify individual fields (cannot encrypt entire payload)

---

## Origin Access Control (OAC) vs Origin Access Identity (OAI)
Used to restrict **S3 access only to CloudFront**.

### Purpose
- Prevent direct S3 access
- Ensure content is served only via intended CloudFront distribution

### OAI (Legacy)
- Special CloudFront user
- S3 bucket policy grants read access

### OAC (Recommended)
- Uses **AWS Signature Version 4**
- Works with **S3 + HTTPS only**
- More secure and flexible
- AWS-recommended replacement for OAI

---

## Signed URLs & Signed Cookies

### Signed URLs
Used to:
- Restrict access to **individual objects**
- Grant **time-limited access**
- Common for paid videos or documents

Primarily for:
- GET access
- **Not for uploads**

### Signed Cookies
Used when protecting **multiple files under one path**.

Example:
/videos/video1.mp4
/videos/video2.mp4
/videos/video3.mp4


Instead of 3 signed URLs:
- One signed cookie for `/videos/*`

---

## CloudFront Origin Request Policy
An origin request policy controls **what CloudFront forwards to the origin**.

Policy types:
- Managed policies (AWS-defined)
- Custom policies

You can control:
- Headers
- Cookies
- Query strings

This balances:
- Caching efficiency
- Security
- Origin load

---

## CloudFront Price Classes
Price classes control CDN cost by limiting edge locations.

### Price Classes
- **Price Class All** → All global edges
- **Price Class 200** → Excludes most expensive regions
- **Price Class 100** → US, Canada, Europe only

### Important Behavior
- Requests are **never blocked**
- Requests route to nearest allowed edge
- Latency may increase, cost decreases
- Price class affects **edge selection only**
- **Never affects origin selection**

---

## CloudFront Security Model
CloudFront **does NOT use security groups**.

Protected using:
- AWS Shield
- AWS WAF

---

## CloudFront with AWS Shield
CloudFront + Shield:
- Absorbs DDoS attacks at the edge
- Protects L3/L4 and L7
- Prevents origin overload
- Improves availability during attacks

Optional:
- AWS WAF for IP, geo, and pattern filtering

---

## S3 Transfer Acceleration vs CloudFront

### Use CloudFront When:
- Objects < **1 GB**
- Total dataset < **1 GB**
- Using PUT / POST via CloudFront
- Global content delivery

### Use S3 Transfer Acceleration When:
- Objects > **1 GB**
- Large data uploads
- Long-distance uploads
- Optimized for ingestion, not caching

---

## Key Exam Takeaways
- CloudFront = HTTP/HTTPS CDN
- ACM certs must be in **us-east-1**
- Use **OAC** over OAI
- Signed URLs = single object
- Signed cookies = multiple objects
- No security groups
- CloudFront ≠ UDP workloads

