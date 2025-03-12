**AWS Hands-On Workshop: Hybrid Cloud Deployment with AWS Site-to-Site VPN and EC2**

## **Objective**
This hands-on workshop will guide participants through deploying a **hybrid cloud architecture** using **AWS Site-to-Site VPN, EC2, and on-premises connectivity**. By the end of this workshop, students will establish **secure communication between an on-premises data center and AWS resources**.

---

## **Scenario: Extending On-Premises Network to AWS**
### **Business Use Case:**
A company wants to extend its **on-premises infrastructure to AWS** to support **cloud bursting, disaster recovery, and seamless hybrid workloads**. This workshop will walk through setting up **AWS Site-to-Site VPN** to securely connect an **on-premises network with AWS resources**.

---

## **Step 1: Setting Up AWS Virtual Private Cloud (VPC)**
### **Create a VPC for Hybrid Networking**
1. Navigate to **AWS Console > VPC > Create VPC**.
2. Name it `Hybrid-VPC` and assign an IPv4 CIDR block `10.10.0.0/16`.
3. Create two **subnets**:
   - `Public-Subnet` (e.g., `10.10.1.0/24`) – for bastion host access.
   - `Private-Subnet` (e.g., `10.10.2.0/24`) – for internal servers.
4. Create an **Internet Gateway** and attach it to the VPC.
5. Update **Route Table** for **public** and **private** subnets.

---

## **Step 2: Configuring AWS Site-to-Site VPN**
### **Create a Virtual Private Gateway (VGW)**
1. Navigate to **AWS Console > VPC > Virtual Private Gateways**.
2. Click **Create Virtual Private Gateway**, name it `Hybrid-VGW`.
3. Attach `Hybrid-VGW` to `Hybrid-VPC`.

### **Create a Customer Gateway (CGW) for On-Premises Connectivity**
1. Navigate to **AWS Console > VPC > Customer Gateways**.
2. Click **Create Customer Gateway**, name it `OnPrem-CGW`.
3. Enter **on-premises public IP address**.
4. Set **BGP ASN** to `65000` (or your assigned ASN).

### **Configure the Site-to-Site VPN**
1. Navigate to **AWS Console > VPC > Site-to-Site VPN Connections**.
2. Click **Create VPN Connection**.
3. Select **Hybrid-VGW** and **OnPrem-CGW**.
4. Choose **Static Routing** or **BGP**.
5. Click **Create VPN** and download the **configuration file**.

---

## **Step 3: Configuring On-Premises Router for VPN**
### **Example Cisco Router Configuration**
1. Open router CLI and enter:
   ```bash
   crypto isakmp policy 10
   encr aes
   hash sha
   authentication pre-share
   group 2
   ```
2. Configure **IKE Phase 1**:
   ```bash
   crypto isakmp key YOUR_PRESHARED_KEY address AWS_VPN_ENDPOINT
   ```
3. Configure **IKE Phase 2**:
   ```bash
   crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
   ```
4. Configure tunnel:
   ```bash
   interface Tunnel1
   ip address 169.254.x.x 255.255.255.252
   tunnel source GigabitEthernet0/0
   tunnel destination AWS_VPN_ENDPOINT
   ```
5. Verify VPN status:
   ```bash
   show crypto isakmp sa
   ```

---

## **Step 4: Deploying EC2 Instances in Hybrid Network**
### **Create EC2 Instances**
1. Navigate to **AWS Console > EC2 > Launch Instance**.
2. Select **Amazon Linux 2 AMI**.
3. Choose **t3.micro**.
4. Attach to **Hybrid-VPC** in **Private-Subnet**.
5. Configure Security Group to allow **VPN traffic** only.
6. Launch and connect to EC2 using:
   ```bash
   ssh -i my-key.pem ec2-user@<private-ip>
   ```

---

## **Step 5: Testing Connectivity from On-Premises to AWS**
1. From **on-premises network**, ping EC2 instance:
   ```bash
   ping 10.10.2.x
   ```
2. From EC2 instance, ping **on-premises server**.
3. Use **traceroute** to confirm VPN path:
   ```bash
   traceroute 192.168.x.x
   ```

---

## **Step 6: Monitoring with AWS CloudWatch & VPN Metrics**
### **Enable CloudWatch Logs for VPN Monitoring**
1. Navigate to **AWS Console > CloudWatch > Logs**.
2. Select **VPN Metrics**.
3. Create **Alarms** for tunnel state changes.

---

## **Final Testing & Validation**
1. Deploy a **web server** on EC2:
   ```bash
   sudo yum install -y httpd
   sudo systemctl start httpd
   ```
2. Access the web server from **on-premises network**.
3. Verify **VPN uptime and network performance**.

---

## **Conclusion**
By completing this hands-on workshop, participants will gain experience in:
✅ **Setting up a hybrid cloud environment using AWS Site-to-Site VPN**  
✅ **Configuring on-premises routers for secure connectivity**  
✅ **Deploying EC2 instances in a private subnet**  
✅ **Ensuring network security between cloud and on-prem infrastructure**  
✅ **Monitoring hybrid cloud resources with AWS CloudWatch**  

This workshop provides **real-world hybrid cloud deployment skills**, preparing students for **enterprise networking on AWS**.

