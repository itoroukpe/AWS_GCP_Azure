# AWS_GCP_Azure
Creating and connecting to virtual machines (VMs) across AWS, GCP, and Azure using MobaXterm involves several steps. Below is a guide to assist you through the process for each platform.

---

**1. Amazon Web Services (AWS)**

**a. Launching an EC2 Instance:**

1. **Sign in to AWS Management Console:**
   - Navigate to [AWS Console](https://aws.amazon.com/console/).

2. **Access EC2 Dashboard:**
   - Click on "Services" and select "EC2" under "Compute."

3. **Launch Instance:**
   - Click on "Launch Instance."

4. **Choose an Amazon Machine Image (AMI):**
   - Select an appropriate AMI (e.g., Amazon Linux 2 AMI).

5. **Select Instance Type:**
   - Choose an instance type (e.g., t2.micro for free tier).

6. **Configure Instance Details:**
   - Set the desired configurations or leave defaults.

7. **Add Storage:**
   - Configure storage as needed.

8. **Configure Security Group:**
   - Add a rule to allow SSH (port 22) access.

9. **Review and Launch:**
   - Review settings and click "Launch."

10. **Key Pair:**
    - Create a new key pair or use an existing one. Download the private key file (.pem) and keep it secure.

**b. Connecting with MobaXterm:**

1. **Convert .pem to .ppk (if needed):**
   - Use PuTTYgen to convert the .pem file to .ppk format.

2. **Open MobaXterm:**
   - Launch MobaXterm on your local machine.

3. **Start a New SSH Session:**
   - Click on "Session" > "SSH."

4. **Configure Session:**
   - **Remote Host:** Enter the public IP address of your EC2 instance.
   - **Specify Username:** Enter `ec2-user` (for Amazon Linux).
   - **Use Private Key:** Check this option and select your .ppk file.

5. **Connect:**
   - Click "OK" to initiate the connection.

---

**2. Google Cloud Platform (GCP)**

**a. Creating a VM Instance:**

1. **Sign in to Google Cloud Console:**
   - Navigate to [GCP Console](https://console.cloud.google.com/).

2. **Access Compute Engine:**
   - Click on the hamburger menu and select "Compute Engine" > "VM instances."

3. **Create Instance:**
   - Click on "Create Instance."

4. **Configure Instance:**
   - **Name:** Provide a name for your VM.
   - **Region and Zone:** Select your preferred region and zone.
   - **Machine Type:** Choose the desired machine type.
   - **Boot Disk:** Select an OS image (e.g., Ubuntu).

5. **Firewall:**
   - Check "Allow HTTP traffic" and "Allow HTTPS traffic" if needed.

6. **Create:**
   - Click "Create" to launch the VM.

**b. Connecting with MobaXterm:**

1. **Generate SSH Keys:**
   - In MobaXterm, go to "Tools" > "MobaKeyGen."
   - Click "Generate" to create a new key pair.
   - Save the private key (.ppk) and copy the public key.

2. **Add SSH Key to GCP:**
   - In the GCP Console, navigate to "Compute Engine" > "Metadata."
   - Select the "SSH Keys" tab and click "Edit."
   - Click "+ Add Item" and paste the public key.
   - Save the changes.

3. **Open MobaXterm:**
   - Launch MobaXterm on your local machine.

4. **Start a New SSH Session:**
   - Click on "Session" > "SSH."

5. **Configure Session:**
   - **Remote Host:** Enter the external IP address of your GCP VM.
   - **Specify Username:** Enter the username associated with your SSH key.
   - **Use Private Key:** Check this option and select your .ppk file.

6. **Connect:**
   - Click "OK" to initiate the connection.

---

**3. Microsoft Azure**

**a. Creating a Virtual Machine:**

1. **Sign in to Azure Portal:**
   - Navigate to [Azure Portal](https://portal.azure.com/).

2. **Access Virtual Machines:**
   - Click on "Virtual Machines" in the left-hand menu.

3. **Create VM:**
   - Click on "Add" > "Virtual Machine."

4. **Configure Basics:**
   - **Subscription:** Select your subscription.
   - **Resource Group:** Create or select an existing resource group.
   - **VM Name:** Provide a name for your VM.
   - **Region:** Choose a region.
   - **Image:** Select an OS image (e.g., Ubuntu Server).
   - **Size:** Choose a VM size. 
