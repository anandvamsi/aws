# How to configure SSO CLI in VM or a ec2

Before  executeing SSO CLI make sure you have SSO username and password
and make sure you are using awscli version 2

## Step 1:- Create SSO profile in ~/.aws/config file 
```bash
[profile dev-account]
sso_start_url = https://XXXXXXps.com/start/
sso_region = us-west-2
sso_account_id = 8518XXXXXX27658
sso_role_name = XXXX-ClXXXXXdAdmin-AdminAccess
region = us-west-2
output = json
```

## Step 2 SSO Login
```
aws sso login --profile dev-account
Attempting to automatically open the SSO authorization page in your default browser.
If the browser does not open or you wish to use a different device to authorize this request, open the following URL:

https://XXXXXXsapps.com/start/#/device

Then enter the code:

MXXXXPP
gio: https://XXXXXXps.com/start/#/device?user_code=MGXXXXXBJPP: Operation not supported
Successfully logged into Start URL: https://d-XXXXXf25XX.awsapps.com/start/
```
Note::- When you login using the AWS CLI  you will get a URL, Use the URL and punch in the code.
