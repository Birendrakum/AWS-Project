Deploy a website that can upload, download and delete files from s3.

🧱 Architecture Overview
🔹 Networking
Custom VPC (192.168.10.0/24) with DNS support.

Subnets:
Public: pub-a, pub-b for internet-facing resources that is Load Balancer and NAT Gateway.
Private: pvt-a, pvt-b for backend EC2 instances.

Routing:
Public route table connected to an Internet Gateway to grant public internet access.
Private route table uses a NAT Gateway for secure outbound access.

🔐 Security
Security Groups:
SgLB: Allows inbound HTTP traffic from the internet.
SgWebapp: Restricts access to HTTP traffic from the Load Balancer only.

IAM Role & Instance Profile:
Grants EC2 access to Secrets Manager for secure credential retrieval and S3 access for storing and deleting files.

⚙️ Compute & Scaling
🖥️ Launch Template
Amazon Linux 2 AMI (ami-0953476d60561c955)
Installs : Git, Python3, pip, Django, boto3, nginx 
Clones Django project from GitHub
Retrive and inject public ip of ec2 into danjgo.conf and moved it inside nginx config folder.
Retrieves secrets from AWS Secrets Manager
Injects credentials into settings.py
Starts Django server on 127.0.0.1:8000

📈 Auto Scaling Group
Launches EC2 instances in public subnets.
Integrated with Target Group for load balancing.
Min size: 1, Max size: 4
Metrics collected every minute.

⚖️ Load Balancing
Application Load Balancer (ALB):
Internet-facing, deployed across public subnets.
Listener on port 80 forwards traffic to EC2 instances.

Target Group:
HTTP protocol on port 80
Targets EC2 instances launched by ASG

📊 Monitoring & Scaling Policies
CloudWatch Alarms:
HighCpu-alarm: Triggers scale-out when CPU > 70%
LowCPU_Alarm: Triggers scale-in when CPU < 50%

Scaling Policies:
Scale out: +2 instances
Scale in: -1 instance
Cooldown: 120 seconds
