# How to Enable logs in NLB to s3 bucket.

Step1: Create a s3 bucket with following permission
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "delivery.logs.amazonaws.com"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::YOUR-S3-BUCKET-NAME/AWSLogs/YOUR-ACCOUNT-ID-HERE/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        }
    ]
}
``` 

Step 2: Navigate to NLB Attibutes and choose the S3 bucket created in step1
