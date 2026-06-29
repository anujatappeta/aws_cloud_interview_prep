# AWS EC2 Interview Mastery — Part 3

## Topic
**Amazon EC2 (Elastic Compute Cloud)**  
Focus Areas: **Lifecycle + Monitoring + Snapshots + Scaling + Availability**  
Coverage: Intermediate → Advanced → Production Concepts

**Goal:** Master EC2 operational behavior, scaling, monitoring, recovery, and high availability.

---

# Section 9 — EC2 Lifecycle

## 66. What are EC2 Instance States?

### Answer

EC2 instances move through different lifecycle states.

Main states:

- Pending  
- Running  
- Stopping  
- Stopped  
- Rebooting  
- Shutting Down  
- Terminated  

Flow:

```text
Launch → Pending → Running → Stop/Reboot → Terminate
```

---

## 67. Difference between Stop and Terminate?

### Answer

| Stop | Terminate |
|--------|-----------|
| Instance shuts down temporarily | Instance deleted permanently |
| Can restart later | Cannot restart |
| EBS usually remains | Root volume may delete |
| Public IP may change | Instance removed permanently |

Use:

- Stop → Temporary shutdown  
- Terminate → Permanent deletion  

---

## 68. Difference between Stop and Reboot?

### Answer

| Stop | Reboot |
|--------|--------|
| Fully shuts down | Restarts operating system |
| Public IP may change | Public IP remains same |
| Billing stops for compute | Billing continues |
| VM powers off | VM stays allocated |

Example:

Use reboot when OS freezes.

---

## 69. What happens when EC2 is Stopped?

### Answer

AWS performs:

- Operating system shuts down  
- CPU billing stops  
- RAM data cleared  
- Public IP released (if not Elastic IP)  
- EBS data remains  

Instance can restart later.

---

## 70. What happens when EC2 is Terminated?

### Answer

AWS performs:

- Instance deleted permanently  
- CPU resources released  
- RAM cleared  
- Public IP removed  
- Root EBS may delete (depends on configuration)  

Instance cannot be recovered directly.

---

## 71. Does Public IP change after Stop/Start?

### Answer

Yes.

Default public IP changes.

Example:

Before stop:

```text
54.201.xx.xx
```

After start:

```text
52.10.xx.xx
```

If Elastic IP attached:

IP remains same.

---

## 72. What happens to EBS after Termination?

### Answer

Depends on **Delete on Termination** setting.

If enabled:

```text
Root EBS deleted
```

If disabled:

```text
Volume remains
```

Additional volumes usually remain.

Always check configuration.

---

# Section 10 — Monitoring

## 73. How do you Monitor EC2?

### Answer

AWS provides monitoring through:

- CloudWatch Metrics  
- Status Checks  
- Logs  
- Performance graphs  

Track:

- CPU utilization  
- Disk usage  
- Network traffic  
- Status checks  

---

## 74. What is CloudWatch?

### Answer

CloudWatch is AWS monitoring service used to monitor AWS resources.

Used for:

- Metrics collection  
- Logs monitoring  
- Alarms  
- Performance tracking  

Example:

Monitor CPU usage of EC2.

---

## 75. What is CPU Utilization?

### Answer

CPU utilization measures percentage of CPU being used.

Example:

```text
CPU = 80%
```

Meaning:

80% processing power is busy.

High CPU may indicate:

- Heavy workload  
- Infinite loop  
- Too many users  
- Insufficient instance size

---

## 76. How do you Monitor Memory in EC2?

### Answer

AWS does not provide memory metrics by default.

Need:

CloudWatch Agent installed manually.

Then monitor:

- RAM usage  
- Memory pressure  
- Swap usage  

---

## 77. How do you Monitor Disk Usage?

### Answer

Methods:

- CloudWatch Agent  
- Linux commands  

Example:

```bash
df -h
```

Check:

- Free space  
- Used space  
- Mounted volumes  

Important for production servers.

---

## 78. What are Status Checks?

### Answer

AWS automatically checks instance health.

Two checks:

- System Status Check  
- Instance Status Check  

Used to detect infrastructure or OS issues.

---

## 79. Difference between System Status Check and Instance Status Check?

### Answer

| System Status Check | Instance Status Check |
|---------------------|----------------------|
| AWS infrastructure issue | OS/Application issue |
| Hardware/network problem | Operating system problem |

Examples:

System issue:

```text
AWS host hardware failed
```

Instance issue:

```text
Linux kernel crash
```

---

# Section 11 — Snapshots

## 80. What is Snapshot?

### Answer

Snapshot is backup copy of EBS volume stored by AWS.

Used for:

- Backup  
- Disaster recovery  
- Volume restoration  

Example:

Backup server disk before update.

---

## 81. Snapshot vs Backup?

### Answer

| Snapshot | Backup |
|-----------|--------|
| Volume-level copy | General data backup |
| Faster restore | Full recovery process |
| Used for EBS | Broader concept |

Snapshots mainly protect storage volumes.

---

## 82. How does Snapshot work?

### Answer

AWS copies EBS data and stores backup internally.

Process:

```text
EBS Volume → Snapshot Storage → Restore Later
```

Snapshots stored redundantly by AWS.

---

## 83. What is Incremental Snapshot?

### Answer

Incremental means only changed data is stored after first snapshot.

Example:

Snapshot 1:

```text
100 GB stored
```

Snapshot 2:

```text
Only changed 5 GB stored
```

Benefits:

- Lower storage cost  
- Faster backup  

---

## 84. Can Snapshot Restore Instance?

### Answer

Indirectly yes.

Process:

1. Create new volume from snapshot  
2. Attach volume to EC2  
3. Launch instance if needed  

Used for disaster recovery.

---

## 85. Can Snapshots be Encrypted?

### Answer

Yes.

If EBS volume encrypted:

Snapshot also encrypted.

Can use AWS KMS.

Benefits:

Protect sensitive data.

---

# Section 12 — Scaling

## 86. What is Auto Scaling?

### Answer

Auto Scaling automatically increases or decreases EC2 instances based on demand.

Purpose:

Handle changing traffic automatically.

Benefits:

- Better availability  
- Lower cost  
- Automatic scaling

---

## 87. Why is Auto Scaling Needed?

### Answer

Traffic may change unexpectedly.

Without scaling:

```text
CPU overload
Website crashes
```

With scaling:

AWS launches more EC2 instances automatically.

Improves availability.

---

## 88. Difference between Scale Out and Scale In?

### Answer

| Scale Out | Scale In |
|------------|----------|
| Add more instances | Remove instances |
| Handles more traffic | Saves cost |

Example:

Traffic increases:

```text
2 EC2 → 10 EC2
```

Traffic decreases:

```text
10 EC2 → 2 EC2
```

---

## 89. Difference between Horizontal Scaling and Vertical Scaling?

### Answer

Horizontal Scaling:

Add more servers.

Example:

```text
2 servers → 5 servers
```

Vertical Scaling:

Increase resources in same server.

Example:

```text
2 GB RAM → 16 GB RAM
```

---

## 90. What is Dynamic Scaling?

### Answer

Scaling based on live metrics.

Example:

Rule:

```text
CPU > 70%
Launch new instance
```

AWS automatically responds to traffic changes.

---

## 91. What is Scheduled Scaling?

### Answer

Scaling based on fixed time schedule.

Example:

Every day:

```text
9 AM → Launch 10 servers
10 PM → Reduce to 2 servers
```

Used for predictable traffic patterns.

---

# Section 13 — Availability

## 92. What is High Availability?

### Answer

High availability means application remains accessible even during failures.

Methods:

- Multiple EC2 instances  
- Multiple Availability Zones  
- Load balancing  
- Auto Scaling  

Goal:

Minimize downtime.

---

## 93. What is Fault Tolerance?

### Answer

Fault tolerance means system continues working even if components fail.

Example:

If one server crashes:

```text
Second server handles traffic
```

No service interruption.

---

## 94. What happens if EC2 Instance Crashes?

### Answer

Possible causes:

- OS failure  
- Application crash  
- Hardware failure  
- CPU overload  

Recovery options:

- Restart instance  
- Restore snapshot  
- Launch replacement instance  

---

# Interview Trap Questions

## CPU reaches 100% continuously. What do you do?

Answer:

Check:

- Running processes  
- Application logs  
- Traffic spike  
- Memory usage  
- Scale instance if required  

Commands:

```bash
top
htop
```

Possible solution:

Scale vertically or horizontally.

---

## Instance failed Status Check. What do you check?

Answer:

Check:

- AWS system check  
- OS logs  
- Disk usage  
- CPU usage  
- Network connectivity  

Determine if AWS issue or OS issue.

---

## Instance accidentally terminated. Recovery?

Answer:

If snapshot exists:

- Create new volume  
- Launch new EC2  
- Attach restored volume  

If no backup:

Recovery difficult.

Best practice:

Regular snapshots.

---

## Why does Public IP change after Stop/Start?

Answer:

Because AWS releases temporary public IP when instance stops.

New IP assigned when restarted.

Use Elastic IP if fixed IP needed.

---

# Top Questions to Memorize

1. EC2 lifecycle states  
2. Stop vs Reboot vs Terminate  
3. What happens after terminate  
4. Why Public IP changes  
5. What is CloudWatch  
6. CPU utilization  
7. System check vs Instance check  
8. What is Snapshot  
9. Snapshot vs Backup  
10. Incremental snapshot  
11. What is Auto Scaling  
12. Scale Out vs Scale In  
13. Horizontal vs Vertical Scaling  
14. Dynamic Scaling  
15. High Availability vs Fault Tolerance  

---
