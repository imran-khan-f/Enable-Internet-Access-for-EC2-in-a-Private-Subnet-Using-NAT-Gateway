# Enable-Internet-Access-for-EC2-in-a-Private-Subnet-Using-NAT-Gateway
Here's a hands-on scenario to help you set up and validate a NAT Gateway in AWS. This is ideal for enabling internet access from private subnets—without exposing them directly.
Enable Internet Access for EC2 in a Private Subnet Using NAT Gateway
🎯 Goal:
Create a VPC with public and private subnets.

Launch EC2 in the private subnet.

Use a NAT Gateway in the public subnet to allow outbound internet access.

🛠️ Step-by-Step Setup
🔹 Step 1: Create a VPC
CIDR block: 10.0.0.0/16

🔹 Step 2: Create Subnets
Public Subnet: 10.0.1.0/24

Private Subnet: 10.0.2.0/24

Choose different AZs for HA if needed.

🔹 Step 3: Create and Attach Internet Gateway (IGW)
Go to VPC → Internet Gateways

Create IGW and attach it to your VPC.

🔹 Step 4: Create Route Table for Public Subnet
Add route: 0.0.0.0/0 → IGW

Associate this route table with the public subnet.

🔹 Step 5: Allocate Elastic IP
Go to EC2 → Elastic IPs

Allocate a new EIP (used by NAT Gateway)

🔹 Step 6: Create NAT Gateway
Go to VPC → NAT Gateways

Select:

Subnet: Public Subnet

Elastic IP: the one you just allocated

Click Create NAT Gateway

Wait until status changes to Available

🔹 Step 7: Create Route Table for Private Subnet
Go to VPC → Route Tables

Create a new route table.

Add route: 0.0.0.0/0 → NAT Gateway ID

Associate this route table with the private subnet

🔹 Step 8: Launch EC2 Instances
EC2-A: In public subnet (for SSH access)

EC2-B: In private subnet (no public IP)

Use same key pair for both. Ensure EC2-B has a security group allowing outbound traffic.

🔹 Step 9: SSH into EC2-A and Connect to EC2-B
bash
ssh -i your-key.pem ec2-user@<EC2-A-public-IP>
ssh ec2-user@<EC2-B-private-IP>
🔹 Step 10: Test Internet Access from EC2-B
From EC2-B, run:

bash
curl https://api.ipify.org
You should see the Elastic IP of the NAT Gateway—confirming outbound access.

✅ Success Criteria
EC2-B (private subnet) can access the internet via NAT Gateway.

No inbound traffic reaches EC2-B directly.
