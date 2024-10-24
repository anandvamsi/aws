# How to Access secrets shared in Account A to be shared in Account B to be used in role to access ec2,lambda

## Assuming the secret named "newkey" is shared in the Account A and its encrypte with KMS Sharekeynew in the same Account.

-  Step 1:- Create a IAM role trusting ec2/lambda in Account B and grab the ARN
-  Step 2: Navigate to the Account A secret manager item "newkey", and create a new resource based policy, Here is the same policy
```bash
{
  "Version" : "2012-10-17",
  "Statement" : [ {
    "Effect" : "Allow",
    "Principal" : {
      "AWS" : "arn:aws:iam::8XXXXX16CCC42:role/forsecrets"------> ARN mentioned in the step 1
    },
    "Action" : "secretsmanager:GetSecretValue",
    "Resource" : "*"
  } ]
}
```

- Step 3: In the Account A, Since the secret is encrypted with CMK , we need modify the respective CMK policy, Here is the policy of KMS CMK
```bash
{
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::8XXXXX16CCC42:role/forsecrets"
            },
            "Action": "kms:Decrypt",
            "Resource": "*"
}
```
- step 4: In the Account B, Create a new policy to the role "forsecrets" and attach to the role create in the step1.
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "secretsmanager:GetSecretValue",
            "Resource": "arn:aws:secretsmanager:us-west-2:28311852:secret:newkey-????"
        },
        {
            "Effect": "Allow",
            "Action": "kms:Decrypt",
            "Resource": "arn:aws:kms:us-west-2:28311852:key/8c5ff1c8-873e-44d1-a3a4-1c271c836"
        }
    ]
}
```
- step 5: Create a ec2/lambda and attach the role created in the step1 and execute below command if its a ec2 instance.
```bash
aws secretsmanager get-secret-value --secret-id arn:aws:secretsmanager:us-west-2:28311852:secret:newkey-???  --region us-west-2
```
