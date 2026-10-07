# AWS Project 2: Secure Multi-Tier VPC Architecture (EC2 & RDS)
A hands-on implementation of a multi-tier cloud infrastructure on AWS focusing on network isolation, high security standards, and the principle of **Least Privilege**.

---

## 📐 Architecture Overview

text
[ Internet ]
│
▼
┌────────────────────────────────────────────────────────┐
│ VPC (10.0.0.0/16)                                      │
│                                                        │
│  ┌────────────────────────┐  ┌──────────────────────┐  │
│  │ Public Subnet          │  │ Private Subnets      │  │
│  │                        │  │                      │  │
│  │  [ EC2 Web Server ]    │  │  [ RDS MySQL DB ]    │  │
│  │  (web-server-sg)       │──┼─►(rds-sg)            │  │
│  │  Port 80 / 22          │  │  Port 3306 ONLY      │  │
│  └────────────────────────┘  └──────────────────────┘  │
└────────────────────────────────────────────────────────┘


---

## 🛠️ Infrastructure Breakdown

* **Custom VPC:** Network range `10.0.0.0/16` with Internet Gateway attached.
* **Public Subnet:** Hosts the EC2 web server accessible from the public internet.
* **Private Subnet Group:** Hosts the Amazon RDS MySQL database spanning multiple Availability Zones with zero public access (`Public Access = No`).
* **Least Privilege Security:** `rds-sg` strictly restricts inbound MySQL traffic (Port 3306) to only accept requests originating from `web-server-sg`.

---

## 🧪 Deployment Verification & Evidence

### 1. Web Server Deployment
The EC2 instance was bootstrapped via `User Data` scripts to automatically configure Apache (`httpd`) and serve the application.

![Web Server Output](images/web-server.png)

---

### 2. Network Security & Inbound Rules
Database access is restricted exclusively to traffic from the Web Server's Security Group ID.

![RDS Security Group Rules](images/security-group.png)

---

### 3. Database Connectivity Test
Verified cross-tier connection from the EC2 instance inside the VPC directly to the isolated RDS database via SSH / Terminal.

![Terminal Connection Proof](images/terminal-db.png)

---

### 4. VPC Resource Mapping
Full visual layout of subnets, route tables, and gateways provisioned across the architecture.

![VPC Map](images/vpc-map.png)

---

## 🔒 Security Takeaways
1. **Network Isolation:** Sensitive workloads (like databases) should never sit in public-facing subnets.
2. **Security Group Referencing:** By referencing `web-server-sg` inside `rds-sg` rules instead of explicit IP addresses, the configuration remains dynamic, scalable, and resilient against IP changes.

---

## 🚀 Next Steps
In **Project 3**, this architecture will be enhanced with High Availability and Auto Scaling using an **Application Load Balancer (ALB)** and **Auto Scaling Groups (ASG)**.
