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
```
#!/bin/bash
apt-get update
apt-get install nginx -y

cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Welcome to $(hostname)</title>
  <style>
    body {
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      color: white;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    h1 {
      font-size: 3em;
      margin: 0.2em 0;
    }
    p {
      font-size: 1.2em;
      color: #cce3ff;
    }
    .hostname {
      background: #ffffff33;
      padding: 0.5em 1em;
      border-radius: 10px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1> Welcome to Your Server!</h1>
  <p>This server is proudly hosted as:</p>
  <div class="hostname">$(hostname)</div>
</body>
</html>
EOF
```

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

<img width="1919" height="850" alt="image" src="https://github.com/user-attachments/assets/f9c8c44c-fdc8-4ad0-8da2-d143075a8605" />

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

<img width="1906" height="815" alt="image" src="https://github.com/user-attachments/assets/613a683a-92a6-4fff-897f-7273dcb05bc1" />

<img width="1918" height="822" alt="image" src="https://github.com/user-attachments/assets/306eefd3-d435-40bd-ab18-e6f117437294" />

<img width="1876" height="491" alt="image" src="https://github.com/user-attachments/assets/687f4b5b-87c0-455e-a9e8-841c29c086d6" />

<img width="1918" height="810" alt="image" src="https://github.com/user-attachments/assets/b34f8fca-45a0-491c-8b00-2757def2bbea" />



🌐 Step 5: Test Load Balancer

Go to EC2 → Load Balancers

Click your ALB to open details

Copy the DNS name, something like:

myalb-123456789.ap-south-1.elb.amazonaws.com

Paste it into a browser like:

Refresh the page multiple times. You should see different outputs like:

Welcome from Server-1 at 13.232.XXX.XX
Welcome from Server-2 at 13.232.XXX.XX
Welcome from Server-3 at 13.232.XXX.XX


<img width="1766" height="899" alt="image" src="https://github.com/user-attachments/assets/0ca5bde7-5a73-4b1b-aee1-47c1d58d5d27" />

<img width="1678" height="853" alt="image" src="https://github.com/user-attachments/assets/3b2d46ba-3170-444e-b507-b20b38d78fd1" />

<img width="1783" height="812" alt="image" src="https://github.com/user-attachments/assets/3b7189c1-f521-4bc6-918f-37c5f25ef100" />


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

