```bash
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  LambdaFunction:
    Type: 'AWS::Lambda::Function'
    Properties:
      FunctionName: ResourceAutogging1
      MemorySize: 500
      Timeout: 50
      Runtime: python3.8
      Handler: ResourceAutogging.lambda_handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Code:
        S3Bucket: cflambdas1
        S3Key: ResourceAutogging.zip
     
  
  LambdaExecutionRole:
    Type: 'AWS::IAM::Role'
    Properties:
      RoleName: ResourceAutoggingRole
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service: lambda.amazonaws.com
            Action: 'sts:AssumeRole'
      Policies:
        - PolicyName: LambdaExecutionPolicy
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - 'logs:CreateLogGroup'
                  - 'logs:CreateLogStream'
                  - 'logs:PutLogEvents'
                  - 'ec2:DescribeTags'
                  - 'ec2:CreateTags'
                  - 'ec2:DescribeSecurityGroupRules'
                  - 'ec2:DescribeSecurityGroups'
                Resource: '*'  
                          
  ResourceAutotaggingRule:
    Type: 'AWS::Events::Rule'
    Properties:
      Name: ResourceAutotaggingRule
      Description: 'Trigger the lambda in case of the Resouce Creation'
      EventPattern: 
        {
        "source": ["aws.ec2"],
        "detail-type": ["AWS API Call via CloudTrail"],
        "detail": {
          "eventSource": ["ec2.amazonaws.com"],
          "eventName": ["RunInstances", "CreateSecurityGroup"]
        }
      }
      
      State: 'ENABLED'
      Targets:
        - Arn: !GetAtt LambdaFunction.Arn
          Id: LambdaFunctionTarget
       
  
  PermissionForEventsToInvokeLambda:
    Type: AWS::Lambda::Permission
    Properties:
      FunctionName: !GetAtt LambdaFunction.Arn
      Action: 'lambda:InvokeFunction'
      Principal: 'events.amazonaws.com'
      SourceArn: !GetAtt ResourceAutotaggingRule.Arn

```

 
