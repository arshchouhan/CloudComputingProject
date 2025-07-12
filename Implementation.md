# 🏗️ AWS Infrastructure Overview

This document provides a comprehensive explanation of the AWS infrastructure design used in this project. It serves as a blueprint for building secure, highly available, and scalable cloud architectures following industry best practices.

---

## 🌐 Virtual Private Cloud (VPC)

A **Virtual Private Cloud (VPC)** is a logically isolated network within AWS that allows you to launch and manage cloud resources with full control over networking, routing, and security.

### 🔑 Key Features:
- Custom CIDR blocks (e.g., `10.0.0.0/16`) for IP range control
- Custom route tables, subnets, and network ACLs
- Serves as the base for public/private subnet separation, enabling tiered architecture
- Fully customizable DNS and DHCP options

---

## 🧭 Public & Private Subnets

Subnets logically divide a VPC's IP address range into smaller segments. This segmentation enables fine-grained access control and separation of concerns.

### 🔓 Public Subnets
- Associated with an **Internet Gateway (IGW)**
- Host public-facing components such as **ALBs**, **Bastion Hosts**, or **static websites**

### 🔐 Private Subnets
- No direct access to the public internet
- Used for internal services like **RDS**, **ECS**, and **Lambda** functions
- Internet access is routed through a **NAT Gateway**

---

## 📍 Availability Zones (AZs)

**Availability Zones (AZs)** are isolated physical data centers within an AWS region. Deploying resources across multiple AZs is critical for building highly available systems.

- Ensure **fault tolerance** and **disaster recovery**
- Allow for **load balancing** across zones
- Required for **Auto Scaling**, **Elastic Load Balancing**, and **multi-AZ RDS setups**

---

## 🎯 Target Groups

Target groups act as routing destinations for Load Balancers.

### ✅ Features:
- Contain **EC2 instances**, **IP addresses**, or **Lambda functions**
- Used in **ALB**, **NLB**, or **GWLB** setups
- Support protocol-specific **health checks**
- Enable **blue/green** or **canary deployments** through versioned target groups

---

## ⚖️ Load Balancers

### 🌀 Application Load Balancer (ALB)
- Operates at **Layer 7 (HTTP/HTTPS)**
- Supports advanced routing: **path-based**, **host-based**, **header-based**
- Ideal for microservices, REST APIs, and web frontends

### 🧷 Network Load Balancer (NLB)
- Operates at **Layer 4 (TCP/UDP)**
- Designed for **extremely low latency** and **high throughput**
- Supports **static IP addresses** and **TLS termination**

---

## 🚪 Internet Gateway vs. NAT Gateway

| Component           | Use Case                                        | Placed In                                |
|--------------------|--------------------------------------------------|-------------------------------------------|
| **Internet Gateway** | Enables public internet access                  | Public Subnet                             |
| **NAT Gateway**       | Enables outbound access for private subnets    | Public Subnet (with routing from private) |

---

## 🔐 Security: Security Groups vs NACLs

| Feature           | Security Groups                        | Network ACLs                              |
|------------------|-----------------------------------------|-------------------------------------------|
| **Level**        | Instance-level                         | Subnet-level                              |
| **Type**         | Stateful (automatic return traffic)     | Stateless (explicit inbound/outbound)     |
| **Use Case**     | Fine-grained access control            | Broad subnet-level restrictions           |

---

## ☁️ EC2 Instances

- Deployed in **public subnets** to allow direct access for testing, debugging, or specific public-facing workloads
- Managed with **Auto Scaling Groups** to ensure elasticity
- IAM roles are assigned for secure access to other AWS services
- Access is secured using **Security Groups** and optionally through **key-pair SSH access** or **Bastion Hosts**

> ⚠️ **Note:** While deploying EC2 instances in public subnets can be suitable for some use cases (e.g., public APIs), ensure strict security measures are enforced.

---

## 📡 Route Tables

Route tables define how network traffic flows within your VPC.

- **Public Subnet** routes go through the **Internet Gateway**
- **Private Subnet** routes go through the **NAT Gateway**
- Enables custom routing and integration with Transit Gateways or VPNs

---

## 🛡️ Why This Architecture?

| Component           | Purpose                                                  |
|--------------------|-----------------------------------------------------------|
| **VPC**            | Secure, isolated networking foundation                    |
| **Public Subnets** | Host internet-facing resources like ALB or EC2            |
| **Private Subnets**| Secure backend components like databases and internal services |
| **Load Balancers** | Ensure high availability and scalability of services      |
| **Target Groups**  | Direct, health-aware routing for distributed workloads    |
| **Availability Zones** | Ensure high uptime and fault-tolerance across regions |
| **NAT Gateway**    | Secure outbound access from internal resources            |
| **Security Groups**| Granular control over traffic to/from EC2 and other services |

---

## 🔧 Optional Enhancements

- **Auto Scaling**: Automatically respond to demand by scaling EC2
- **CloudWatch**: Real-time monitoring, logging, and alerting
- **IAM Roles & Policies**: Apply least privilege access principles
- **Bastion Hosts**: Secure jump-box for accessing private resources
- **Terraform/CloudFormation**: Infrastructure-as-Code (IaC) deployments
- **S3 + CloudFront**: Global content delivery and static hosting

---

## 📁 Suggested VPC Architecture

