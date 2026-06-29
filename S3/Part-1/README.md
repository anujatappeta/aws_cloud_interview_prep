# AWS S3 Interview Mastery — Part 1

## Topic

**Amazon S3 (Simple Storage Service)**
Focus Areas: **Fundamentals + Architecture + Uploading**
Coverage: **Q1 – Q22**

**Goal:** Prepare answers exactly how top MNC interviewers expect (Amazon, Microsoft, Oracle, Deloitte, Accenture, Google).

---

# Section 1 — S3 Fundamentals

## 1. What is S3?

### Answer

Amazon S3 (Simple Storage Service) is an AWS fully managed **object storage service** used to store and retrieve unlimited amounts of data over the internet.

### Main Features

* Highly scalable
* 99.999999999% durability
* 99.99% availability
* Secure through IAM, encryption, bucket policies

Data is stored as **objects inside buckets**.

### Interview Follow-up

**Q:** What type of storage is S3?

**A:** Object Storage

---

## 2. Why do we use S3?

### Answer

S3 is used when we need highly scalable and durable cloud storage.

### Common Use Cases

* Static website hosting
* Backup and recovery
* Media storage (images/videos)
* Data lakes
* Log storage
* Archive storage
* Application file storage

### Example

An e-commerce company stores product images in S3.

---

## 3. What type of storage is S3?

### Answer

S3 provides **Object Storage**.

Data is stored as objects.

Each object contains:

* Actual data
* Metadata
* Unique identifier (Key)

Unlike traditional storage, S3 does not behave like a hard disk.

---

## 4. Difference between Object Storage, Block Storage, and File Storage?

| Storage Type   | AWS Service | Description                         |
| -------------- | ----------- | ----------------------------------- |
| Object Storage | Amazon S3   | Stores files as objects             |
| Block Storage  | Amazon EBS  | Works like hard disk attached to VM |
| File Storage   | Amazon EFS  | Shared network file system          |

### Example

* S3 → Images
* EBS → EC2 disk
* EFS → Shared files across servers

---

## 5. What is a Bucket in S3?

### Answer

A bucket is a logical container used to store objects in S3.

### Example

```text
Bucket: company-data

Objects:
resume.pdf
image.jpg
backup.zip
```

Every object must be stored inside a bucket.

---

## 6. Can Bucket Names be Duplicated Globally?

### Answer

No.

Bucket names must be unique across all AWS accounts globally.

### Example

If another AWS user creates:

```text
mycompany-data
```

Nobody else can create the same name.

---

## 7. Why must S3 Bucket Names be Globally Unique?

### Answer

Because AWS creates a unique DNS endpoint for each bucket.

### Example

```text
https://mybucket.s3.amazonaws.com
```

If multiple users had the same bucket name, AWS could not generate unique endpoints.

---

## 8. What is an Object in S3?

### Answer

An object is the actual file stored in S3.

Each object contains:

* Data
* Key
* Metadata
* Version ID (if versioning enabled)

### Example

```text
Bucket → myfiles

Object → image.png
```

---

## 9. What is the Maximum Object Size Allowed in S3?

### Answer

Maximum size:

**50 TB per object**

Important:

Use **Multipart Upload** for large objects.

---

## 10. What is Metadata in S3?

### Answer

Metadata is information describing an object.

### Example

For file photo.jpg

```text
File Type → image/jpeg
Size → 2 MB
Last Modified → Date
Custom Metadata → project=website
```

Used for management and identification.

---

# Section 2 — Architecture

## 11. Is S3 Regional or Global?

### Answer

S3 buckets are created inside a specific AWS region.

### Example

* Mumbai → ap-south-1
* Virginia → us-east-1

But bucket names are globally unique.

**Conclusion**

Storage is regional, namespace is global.

---

## 12. How does AWS Maintain High Availability in S3?

### Answer

AWS stores data across multiple servers in multiple Availability Zones.

If one server fails, another copy is available.

Provides:

**99.99% availability**

Users can still access data even if hardware fails.

---

## 13. What are Availability Zones in S3 Architecture?

### Answer

Availability Zones are physically separate data centers inside a region.

### Example

Mumbai region:

* AZ 1
* AZ 2
* AZ 3

AWS stores copies across multiple AZs.

If one AZ fails, data remains available.

---

## 14. Does S3 Automatically Replicate Data?

### Answer

Yes.

Inside a region, AWS automatically stores copies across multiple Availability Zones.

Internal replication is automatic.

For manual replication:

* CRR → Cross Region Replication
* SRR → Same Region Replication

---

## 15. How does S3 Achieve 99.999999999% Durability?

### Answer

AWS stores multiple copies of data across:

* Multiple devices
* Multiple Availability Zones

If one device or data center fails, other copies remain.

AWS guarantees:

**11 Nines Durability**

Meaning data loss is extremely unlikely.

---

## 16. What Happens when One AWS Data Center Fails?

### Answer

Nothing happens to the user.

Because AWS stores replicated copies in multiple Availability Zones.

If one data center fails:

* Other copies remain active
* Users continue accessing data

No data loss occurs.

---

# Section 3 — Uploading

## 17. How do you Upload Files to S3?

### Answer

Multiple methods:

* AWS Console
* AWS CLI
* SDKs (Python boto3, Java, Node.js)
* API Calls
* Multipart Upload

### Example CLI

```bash
aws s3 cp file.txt s3://mybucket/
```

---

## 18. What is Multipart Upload?

### Answer

Multipart Upload divides a large file into smaller parts and uploads each part separately.

### Example

100 GB file:

* Part 1
* Part 2
* Part 3

AWS combines all parts after upload.

### Benefits

* Faster upload
* Retry failed parts only
* Better reliability

---

## 19. Why use Multipart Upload?

### Answer

Reasons:

* Faster upload through parallel upload
* Network failure affects only failed part
* Reliable large file upload
* Resume upload possible

### Example

Uploading 200 GB backup.

Without multipart:

Entire upload restarts if connection fails.

With multipart:

Only failed chunk uploads again.

---

## 20. Minimum File Size Recommended for Multipart Upload?

### Answer

AWS recommendation:

Use Multipart Upload for files **100 MB or larger**

### Important Limits

* Minimum part size → 5 MB
* Maximum parts → 10,000

---

## 21. Can Upload be Resumed after Interruption?

### Answer

Yes.

If Multipart Upload is used, only incomplete parts need re-uploading.

### Example

10 parts uploaded.

Part 7 fails.

Only Part 7 uploads again.

Entire file is not restarted.

---

## 22. How many Parts can Multipart Upload Have?

### Answer

Maximum:

**10,000 parts**

### Rules

* Minimum part size → 5 MB to 5GB
* Last part can be smaller

### Example

A 5 TB file can be divided into thousands of parts.

---

# Interview Trap Questions

## Can S3 run a Python Backend?

### Answer

No.

S3 only stores files.

It cannot execute backend code.

For backend use:

* EC2
* AWS Lambda

---

## Why is S3 not Block Storage?

### Answer

Because S3 stores data as objects.

It cannot be mounted like a hard disk.

Example of block storage:

Amazon EBS

---

## Can S3 Store Unlimited Data?

### Answer

Yes.

S3 storage is virtually unlimited.

Only limit:

**5 TB per object**

---

## Why do Companies use S3 instead of Local Servers?

### Answer

Because S3 provides:

* High durability
* Global scalability
* Lower maintenance
* Lower cost
* Automatic replication
* No hardware management

---

# Must Memorize Questions

1. What is S3?
2. Difference between S3, EBS, and EFS
3. What is a Bucket?
4. How does S3 provide 11 nines durability?
5. What is Multipart Upload?

---
