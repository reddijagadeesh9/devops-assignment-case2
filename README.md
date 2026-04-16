Devops Assignment

# Case Study 2: Secure Public Bucket Storage

##  Architecture

User → CloudFront → S3 (Private via Origin Access Control)

---

##  Objective

To securely host and deliver static website content from Amazon S3 while ensuring:

* Direct access to S3 object URLs is blocked
* Content is only accessible through a controlled and scalable layer
* High performance delivery using CDN

---

##  Architecture Diagram

```
User
  ↓
CloudFront (CDN + Security Layer)
  ↓
S3 Bucket (Private, OAC Enabled)
```

---

##  Implementation Steps

1. Created an S3 bucket and uploaded static website content (index.html).
2. Enabled **versioning** and **server-side encryption**.
3. Blocked **all public access** to the bucket.
4. Created a **CloudFront distribution** with S3 as origin.
5. Enabled **Origin Access Control (OAC)**.
6. Updated **bucket policy** to allow access only from CloudFront.
7. Configured **default root object (index.html)**.
8. Verified secure access and content delivery.

---

##  Security Controls Implemented

* S3 bucket is **fully private**
* Direct access to S3 URLs is **blocked (AccessDenied)**
* Access restricted using **CloudFront Origin Access Control (OAC)**
* **Server-side encryption** enabled
* **Versioning** enabled for data protection

---

##  Testing & Validation

| Component      | Expected Result | 
| -------------- | --------------- | 
| S3 Direct URL  | Access Denied ❌  
| EC2 Public IP  | Access Denied ❌  
| ALB Endpoint   | Access Denied ❌  
| CloudFront URL | Website Loads ✅ 

---

##  Proofs

Screenshots are available in the `proofs/` directory:

* `cloudfront-working.png` → Website via CDN
* `s3-access-denied.png` → Direct S3 blocked
* `alb-access-denied.png` → ALB blocked
* `ec2-access-denied.png` → EC2 blocked
* `bucket-policy.png` → OAC policy
* `cloudfront-config.png` → Distribution config

---

##  Design Decision

Initially, an architecture using:

```
CloudFront → ALB → EC2 → S3
```

was implemented.

However, to meet strict **security and scalability requirements**, the design was optimized to:

```
CloudFront → S3 (via OAC)
```

### ✔ Why this change?

* Eliminates need for load balancer authentication
* Prevents direct S3 access completely
* Uses AWS recommended **OAC mechanism**
* Improves performance and reduces latency

---

##  Performance & Scalability

* CloudFront caches content at edge locations globally
* Reduces latency and improves load times
* Handles high traffic efficiently without backend scaling

---

##  Cost Optimization

* Removed dependency on EC2 and ALB
* Reduced infrastructure cost
* Leveraged CDN caching to minimize S3 requests

---

##  Deployment Details

* Region: eu-north-1
* Storage: Amazon S3
* CDN: Amazon CloudFront
* Security: Origin Access Control (OAC)

---

##  Compliance with Assignment Requirements

✔ Secure object storage
✔ Block direct object access
✔ Controlled access via CDN
✔ High availability and scalability
✔ Security best practices implemented

---

##  Conclusion

A secure, scalable, and production-grade static website architecture was successfully implemented using AWS services. The solution ensures strict access control while delivering content efficiently through a global CDN.

---

## 🔗 Live URL

https://da79hqbu8dksk.cloudfront.net
