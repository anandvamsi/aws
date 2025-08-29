# How to add module to lambda.

- step 1: Package requests with your Lambda deployment ZIP (manual way)
```bash
mkdir lambda-requests
cd lambda-requests
```
- step 2:- Add your Lambda function code (e.g., lambda_function.py) inside the same folder
```bash
import requests

def lambda_handler(event, context):
    response = requests.get("https://api.github.com")
    return {"statusCode": response.status_code, "body": response.text}
```
- step 3:- Zip Every thing
```bash
zip -r lambda_function.zip .
```
- step 4:- Upload the ZIP to Lambda (either via Console or AWS CLI):
```bash
aws lambda update-function-code --function-name MyLambda --zip-file fileb://lambda_function.zip
```
