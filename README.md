# AWS_GCP_Azure
Creating and connecting to virtual machines (VMs) across AWS, GCP, and Azure using MobaXterm involves several steps. Below is a guide to assist you through the process for each platform.
________________________________________
1. Amazon Web Services (AWS)
a. Launching an EC2 Instance:
1.	Sign in to AWS Management Console:
o	Navigate to AWS Console.
2.	Access EC2 Dashboard:
o	Click on "Services" and select "EC2" under "Compute."
3.	Launch Instance:
o	Click on "Launch Instance."
4.	Choose an Amazon Machine Image (AMI):
o	Select an appropriate AMI (e.g., Amazon Linux 2 AMI).
5.	Select Instance Type:
o	Choose an instance type (e.g., t2.micro for free tier).
6.	Configure Instance Details:
o	Set the desired configurations or leave defaults.
7.	Add Storage:
o	Configure storage as needed.
8.	Configure Security Group:
o	Add a rule to allow SSH (port 22) access.
9.	Review and Launch:
o	Review settings and click "Launch."
10.	Key Pair:
o	Create a new key pair or use an existing one. Download the private key file (.pem) and keep it secure.
b. Connecting with MobaXterm:
1.	Convert .pem to .ppk (if needed):
o	Use PuTTYgen to convert the .pem file to .ppk format.
2.	Open MobaXterm:
o	Launch MobaXterm on your local machine.
3.	Start a New SSH Session:
o	Click on "Session" > "SSH."
4.	Configure Session:
o	Remote Host: Enter the public IP address of your EC2 instance.
o	Specify Username: Enter ec2-user (for Amazon Linux).
o	Use Private Key: Check this option and select your .ppk file.
5.	Connect:
o	Click "OK" to initiate the connection.
________________________________________
2. Google Cloud Platform (GCP)
a. Creating a VM Instance:
1.	Sign in to Google Cloud Console:
o	Navigate to GCP Console.
2.	Access Compute Engine:
o	Click on the hamburger menu and select "Compute Engine" > "VM instances."
3.	Create Instance:
o	Click on "Create Instance."
4.	Configure Instance:
o	Name: Provide a name for your VM.
o	Region and Zone: Select your preferred region and zone.
o	Machine Type: Choose the desired machine type.
o	Boot Disk: Select an OS image (e.g., Ubuntu).
5.	Firewall:
o	Check "Allow HTTP traffic" and "Allow HTTPS traffic" if needed.
6.	Create:
o	Click "Create" to launch the VM.
b. Connecting with MobaXterm:
1.	Generate SSH Keys:
o	In MobaXterm, go to "Tools" > "MobaKeyGen."
o	Click "Generate" to create a new key pair.
o	Save the private key (.ppk) and copy the public key.
2.	Add SSH Key to GCP:
o	In the GCP Console, navigate to "Compute Engine" > "Metadata."
o	Select the "SSH Keys" tab and click "Edit."
o	Click "+ Add Item" and paste the public key.
o	Save the changes.
3.	Open MobaXterm:
o	Launch MobaXterm on your local machine.
4.	Start a New SSH Session:
o	Click on "Session" > "SSH."
5.	Configure Session:
o	Remote Host: Enter the external IP address of your GCP VM.
o	Specify Username: Enter the username associated with your SSH key.
o	Use Private Key: Check this option and select your .ppk file.
6.	Connect:
o	Click "OK" to initiate the connection.
________________________________________
3. Microsoft Azure
a. Creating a Virtual Machine:
1.	Sign in to Azure Portal:
o	Navigate to Azure Portal.
2.	Access Virtual Machines:
o	Click on "Virtual Machines" in the left-hand menu.
3.	Create VM:
o	Click on "Add" > "Virtual Machine."
4.	Configure Basics:
o	Subscription: Select your subscription.
o	Resource Group: Create or select an existing resource group.
o	VM Name: Provide a name for your VM.
o	Region: Choose a region.
o	Image: Select an OS image (e.g., Ubuntu Server).
o	Size: Choose a VM size.

