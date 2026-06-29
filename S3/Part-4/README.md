# AWS S3 Interview Mastery — Advanced S3 (Final Master Set)

## Topic
Amazon S3 (Advanced Concepts Only)

Focus Areas:
- Permissions & Access Control
- Replication
- Monitoring & Logging
- Cost Optimization
- Advanced Features
- Scenario-Based Questions
- Hard Interview Questions

Goal:
Master every remaining S3 concept required to crack difficult AWS cloud interviews.

---

# Section 10 — Permissions & Access Control

## 75. Why does S3 show AccessDenied?

### Answer

S3 returns AccessDenied when AWS denies access due to permission issues.

Common reasons:

- Wrong IAM policy
- Incorrect bucket policy
- Block Public Access enabled
- Explicit Deny exists
- ACL permissions issue

Always check:

- IAM Policy
- Bucket Policy
- ACL
- Public Access Settings

---

## 76. What happens if Bucket Policy is wrong?

### Answer

AWS denies access to bucket operations.

Possible effects:

- Cannot upload files
- Cannot download objects
- Website stops working
- Users get AccessDenied error

Example:

```json
{
  "Effect":"Deny"
}
```

Any deny statement blocks access.

---

## 77. What is Bucket Policy Evaluation Logic?

### Answer

AWS checks permissions in this order:

1. Check explicit Deny  
2. Check explicit Allow  
3. If no allow → access denied by default  

Rule:

```text
Explicit Deny always wins
```

Example:

IAM says Allow.

Bucket Policy says Deny.

Final result:

Access denied.

---

## 78. Can IAM user upload but not delete?

### Answer

Yes.

Using IAM policy.

Allow upload:

```json
{
  "Action":"s3:PutObject",
  "Effect":"Allow"
}
```

No delete permission:

```text
No s3:DeleteObject permission
```

User can upload but cannot delete.

---

## 79. How do you restrict access to a specific folder?

### Answer

Use Bucket Policy or IAM Policy.

Example:

Allow access only to:

```text
s3://mybucket/images/
```

Policy:

```json
{
 "Action":"s3:GetObject",
 "Resource":"arn:aws:s3:::mybucket/images/*"
}
```

Other folders remain inaccessible.

---

## 80. What happens if both IAM Policy and Bucket Policy exist?

### Answer

AWS evaluates both together.

Rules:

- Explicit Deny wins
- Both policies must allow access

Example:

IAM → Allow

Bucket Policy → Deny

Result:

Access denied.

---

## 81. Difference between Allow and Explicit Deny?

### Answer

Allow:

```text
Permission granted
```

Deny:

```text
Permission blocked immediately
```

Important rule:

```text
Explicit Deny always overrides Allow
```

---

# Section 11 — Replication

## 82. What is Cross Region Replication (CRR)?

### Answer

CRR automatically copies objects from one AWS region to another.

Example:

```text
Mumbai → Singapore
```

Used for:

- Disaster recovery
- Global compliance
- Multi-region backup

---

## 83. What is Same Region Replication (SRR)?

### Answer

SRR replicates objects between buckets inside same AWS region.

Example:

```text
Bucket A → Bucket B
(ap-south-1)
```

Used for:

- Backup
- Data separation
- Log aggregation

---

## 84. Why use Replication?

### Answer

Replication improves availability and backup strategy.

Benefits:

- Disaster recovery
- Backup copies
- Compliance requirements
- Geographic redundancy

---

## 85. Does Replication work without Versioning?

### Answer

No.

Versioning must be enabled on both source and destination buckets.

Without versioning:

Replication will fail.

---

## 86. Can deleted files replicate?

### Answer

Yes.

Delete markers can replicate if configured.

When object deleted:

- Delete marker added
- Marker replicated to destination bucket

Depends on replication rule.

---

## 87. Can replication work between AWS accounts?

### Answer

Yes.

Cross-account replication is supported.

Requirements:

- Proper IAM permissions
- Destination bucket policy
- Replication role

---

# Section 12 — Monitoring & Logging

## 88. How to monitor S3?

### Answer

Methods:

- CloudWatch metrics
- Server Access Logging
- AWS CloudTrail
- S3 Storage Lens

Monitor:

- Requests
- Errors
- Latency
- Storage usage

---

## 89. What logs are available in S3?

### Answer

Main logs:

- Server Access Logs
- CloudTrail Logs
- Storage Lens Reports

Track:

- Upload requests
- Download requests
- Failed access attempts

---

## 90. What is Server Access Logging?

### Answer

S3 records all requests made to a bucket.

Logs include:

- Who accessed bucket
- IP address
- Request type
- Request time

Useful for auditing.

---

## 91. How do you track who accessed bucket?

### Answer

Use:

- Server Access Logging
- CloudTrail Logs

Information available:

- IAM user
- IP address
- API request type
- Timestamp

Used for security auditing.

---

# Section 13 — Cost Optimization

## 92. How to reduce S3 cost?

### Answer

Methods:

- Use lifecycle rules
- Move old files to Glacier
- Delete unused files
- Use Intelligent Tiering
- Compress data
- Remove incomplete multipart uploads

---

## 93. Which storage class saves maximum cost?

### Answer

S3 Glacier Deep Archive.

Best for long-term storage.

Example:

10-year compliance records.

---

## 94. How does Intelligent Tiering work?

### Answer

AWS automatically moves objects between access tiers based on usage.

Example:

Frequently used file:

```text
Standard Tier
```

Unused file:

```text
Archive Tier
```

Purpose:

Automatic cost optimization.

---

## 95. What causes unexpected S3 billing?

### Answer

Common reasons:

- Unused stored data
- Old object versions
- Incomplete multipart uploads
- Frequent retrieval charges
- Data transfer charges

Always monitor storage usage.

---

# Section 14 — Advanced S3 Features

## 96. What is Pre-signed URL?

### Answer

Pre-signed URL gives temporary access to private S3 object.

Example:

Private file shared for 5 minutes.

Flow:

```text
Generate URL → Temporary Access → URL Expires
```

Used for secure file sharing.

---

## 97. Why use Pre-signed URL?

### Answer

Used when private objects need temporary public access.

Example:

User downloads invoice for 10 minutes.

No bucket public access required.

---

## 98. Can private files be shared temporarily?

### Answer

Yes.

Using Pre-signed URL.

Access expires automatically after defined time.

---

## 99. What is S3 Consistency Model?

### Answer

Consistency means how quickly changes become visible.

S3 now provides:

```text
Strong Read-After-Write Consistency
```

Meaning:

Immediately after upload, object becomes visible.

---

## 100. Difference between Eventual Consistency and Strong Consistency?

### Answer

Eventual Consistency:

Changes visible after some delay.

Strong Consistency:

Changes visible immediately.

S3 now supports:

Strong consistency.

---

## 101. What is Object Lock?

### Answer

Object Lock prevents deletion or modification of objects for fixed period.

Modes:

- Governance Mode
- Compliance Mode

Used for:

- Legal records
- Compliance requirements

---

## 102. What is MFA Delete?

### Answer

MFA Delete requires multi-factor authentication before deleting objects.

Purpose:

Prevent accidental or malicious deletion.

Example:

Delete request requires OTP.

---

## 103. What is Object Ownership?

### Answer

Defines ownership of uploaded objects.

Modes:

- Bucket owner preferred
- Object writer
- Bucket owner enforced

Used when multiple users upload files.

---

## 104. What is Requester Pays?

### Answer

Normally bucket owner pays transfer cost.

In Requester Pays:

Person downloading pays transfer cost.

Used for public datasets.

---

## 105. What is Transfer Acceleration?

### Answer

Transfer Acceleration speeds up long-distance uploads using AWS edge network.

Example:

User in India uploads to US bucket faster.

Useful for global uploads.

---

# Section 15 — Hard Scenario Questions

## 106. User accidentally deleted production file. Recovery?

### Answer

If versioning enabled:

- Remove delete marker
- Restore previous version

If versioning disabled:

Recovery difficult.

Best practice:

Enable versioning.

---

## 107. Company stores 5 PB data for 15 years. Design storage.

### Answer

Architecture:

```text
Upload → S3 Standard
After 30 days → Standard-IA
After 90 days → Glacier
After 1 year → Deep Archive
```

Use lifecycle rules.

---

## 108. Bucket accidentally became public. Fix?

### Answer

Immediately:

- Enable Block Public Access
- Check Bucket Policy
- Remove public ACL
- Audit access logs

Security first.

---

## 109. Need secure private file sharing for 5 minutes. Solution?

### Answer

Use:

```text
Pre-signed URL
```

Temporary access expires automatically.

No public bucket required.

---

## 110. Large upload failed at 90%. What happens?

### Answer

If Multipart Upload used:

Only failed part uploads again.

If normal upload:

Entire upload restarts.

Best practice:

Use Multipart Upload.

---

# Hard Interview Questions

## Explain complete S3 architecture.

### Answer

S3 is AWS object storage service.

Flow:

```text
User Uploads File
↓
Object Stored Inside Bucket
↓
AWS Replicates Across Multiple Availability Zones
↓
Data Protected with Encryption
↓
Versioning Maintains Older Versions
↓
Lifecycle Rules Move Old Data
↓
Replication Creates Backup Copies
↓
Monitoring Tracks Access and Errors
```

Provides:

- High scalability
- 11 nines durability
- Global reliability
- Secure access control

---

## Why is S3 cheaper than traditional servers?

### Answer

Because AWS manages infrastructure.

Benefits:

- No hardware cost
- No maintenance cost
- Automatic scaling
- Pay only for usage
- No server management

---

# Top 20 Must Memorize Questions

1. What is S3  
2. What is Bucket  
3. What is Object  
4. S3 vs EBS vs EFS  
5. Multipart Upload  
6. Versioning  
7. Delete Marker  
8. Lifecycle Rules  
9. Storage Classes  
10. Bucket Policy  
11. IAM vs Bucket Policy  
12. Block Public Access  
13. SSE-S3 vs SSE-KMS  
14. Pre-signed URL  
15. CRR vs SRR  
16. Why AccessDenied happens  
17. Strong Consistency  
18. Object Lock  
19. MFA Delete  
20. Explain S3 architecture  

---

S3 Mastery Complete.
