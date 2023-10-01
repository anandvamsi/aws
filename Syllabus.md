# Understanding Linux OS and commands
- OS installation
- Shell and Linux kernel
- Linux Distributions
- File systems
- Ubuntu app/putty login
- Linux commands with respective  file,directories
- Disk Partitioning
- Process Management
- Package Management
- User Management
- Services Monitoring

# AWS
- Understanding AZs and Regions
- AWS Shared Responsibility Model
- Understanding the terms Elasticity, AZ, Region, Health, High availability, Load Balancing, Fault Tolerence

## IAM
- Understanding IAM users
- Activating MFA
- Generate Access and Security Key pair
- Keypair rotation policy
- IAM user, Roles, policies and best practices
- Policy creation methods
- Controlling access using policies
- Managed
- Inline policies
- Understanding SCPs and live usecases
- Cross Account Access
- Understanding permission Boundary
- AWS credential report generation
- IAM roles Anywhere :- Theory and usecases
- Industry Best practices for IAM
-

## VPC
- Basic Networking fundamentals
- Subnet creation logic
- Using subnet calculators
- VPC creation
- Subents
- Route tables
- IGW
- NAT Gateway
- VPC peering
- VPC endpoints
- Transit Gateway :- Theory and Usecases
- Network Monitoring

## Load Balancing
- What is Load Balancing
- Types of Load Balancing
- Classic Load Balancer
- Application Load Balancer
- Target Groups,LB Health
- ALB usecases::- Path based routing

## EC2
- what is an EC2
- Instance Type
- Instance Family
- Purchase options (ondemand,Reservation, spot)
- Spinning for new VM
- Understanding Security groups
- private and public key (PPK and Pem)
- Converting pem to PPK
- Root Volumes
- AMI  creation
- Usecase:: Golden AMI and tranfer AMI Across accounts
- volumes and Snapshots
- EBS volume creation and types
- EBS volume attachment and disk partitioning 
- Extending the root volume 
- userdata and ec2 resume during a key lost
- EC2 Tagging and Best Stratagies
- AMI cross region
- AWS CLI Installation and profile creation.
- Role Attachment
- AWS Resource metadata URL
- Upgrading EC2 instance type
- EC2 Lost Key pair:- How to login
  - Inject new key with user data
  - Root volume method


## Auto Scaling
- Understanding horizontal and vertical Scaling
- Auto scaling group
- configuration template
- simulation of AutoScaling

## AWS S3
- what is object storage
- S3 and usecases
- S3 creation and object uploading with UI and CLI
- S3 versioning
- S3 life cycle policy
- S3 website
- S3 Cross Account replication
- Presign URL creation with UI and CLI
- S3 bucket notification

## EFS
- what is EFS and advantages
- EFS creation and types
- Mouting EFS to EC2

## CLoudwatch
- Need for monitoring
- what is Cloudwatch
- CLoudwatch resources and metrics
- Cloudwatch alarm creation
- CLoudwatch agent installation
- Pushing logs to cloudwatch logs
- CLoud log insights
- Cloudwatch dashboard.

## CloudTrail
- What is CLoudtrail and usecases
- Creation of cloudtrail and integration with CW logs
- QUerying the Cloudtrail using log sights

## SNS
- SNS topic creation
- mail Subscription

## AWS Events
- What is AWS events
- Configure AWS event {scheduled and triggers}
- Integration with SNS topic

## RDS
- what is RDS and need
- RDS instance and types
- RDS creation and login
- Creation of Read replica
- RDS backups
- Paramter group.

## Systems Manager
- what is system manager
- Automation
- Inventory
- Managed and Unmanaged instances
- compliance and Patch Management
- Parameter store

## AWS Secret Manager
- what is vault and usecases
- Storing and retrieving values using UI and CLI

## AWS config,Guardduty,Inspector
- Need of AWS config,Guarduty
- query with config query tool
- Explaing Alerts of Guardduty
- Inspector Agent Installation


## AWS billing and cost explorer
- What is AWS billing
- Explain AWS billing components
- Creating a adhoc billing template
- Cost explorer

## steampipe::- Third party tool
- Configure inventory
- compliance
- Thirfty

## Cloudquery:: Third party tool
- what is cloud query
- Installing CQ
- CQ source and destination
- CQ Sync

#Grafana Integration
- what is Grafana
- Grafana installation
- Integration with Cloud query
