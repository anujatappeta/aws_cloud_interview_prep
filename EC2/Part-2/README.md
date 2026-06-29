# AWS EC2 Interview Mastery — Part 2

## Topic
**Amazon EC2 (Elastic Compute Cloud)**  
Focus Areas: **Networking + Security + Access + Storage**  
Coverage: Intermediate → Advanced → Real Interview Questions

**Goal:** Master networking, security, access control, and storage concepts in EC2.

---

# Section 5 — EC2 Networking

## 34. What is Public IP in EC2?

### Answer

Public IP is an IP address assigned to EC2 instance that allows communication over the internet.

Purpose:

- Access instance from outside AWS  
- SSH from local machine  
- Host public websites  

Example:

```text
54.210.xx.xx
```

Anyone on internet can reach instance if security rules allow.

---

## 35. What is Private IP in EC2?

### Answer

Private IP is internal IP address used for communication inside AWS network.

Purpose:

- Communication between EC2 instances  
- Internal application communication  
- Database connection inside VPC  

Example:

```text
172.31.xx.xx
```

Private IP cannot be accessed directly from internet.

---

## 36. Difference between Public IP and Private IP?

### Answer

| Public IP | Private IP |
|------------|------------|
| Accessible over internet | Internal AWS communication |
| Can change after stop/start | Usually remains same |
| Used for SSH/web access | Used inside private network |

Example:

Public IP → Website access

Private IP → App server connecting to DB server

---

## 37. What is Elastic IP?

### Answer

Elastic IP is a static public IP address provided by AWS.

Benefits:

- Does not change after stop/start  
- Permanent public address  
- Can be reassigned to another instance  

Used when fixed public IP is required.

---

## 38. Why is Elastic IP Needed?

### Answer

Normal public IP may change when instance stops and starts.

Elastic IP solves this.

Use cases:

- Production server  
- DNS mapping  
- Static public endpoint  

Example:

Company website requires permanent IP.

---

## 39. Can Public IP change after Stop/Start?

### Answer

Yes.

If using default public IP:

```text
Stop → Start → New Public IP
```

If using Elastic IP:

```text
IP remains same
```

---

## 40. What is DNS Name in EC2?

### Answer

AWS automatically assigns DNS hostname.

Example:

```text
ec2-54-210-xx-xx.compute.amazonaws.com
```

Purpose:

Use domain-style access instead of remembering IP address.

---

## 41. What happens when EC2 gets IP address?

### Answer

When EC2 launches:

1. AWS attaches network interface  
2. Private IP assigned internally  
3. Public IP assigned if enabled  
4. Route tables handle communication  

Instance becomes reachable.

---

# Section 6 — Security Groups

## 42. What is Security Group?

### Answer

Security Group is a virtual firewall controlling traffic to EC2 instance.

Controls:

- Inbound traffic  
- Outbound traffic  

Used to allow or deny network traffic.

Example:

Allow SSH on port 22.

---

## 43. Why is Security Group called Virtual Firewall?

### Answer

Because it filters incoming and outgoing traffic just like firewall.

Example:

Allow:

```text
Port 22 → SSH
Port 80 → HTTP
Port 443 → HTTPS
```

Block all other ports.

Protects EC2 from unauthorized access.

---

## 44. What are Inbound Rules?

### Answer

Inbound rules control incoming traffic to EC2.

Example:

```text
Port 22 → SSH access
Port 80 → Website traffic
Port 443 → HTTPS traffic
```

Without inbound permission:

Traffic cannot enter instance.

---

## 45. What are Outbound Rules?

### Answer

Outbound rules control traffic leaving EC2.

Example:

EC2 sending request to:

- Internet  
- Database  
- External API  

Default:

Allow all outbound traffic.

---

## 46. What does Stateful Firewall mean?

### Answer

Stateful means if incoming traffic is allowed, response traffic is automatically allowed.

Example:

User sends SSH request.

Security group allows:

```text
Port 22 inbound
```

Response automatically allowed.

No separate outbound rule needed.

---

## 47. Can one EC2 have Multiple Security Groups?

### Answer

Yes.

One instance can attach multiple security groups.

Example:

Security Group 1:

```text
Allow SSH
```

Security Group 2:

```text
Allow HTTP
```

AWS combines rules.

---

## 48. What happens if Port 22 is closed?

### Answer

SSH connection fails.

Result:

```text
Connection Timed Out
```

Because SSH uses:

```text
Port 22
```

Without inbound rule:

Server cannot be accessed remotely.

---

## 49. Why cannot SSH connect?

### Answer

Possible reasons:

- Port 22 closed  
- Wrong PEM key  
- Wrong username  
- Security Group misconfiguration  
- Instance stopped  
- Network ACL issue  
- Firewall blocking traffic  

Always debug step by step.

---

# Section 7 — Access Control

## 50. What is Key Pair?

### Answer

Key Pair is authentication method used to securely connect to EC2.

Contains:

- Public Key → Stored in AWS  
- Private Key → Stored with user (.pem file)

Used for SSH login.

---

## 51. Why is PEM File used?

### Answer

PEM file stores private key used for secure authentication.

Purpose:

Verify identity before server access.

Without PEM file:

SSH login fails.

---

## 52. Difference between PEM and PPK?

### Answer

| PEM | PPK |
|------|-----|
| Used in Linux/Mac/OpenSSH | Used in PuTTY (Windows) |
| Standard format | PuTTY format |

Example:

```text
.pem → SSH command
.ppk → PuTTY software
```

---

## 53. How does SSH work internally?

### Answer

Process:

1. User sends SSH request  
2. Server checks public key  
3. Private key verifies identity  
4. Encrypted connection established  
5. Secure terminal opens  

Provides secure remote login.

---

## 54. Can EC2 be accessed without Key Pair?

### Answer

Normally no.

Alternatives:

- EC2 Instance Connect  
- Session Manager  
- Password login (not recommended)

Best practice:

Use key pair authentication.

---

## 55. What happens if PEM file is lost?

### Answer

Direct SSH access becomes impossible.

Recovery options:

- Create new instance  
- Attach volume to another instance  
- Replace authorized_keys manually  

Best practice:

Always backup PEM file.

---

## 56. Why does SSH connection timeout occur?

### Answer

Common reasons:

- Port 22 blocked  
- Wrong security group  
- Public IP changed  
- Instance stopped  
- Internet gateway missing  
- Route table issue  

Check networking first.

---

# Section 8 — IAM + Storage

## 57. Why attach IAM Role to EC2?

### Answer

IAM Role allows EC2 to access AWS services securely without storing credentials.

Example:

EC2 accessing:

- S3  
- DynamoDB  
- Secrets Manager  

More secure than access keys.

---

## 58. IAM Role vs IAM User?

### Answer

| IAM User | IAM Role |
|-----------|----------|
| Permanent identity | Temporary permissions |
| Used by humans | Used by AWS services |
| Has credentials | No permanent credentials |

Example:

Developer → IAM User

EC2 → IAM Role

---

## 59. Can EC2 access S3 without Access Keys?

### Answer

Yes.

Attach IAM Role with S3 permissions.

Example policy:

```text
Allow: s3:GetObject
```

EC2 automatically gets temporary credentials.

More secure than hardcoding access keys.

---

## 60. What is EBS?

### Answer

EBS (Elastic Block Store) is persistent block storage attached to EC2.

Characteristics:

- Works like hard disk  
- Stores operating system  
- Data persists after reboot  

Used for application storage.

---

## 61. Why is EBS Needed?

### Answer

EC2 requires storage for:

- Operating system  
- Application files  
- Logs  
- Database files  

EBS acts like server hard disk.

Without EBS:

Instance cannot store data permanently.

---

## 62. What is Root Volume?

### Answer

Root volume stores operating system.

Example:

Ubuntu instance root volume contains:

- Linux OS  
- System files  
- Boot files  

Without root volume:

Instance cannot boot.

---

## 63. What is Additional Volume?

### Answer

Additional volume is extra storage attached beyond root volume.

Used for:

- Database storage  
- Large files  
- Backup storage  

Example:

```text
/dev/xvdf
```

Separate from operating system disk.

---

## 64. Difference between Instance Store and EBS?

### Answer

| Instance Store | EBS |
|---------------|-----|
| Temporary storage | Persistent storage |
| Lost after stop/terminate | Data remains |
| Physically attached storage | Network attached storage |

Use:

Instance Store → Temporary cache

EBS → Important persistent data

---

## 65. What are EBS Types?

### Answer

Main EBS types:

### gp3

General purpose SSD.

Best for:

Normal workloads.

### io2

High performance SSD.

Best for:

Databases requiring high IOPS.

### st1

Throughput optimized HDD.

Best for:

Big data workloads.

### sc1

Cold HDD storage.

Best for:

Rarely accessed data.

---

# Interview Trap Questions

## Why website hosted on EC2 is not opening?

Answer:

Possible reasons:

- Port 80 blocked  
- Web server not running  
- Security Group issue  
- Wrong public IP  
- DNS issue  
- Application crashed

---

## Why SSH connection timeout happens?

Answer:

Check:

- Port 22  
- Security Group  
- Public IP  
- Route table  
- Internet Gateway  
- PEM key

---

## Why use IAM Role instead of Access Key?

Answer:

Because access keys can be leaked.

IAM Role provides temporary secure credentials.

Best practice:

Never hardcode access keys.

---

## Why use EBS instead of local disk?

Answer:

Because EBS provides persistent storage.

Data survives reboot.

Can create snapshots for backup.

---

# Top Questions to Memorize

1. Public IP vs Private IP  
2. What is Elastic IP  
3. Security Group  
4. Inbound vs Outbound Rules  
5. Stateful Firewall  
6. Why SSH timeout happens  
7. What is Key Pair  
8. PEM vs PPK  
9. What is IAM Role  
10. IAM Role vs IAM User  
11. What is EBS  
12. Root Volume vs Additional Volume  
13. EBS vs Instance Store  
14. Why website not opening  
15. Can EC2 access S3 without access key  

---
