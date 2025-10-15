# How to Upgrade AWS lambda Run time 
Its often get into situation to upgrade old lambdas get into deprecated version and AWS send email asking to upgrade the to latest lambda version
Here are steps to upgrade the Run time 

Note:- Make sure you have set the profile for the lambdas
Step 1:- List all the lambdas in account with older python versions
```bash
aws lambda list-functions --query "Functions[?Runtime=='python3.9'].[FunctionName,Runtime]" --output table --profile hv-XXXXXX
```
step 2:- Create the function which will generate the url to donwload the lambda
```bash
aws lambda get-function --function-name HV-IXXXXXXn --query 'Code.Location' --region us-west-2 --output text  --profile hv-XXXXX |  xargs curl -o my-function
```
step 3:- Unzip the lambda
```bash
unzip my-function.zip
```

step 4:- Update the lambda run time
```bash
aws lambda update-function-configuration   --function-name HV-XXXXXXXiXXXon   --runtime python3.11 --region us-west-2 --profile  hv-XXXX```
```
