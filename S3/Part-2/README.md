# AWS S3 Interview Mastery — Part 2

## Topic
**Amazon S3 (Simple Storage Service)**  
Focus Areas: **Storage Classes + Security + Encryption**  
Coverage: **Q23 – Q50**

**Goal:** Master the most frequently asked AWS S3 interview questions.

---

# Section 4 — Storage Classes

## 23. What are S3 Storage Classes?

### Answer

S3 provides multiple storage classes for different access patterns and cost optimization.

### Types

- S3 Standard  
- S3 Intelligent-Tiering  
- S3 Standard-IA  
- S3 One Zone-IA  
- S3 Glacier Instant Retrieval  
- S3 Glacier Flexible Retrieval  
- S3 Glacier Deep Archive  

Purpose:

Choose storage based on frequency of access and cost.

---

## 24. Difference between S3 Standard and Standard-IA?

### Answer

| Feature | S3 Standard | S3 Standard-IA |
|----------|-------------|---------------|
| Access Frequency | Frequent | Infrequent |
| Cost | Higher | Lower |
| Retrieval Fee | No | Yes |
| Availability | 99.99% | 99.9% |

Use:

- Standard → Frequently accessed data  
- Standard-IA → Rarely accessed backups  

---

## 25. Difference between Glacier and Deep Archive?

### Answer

| Feature | Glacier Flexible Retrieval | Glacier Deep Archive |
|----------|--------------------------|----------------------|
| Retrieval Speed | Minutes/Hours | 12–48 Hours |
| Cost | Low | Lowest |
| Use Case | Backups | Long-term archive |

Example:

- Glacier → Monthly backup  
- Deep Archive → 10-year records  

---

## 26. Cheapest S3 Storage Class?

### Answer

**S3 Glacier Deep Archive**

Best for long-term storage where data is rarely accessed.

Example:

Legal records, tax records, old backups.

---

## 27. Which Storage Class is Best for Backups?

### Answer

Depends on backup frequency.

- Frequently needed backup → Standard-IA  
- Rarely needed backup → Glacier Flexible Retrieval  
- Very old backup → Deep Archive  

---

## 28. Which Storage Class is Best for Frequently Accessed Data?

### Answer

**S3 Standard**

Because:

- Low latency  
- High throughput  
- No retrieval fee  

Example:

Application images, active website files.

---

## 29. Which Storage Class is Best for Logs?

### Answer

Best practice:

- Recent logs → Standard or Intelligent-Tiering  
- Old logs → Glacier  

Example:

Application logs older than 90 days moved automatically.

---

## 30. Which Storage Class is Best for Archive Data?

### Answer

**S3 Glacier Deep Archive**

Best for data rarely accessed over years.

Example:

Government documents, compliance records.

---

# Section 5 — Security

## 31. How do you Secure an S3 Bucket?

### Answer

Multiple ways:

- IAM Policies  
- Bucket Policies  
- Block Public Access  
- Encryption  
- Versioning  
- MFA Delete  
- Disable ACLs  

Best practice:

Follow least privilege principle.

---

## 32. What is Block Public Access?

### Answer

Block Public Access prevents accidental public access to S3 buckets.

When enabled:

- Public bucket policies blocked  
- Public ACL blocked  
- Internet users cannot access bucket  

Used for security.

---

## 33. What Happens if Block Public Access is Enabled?

### Answer

Even if bucket policy allows public access:

```json
{
 "Effect":"Allow",
 "Principal":"*"
}
```

AWS ignores it.

Result:

Public access is denied.

---

## 34. How do you Make a Bucket Public?

### Answer

Steps:

1. Disable Block Public Access  
2. Add bucket policy  

Example:

```json
{
  "Version":"2012-10-17",
  "Statement":[
   {
    "Effect":"Allow",
    "Principal":"*",
    "Action":"s3:GetObject",
    "Resource":"arn:aws:s3:::mybucket/*"
   }
  ]
}
```

Used for public websites.

---

## 35. What is Bucket Policy?

### Answer

Bucket Policy is a JSON-based policy attached directly to the bucket.

Purpose:

Controls who can access the bucket.

Example:

Allow read access.

```json
{
 "Effect":"Allow",
 "Action":"s3:GetObject"
}
```

---

## 36. Difference between Bucket Policy and IAM Policy?

### Answer

| Bucket Policy | IAM Policy |
|---------------|------------|
| Applied to bucket | Applied to user/role |
| Resource based | Identity based |
| Controls bucket access | Controls user permissions |

Interview line:

IAM controls **who** can access.

Bucket Policy controls **what** can access bucket.

---

## 37. Can IAM Policy Override Bucket Policy?

### Answer

No direct override.

AWS evaluates both together.

Rules:

- Explicit Deny always wins  
- Allow works only if no Deny exists  

Example:

If bucket policy denies access, IAM allow will not work.

---

## 38. What is ACL in S3?

### Answer

ACL = Access Control List.

Older access control method for buckets and objects.

Can provide:

- Read access  
- Write access  

AWS now recommends using IAM and Bucket Policies instead.

---

## 39. Difference between ACL and Bucket Policy?

### Answer

| ACL | Bucket Policy |
|------|--------------|
| Old access control | Modern access control |
| Limited permissions | Advanced permissions |
| Object/Bucket level | Bucket level |
| Harder to manage | Easier to manage |

AWS recommends disabling ACL.

---

## 40. Why does AWS Recommend Disabling ACL?

### Answer

Because ACL is older and more complex.

Problems:

- Hard to manage  
- Causes accidental public access  
- Bucket policies are more secure  

Best practice:

Disable ACL and use IAM + Bucket Policy.

---

## 41. What is Least Privilege in S3?

### Answer

Least privilege means giving only minimum required permissions.

Example:

Instead of:

```text
Allow: s3:*
```

Use:

```text
Allow: s3:GetObject
```

Benefits:

Improves security.

---

# Section 6 — Encryption

## 42. How do you Encrypt S3 Data?

### Answer

Three main methods:

- SSE-S3  
- SSE-KMS  
- SSE-C  

Also:

- Client-side encryption

---

## 43. What is SSE-S3?

### Answer

SSE = Server Side Encryption

SSE-S3 means AWS encrypts data using AWS-managed keys.

Flow:

```text
Upload → AWS Encrypts → Stored Securely
```

Simple default encryption.

---

## 44. What is SSE-KMS?

### Answer

SSE-KMS uses AWS Key Management Service for encryption.

Benefits:

- Customer control over keys  
- Audit logs using CloudTrail  
- Key rotation supported  

More secure than SSE-S3.

---

## 45. What is SSE-C?

### Answer

SSE-C = Server Side Encryption with Customer Provided Keys.

Customer provides encryption key during upload.

AWS uses that key to encrypt.

AWS does not store the key.

Used when customer wants full control.

---

## 46. Difference between SSE-S3 and SSE-KMS?

### Answer

| Feature | SSE-S3 | SSE-KMS |
|----------|--------|---------|
| Key Management | AWS manages | KMS manages |
| Audit Logging | No | Yes |
| Key Rotation | No | Yes |
| Security | Good | Better |

Use:

- SSE-S3 → Normal apps  
- SSE-KMS → Production workloads

---

## 47. What is Client-Side Encryption?

### Answer

Data is encrypted before uploading to S3.

Flow:

```text
User Encrypts → Upload → AWS Stores Encrypted File
```

AWS never sees original data.

Highest security level.

---

## 48. Which Encryption Method is Most Secure?

### Answer

**Client-Side Encryption**

Reason:

AWS never has access to original data or encryption keys.

Security order:

1. Client-side encryption  
2. SSE-KMS  
3. SSE-S3  

---

## 49. Can AWS Employees Access Encrypted Data?

### Answer

Depends on encryption type.

- SSE-S3 → AWS manages keys  
- SSE-KMS → Controlled access through KMS  
- Client-side encryption → AWS cannot access original data  

Best protection:

Client-side encryption.

---

## 50. What is KMS?

### Answer

KMS = Key Management Service.

AWS service used to create and manage encryption keys.

Features:

- Key rotation  
- Access control  
- Audit logs  
- Secure encryption management  

Used with:

- S3  
- EBS  
- RDS  
- Lambda  

---

# Interview Trap Questions

## Which S3 Storage Class Saves Maximum Cost?

Answer:

S3 Glacier Deep Archive

---

## If I Accidentally Make Bucket Public, What Should I Do?

Answer:

- Enable Block Public Access  
- Check Bucket Policy  
- Remove public ACL  
- Audit access logs  

---

## Which Encryption is Best for Production?

Answer:

SSE-KMS or Client-Side Encryption

---

## Why Disable ACL?

Answer:

Because ACL is older, harder to manage, and can cause accidental exposure.

---

# Must Memorize Questions

1. Storage classes in S3  
2. Standard vs Standard-IA  
3. Bucket Policy vs IAM Policy  
4. What is Block Public Access  
5. SSE-S3 vs SSE-KMS  
6. What is KMS  
7. What is Least Privilege  
8. Why disable ACL  
9. Cheapest storage class  
10. Most secure encryption method  

---
