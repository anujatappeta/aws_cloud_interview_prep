# AWS S3 Interview Mastery — Part 3

## Topic
**Amazon S3 (Simple Storage Service)**  
Focus Areas: **Versioning + Lifecycle Rules + Static Website Hosting**  
Coverage: **Q51 – Q74**

**Goal:** Master advanced S3 concepts frequently asked in cloud interviews.

---

# Section 7 — Versioning

## 51. What is Versioning in S3?

### Answer

Versioning is an S3 feature that stores multiple versions of the same object.

If a file is modified or overwritten, AWS keeps the old version instead of permanently replacing it.

### Example

```text
resume.pdf (v1)
resume.pdf (v2)
resume.pdf (v3)
```

Purpose:

- Prevent accidental deletion  
- Recover previous versions  
- Protect against overwrites  

---

## 52. Why use Versioning?

### Answer

Versioning is used for data protection and recovery.

Benefits:

- Recover deleted files  
- Restore previous versions  
- Prevent accidental overwrite  
- Improve backup reliability  

Example:

A developer accidentally overwrites production configuration.

Old version can be restored.

---

## 53. What happens if file is deleted with Versioning enabled?

### Answer

AWS does not immediately delete the actual object.

Instead, AWS adds a **Delete Marker**.

Result:

- Object appears deleted  
- Previous versions still exist internally  

Old versions can still be recovered.

---

## 54. What is Delete Marker?

### Answer

A Delete Marker is a placeholder created when deleting an object in a version-enabled bucket.

It makes the object appear deleted.

Example:

```text
report.pdf
Delete Marker Added
Old Versions Still Exist
```

The real object data is not removed.

---

## 55. Can deleted object be recovered?

### Answer

Yes.

If versioning is enabled:

- Delete marker can be removed  
- Previous versions remain available  

Recovery is possible unless versions are permanently deleted manually.

---

## 56. Can you disable Versioning?

### Answer

No.

Once enabled, versioning cannot be permanently disabled.

AWS only allows:

**Suspend Versioning**

Meaning:

- Existing versions remain  
- New uploads stop creating versions  

---

## 57. Difference between Suspend and Disable Versioning?

### Answer

S3 does not support permanent disable.

| Suspend Versioning | Disable Versioning |
|-------------------|-------------------|
| Supported | Not Supported |
| Old versions remain | Not possible |
| New versions stop | Feature unavailable |

AWS allows only suspension.

---

# Section 8 — Lifecycle Rules

## 58. What is Lifecycle Management?

### Answer

Lifecycle Management automatically moves objects between storage classes or deletes objects after a certain time.

Purpose:

Reduce storage cost automatically.

---

## 59. Why use Lifecycle Rules?

### Answer

Used for cost optimization.

Benefits:

- Move old files to cheaper storage  
- Delete unnecessary data automatically  
- Reduce manual work  
- Optimize long-term storage cost  

Example:

Logs older than 90 days moved automatically.

---

## 60. Can objects automatically move to Glacier?

### Answer

Yes.

Lifecycle Rules can automatically transition objects.

Example:

```text
Day 30 → Standard-IA
Day 90 → Glacier
```

No manual work required.

---

## 61. Can objects automatically delete after 30 days?

### Answer

Yes.

Lifecycle expiration rule can automatically delete objects.

Example:

Temporary application logs.

```text
Delete after 30 days
```

Used to reduce unnecessary storage costs.

---

## 62. Can Lifecycle Rules reduce cost?

### Answer

Yes.

Lifecycle rules automatically move data to cheaper storage classes.

Example:

```text
Frequently used → Standard
Rarely used → Glacier
Old archive → Deep Archive
```

This reduces monthly storage cost.

---

## 63. Give real example of Lifecycle Rules

### Answer

Example for company backups:

```text
Day 1   → S3 Standard
Day 30  → Standard-IA
Day 90  → Glacier Flexible Retrieval
Day 365 → Delete
```

Purpose:

Automatic cost optimization.

---

# Section 9 — Static Website Hosting

## 64. Can S3 host websites?

### Answer

Yes.

S3 can host **static websites**.

Supported files:

- HTML  
- CSS  
- JavaScript  
- Images  

Example:

Portfolio website hosted in S3.

---

## 65. What is Static Website Hosting?

### Answer

Static website hosting means hosting frontend files without backend processing.

Supported:

- HTML  
- CSS  
- JavaScript  

Not supported:

- Python  
- Java  
- Node.js backend  

S3 only serves files.

---

## 66. Difference between Static Website and Dynamic Website?

### Answer

| Static Website | Dynamic Website |
|---------------|----------------|
| Fixed content | Changes based on user |
| HTML/CSS/JS | Backend processing |
| No database | Uses database |
| Faster | More complex |

Example:

- Portfolio → Static  
- Amazon shopping site → Dynamic

---

## 67. Can backend run in S3?

### Answer

No.

S3 only stores and serves files.

It cannot execute backend code.

Not supported:

- Python  
- Flask  
- Java  
- Node.js runtime  

For backend use:

- EC2  
- Lambda  

---

## 68. Why cannot Flask/Python run in S3?

### Answer

Flask needs:

- CPU processing  
- Memory  
- Runtime environment  
- Server execution  

S3 is only object storage.

It cannot execute code.

Use:

- EC2 for servers  
- Lambda for serverless backend  

---

## 69. Which files are needed for S3 Static Hosting?

### Answer

Minimum required files:

```text
index.html
error.html
```

Explanation:

- index.html → Default homepage  
- error.html → Custom error page  

Optional:

- CSS files  
- JavaScript files  
- Images  

---

## 70. How to enable Static Website Hosting?

### Answer

Steps:

1. Create S3 bucket  
2. Upload website files  
3. Enable Static Website Hosting  
4. Set index.html  
5. Set error.html  
6. Configure Bucket Policy for public access  
7. Access website endpoint  

---

## 71. Difference between S3 Website Endpoint and Bucket Endpoint?

### Answer

| Website Endpoint | Bucket Endpoint |
|------------------|----------------|
| Used for website hosting | Used for API access |
| Supports index.html | Does not support website hosting |
| Public access required | Used with IAM/OAC |

Example:

Website Endpoint:

```text
bucket.s3-website-region.amazonaws.com
```

Bucket Endpoint:

```text
bucket.s3.region.amazonaws.com
```

---

## 72. Why does Website Endpoint work but Bucket Endpoint gives AccessDenied?

### Answer

Because both serve different purposes.

Website endpoint:

- Used for public website hosting  
- Supports direct browser access  

Bucket endpoint:

- Used for API requests  
- Requires IAM permissions or CloudFront OAC  

If bucket is private:

Bucket endpoint returns AccessDenied.

---

## 73. Can HTTPS work directly with S3 Static Website Hosting?

### Answer

No.

S3 static website endpoint supports only HTTP.

For HTTPS use:

CloudFront.

Architecture:

```text
User → CloudFront (HTTPS) → S3
```

CloudFront handles SSL/TLS certificates.

---

## 74. Why use CloudFront with S3 Website Hosting?

### Answer

CloudFront improves performance and security.

Benefits:

- HTTPS support  
- Global CDN delivery  
- Low latency  
- Content caching  
- Faster website loading  
- DDoS protection through AWS Shield  

Architecture:

```text
User → CloudFront → S3 Bucket
```

---

# Interview Trap Questions

## If User Deletes Production File Accidentally, How Do You Recover?

Answer:

If versioning enabled:

- Remove delete marker  
- Restore old version  

If versioning disabled:

Recovery is difficult.

---

## Why cannot S3 host backend applications?

Answer:

Because S3 is object storage only.

It cannot execute server-side code.

---

## Why does S3 website endpoint work but bucket endpoint fail?

Answer:

Website endpoint is for public static hosting.

Bucket endpoint requires permissions or signed access.

---

## Why use Lifecycle Rules?

Answer:

Automatic cost optimization.

Move old data to cheaper storage automatically.

---

# Must Memorize Questions

1. What is Versioning  
2. What is Delete Marker  
3. Can deleted files be recovered  
4. What is Lifecycle Rule  
5. Static Website Hosting  
6. Website Endpoint vs Bucket Endpoint  
7. Why Flask cannot run in S3  
8. Can S3 host websites  
9. Why use CloudFront with S3  
10. Can HTTPS work directly with S3  

---
