# 🚀 Scalable AWS Two-Tier Infrastructure Project

A real-world cloud project demonstrating how to architect a secure, scalable, and highly available web application environment using AWS services. This setup is ideal for backend applications, student startups, or infrastructure portfolios.

---

## 📁 Project Structure

| File | Description |
|------|-------------|
| [`ProjectDetails.md`](./ProjectDetails.md) | In-depth explanation of VPC, subnets, load balancer, and other AWS components |
| [`Implementation.md`](./Implementation.md) | Step-by-step deployment instructions (via AWS Console or CLI) |
| 🎥 **Video Walkthrough** | Detailed project explanation and setup demo *(link below)* |

---

## 📷 Architecture Overview

> Check the `project-details.md` for a complete architecture diagram and design rationale.

Key components:

- **Custom VPC** with defined CIDR
- **Public & Private Subnets** across multiple Availability Zones
- **Application Load Balancer (ALB)** with Target Groups
- **Internet Gateway** and **NAT Gateway**
- **EC2 Instances** in public subnets
- **RDS/Backend Services** in private subnets
- **Security Groups & NACLs** for access control
- **Route Tables** for traffic management

---

## 🎬 Video Recording

📺 Watch the full setup and explanation here:  
[🔗 YouTube Video](#) *(Insert your video link)*

---

## 🔧 Tools & AWS Services Used

- **Amazon VPC**
- **EC2 Instances**
- **Application Load Balancer (ALB)**
- **NAT Gateway & Internet Gateway**
- **RDS (optional)**
- **Security Groups & Network ACLs**

---

## 👨‍💻 Author

**Arsh Chauhan**  
📬 [GitHub Profile](https://github.com/arshchouhan)

---

## 🎓 Mentor

**Omkar Sharma**  
🌐 [GitHub Profile](https://github.com/omkarsharma2821)

---

## 🪄 How to Use

1. Clone or fork this repository  
2. Read `project-details.md` to understand the architecture  
3. Follow `implementation.md` to deploy the infrastructure  
4. Use the video walkthrough for visual guidance  
5. Replicate or adapt it in your own AWS account/project

---

## 🧠 Summary

This project demonstrates a robust, production-ready cloud architecture using AWS — combining performance, security, and scalability through well-structured VPC design and efficient resource placement.

> 💡 Use this as a template for launching secure, scalable web apps in the cloud 🚀
