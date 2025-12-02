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
  1. Fetch the PDF object from S3.
  2. Convert the binary PDF data into a **Base64 string** (essential for API Gateway transmission).
  3. Return a JSON response with the header `Content-Type: application/pdf` and `isBase64Encoded: true`.

### 3. API Gateway Integration
- Configured an **HTTP API** in API Gateway.
- Created a `GET /resume` route triggered by the Lambda function.
- Enabled **CORS** (Cross-Origin Resource Sharing) to allow browsers to request the file.
- Deployed the API to a production stage to generate a public Invoke URL.

### 4. Verification
- Tested the API using `curl` to inspect headers.
- Accessed the API Gateway URL in a web browser to confirm the PDF renders/downloads correctly.

---

## Commands & Code Snippet

### AWS CLI Commands used for verification:
```
# Verify file exists in bucket
aws s3 ls s3://janmarc-resume-storage

# Test the API functionality via terminal
curl -v https://<api-id>.execute-api.<region>[.amazonaws.com/resume](https://.amazonaws.com/resume)
```

### Key Python Logic (Lambda):
```
import boto3
import base64
import os

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    bucket = os.environ['BUCKET_NAME']
    key = 'resume.pdf'
    
    # Fetch and encode
    file_content = s3.get_object(Bucket=bucket, Key=key)['Body'].read()
    encoded_pdf = base64.b64encode(file_content).decode('utf-8')
    
    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'application/pdf', 
            'Content-Disposition': 'inline; filename="resume.pdf"'
        },
        'body': encoded_pdf,
        'isBase64Encoded': True
    }
```

---

## Notes / Lessons Learned
- **Binary Data Handling**: API Gateway and Lambda usually exchange JSON strings. To serve a PDF, I learned I had to Base64 encode the binary data in Python and explicitly tell API Gateway `isBase64Encoded: True` so it decodes it back to a binary file for the user.
- **Least Privilege Security:** Instead of making the S3 bucket public (which is risky), I used an IAM Role. This ensures only my specific Lambda function can read the resume, maintaining strict security boundaries.
- **CORS:** I initially encountered errors when calling the API from a browser fetch. I learned that enabling CORS headers (`Access-Control-Allow-Origin`) in the response is mandatory for web integration.
- **Cost Efficiency:** Because this uses Serverless (Lambda/S3), I effectively pay nothing ($0.00) while the resume is idle, only incurring costs when the API is actually clicked.

---

## Screenshots

### Architecture Diagram
![EC2 Dashboard](screenshots/ec2-dashboard.png)

### S3 Bucket Configuration
![SSH Terminal](screenshots/ssh-terminal.png)

### Lambda Function Code
![Linux Commands](screenshots/linux-commands.png)

### Successful PDF Render in Browser
![Linux Commands](screenshots/linux-commands.png)
