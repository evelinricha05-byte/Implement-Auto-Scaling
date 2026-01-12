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

## 🧱 Step 2: Create an Application Load Balancer (ALB)

1. Go to **EC2 → Load Balancers**
2. Click **Create Load Balancer**
3. Select **Application Load Balancer**
4. Configure:
   - Scheme: Internet-facing
   - IP address type: IPv4
   - VPC: Select your VPC
   - Availability Zones: Select at least 2 public subnets
5. Create a **Target Group**:
   - Target type: Instance
   - Protocol: HTTP
   - Port: 80
   - Health check path: `/`
6. Attach the Target Group to the Load Balancer
7. Create the Load Balancer ✅

---

## 🧱 Step 3: Create Auto Scaling Group (ASG)

1. Go to **EC2 → Auto Scaling Groups**
2. Click **Create Auto Scaling Group**
3. Enter ASG name
4. Select the **Launch Template**
5. Choose:
   - VPC
   - Multiple subnets (recommended)
6. Attach the **Application Load Balancer**
7. Select the **Target Group**
8. Configure capacity:
   - Minimum: 1
   - Desired: 1
   - Maximum: 3
9. Create Auto Scaling Group ✅

---

## 📊 Step 4: Configure Scaling Policy

### Target Tracking Scaling Policy
- Metric: Average CPU Utilization
- Target value: 50%

Auto Scaling automatically:
- Scales out when CPU > 50%
- Scales in when CPU < 50%

---

## 📈 Step 5: Monitoring with CloudWatch

- Go to **CloudWatch → Metrics**
- Monitor:
  - CPU Utilization
  - InService instances
  - Healthy & Unhealthy instances
- Auto Scaling responds to alarms automatically

---

## 🧪 Step 6: Test Auto Scaling

Run the following command inside EC2 to increase CPU load:
```bash
yes > /dev/null &

