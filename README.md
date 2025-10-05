## Dynamic-Web-Scaling-with-AWS-Auto-Scaling-Application-Load-Balancer
 ![AWS Logo](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws&logoColor=white)
![EC2](https://img.shields.io/badge/EC2-Compute-blue?logo=amazonec2)
![ALB](https://img.shields.io/badge/ALB-Load%20Balancer-green?logo=amazonaws)
![ASG](https://img.shields.io/badge/ASG-Auto%20Scaling-purple?logo=amazonaws)

> **_Empower your cloud skills!_**  
> Build a self-healing, auto-scaling, and load-balanced web architecture on AWS, step by step.  
> _Perfect for your resume, LinkedIn, or interview portfolio!_

---

## 📚 Table of Contents

1. [Prerequisites](#-prerequisites)
2. [Step 1: Launch EC2 Instance](#-step-1-launch-ec2-instance)
3. [Step 2: Create a Launch Template](#-step-2-create-a-launch-template)
4. [Step 3: Set Up Application Load Balancer (ALB)](#-step-3-set-up-application-load-balancer-alb)
5. [Step 4: Create an Auto Scaling Group (ASG)](#-step-4-create-an-auto-scaling-group-asg)
6. [Step 5: Test Auto Scaling](#-step-5-test-auto-scaling)
7. [Step 6: Cleanup Resources](#-step-6-cleanup-resources)
8. [Key Learnings](#key-learnings)
9. [Impressive LinkedIn Caption](#-impressive-linkedin-caption)

---

## 🧰 Prerequisites

- **AWS Account** with administrator access.
- Basic understanding of **EC2 Instances**.
- *(Optional)* A simple web application (e.g., `index.html`) to deploy.

---

## 🏗️ Architecture Overview

![Auto Scaling + Load Balancer Architecture](./images/image1.png)  
*Figure 1: High-level AWS Auto Scaling + Load Balancer Architecture*

---

## 🟢 Step 1: Launch EC2 Instance

1. **Log in** to AWS Console → EC2 → Launch Instance.
2. **Choose AMI**: Amazon Linux 2 (Free tier eligible).
3. **Instance Type**: `t2.micro` (Free tier).
4. **Configure Network**: Default VPC & subnet.
5. **Add Storage**: Default (8 GB).
6. **Add Tags** (Optional): `Name = DemoServer`.
7. **Security Group**: Allow HTTP (port 80) and SSH (port 22).
8. **Launch** with a key pair you control.
9. **Connect** via SSH, then run:
    ```bash
    sudo yum update -y
    sudo yum install -y httpd
    sudo systemctl start httpd
    sudo systemctl enable httpd
    echo "<h1>Welcome to Demo EC2 Instance</h1>" | sudo tee /var/www/html/index.html
    ```
10. **Test**: Visit the EC2 public IP in a browser. You should see your welcome page.

![Sample Web Page](./images/image2.png)
*Figure 2: Testing the EC2 instance — sample HTML page served*

---

## 🟡 Step 2: Create a Launch Template

1. **EC2** → Launch Templates → Create Launch Template.
2. **Name**: `AutoScalingTemplate`.
3. **AMI**: Select the Amazon Linux 2 AMI.
4. **Instance Type**: `t2.micro`.
5. **Key Pair**: Select your key.
6. **Security Group**: HTTP & SSH allowed.
7. **User Data** (optional, for automatic setup):
    ```bash
    #!/bin/bash
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Welcome to Auto Scaled Web Server</h1>" > /var/www/html/index.html
    ```
8. **Save** the Launch Template.

---

## 🟠 Step 3: Set Up Application Load Balancer (ALB)

1. **EC2** → Load Balancers → Create Load Balancer → Application Load Balancer.
2. **Name**: `DemoALB`.
3. **Scheme**: Internet-facing.
4. **IP Type**: IPv4.
5. **Listeners**: HTTP:80.
6. **VPC & Subnets**: Select VPC and at least 2 subnets (across different AZs).
7. **Security Group**: Allow HTTP (80).
8. **Target Group**:
    - Target type: Instances
    - Protocol: HTTP, Port 80
    - Health Check: HTTP, `/index.html`
    - Register your EC2 instance for test.
9. **Create** the ALB.
10. **Test**: Access the ALB DNS in a browser — you should see your web page.

---

## 🔵 Step 4: Create an Auto Scaling Group (ASG)

1. **EC2** → Auto Scaling → Create Auto Scaling Group.
2. **Name**: `DemoASG`.
3. **Launch Template**: Use the one from Step 2.
4. **VPC & Subnets**: Same as ALB (at least 2 subnets).
5. **Attach to Load Balancer**: Select `DemoALB` and its target group.
6. **Group Size**:
    - Minimum: 1
    - Desired: 1
    - Maximum: 3 (for cost control)
7. **Scaling Policies**:
    - **Target tracking**: Maintain average CPU utilization at 50%
    - *(or)* **Step scaling**:
        - Add instance if CPU > 70% for 5 min
        - Remove instance if CPU < 30% for 5 min
8. **Review & Create** the ASG.

![EC2 Instances Console](./images/image3.png)
*Figure 3: EC2 Console showing multiple instances managed by the Auto Scaling Group*

---

## 🟣 Step 5: Test Auto Scaling

1. **Install** a load-testing tool (e.g., Apache JMeter, stress-ng).
2. **Send traffic** to the ALB’s DNS endpoint.
3. **Observe**: ASG should launch more EC2 instances as traffic spikes.
4. **Reduce traffic**: ASG should terminate instances when load drops.
5. **Check** CloudWatch metrics for CPU and scaling events.

![stress-ng Load Testing](./images/image4.png)
*Figure 4: Using stress-ng to simulate load on the EC2 instances*

---

## 🔴 Step 6: Cleanup Resources

- **Delete**: Auto Scaling Group, ALB, Launch Template, and EC2 Instances.
- **Remove** security groups if no longer needed.
- **Avoid unwanted AWS charges!**

---

## 🎯 Key Learnings

- **Hands-on AWS**: Experience with EC2, ALB, ASG, and CloudWatch.
- **Scalable Architecture**: Build fault-tolerant, highly available systems.
- **Cost Management**: Automatic scaling optimizes resource usage.
- **Monitoring**: Use CloudWatch for real-time insights.
- **DevOps Skills**: Understand load balancing, health checks, and auto-scaling policies.

---



**Connect with me to discuss cloud, DevOps, and scalable architectures!**  
