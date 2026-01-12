# ⚙️ Implement Auto Scaling in AWS

This repository explains how to **implement Auto Scaling in AWS** to automatically adjust EC2 instances based on workload, ensuring high availability, scalability, and cost efficiency.

---

## 📘 What is Auto Scaling?

AWS Auto Scaling automatically:
- Adds EC2 instances when demand increases 📈
- Removes EC2 instances when demand decreases 📉
- Maintains desired capacity
- Replaces unhealthy instances automatically

---

## 🎯 Benefits of Auto Scaling

- ✅ High availability
- 💰 Cost optimization
- 🔄 Automatic scaling
- 🛡️ Fault tolerance
- ⚡ Better performance during traffic spikes

---

## 🛠️ Services Used

- Amazon EC2
- Launch Template
- Auto Scaling Group (ASG)
- Application Load Balancer (ALB)
- CloudWatch

---

## 📌 Prerequisites

- AWS Account
- IAM user with EC2 & Auto Scaling permissions
- VPC with public subnets
- Key pair created

---

## 🧱 Step 1: Create a Launch Template

1. Go to **EC2 → Launch Templates**
2. Click **Create launch template**
3. Configure:
   - AMI: Amazon Linux 2
   - Instance Type: `t2.micro`
   - Key pair
   - Security Group (HTTP & SSH allowed)
4. Add User Data:
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Auto Scaling Instance" > /var/www/html/index.html
yes > /dev/null &

aws-autoscaling/
├── README.md
└── screenshots/
