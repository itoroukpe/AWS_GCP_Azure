**AWS Hands-On Workshop: Deploying a Secure and Scalable Web Application**

## **Objective**
This hands-on workshop will guide participants through setting up a **secure, scalable, and highly available web application** on AWS. The focus will be on **network security, database management, CI/CD deployment, and monitoring** using AWS services. By the end of this workshop, participants will deploy a **full-stack web application** utilizing **EC2, RDS, IAM, CodePipeline, and CloudWatch.**

---

## **Scenario: Hosting a Secure E-Commerce Web Application**
### **Business Use Case:**
A startup wants to deploy an **e-commerce platform** on AWS, ensuring **security, scalability, and automation** for deployments. This workshop will cover **network setup, application deployment, database integration, and monitoring** to support a real-world application.

---

## **Step 1: Setting Up Secure Networking with AWS VPC & Security Groups**
### **Create a Virtual Private Cloud (VPC)**
1. Navigate to **AWS Console > VPC > Create VPC**.
2. Name it `ECommerce-VPC` and assign an IPv4 CIDR block `10.0.0.0/16`.
3. Create two **subnets** in different availability zones:
   - **Public-Subnet (10.0.1.0/24)** for the web application.
   - **Private-Subnet (10.0.2.0/24)** for the database.
4. Create an **Internet Gateway** and attach it to the VPC.
5. Update the **Route Table** for Public-Subnet to allow internet access.
6. Configure **Security Groups**:
   - Allow **HTTP (80), HTTPS (443), and SSH (22)** for public instances.
   - Restrict database access to only the **private subnet**.

---

## **Step 2: Launching an EC2 Instance for the Web Application**
### **Deploy EC2 with Secure Configuration**
1. Navigate to **AWS Console > EC2 > Launch Instance**.
2. Select **Amazon Linux 2** as the AMI.
3. Choose **t2.micro (free-tier eligible)**.
4. Attach to `ECommerce-VPC` in **Public-Subnet**.
5. Create a **new Security Group**, allowing only HTTP and SSH access.
6. Launch the instance and **connect via SSH**:
   ```bash
   ssh -i my-key.pem ec2-user@<public-ip>
   ```

### **Install and Configure Web Server**
1. Update and install required packages:
   ```bash
   sudo yum update -y
   sudo yum install -y httpd php mysql php-mysql
   ```
2. Start Apache and configure auto-restart:
   ```bash
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```
3. Create a **test HTML page**:
   ```bash
   echo "<h1>Welcome to the E-Commerce Store</h1>" | sudo tee /var/www/html/index.html
   ```

---

## **Step 3: Configuring Amazon RDS for Database Storage**
### **Deploy a Secure MySQL Database**
1. Navigate to **AWS Console > RDS > Create Database**.
2. Select **MySQL** and choose **db.t3.micro (Free-tier eligible)**.
3. Place the database in **Private-Subnet** to restrict direct access.
4. Enable **Multi-AZ Deployment** for high availability.
5. Set up a **new security group** allowing access only from the EC2 web server.
6. Store database credentials securely using **AWS Secrets Manager**.

---

## **Step 4: Implementing CI/CD with AWS CodePipeline & CodeDeploy**
### **Set Up Continuous Deployment**
1. Navigate to **AWS CodePipeline > Create Pipeline**.
2. Connect **GitHub repository** as the source.
3. Select **AWS CodeBuild** to build the application.
4. Deploy changes to the **EC2 instance** using **AWS CodeDeploy**.
5. Verify the pipeline is successfully automating code deployment.

---

## **Step 5: Monitoring & Logging with Amazon CloudWatch**
### **Enable CloudWatch for Performance Monitoring**
1. Navigate to **AWS CloudWatch > Alarms > Create Alarm**.
2. Select **EC2 CPU Utilization > Set threshold > 75%**.
3. Configure **notifications via Amazon SNS** for alerts.
4. Enable **CloudWatch Logs** to capture web server activity.

---

## **Step 6: Scaling with Auto Scaling & Load Balancing**
### **Configure Auto Scaling**
1. Navigate to **AWS Auto Scaling > Create Auto Scaling Group**.
2. Attach to **EC2 instance launch template**.
3. Define **min = 1, max = 4** instances.

### **Set Up Application Load Balancer (ALB)**
1. Navigate to **AWS ELB > Create Load Balancer**.
2. Choose **Application Load Balancer** and attach it to `ECommerce-VPC`.
3. Create a **target group** and add the **EC2 instance**.
4. Enable **session stickiness** for improved user experience.

---

## **Final Testing & Validation**
1. Copy **Load Balancer DNS** from the ALB console.
2. Open it in a browser to confirm the web application is running.
3. Simulate **traffic load** to test Auto Scaling:
   ```bash
   sudo yum install -y stress
   stress --cpu 4 --timeout 120
   ```
4. Monitor **new EC2 instances** being created in response to demand.

---

## **Conclusion**
By completing this workshop, participants will gain hands-on experience in:
✅ **Provisioning AWS infrastructure for a secure, scalable web application**  
✅ **Deploying a database-backed web application using RDS**  
✅ **Implementing CI/CD with AWS CodePipeline**  
✅ **Monitoring & optimizing application performance with CloudWatch**  
✅ **Scaling applications with Auto Scaling & Load Balancer**  

This hands-on workshop prepares students for **real-world DevOps challenges** and reinforces AWS best practices for **high availability, security, and automation**.

