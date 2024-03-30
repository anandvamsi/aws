# Assume Role
Returns a set of temporary security credentials that you can use to access AWS resources. 
These temporary credentials consist of an access key ID, a secret access key, and a security token. Typically, you use AssumeRole within your account or for cross-account access

## Advantages of Assume Role


## Steps to create Assume Role and attach to IAM user.
- Create a IAM user with web access and no policy
- Create a IAM role Trusting Same Account "This Account" and attach any policy the user would like to perform
- Attaching the user to the Role Using below Policy ::- Create below inline policy and attach to the user.

```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "user-1 Policy",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "<ARN of the Role created Above>"
        }
    ]
}
```
- Now to Navigate to the role created select the option "Link of switch role created" and click on switch Role.
