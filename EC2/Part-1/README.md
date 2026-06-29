# AWS EC2 Interview Mastery — Part 1

## Topic
**Amazon EC2 (Elastic Compute Cloud)**  
Focus Areas: **Fundamentals + Core Architecture + Instance Basics + AMI + Pricing Models**  
Coverage: Beginner → Intermediate → Interview Trap Questions

**Goal:** Build strong EC2 fundamentals for top cloud interviews.

---

# Section 1 — EC2 Fundamentals

## 1. What is EC2?

### Answer

Amazon EC2 (Elastic Compute Cloud) is an AWS service that provides **resizable virtual servers** in the cloud.

It allows users to launch and manage computing resources without buying physical hardware.

Main features:

- Virtual servers on demand  
- Scalable compute capacity  
- Pay only for usage  
- Full control over operating system  
- Supports multiple workloads  

Example:

Hosting a web application on Linux server.

---

## 2. Why do we use EC2?

### Answer

EC2 is used when applications require computing power in the cloud.

Common use cases:

- Website hosting  
- Running backend applications  
- Database servers  
- Machine learning workloads  
- Batch processing  
- Development/testing environments  
- Microservices deployment  

Example:

Running a Python Flask backend on Ubuntu server.

---

## 3. Why is EC2 called a Virtual Server?

### Answer

EC2 is called virtual server because it behaves like a physical server but runs virtually inside AWS infrastructure.

It provides:

- CPU  
- RAM  
- Storage  
- Operating system  
- Network connectivity  

Difference:

No physical hardware ownership required.

---

## 4. What is Virtualization?

### Answer

Virtualization is technology that allows multiple virtual machines to run on one physical server.

Example:

One physical machine can run:

- Linux VM  
- Windows VM  
- Ubuntu VM  

Benefits:

- Better resource utilization  
- Lower cost  
- Efficient infrastructure usage  

---

## 5. Difference between Physical Server and EC2?

### Answer

| Physical Server | EC2 |
|---------------|-----|
| Need hardware purchase | No hardware purchase |
| Manual maintenance | AWS manages infrastructure |
| Limited scalability | Highly scalable |
| Fixed resources | Resize anytime |
| High upfront cost | Pay as you use |

Example:

Buying Dell server vs launching EC2 instance.

---

## 6. What is Hypervisor?

### Answer

Hypervisor is software that creates and manages virtual machines on physical hardware.

Its job:

- Divide hardware resources  
- Allocate CPU and RAM  
- Isolate multiple virtual machines  

Types:

- Type 1 → Runs directly on hardware  
- Type 2 → Runs on operating system  

AWS uses virtualization technology internally.

---

## 7. What is AWS Nitro System?

### Answer

Nitro System is AWS architecture used to improve EC2 performance and security.

It offloads virtualization functions to dedicated hardware.

Benefits:

- Better performance  
- Better security isolation  
- Lower virtualization overhead  
- Faster networking and storage  

Used in modern EC2 instances.

---

## 8. Is EC2 Regional or Global?

### Answer

EC2 is a regional service.

Instances are launched inside specific AWS regions.

Example:

- Mumbai → ap-south-1  
- Virginia → us-east-1  

Resources stay inside selected region.

---

## 9. What happens internally when EC2 launches?

### Answer

When EC2 launches:

1. AWS selects physical server  
2. Hypervisor creates virtual machine  
3. AMI loads operating system  
4. CPU and RAM allocated  
5. Storage attached  
6. Network interface created  
7. IP address assigned  
8. Instance starts running  

Result:

Virtual server becomes available.

---

## 10. Which resources are allocated to EC2?

### Answer

AWS allocates:

- vCPU  
- RAM  
- Storage  
- Network bandwidth  
- IP address  
- Operating system from AMI  

Resources depend on instance type.

Example:

t3.micro:

- 2 vCPU  
- 1 GB RAM

---

# Section 2 — Instance Basics

## 11. What is an EC2 Instance?

### Answer

An EC2 instance is a virtual machine running inside AWS cloud.

Each instance contains:

- CPU  
- Memory  
- Storage  
- Operating system  
- Network interface  

Example:

Ubuntu server running application.

---

## 12. Difference between Instance and Server?

### Answer

| Server | Instance |
|---------|----------|
| General term | AWS virtual machine |
| Can be physical | Always virtual in AWS |
| Can exist anywhere | Runs in AWS cloud |

Simple meaning:

EC2 instance is AWS cloud server.

---

## 13. What are Instance Types?

### Answer

Instance types define hardware configuration.

They specify:

- CPU  
- RAM  
- Storage performance  
- Network performance  

AWS provides different families based on workload.

---

## 14. What are EC2 Instance Families?

### Answer

Main instance families:

### T Series

Burstable general purpose.

Example:

```text
t2.micro
t3.micro
```

### M Series

Balanced compute + memory.

Example:

```text
m5.large
```

### C Series

Compute optimized.

Best for:

- High CPU workloads  
- Video processing  

Example:

```text
c5.large
```

### R Series

Memory optimized.

Best for:

- Large databases  
- Analytics  

Example:

```text
r5.large
```

### P Series

GPU optimized.

Best for:

- Machine learning  
- AI training  

Example:

```text
p3.large
```

### I Series

Storage optimized.

Best for:

High IOPS workloads.

---

## 15. What is vCPU?

### Answer

vCPU = Virtual CPU.

It represents virtual processing power assigned to EC2.

Example:

2 vCPU means AWS gives two virtual processing units.

More vCPU:

- Faster processing  
- Better parallel execution

---

## 16. How does AWS allocate RAM?

### Answer

RAM depends on instance type.

Examples:

```text
t2.micro → 1 GB RAM
m5.large → 8 GB RAM
r5.large → 16 GB RAM
```

AWS allocates memory based on selected instance family.

---

## 17. What is Bare Metal Instance?

### Answer

Bare Metal means EC2 runs directly on physical server without virtualization layer.

Benefits:

- Full hardware access  
- Better performance  
- Suitable for specialized workloads  

Used when applications need direct hardware control.

---

## 18. What is Dedicated Host?

### Answer

Dedicated Host gives entire physical server exclusively to one customer.

Benefits:

- Full server isolation  
- License compliance  
- Better control  

No resource sharing with other AWS customers.

---

## 19. Difference between Shared Tenancy and Dedicated Tenancy?

### Answer

| Shared Tenancy | Dedicated Tenancy |
|---------------|------------------|
| Hardware shared with others | Dedicated hardware |
| Lower cost | Higher cost |
| Default option | Special isolation |

Most users use shared tenancy.

---

# Section 3 — AMI

## 20. What is AMI?

### Answer

AMI = Amazon Machine Image.

AMI is a template used to launch EC2 instances.

It contains:

- Operating system  
- Software packages  
- Configuration settings  
- Boot instructions  

Example:

Ubuntu 22.04 AMI

---

## 21. Why AMI is Needed?

### Answer

AMI provides preconfigured system image.

Without AMI:

EC2 would not know which operating system to install.

Example:

Launch Ubuntu server instantly.

---

## 22. What does AMI contain?

### Answer

AMI contains:

- Operating system  
- Application software  
- Configuration files  
- Root volume snapshot  
- Boot configuration  

Used as template for launching instance.

---

## 23. Public AMI vs Private AMI?

### Answer

| Public AMI | Private AMI |
|------------|-------------|
| Available to everyone | Available only to owner |
| AWS marketplace images | Custom organization image |

Example:

Ubuntu AMI = Public

Company internal image = Private

---

## 24. Can we Create Custom AMI?

### Answer

Yes.

Steps:

1. Launch EC2  
2. Install applications  
3. Configure server  
4. Create image  

Benefits:

Launch multiple identical servers quickly.

---

## 25. What happens internally when launching from AMI?

### Answer

AWS performs:

1. Reads AMI template  
2. Creates storage volume  
3. Copies operating system  
4. Creates virtual machine  
5. Allocates CPU/RAM  
6. Starts operating system  

Instance becomes ready.

---

# Section 4 — Pricing Models

## 26. What are EC2 Pricing Models?

### Answer

Main pricing models:

- On Demand  
- Reserved Instance  
- Spot Instance  
- Savings Plans  
- Dedicated Hosts  

Choose based on workload.

---

## 27. What is On Demand Pricing?

### Answer

Pay only for actual usage.

Benefits:

- No commitment  
- Flexible  
- Suitable for testing  

Example:

Use server for 5 hours → pay for 5 hours.

---

## 28. What is Reserved Instance?

### Answer

Reserve instance for long period.

Terms:

- 1 year  
- 3 years  

Benefits:

Lower cost compared to On Demand.

Best for predictable workloads.

---

## 29. What is Spot Instance?

### Answer

Spot Instance uses unused AWS capacity.

Benefits:

Very cheap.

Limitation:

AWS can terminate anytime.

Best for:

- Batch jobs  
- Temporary workloads

---

## 30. Why is Spot Instance Cheap?

### Answer

Because AWS sells unused infrastructure capacity.

AWS can reclaim resources anytime.

Since interruption is possible:

Price is lower.

Sometimes 70–90% cheaper.

---

## 31. What is Savings Plan?

### Answer

Savings Plan gives discounted pricing if customer commits usage over time.

Example:

Commit compute usage for 1 year.

Benefit:

Lower cost.

More flexible than Reserved Instance.

---

## 32. When should Reserved Instance be used?

### Answer

Use Reserved when workload runs continuously.

Example:

Production application running 24/7.

Benefits:

Cheaper than On Demand.

---

## 33. Why is EC2 cheaper than Physical Infrastructure?

### Answer

Because AWS manages infrastructure.

Benefits:

- No hardware purchase  
- No maintenance cost  
- No cooling/electricity cost  
- No server room required  
- Pay only for usage  

Lower operational cost.

---

# Interview Trap Questions

## Why is EC2 called Elastic Compute Cloud?

Answer:

Elastic means resources can scale up or down based on demand.

Compute means processing power.

Cloud means infrastructure delivered over internet.

---

## Why is Spot Instance risky?

Answer:

Because AWS can terminate it anytime when capacity needed elsewhere.

Not suitable for production.

---

## Why AMI is important?

Answer:

Because AMI contains operating system and configuration needed to launch EC2.

Without AMI instance cannot boot.

---

## Why does AWS use Virtualization?

Answer:

To allow multiple virtual machines on one physical server.

Benefits:

- Better utilization  
- Lower cost  
- Efficient resource sharing

---

# Top Questions to Memorize

1. What is EC2  
2. What is Virtualization  
3. What is Hypervisor  
4. What is AWS Nitro System  
5. What happens internally when EC2 launches  
6. What is AMI  
7. Public vs Private AMI  
8. What is vCPU  
9. Spot vs Reserved vs On Demand  
10. Why Spot Instance is cheap  

---
