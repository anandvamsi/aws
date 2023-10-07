# IAM
- what is IAM
- IAM Features
- IAM users,Roles,Policies
- IAM Group
- IAM Role Usecases
- IAM User Access/Secret Key
- Key Rotation Policy

## Scenario
Point 1
- Imagine you joined a company and you would get Access card and subject to access spefic rooms
 - you wont access to HR room officials
 - you wont access to  Company Executives
 etc
 
 In short IT team will give you access according to the job -role
  when you get promoted you will be give more access

Point 2
Imagine you Another team member who got access they have a different access than yours.


## IAM 
IAM is Global resource.
AWS Identity and Access Management is a core infrastructure service that provides the ```foundation for access control based on identities within AWS.```

AWS Identity and Access Management (IAM) is a web service that helps you securely
control access to AWS resources.
With IAM, you can centrally manage permissions that control which AWS resources
users can access.


## IAM Features
- Shared access to your AWS account
- Granular permissions
- Secure access to AWS resources for applications that run on Amazon EC2
- Multi-factor authentication (MFA)
- Identity federation
- PCI DSS Compliance
- Integrated with many AWS services
- Free to use

## How you use IAM differs, depending on the work that you do in AWS.
- ```Service user``` – If you use an AWS service to do your job, then your administrator provides you with the credentials and permissions that you need.
- ```Service administrator``` - If you're in charge of an AWS resource at your company you probably have full access to IAM.
- It's your job to determine which IAM features and resources your service users should access.
- As you use more advanced features to do your work, you might need additional permissions.
- ```IAM administrator``` – If you're an IAM administrator, you manage IAM identities and write policies to manage access to IAM.

## IAM Users
An IAM user is an identity within your AWS account that has specific permissions for a single person or application. Where possible, we recommend relying on temporary
credentials instead of creating IAM users who have long-term ```credentials such as passwords and access keys```. However, if you have specific use cases that require long-term
credentials with IAM users, we recommend that you rotate access keys.

## IAM Group
It is an identity that specifies a collection of IAM users. You can use groups to specify permissions for multiple users at a time. Groups make permissions easier to manage for large sets of users.

<img src="IAM-User-Group.PNG" width="600">
