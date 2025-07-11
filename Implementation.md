# 🏗️ AWS Infrastructure Overview

This document provides an in-depth explanation of the AWS infrastructure design used in this project. It covers all essential components for building a secure, highly available, and scalable cloud architecture.

---

## 🌐 Virtual Private Cloud (VPC)

A **Virtual Private Cloud (VPC)** is a logically isolated network in AWS where all cloud resources are deployed.

### 🔑 Key Features:
- Defined CIDR block, e.g., `10.0.0.0/16`
- Customizable IP ranges, route tables, and security groups
- Foundation for public and private subnets

---

## 🧭 Public & Private Subnets

Subnets divide a VPC into smaller, manageable sections.

### 🔓 Public Subnets
- Connected to the internet via an **Internet Gateway (IGW)**
- Host public-facing resources like **ALBs** and **Bastion Hosts**

### 🔐 Private Subnets
- No direct internet access
- Used for EC2 instances, databases, internal services
- Outbound internet access via a **NAT Gateway**

---

## 📍 Availability Zones (AZs)

Availability Zones are isolated locations within an AWS region.

- At least **two AZs** are used for high availability
- Resources are **distributed** across AZs
- Provides **fault tolerance** in case one AZ goes down

---

## 🎯 Target Groups

Target groups are used by load balancers to route traffic.

### ✅ Features:
- Group EC2 instances, IPs, or Lambda functions
- Support health checks to ensure high availability
- Multiple target groups can serve different microservices

---

## ⚖️ Load Balancers

### 🌀 Application Load Balancer (ALB)
- Layer 7 (HTTP/HTTPS)
- Path-based or host-based routing
- Ideal for web apps & APIs

### 🧷 Network Load Balancer (NLB)
- Layer 4 (TCP/UDP)
- Static IPs, ultra-low latency
- Great for real-time or high-throughput applications

---

## 🚪 Internet Gateway vs. NAT Gateway

| Component         | Use Case                                        | Placed In         |
|------------------|--------------------------------------------------|-------------------|
| **Internet Gateway** | Enables public internet access               | Public Subnet     |
| **NAT Gateway**       | Enables outbound internet for private subnets | Public Subnet (for routing private subnet traffic) |

---

## 🔐 Security: Security Groups vs NACLs

| Feature           | Security Groups                           | Network ACLs                           |
|------------------|--------------------------------------------|----------------------------------------|
| Level            | Instance-level                             | Subnet-level                           |
| Type             | Stateful (automatic response traffic)      | Stateless (explicit inbound/outbound)  |
| Use              | Fine-grained access                        | Broad subnet control                   |

---

## ☁️ EC2 Instances

- Deployed in **private subnets**
- Managed using **Auto Scaling**, **IAM Roles**, and **Security Groups**
- Accessed via **ALB** or **Bastion Host** in public subnet

---

## 📡 Route Tables

Route tables define where traffic is directed within the VPC.

- **Public subnets** route through the **Internet Gateway**
- **Private subnets** route through the **NAT Gateway**
- Each subnet is associated with a custom or main route table

---

## 🛡️ Why This Architecture?

| Component         | Purpose                                           |
|------------------|---------------------------------------------------|
| VPC              | Isolated environment for all resources            |
| Public Subnets   | Host internet-facing services like ALB            |
| Private Subnets  | Isolate backend resources like EC2 and databases  |
| Load Balancer    | Distributes traffic efficiently and intelligently |
| Target Groups    | Health-aware and flexible routing                 |
| Availability Zones | Ensures uptime and fault-tolerance              |
| NAT Gateway      | Secure outbound access from private subnets       |
| Security Groups  | Fine-grained instance-level access control        |

---

## 🔧 Optional Enhancements

- **Auto Scaling**: Dynamic resource scaling based on traffic
- **CloudWatch**: Monitoring, logs, and alarms
- **IAM Roles**: Secure access to AWS services
- **Bastion Hosts**: Secure jump-box access to private EC2 instances
- **Terraform / CloudFormation**: Infrastructure-as-Code provisioning

---

## 📁 Suggested VPC Architecture



---

## ✅ Summary

This AWS infrastructure setup provides a well-architected framework that prioritizes:

- **Security**: No direct exposure of backend services
- **High Availability**: Resource distribution across multiple AZs
- **Scalability**: Load balancing and auto scaling
- **Resilience**: Health checks and failover support

---

*Built with best practices for modern cloud-native infrastructure.*


