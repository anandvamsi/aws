# EKS

## Installation steps
#### prerequite
- Install awscli
- Install kubectl
```bash
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.23.15/2023-01-11/bin/linux/amd64/kubectl
chmod +x kubectl
mv kubectl /usr/bin
```

step1:- 
Create a IAM role with trust as "EKS cluster" with below policies
- AmazonEKSClusterPolicy

step2:- Lauch a EKS cluster providing 
- Name of the cluster
- provide the role created in step1
- Select the VPC,subnet,security group
- In the section "Cluster endpoint access" choose private if you dont want to access cluster componets outside.
- Click create ::- will take around 20mins

step3: Create a role for EKSWorker node with below policies
- AmazonEC2ContainerRegistryReadOnly
- AmazonEKS_CNI_Policy
- AmazonEKSClusterPolicy
- AmazonEKSWorkerNodePolicy

Step4: Go back to EKS console and select ```compute```tab and select ```Add node group```
- Use the instance type and role created in step3 and go with defautl options


step5: Login to client node where awscli and kubectl is installed 
Note: To Authenticate to eks cluster either we can use IAM user / okta user.

Execute below command to download the kube config
```bash
aws eks --region us-west-2 update-kubeconfig --name Nginx --profile hv-XXXX
```
This will download the kubeconfig file in the users home directory

```bash
kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   172.20.0.1   <none>        443/TCP   76m

kubectl get pods
 NAME                                          STATUS   ROLES    AGE   VERSION
ip-10-XXXXX.us-west-2.compute.internal   Ready    <none>   39m   v1.28.3-eks-e71965b
ip-10-XXXXX.us-west-2.compute.internal   Ready    <none>   40m   v1.28.3-eks-e71965b
```
