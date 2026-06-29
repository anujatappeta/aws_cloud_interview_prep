# AWS EC2 Interview Mastery — Part 4

## Topic
**Amazon EC2 (Elastic Compute Cloud)**  
Focus Areas:
- Advanced EC2 Concepts
- Performance Optimization
- Deep Troubleshooting
- Production Architecture
- Hard Scenario-Based Questions

Coverage: Advanced → Expert → Real Production Interviews

Goal:
Master difficult EC2 interview questions and production troubleshooting.

---

# Section 14 — Advanced EC2 Concepts

## 95. What is User Data Script?

### Answer

User Data Script is a script that automatically runs when EC2 launches for the first time.

Used for:

- Installing packages
- Updating OS
- Configuring server
- Starting services automatically

Example:

```bash
#!/bin/bash
apt update
apt install nginx -y
systemctl start nginx
```

Purpose:

Automate server initialization.

---

## 96. What is Bootstrapping?

### Answer

Bootstrapping means automatically configuring EC2 during launch.

Example:

EC2 launches and automatically:

- Installs Python
- Installs Docker
- Starts application

Bootstrapping usually uses User Data.

Purpose:

Avoid manual configuration.

---

## 97. What is Instance Metadata?

### Answer

Instance Metadata is internal information about EC2 instance.

Contains:

- Instance ID
- Public IP
- Private IP
- IAM Role
- Security Group
- Region

Can be accessed inside instance.

Example:

```bash
curl http://169.254.169.254/latest/meta-data/
```

Used by applications running on EC2.

---

## 98. What is Instance Profile?

### Answer

Instance Profile is a container that attaches IAM Role to EC2.

Purpose:

Allow EC2 to access AWS services securely.

Example:

EC2 accessing S3 bucket without storing credentials.

Flow:

```text
IAM Role → Instance Profile → EC2
```

---

## 99. What is Placement Group?

### Answer

Placement Group controls how EC2 instances are physically placed inside AWS infrastructure.

Purpose:

Improve performance or fault tolerance.

Types:

- Cluster
- Partition
- Spread

---

## 100. What is Cluster Placement Group?

### Answer

Cluster Placement Group places EC2 instances close together in same hardware group.

Benefits:

- Low latency
- High network speed

Best for:

- High performance computing
- Distributed systems

---

## 101. What is Partition Placement Group?

### Answer

Partition Group separates instances into isolated partitions.

Purpose:

Reduce risk of simultaneous failure.

Best for:

- Large distributed systems
- Big data clusters

Example:

Hadoop clusters.

---

## 102. What is Spread Placement Group?

### Answer

Spread Group places each EC2 on separate hardware.

Purpose:

Maximum fault isolation.

Best for:

Critical applications where one hardware failure must not affect others.

---

## 103. What is Hibernation in EC2?

### Answer

Hibernation stops instance while preserving RAM contents.

Normal stop:

```text
RAM cleared
```

Hibernation:

```text
RAM saved to disk
```

When restarted:

Application resumes from same state.

---

## 104. Difference between Stop and Hibernate?

### Answer

| Stop | Hibernate |
|--------|-----------|
| RAM cleared | RAM preserved |
| Full reboot later | Resume previous state |
| Slower startup | Faster resume |

Use hibernation when application state must remain.

---

# Section 15 — Performance Optimization

## 105. What are CPU Credits?

### Answer

Burstable instances like T-series use CPU credits.

Credits allow temporary high CPU usage.

Example:

```text
t3.micro
```

Can burst above baseline temporarily.

---

## 106. Why is t2.micro Cheap?

### Answer

Because it provides low baseline CPU performance.

Uses CPU credit system.

Suitable for:

- Small websites
- Testing
- Low traffic workloads

Not ideal for heavy production workloads.

---

## 107. What happens when CPU Credits are Exhausted?

### Answer

Instance performance drops.

Example:

CPU may reduce from:

```text
100% → baseline performance
```

Application becomes slower.

Solution:

Upgrade instance or choose non-burstable instance.

---

## 108. Why choose C Series over M Series?

### Answer

C Series = Compute optimized.

Best for:

- CPU intensive applications
- Video processing
- Scientific computing

M Series = Balanced compute + memory.

Use C series when CPU demand is high.

---

## 109. Why choose R Series over C Series?

### Answer

R Series = Memory optimized.

Best for:

- Databases
- In-memory caching
- Analytics

Use R series when memory requirement is higher than CPU.

---

# Section 16 — Deep Troubleshooting

## 110. Why SSH Connection is Refused?

### Answer

Possible reasons:

- SSH service not running
- Wrong port
- Firewall blocking connection
- SSH daemon crashed

Check:

```bash
systemctl status ssh
```

---

## 111. Why SSH Connection Times Out?

### Answer

Possible reasons:

- Port 22 blocked in Security Group
- Wrong route table
- No internet gateway
- Instance stopped
- Wrong public IP

Check networking first.

---

## 112. Why is EC2 Unreachable?

### Answer

Possible causes:

- Instance stopped
- Network issue
- Security Group blocking traffic
- System status check failed
- CPU overloaded

Check:

- Status checks
- Networking
- Security Groups

---

## 113. Why Website is Not Opening on Port 80?

### Answer

Possible reasons:

- Port 80 blocked
- Web server not running
- DNS issue
- Firewall issue
- Application crashed

Check:

```bash
systemctl status nginx
```

or

```bash
systemctl status apache2
```

---

## 114. Why Instance is Stuck in Stopping State?

### Answer

Possible reasons:

- Operating system freeze
- Internal AWS issue
- Unresponsive shutdown process

Solution:

Force stop instance from AWS console.

---

## 115. Why Disk Becomes Full?

### Answer

Possible reasons:

- Large logs
- Database growth
- Temporary files
- Backup accumulation

Check:

```bash
df -h
```

Solution:

- Delete unnecessary files
- Extend EBS volume

---

## 116. Cannot Attach EBS Volume. Why?

### Answer

Possible reasons:

- Wrong Availability Zone
- Volume already attached
- Incorrect permissions

Important:

EBS can attach only in same Availability Zone.

---

## 117. Cannot Detach EBS Volume. Why?

### Answer

Possible reasons:

- Volume in use by application
- Mounted filesystem active
- Instance busy writing data

Solution:

Unmount first.

Example:

```bash
umount /dev/xvdf
```

Then detach safely.

---

## 118. Why Instance Fails Health Checks?

### Answer

Possible causes:

- OS corruption
- Kernel crash
- Hardware issue
- Network issue

Check:

- System logs
- CPU usage
- Disk usage
- AWS status checks

---

# Section 17 — Hard Production Scenarios

## 119. Production Server Accidentally Terminated. Recovery?

### Answer

If backup exists:

1. Create new EC2  
2. Restore EBS snapshot  
3. Attach restored volume  
4. Restart application  

Best practice:

Daily snapshots.

---

## 120. Traffic Suddenly Increases from 1,000 to 10 Million Users. Design Solution.

### Answer

Architecture:

```text
Auto Scaling Group
↓
Multiple EC2 Instances
↓
Load Balancer
↓
Multiple Availability Zones
```

Purpose:

Handle traffic automatically without downtime.

---

## 121. User Cannot SSH into EC2. Debug Step by Step.

### Answer

Check:

1. Security Group port 22  
2. Public IP  
3. PEM file  
4. Internet Gateway  
5. Route Table  
6. SSH service running  
7. Network ACL  
8. Instance status check  

Always debug networking first.

---

## 122. Website Returns 502 Bad Gateway. What do You Check?

### Answer

Check:

- Application running?
- Web server running?
- Reverse proxy configuration?
- Backend service healthy?
- Port mismatch?
- Application logs?

Example:

```bash
journalctl -xe
```

---

## 123. Disk Utilization Suddenly Becomes 100%. Fix?

### Answer

Check:

```bash
df -h
du -sh *
```

Possible solutions:

- Delete logs
- Remove temp files
- Increase EBS size
- Move old data

---

## 124. PEM Key Lost. Recovery?

### Answer

Method:

1. Stop instance  
2. Detach root EBS  
3. Attach volume to another EC2  
4. Edit authorized_keys  
5. Attach back to original instance  

Alternative:

Create new instance.

---

## 125. Application Works Locally but Not on EC2. Possible Reasons?

### Answer

Check:

- Security Group
- Missing dependencies
- Wrong environment variables
- Firewall blocking port
- Application binding issue

Common mistake:

```python
127.0.0.1
```

Should use:

```python
0.0.0.0
```

---

## 126. Need Zero Downtime Deployment. Design Solution.

### Answer

Use:

```text
Load Balancer
↓
Old EC2 running
↓
Deploy new EC2
↓
Switch traffic gradually
↓
Remove old EC2
```

Methods:

- Blue-Green Deployment
- Rolling Deployment

No downtime for users.

---

## 127. Need Cheapest Compute for Batch Processing. Which Pricing Model?

### Answer

Use:

```text
Spot Instances
```

Reason:

Batch jobs tolerate interruption.

Can save up to 90% cost.

---

# Section 18 — Hard Interview Questions

## Explain Complete EC2 Architecture.

### Answer

EC2 architecture flow:

```text
User launches EC2
↓
AWS Hypervisor creates Virtual Machine
↓
AMI loads Operating System
↓
vCPU + RAM allocated
↓
EBS attached as storage
↓
Network Interface attached
↓
Private/Public IP assigned
↓
Security Group controls traffic
↓
Application deployed
↓
CloudWatch monitors instance
```

Provides scalable cloud compute infrastructure.

---

## Why does AWS Use Hypervisor?

### Answer

Hypervisor allows multiple virtual machines to run on one physical server.

Benefits:

- Better resource utilization
- Isolation between customers
- Lower cost
- Efficient hardware usage

---

## Why is EC2 Called Elastic?

### Answer

Because compute resources can scale dynamically.

Example:

```text
Small server → Large server
1 instance → 100 instances
```

Resources change based on demand.

---

# Top 20 Must Memorize Questions

1. User Data Script  
2. Bootstrapping  
3. Instance Metadata  
4. Instance Profile  
5. Placement Group  
6. Cluster vs Partition vs Spread  
7. Hibernate vs Stop  
8. CPU Credits  
9. Why t2.micro is cheap  
10. Why SSH refused  
11. Why SSH timeout  
12. Why EC2 unreachable  
13. Why website not opening  
14. Why disk full  
15. Cannot attach EBS  
16. Lost PEM recovery  
17. High traffic architecture  
18. 502 Bad Gateway troubleshooting  
19. Zero downtime deployment  
20. Explain EC2 architecture  

---

EC2 Mastery Complete.
