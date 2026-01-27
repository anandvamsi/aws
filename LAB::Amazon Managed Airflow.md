# How to Create Amazon Managed Airflow system.
There are Multiple ways we can install either in VPC within the accounts or shared VPC account.
- Customer managed endpoints ::- Manual endpoints.
- Service managed endpoints ::- AWS will do necessary endpoints

## Below steps are to create in shared VPC
- In  AWS managed Airflow , select the s3 bucket,shared-vpc,subnets and select ```Customer managed endpoints``` Airflow will iniate the ```create``` phase.
- When it reaches ```pending state``` web and DB endpoint will created  ````com.amazonaws.vpce.us-west-2.vpce-svc-XXXXXXc36a7a9```(web) and ```com.amazonaws.vpce.us-west-2.vpce-svc-02cXXXXX```(DB)

### In the shared(Parent) Account where the VPC created

### Endpoints-Creation-1
- Grab two vpc-endservices strngs (com.amazonaws.vpce.us-west-2.vpce-svc-XXXXXXc36a7a9) for db and web will be optained from apache airflow interface.
- Create VPC endpoint and select ```PrivateLink Ready partner services```  and use endpoints mentioned in step 1 (com.amazonaws.vpce.us-west-2.vpce-svc-XXXXXXc36a7a9(for both web and DB) ) , select the vpc and subnets which are used while creation.
- select the security group which allow all the traffic to the CIDR.

### Endpoints-Creation-2
- Create VPC endpoints for below components from the master account.
```bash
com.amazonaws.ap-south-1.airflow-env
com.amazonaws.ap-south-1.logs
com.amazonaws.ap-south-1.monitoring
```
