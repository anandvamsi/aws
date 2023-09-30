# AWS Account and  Best Practices
  - An AWS account is a ```container for your AWS resources```. You create and manage your AWS resources in an AWS account, a
    and the AWS account provides administrative capabilities for access and billing.
  - An Account will Account Name and account number.

# Types of Account 
  - Managment Account
  - Member Account

![AWS Account](org.jpg)

## Management Account 
  -  A management account/Master Account which is used to govern all the other accounts and consolidate billing and other services.
  -  The administrative root is the top-most container in your organization’s hierarchy.
  -  Under this root, you can create OUs to logically group your accounts and organize these OUs into a hierarchy that best matches your business needs.

    

# Best practices     
  -
  - A Production account for holding all their main production systems
  - A sandbox account where developers can play around and experiment with AWS services
  - A security account used for the cyber-security team with read only access to logs and other security services.
  - A shared services account which follows a hub-spoke model for services which are used across accounts e.g. storage of logs, networking etc.
