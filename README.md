# 🌐 Deploy a Website with S3 File Operations

A fully automated AWS architecture to **upload**, **download**, and **delete** files from Amazon S3 using a Django web application.

---

## 🧱 Architecture Overview

---

### 🔹 Networking
- **Custom VPC:** `192.168.10.0/24` with DNS support  
- **Subnets:**
  - 🌍 *Public:* `pub-a`, `pub-b` → Internet-facing resources (**Load Balancer**, **NAT Gateway**)  
  - 🔒 *Private:* `pvt-a`, `pvt-b` → Backend **EC2 instances**  
- **Routing:**
  - 🌐 *Public Route Table:* Connected to an **Internet Gateway** for public access  
  - 🔐 *Private Route Table:* Uses **NAT Gateway** for secure outbound traffic  

---

### 🔐 Security
- **Security Groups:**
  - 🛡️ **SgLB:** Allows inbound **HTTP traffic** from the internet  
  - 🔒 **SgWebapp:** Restricts access to **HTTP traffic** only from the Load Balancer  
- **IAM Role & Instance Profile:**
  - ✅ Grants **EC2 access** to **Secrets Manager** for secure credential retrieval  
  - ✅ Provides **S3 permissions** for file storage and deletion  

---

### ⚙️ Compute & Scaling

#### 🖥️ Launch Template
- **AMI:** Amazon Linux 2 (`ami-0953476d60561c955`)  
- **Installs:** Git, Python3, pip, Django, boto3, nginx  
- **Actions:**
  - 📂 Clones Django project from GitHub  
  - 🔑 Retrieves secrets from **AWS Secrets Manager**  
  - 📝 Injects credentials into `settings.py` (automated via **sed**)  
  - 🚀 Starts Django server on `0.0.0.0:8000`  

#### 📈 Auto Scaling Group
- Launches **EC2 instances** in public subnets  
- Integrated with **Target Group** for load balancing  
- **Min size:** 1 | **Max size:** 4  
- 📊 Metrics collected every minute  

---

### ⚖️ Load Balancing
- **Application Load Balancer (ALB):**
  - 🌐 Internet-facing, deployed across public subnets  
  - Listener on **port 80** forwards traffic to EC2 instances  
- **Target Group:**
  - Protocol: **HTTP (port 80)**  
  - Targets: EC2 instances launched by **ASG**  

---

### 📊 Monitoring & Scaling Policies
- **CloudWatch Alarms:**
  - 🔺 *HighCpu-alarm:* Scale-out when CPU > 70%  
  - 🔻 *LowCPU_Alarm:* Scale-in when CPU < 50%  
- **Scaling Policies:**
  - ➕ *Scale Out:* +2 instances  
  - ➖ *Scale In:* -1 instance  
  - ⏱️ *Cooldown:* 120 seconds  

---

## 🧩 Key Highlights
- ✅ Secure architecture using **IAM** and **Secrets Manager**  
- ✅ Automated dynamic scaling with **CloudWatch** and **Auto Scaling Group**  
- ✅ Load-balanced traffic via **ALB**  
- ✅ Fully integrated **Django web app** with **S3 file operations**  
- ✅ Automated secret injection using **sed function**  

---

## 🧠 Tech Stack
**AWS | EC2 | S3 | VPC | ALB | ASG | CloudWatch | Secrets Manager | Django | Python | Nginx**

---

⭐ *“Scalable, secure, and automated — the perfect blend of cloud engineering and DevOps.”*
