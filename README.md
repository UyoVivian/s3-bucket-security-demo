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

---

## Result: Anyone with the object URL could download the file
