# Send the message to SQS via SNS
we need a SNS and SQS to be created ; Note th SQS has to ```standad type```.

- Step1 :: Creae a SNS topic attach below policy to SNS topic
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Allow-Subscription",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::ACCOUNT-ID:root"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:REGION:ACCOUNT-ID:TOPIC-NAME"
        },
        {
            "Sid": "Allow-Publish",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Publish",
            "Resource": "arn:aws:sns:REGION:ACCOUNT-ID:TOPIC-NAME"
        }
    ]
}
```
Note::- replace the ARN of SNS topic in line numern 16 and line 17

- Step 2: Create a SQS standard Queue with all the basic paramters.
Attach below policy to the created SQS queue
```json
{
    "Version": "2012-10-17",
    "Id": "MyPolicy",
    "Statement": [
        {
            "Sid": "SNSToSQSPublish",
            "Effect": "Allow",
            "Principal": {
                "Service": "sns.amazonaws.com"
            },
            "Action": "sqs:SendMessage",
            "Resource": "arn:aws:sqs:REGION:ACCOUNT-ID:QUEUE-NAME",
            "Condition": {
                "ArnEquals": {
                    "aws:SourceArn": "arn:aws:sns:REGION:ACCOUNT-ID:TOPIC-NAME"
                }
            }
        }
    ]
}
```
Note: replace Line no 49 and 46 with Proper ARN

- step 3: Create a subscription for topic created in step1 with type "SQS" and select the SQS created in step2
Now public the message via SNS , Navigate to SQS you should be able to see the message count under "Messages available"
