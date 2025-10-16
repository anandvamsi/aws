# How to Patching Machines Cross Account.
For this we need one Management Account and Target Account.



step 1:- Create a Resource Group in the Target Account with instance with tag names.

step 2:- Create a IAM Role that needs to trust the Role of Target group
- Name of the Role :- AWS-SystemsManager-AutomationExecutionRole
- Policy JSON
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AutomationAdminPermissions",
            "Effect": "Allow",
            "Action": [
                "ssm:SendCommand",
                "ssm:StartAutomationExecution",
                "ssm:GetAutomationExecution",
                "resource-groups:ListGroupResources",
                "ssm:StartAutomationExecution",
                "ssm:StopAutomationExecution",
                "ssm:DescribeAutomationExecutions",
                "ssm:DescribeAutomationStepExecutions",
                "ssm:GetAutomationExecution",
                "ssm:ListDocuments",
                "ssm:GetDocument",
                "ssm:ListTagsForResource",
                "ssm:AddTagsToResource",
                "ssm:RemoveTagsFromResource",
                "iam:PassRole",
                "iam:GetRole",
                "ec2:DescribeInstances",
                "ec2:DescribeImages",
                "ec2:DescribeTags",
                "ec2:DescribeVolumes",
                "ec2:DescribeSnapshots",
                "ec2:DescribeInstanceStatus",
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:RebootInstances",
                "ec2:CreateImage",
                "ec2:CreateTags",
                "ec2:DeleteTags",
                "sts:AssumeRole"
            ],
            "Resource": "*"
        }
    ]
}
```
- Trust of the policy should be
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<managment Account >:role/AWS-SystemsManager-AutomationAdministrationRole"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```
- Note: AWS-SystemsManager-AutomationAdministrationRole is yet to be created in the Management Account.


## Target Account 
step 3:- Create a AWS-SystemsManager-AutomationAdministrationRole in the target Account with IAM policy
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartAutomationExecution",
                "ssm:GetAutomationExecution",
                "ssm:DescribeAutomationExecutions",
                "ssm:DescribeAutomationStepExecutions",
                "ssm:DescribeInstanceInformation",
                "ssm:GetCommandInvocation",
                "ssm:ListCommands",
                "ssm:ListCommandInvocations",
                "ssm:DescribeInstancePatchStates",
                "ssm:DescribeInstancePatchStatesForPatchGroup",
                "ssm:ListResourceComplianceSummaries",
                "ssm:GetPatchBaseline"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sts:AssumeRole"
            ],
            "Resource": "arn:aws:iam::<target Account>:role/AWS-SystemsManager-AutomationExecutionRole"
        }
    ]
}
```

- Trust Policy
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "organizations.amazonaws.com",
                    "ssm.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```


step 4:- Create System manager Automation Document In the Management Account
```
description: Automation document to execute the Command document AWS-RunPatchBaseline
schemaVersion: '0.3'
assumeRole: '{{AutomationAssumeRole}}'
parameters:
  AutomationAssumeRole:
    type: String
    description: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.
    default: ''
  Operation:
    allowedValues:
      - Scan
      - Install
    description: (Required) The update or configuration to perform on the instance. The system checks if patches specified in the patch baseline are installed on the instance. The install operation installs patches missing from the baseline.
    type: String
  SnapshotId:
    default: ''
    description: (Optional) The snapshot ID to use to retrieve a patch baseline snapshot.
    type: String
    allowedPattern: (^$)|^[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}$
  InstanceId:
    description: (Required) EC2 InstanceId to which we apply the patch-baseline
    type: String
  InstallOverrideList:
    default: ''
    description: (Optional) An https URL or an Amazon S3 path-style URL to the list of patches to be installed. This patch installation list overrides the patches specified by the default patch baseline.
    type: String
    allowedPattern: (^$)|^https://.+$|^s3://([^/]+)/(.*?([^/]+))$
mainSteps:
  - name: runPatchBaseline
    action: aws:runCommand
    maxAttempts: 3
    timeoutSeconds: 7200
    isEnd: true
    onFailure: Abort
    inputs:
      Parameters:
        SnapshotId: '{{SnapshotId}}'
        InstallOverrideList: '{{InstallOverrideList}}'
        Operation: '{{Operation}}'
      InstanceIds:
        - '{{InstanceId}}'
      DocumentName: AWS-RunPatchBaseline
outputs:
  - runPatchBaseline.Output
```

Step 5: Execute the Automation Document with following parameters
Multi-account and Region
### Target accounts and Regions
Mention the Account target Account number
Mention the region 
Automation Execution Role Name :- ```AWS-SystemsManager-AutomationExecutionRole```

### Targets 
paramters will ```InstanceId```
targets will be the ```Resource Group```

### Input parameters
Mention the role of the Managment account : ```AWS-SystemsManager-AutomationAdministrationRole```
Operation ```Install```

Execute the role.
