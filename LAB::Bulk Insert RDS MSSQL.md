# DATA BULK INSERT TO RDS
bulk Insert from a CSV file directly to RDS to not possible, first we need to stage data to RDS from S3 and use bulk insert command

### Step 1: Upload the file to s3 bucket

### step 2: Create a IAM Policy
```bash

    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:AbortMultipartUpload",
                "s3:ListBucket",
                "s3:GetBucketAcl",
                "s3:GetBucketLocation",
                "s3:ListMultipartUploadParts"
            ],
            "Resource": [
                "arn:aws:s3:::<bucket-name>",
                "arn:aws:s3:::*/*"
            ]
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "*"
        }
    ]
}
```
### Step 3: Create a role out of this with RDS as the Trust
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "Service": "rds.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

### Step 4: Navigate to RDS and attach the IA role created in step3
Go to the section "Manage IAM roles" and select the IAM role and use the feature "S3_INTEGRATION"


### Step5 :: Stagging the file to RDS
```bash
exec msdb.dbo.rds_download_from_s3
 @s3_arn_of_file='arn:aws:s3:::ypersddddersbu-data/sdfddc/xaas.csv',
 @rds_file_path='D:\S3\yperscalddddddsbu-data\sdfddc\xaas.csv',
 @overwrite_file=1;
```
```bash
D:\ is the path in RDS mention as it is
```

### Step 6: Execute the Bulk insert command
```bash
BULK INSERT accounts
FROM 'D:\S3\yperscalddddddsbu-data\sdfddc\xaas.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2  -- if your file has a header row
);
