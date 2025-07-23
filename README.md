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
