**Rondus DevOps Masterclass – AWS Hands-On Guide**

## **Objective**
This hands-on guide is designed to help **Rondus DevOps Masterclass students** gain practical experience provisioning and managing **AWS resources**, including **EC2, AWS Storage Services, AWS ELB, Auto Scaling, Amazon CloudWatch, and AWS VPC**. By following this guide, students will build a **highly available and scalable web application infrastructure** on AWS.

---

## **Scenario: Deploying a Scalable Web Application on AWS**
### **Business Use Case:**
A company needs to host a **highly available web application** that can handle varying traffic loads. The solution must include **load balancing, auto-scaling, monitoring, and secure networking**. You will set up the required AWS infrastructure components step by step.

---

## **Step 1: Setting Up a Virtual Private Cloud (VPC)**
### **Create a Custom VPC**
1. Sign in to AWS Console and navigate to **VPC**.
2. Click **Create VPC** → Name it `DevOps-VPC`.
3. Set **IPv4 CIDR block** to `10.0.0.0/16`.
4. Click **Create VPC**.

### **Create Subnets**
1. Navigate to **Subnets** → Click **Create Subnet**.
2. Choose `DevOps-VPC`.
3. Create **two subnets**:
   - `Public-Subnet` (e.g., `10.0.1.0/24`) – Select **a public availability zone**.
   - `Private-Subnet` (e.g., `10.0.2.0/24`) – Select **a different availability zone**.

### **Create an Internet Gateway**
1. Navigate to **Internet Gateways** → Click **Create Internet Gateway** → Name it `DevOps-IGW`.
2. Attach `DevOps-IGW` to `DevOps-VPC`.

### **Update Route Table**
1. Navigate to **Route Tables**.
2. Select the main route table → Click **Edit Routes**.
3. Add a route:
   - Destination: `0.0.0.0/0`
   - Target: `DevOps-IGW`
4. Associate **Public-Subnet** with this route table.

---

## **Step 2: Launching EC2 Instances**
### **Create an EC2 Instance**
1. Navigate to **EC2** → Click **Launch Instance**.
2. Select **Amazon Linux 2 AMI**.
3. Choose **t2.micro (free tier eligible)**.
4. Attach to `DevOps-VPC` → Place it in **Public-Subnet**.
5. Enable **Auto-assign Public IP**.
6. Create a new **Security Group**:
   - Allow `HTTP (80)`, `HTTPS (443)`, and `SSH (22)` from `0.0.0.0/0`.
7. Launch the instance and download the **key pair**.

### **Connect to EC2 & Install Web Server**
1. SSH into the instance:
   ```bash
   ssh -i my-key.pem ec2-user@<public-ip>
   ```
2. Install Apache:
   ```bash
   sudo yum update -y
   sudo yum install -y httpd
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```
3. Create a sample webpage:
   ```bash
   echo "<h1>Rondus DevOps Web Server</h1>" | sudo tee /var/www/html/index.html
   ```

---

## **Step 3: Configuring Elastic Load Balancer (ELB)**
1. Navigate to **EC2 > Load Balancers**.
2. Click **Create Load Balancer** → Choose **Application Load Balancer**.
3. Name it `DevOps-ALB` and assign it to `DevOps-VPC`.
4. Select **at least two availability zones**.
5. Create a new **target group** → Name it `DevOps-TG` → Protocol `HTTP`.
6. Register the EC2 instance with the target group.
7. Click **Create Load Balancer**.

---

## **Step 4: Implementing Auto Scaling**
1. Navigate to **EC2 > Auto Scaling Groups** → Click **Create Auto Scaling Group**.
2. Name it `DevOps-ASG` and attach it to `DevOps-TG`.
3. Set **minimum = 1**, **desired = 2**, **maximum = 4**.
4. Choose **Launch Template** or existing EC2 instance as template.
5. Configure health check settings → Use **ELB Health Check**.
6. Click **Create Auto Scaling Group**.

---

## **Step 5: Configuring AWS Storage Services (EBS & S3)**
### **Amazon EBS for Persistent Storage**
1. Navigate to **EC2 > Elastic Block Store**.
2. Click **Create Volume** → Choose `gp3`, size = `10 GiB`.
3. Attach to an existing EC2 instance.

### **Amazon S3 for Static Website Hosting**
1. Navigate to **S3 > Create Bucket** → Name it `devops-static-site`.
2. Enable **public access** → Upload static website files.
3. Under **Properties**, enable **Static Website Hosting**.

---

## **Step 6: Monitoring with Amazon CloudWatch**
1. Navigate to **CloudWatch > Alarms** → Click **Create Alarm**.
2. Choose `EC2 CPU Utilization` metric.
3. Set threshold: `CPU Utilization > 70%`.
4. Create an SNS topic to send alerts.
5. Enable detailed monitoring for EC2.

---

## **Final Validation & Testing**
1. Copy the **Load Balancer DNS Name** from the **ELB console**.
2. Open in a browser → You should see `Rondus DevOps Web Server`.
3. **Simulate high traffic** using:
   ```bash
   sudo yum install -y stress
   stress --cpu 2 --timeout 60
   ```
4. Verify auto-scaling by monitoring instances in **EC2 Dashboard**.

---


