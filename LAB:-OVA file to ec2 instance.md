# How to create a ec2 server from OVA file.

## step 1
First upload the OVA file (VMware disk file) to a s3 bucket.

## Step 2.
AWS needs permission to access your S3 bucket and import the image.
```bash
aws iam create-role \
  --role-name vmimport \
  --assume-role-policy-document '{
    "Version":"2012-10-17",
    "Statement":[{
      "Effect":"Allow",
      "Principal":{"Service":"vmie.amazonaws.com"},
      "Action":"sts:AssumeRole",
      "Condition":{"StringEquals":{"sts:ExternalId":"vmimport"}}}]
  }'
```
## Create a IAM policy for the above role
```bash
aws iam put-role-policy \
  --role-name vmimport \
  --policy-name vmimport \
  --policy-document '{
    "Version":"2012-10-17",
    "Statement":[{
      "Effect":"Allow",
      "Action":[
        "s3:GetBucketLocation",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject",
        "ec2:ModifySnapshotAttribute",
        "ec2:CopySnapshot",
        "ec2:RegisterImage",
        "ec2:Describe*"
      ],
      "Resource":"*"
    }]
  }'
```
## Create a Direct Inline policy 
```
aws ec2 import-image \
  --description "Windows Server VM import" \
  --disk-containers '[
    {
      "Description": "Windows Disk 1",
      "Format": "ova",
      "UserBucket": {
        "S3Bucket": "<Mention the bucket name>",
        "S3Key": "<OVA file name>"
      }
    }
  ]'
```
## This will create a import job with image job-id
```bash
{
    "Description": "Windows Server VM import",
    "ImportTaskId": "import-ami-6660066cXXXX56t",
    "Progress": "1",
    "SnapshotDetails": [
        {
            "Description": "Windows Disk 1",
            "DiskImageSize": 0.0,
            "Format": "ova",
            "UserBucket": {
                "S3Bucket": "aws-oag-ova",
                "S3Key": "oag-20XX25.10.0XXXXX24c5b8 (1).ova"
            }
        }
    ],
    "Status": "active",
    "StatusMessage": "pending"
}
```
### To check the status you convesation
```bash
aws ec2 describe-import-image-tasks   --import-task-ids import-ami-6660XXXXXX6cXX3c3756t
```

### Allow 5-10 mins based on the image make sure status is completed
```
bash
 aws ec2 describe-import-image-tasks   --import-task-ids import-ami-6660066c433c3756t
{
    "ImportImageTasks": [
        {
            "Architecture": "x86_64",
            "Description": "Windows Server VM import",
            "ImageId": "ami-09801f4XXXXXb61XXXX5c3",
            "ImportTaskId": "import-ami-66XXXXc433c3756t",
            "LicenseType": "BYOL",
            "Platform": "Linux",
            "SnapshotDetails": [
                {
                    "DeviceName": "/dev/sda1",
                    "DiskImageSize": 4057334272.0,
                    "Format": "VMDK",
                    "SnapshotId": "snap-0384XXXX136660",
                    "Status": "completed",
                    "UserBucket": {
                        "S3Bucket": "aws-oag-ova",
                        "S3Key": "oag-2025.10.0-5XXXXc5b8 (1).ova"
                    }
                }
            ],
            "Status": "completed",
            "Tags": []
        }
    ]
}
```
### Final step is to create ec2 instance out of the AMI

