# Remove the ENI of first source A1 and add to another machine as Destination machine
Note:- we cannot remove the primary of any machine

## step 1:: We can terminate the primary machine (this will also remove the ENI).
## step 2:: Go to the network interfaces and create a new ENI and select the custom option and specify the primary IP address
- select the sunbet and VPC.

## step3:- Select the ENI created in the step2 and use the attach option to select the Destination instance.
Note:- The newly attached ENI will secondary for the Destination instance.
