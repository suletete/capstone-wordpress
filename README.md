
---

# 🧱 Capstone Project: WordPress Site on AWS

## 📝 Project Scenario

A digital marketing agency, **DigitalBoost**, wants a **high-performance, scalable, and secure WordPress site** on AWS. Your job as an AWS Solutions Architect is to implement this using services like **VPC**, **EC2**, **RDS**, **EFS**, and more.

---

## 📦 Prerequisites

- Understanding of **TechOps Essentials**
- Completion of **Core 2 Courses and Mini Projects**

---

## 🏗️ Project Architecture Overview

The architecture includes:

- Public and private subnets across **2 Availability Zones**
- NAT Gateways for outbound traffic from private subnets
- Auto-scaling EC2 web servers
- Amazon RDS for MySQL database
- EFS for shared WordPress files
- Application Load Balancer
- Route 53 for DNS
- Secure configuration using **Security Groups**

![WordPress Architecture](attachment from above)

---

## 🧩 Project Components and Steps

---

### 1. 🔧 VPC Setup

**Objective:** Create a secure and isolated network environment for WordPress.

**Steps:**

- Define CIDR block: `10.0.0.0/16`
- Create the VPC.
- Create **public subnets** (e.g., `10.0.0.0/24`, `10.0.1.0/24`)
- Create **private subnets** (e.g., `10.0.2.0/24`, `10.0.3.0/24`)
- Associate subnets with route tables.
- Attach an **Internet Gateway** to allow public access.

---

### 2. 🌐 Public & Private Subnet with NAT Gateway

**Objective:** Allow internet access for private subnet instances securely.

**Steps:**

- Public Subnets:
  - Deploy **NAT Gateways** in public subnets.
  - Associate public route tables with Internet Gateway.
- Private Subnets:
  - Set route table with **default route to NAT Gateway** for outbound access.
  - Use for deploying **Web Servers and RDS** securely.

---

### 3. 🗄️ AWS RDS for MySQL

**Objective:** Use Amazon RDS for managing WordPress database.

**Steps:**

- Launch RDS instance (MySQL) in private subnet.
- Enable Multi-AZ for high availability.
- Create RDS **Security Group** to allow MySQL (port 3306) from Web Server SG.
- Provide RDS endpoint in WordPress configuration during setup.

---

### 4. 📁 EFS Setup for WordPress Files

**Objective:** Provide a shared, scalable file system for WordPress.

**Steps:**

- Create an **EFS File System**.
- Mount EFS targets in both availability zones.
- Attach EFS to web servers for shared file access.
- Update WordPress config (`wp-content`) to use EFS.

---

### 5. ⚖️ Application Load Balancer (ALB)

**Objective:** Distribute traffic across multiple EC2 instances.

**Steps:**

- Create ALB in public subnets.
- Add listener on **port 80 and 443**.
- Create **target group** pointing to web servers.
- Configure **Health Checks**.
- ALB Security Group:
  - Allow inbound on ports **80, 443** from `0.0.0.0/0`.

---

### 6. 📈 Auto Scaling Group

**Objective:** Automatically scale web server instances based on load.

**Steps:**

- Create a **Launch Template** or Configuration.
- Set up **Auto Scaling Group** across private subnets.
- Attach to ALB target group.
- Define scaling policies (e.g., CPU > 70% triggers scale-out).
- Ensure instances mount EFS and connect to RDS at boot time.

---

## 🔐 Security Group Summary

| Component                | Ports         | Source                               |
|-------------------------|---------------|--------------------------------------|
| ALB SG (1)              | 80, 443       | 0.0.0.0/0                             |
| SSH SG (2)              | 22            | Your IP Address                       |
| Web Server SG (3)       | 80, 443, 22   | ALB SG, SSH SG                        |
| Database SG (4)         | 3306          | Web Server SG                         |
| EFS SG (5)              | 2049, 22      | Web Server SG, EFS SG, SSH SG        |

---

## 🚀 Demonstration Tasks

- ✅ Deploy WordPress and verify it loads correctly via ALB.
- ✅ Show EFS is mounted and sharing content across EC2s.
- ✅ Simulate traffic using tools (e.g., Apache Benchmark) to trigger Auto Scaling.
- ✅ Verify new instances auto-register with ALB and maintain WordPress state via EFS and RDS.

---

## ✅ Deliverables

- 📄 Documentation of all setup components.
- 🔒 Explanation of all security configurations.
- 🌐 Live demo of WordPress site.
- 📊 Proof of auto-scaling functionality.

---

