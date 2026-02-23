# Opensearch Back
Strategy to backup opensearch cluster indicies to s3 bucket, considering the condition that you have an opensearch cluster with indices in that and able to login to kibana with kiban URL

## Create a s3 bucket and copy the ARN
## Create iam IAM role with es and ec2 as the trust as elasticsearch and ec2
Here is the policy
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::hipiXXXXXXackup"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::hipXXXXXXXckup/*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::9XXXXXXX15:role/hipinprobackup"
        },
        {
            "Effect": "Allow",
            "Action": "es:ESHttp*",
            "Resource": "arn:aws:es:us-west-2:9XXXXXXX915:domain/hipinpro/*"
        }
    ]
}
```
Note: you can add the iam:PassRole post creating the IAM role , since you need to add the ARN of the role.

## Below is trust to the IAM Role
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "ec2.amazonaws.com",
                    "es.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```
### Create a opensearce resource based policy
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "es:*",
      "Resource": "arn:aws:es:us-west-2:93XXXXXX7915:domain/XXXX/*"
    }
  ]
}
```


### Create a EC2 instance and attach above role created 
#### Two steps are involved 
##### a. SNAPSHOT registration , this is one time step preformed at ec2
##### b. SNAPSHOT initate, performed in the kibana dev console.

#### Step a
Install below python packages in the ec2 instance
```bash
 pip3 install boto3
 pip3 install requests_aws4auth
 pip3 install requests
```
### Execute snapshot registration script
```bash
import boto3
import requests
from requests_aws4auth import AWS4Auth
host = 'https://vXXXXXXXXXXXwest-2.es.amazonaws.com'
region = 'us-west-2'
service = 'es'
credentials = boto3.Session().get_credentials()
awsauth = AWS4Auth(credentials.access_key, credentials.secret_key, region, service, session_token=credentials.token)
# Register repository
path = '/_snapshot/hipinprobackup'  # the OpenSearch API endpoint, here under repository-name enter a name you want to create
url = host + path
payload = {
 "type": "s3",
 "settings": {
  "bucket": "<bucket name>",
  "region": "us-west-2",
  "role_arn": "<ARN of the IAM role attached>"
 }
}
headers = {"Content-Type": "application/json"}
r = requests.put(url, auth=awsauth, json=payload, headers=headers)
print(r.status_code)
print(r.text)
```
### Post execution of you should 200 as the response code
```
python3 hipiXXXXXup.py
200
{"acknowledged":true}
```

#### Step b.
### Login to Kibana console and in developer mode ,execute below commands
```bash
PUT _snapshot/hipinprobackup/backup-23-02-2026 // for ingesting backups

GET _snapshot/hipinprobackup/_all?pretty // check the status look for state "SUCCESS"

```

