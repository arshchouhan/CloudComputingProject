🛠️ Step-by-Step Implementation Guide

This guide helps you deploy a scalable and fault-tolerant web application on AWS using a custom VPC, EC2 instances, and an Application Load Balancer (ALB). Ideal for hands-on learners and beginners in AWS networking.

✅ Step 1: Create a Custom VPC

Go to the VPC Dashboard → click Create VPC

Choose the "VPC only" option

Enter the following:

Name tag: MyVPC

IPv4 CIDR block: 10.0.0.0/16

Leave rest as default

AWS will automatically create:

Two public subnets (in different AZs)

A Route Table

An Internet Gateway (attached to the VPC)

Leave VPC Endpoints unchecked or hidden

<img width="1905" height="771" alt="image" src="https://github.com/user-attachments/assets/a3136862-d01b-4e4f-95db-ceda385e358d" />

<img width="1906" height="821" alt="image" src="https://github.com/user-attachments/assets/9b4f429d-f223-40e5-9ee1-57df272ca1c1" />

<img width="1911" height="814" alt="image" src="https://github.com/user-attachments/assets/5338dc74-eb36-47a7-8334-05ff445c89a8" />

<img width="1898" height="815" alt="image" src="https://github.com/user-attachments/assets/763b3f2f-ab9f-4ee8-9fe0-eb6bb0ba9122" />



🖥️ Step 2: Launch EC2 Instances (Web Servers)

Repeat this process for:

Server-1

Server-2

Server-3

Configuration:

Go to EC2 Dashboard → Launch Instance

Use:

AMI: Amazon Linux 2 or Ubuntu

Instance Type: t2.micro

<img width="1897" height="823" alt="image" src="https://github.com/user-attachments/assets/21ec6082-7dc4-4693-9809-4429d5fd5382" />

<img width="1902" height="772" alt="image" src="https://github.com/user-attachments/assets/b3602ba7-cd90-4289-adac-55d81dd33fee" />

In Network Settings:

VPC: MyVPC

Subnet: One of the public subnets

Enable Auto-assign Public IP

In Advanced → User data, paste:

#!/bin/bash
yum update -y
yum install nginx -y
systemctl start nginx
EC2_IP=$(curl http://169.254.169.254/latest/meta-data/public-ipv4)
echo "Welcome from Server-1 at $EC2_IP" > /usr/share/nginx/html/index.html

<img width="1908" height="774" alt="image" src="https://github.com/user-attachments/assets/b9f6f309-0977-455e-9f74-dc024ac4a85c" />

<img width="1910" height="816" alt="image" src="https://github.com/user-attachments/assets/188da5b7-c73e-4990-b699-03ed200181bc" />

In Security Group, allow:

✅ HTTP (port 80) – for web access

✅ (Optional) SSH (port 22) – for terminal access

<img width="1916" height="870" alt="image" src="https://github.com/user-attachments/assets/4add12d7-3835-45f4-b912-7f2356a868ab" />


🎯 Step 3: Create Target Group

Go to EC2 → Target Groups

Click Create target group

Set:

Name: TG-1

Target type: Instance

Protocol: HTTP, Port: 80

VPC: MyVPC

After creating, click Register targets

Select all 3 EC2 instances and add them

📸 Insert screenshot of Target Group with all registered instances

⚖️ Step 4: Create Application Load Balancer (ALB)

Go to Load Balancers → click Create Load Balancer

Choose Application Load Balancer

Configure:

Name: MyALB

Scheme: Internet-facing

Listeners: HTTP (port 80)

Choose both public subnets created with MyVPC (in different AZs)

Select or create a Security Group that allows:

✅ HTTP (port 80)

In Listener configuration, forward traffic to Target Group: TG-1

📸 Insert screenshot of ALB configuration and listener setup

🌐 Step 5: Test Load Balancer

Go to EC2 → Load Balancers

Click your ALB to open details

Copy the DNS name, something like:

myalb-123456789.ap-south-1.elb.amazonaws.com

Paste it into a browser like:

http://myalb-123456789.ap-south-1.elb.amazonaws.com

Refresh the page multiple times. You should see different outputs like:

Welcome from Server-1 at 13.232.XXX.XX
Welcome from Server-2 at 13.232.XXX.XX
Welcome from Server-3 at 13.232.XXX.XX

📸 Add screenshot of browser showing load-balanced responses

📌 Recap of Resources

Resource

Description

VPC

Custom isolated network (MyVPC)

Subnets

Two public subnets across different AZs

EC2 Instances

3 NGINX web servers with dynamic IP responses

Security Group

Allows HTTP access on port 80

Target Group

Attached EC2 instances for routing

ALB

Internet-facing load balancer

🚀 What's Next?

Here’s how you can expand this setup:

🔄 Add Auto Scaling to dynamically scale EC2 instances

📊 Monitor using CloudWatch for metrics & logs

🔐 Enable HTTPS with SSL from AWS Certificate Manager (ACM)

🛡️ Add Private Subnets for databases or internal services

🌍 Use Route 53 to link a custom domain

🧱 Automate with Terraform or CloudFormation

📚 References

📁 Read project-details.md for architecture concepts

📽️ Watch walkthrough in recording.mp4 (if available)

🙋 Contributors

👨‍💻 Author: Arsh Chauhan🎓 Mentor: Omkar Sharma

