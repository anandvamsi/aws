# Login to the cluster using Identity Center


## Make sure you have configured the identity center configration
Here is the sample one
```bash
[profile hv-XXXXt-prd]
sso_start_url = https://d-XXXXX.awsapps.com/start/
sso_region = us-west-2
sso_account_id = 6231XXXXXX5532
sso_role_name = HV-XXXXn-AdminAccess
region = us-west-2
output = json
```

## From your desktop console login via SSO
```bash
aws sso login --profile hv-XXXX-dev
```

## Download the kubeconfig >> Do this after installing cluster 
```
aws eks update-kubeconfig --region us-west-2 --name Apps-dev --profile hv-XXXX-dev
Added new context arn:aws:eks:us-west-2:50XXXXXX285:cluster/Apps-dev to /home/anand/.kube/config
```

## Install the Kubectl client 
```bash
step3 install Kubectl
curl -LO "https://dl.k8s.io/release/v1.36.0/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

## Download the nodes
```bash
kubectl get nodes
NAME                                          STATUS   ROLES    AGE     VERSION
ip-10-8XXXX-237.us-west-2.compute.internal   Ready    <none>   4m31s   v1.36.4-eks-b9510e3
ip-10-XXX -72.us-west-2.compute.internal    Ready    <none>   6m23s   v1.36.4-eks-b9510e3
ip-10-XXX88.us-west-2.compute.internal    Ready    <none>   6m23s   v1.36.4-eks-b9510e3
```
