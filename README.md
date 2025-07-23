
# s3-bucket-security-demo
AWS S3 and IAM security project

##  Objective
Demonstrate how insecure Amazon S3 buckets and IAM policies can lead to security risks, and how to properly secure them using AWS best practices.

---

##  Tools Used
- AWS Console
- S3
- IAM
- (Optional) AWS CLI

---

##  Steps Performed

###  1. Created a New S3 Bucket
- **Bucket name:** `vivian-cloudsec-demo`
- Region and settings set to default
- Purpose: simulate a real-world use case

###  2. Uploaded a Sample File
- File uploaded: `sensitive.txt`
- Purpose: simulate a sensitive asset (e.g., credentials or personal info)

###  3. Simulated Misconfiguration (Public Access)
- **Disabled block public access** on the bucket
- Applied the following **public-read bucket policy**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::vivian-cloudsec-demo/*"
    }
  ]
}
```

- Result: Anyone with the object URL could download the file

###  4. Created IAM User
IAM user created: junior-analyst

Attached full S3 access policy using this JSON:

```json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*"
    }
  ]
}
```
-This user could now manage and interact with all S3 buckets and objects

###  5. Secured the Bucket
A. Removed Misconfiguration

Deleted the public bucket policy

Re-enabled block public access

B. Applied Least Privilege Policy

New bucket policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificUserAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::873516289012:user/junior-analyst"
      },
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::vivian-cloudsec-demo",
        "arn:aws:s3:::vivian-cloudsec-demo/*"
      ]
    }
  ]
}
```

###  6. Enabled S3 Server Access Logging
Created a new bucket: vivian-logs

Configured vivian-cloudsec-demo to send logs to vivian-logs

Verified logging works as expected

## Outcome
This project demonstrated:

How public access to S3 objects exposes sensitive data

How to use IAM and bucket policies to enforce least privilege

How to enable logging for auditing and traceability


## See Screenshots below

![S3 bucket](https://github.com/user-attachments/assets/03f98c9f-6feb-4664-9d9f-1af3865791bd)
![sensitive file](https://github.com/user-attachments/assets/5bc7abfb-2da3-464d-aa3f-04d962b43ec0)
![Public S3 bucket policy](https://github.com/user-attachments/assets/031ea929-5ac7-4bce-93ba-15b92e8083ed)
![S3 bucket public access - Unblocked](https://github.com/user-attachments/assets/b504a512-82ba-4cf7-a355-19610e07b107)
![publicly accessible url](https://github.com/user-attachments/assets/7a723797-8021-460c-9932-33b1ade2024e)
![IAM user credentials](https://github.com/user-attachments/assets/bcf5f7c9-771e-47f2-831e-55490da90ae9)
![Restricted S3 bucket policy](https://github.com/user-attachments/assets/b8ff5999-36c1-4faf-8d12-d022ef95f557)
![Restricted url](https://github.com/user-attachments/assets/67230ba4-77a4-40a8-9e1c-0f1394b13e8a)
![Enabled Server Access logging](https://github.com/user-attachments/assets/b84fd20e-0c5c-4fda-8043-86e429fac792)
![Server Access logging successful](https://github.com/user-attachments/assets/08e4e8a0-497b-4107-a2b7-523814f50f8a)
