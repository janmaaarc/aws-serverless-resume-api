# AWS Serverless Resume API (PDF)

## Objectives
- Architect a serverless backend using AWS Lambda and API Gateway.
- Securely store a resume PDF in a private Amazon S3 bucket.
- Solve the technical challenge of serving binary files (PDFs) via a JSON-based API.
- Implement Python logic to fetch and Base64 encode files for HTTP transmission.
- Verify the public API endpoint via web browser.

---

## Steps

### 1. S3 Storage Setup
- Created a unique S3 bucket (e.g., `janmarc-resume-storage`).
- Uploaded `resume.pdf` to the bucket.
- **Security Configuration:** Blocked all public access to the bucket to ensure the file is only accessible via the API, not the public internet.

### 2. Lambda Function Configuration
- Created a Python 3.x Lambda function.
- Assigned an IAM Role with specific permissions (`s3:GetObject`) to read only from the resume bucket.
- Wrote a Python script using `boto3` to:
  #### 1. Fetch the PDF object from S3.
  #### 2. Convert the binary PDF data into a Base64 string (essential for API Gateway transmission).

### 3. Connect to EC2 via SSH (MacBook)
```bash
# Navigate to key folder
cd ~/Documents/documents/aws-keys

# Secure the key
chmod 400 project1-key.pem

# Connect to EC2 instance
ssh -i project1-key.pem ec2-user@<EC2-PUBLIC-IP>
```

### 4. Verify Instance
```bash
# Check OS info
uname -a

# Check disk usage
df -h

# Check uptime
uptime

# Update packages
sudo yum update -y

# Install Git
sudo yum install git -y
```

---

## Commands Used
```
cd ~/Documents/documents/aws-keys
chmod 400 project1-key.pem
ssh -i project1-key.pem ec2-user@3.27.13.213
uname -a
df -h
uptime
sudo yum update -y
sudo yum install git -y
```

---

## Notes / Lessons Learned
- Launching an EC2 instance on AWS Free Tier is straightforward and beginner-friendly.
- SSH connection requires the `.pem` key to have proper permissions (`chmod 400`).
- Security group must allow your current IP for port 22 to enable SSH access.
- Basic Linux commands (`uname -a`, `df -h`, `uptime`) are useful to verify instance health and configuration.
- Always update the instance (`sudo yum update -y`) before installing additional packages.
- Installing Git (`sudo yum install git -y`) prepares the instance for future cloud projects.
- Documenting each step and taking screenshots makes troubleshooting easier and ensures reproducibility.
- AWS Free Tier limits should be monitored to avoid unexpected charges.

---

## Screenshots

### EC2 Dashboard
![EC2 Dashboard](screenshots/ec2-dashboard.png)

### SSH Terminal Connection
![SSH Terminal](screenshots/ssh-terminal.png)

### Linux Commands Output
![Linux Commands](screenshots/linux-commands.png)
