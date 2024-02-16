# Elastic container registry.

Step1: Create a repository in the ECR.
step2: Tag the image.
```bash
docker tag <image-name> 2100dddd0XX52.dkr.ecr.us-west-2.amazonaws.com/msimages:latest
```
step3: ECR login
```bash
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 2100dddd0XX52.dkr.ecr.us-west-2.amazonaws.com
```
step 4: Push the image.
```push
docker push 2100dddd0XX52.dkr.ecr.us-west-2.amazonaws.com/msimages:latest
The push refers to repository [2100dddd0XX52.dkr.ecr.us-west-2.amazonaws.com/msimages]
8a7a2e028ba2: Pushed
d6795f9b40a5: Pushed
18e2ffa348b5: Pushed
97fed1e002d9: Pushed
451ce86d212e: Pushed
347b5e94b56a: Pushed
d4fc045c9e3a: Pushed
latest: digest: sha256:3c2bd2f9cc4040a7246696eaf602d730ab7537a06ac42838d2dc526d5f627ec9 size: 1788
```
