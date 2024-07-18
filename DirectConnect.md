# Direct connect
# How to share the Direct connect to cross Account 

## What is Direct connect. 


## Share the Direct connect to Cross Account.
Imagine we have direct connect created in Account 101 and we have a vpc created under the  child account 201 which  needs to access direct in Account 101.
Note:: Every active direct connect will have a serial/series number

Step 1: Create a VPG in the child account 201 and link to the VPC you have created.

step2:  In the child account 201, Navigate to the Direct connect and select the virtual private gateway.

Step3: Navigate "Virtual private Gateway" and select the VPG which was created in the step 2 which will ask for the serial number and DC Account number.
Note:- This will send acceptance/denial notifcation to Direct connect Account 101, this can be seen under the "Direct connect gateways"

step4: Now login to the Direct connect Account 101 and select the Direct connect service and select "Direct connect gateways"
Accept or deny the Direct connect request from the child account.

Note:- Connection Acceptance will take around 10-15 mins.

step 5. Modify the route tables in Child account 101 with CIDR as the source and target as the VGW created in step2.
