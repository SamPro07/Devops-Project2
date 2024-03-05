# Provisioning the Amazon EKS cluster using Terraform with ArgoCD and CircleCI
This repository contains the terraform file code, which we can use to provision the **Amazon EKS** cluster In this comprehensive project, I dive deep into the world of **GitOps and ArgoCD**, demonstrating how to implement these essential DevOps practices step by step by **dockerizing** the application and provisioning the infrastructure using **Terraform**(this repo).



## Architecture Diagram

![Architecture Diagram](https://cdn-images-1.medium.com/max/800/1*T5IRoSoiqT8qnYLUprsRUQ.png)


## Installation of Terraform
Follow the below steps to Install the Terraform and another dependency.

1️⃣ Clone the repo

``` git clone https://github.com/SamPro07/terraformfiles.git ```
``` git clone https://github.com/SamPro07/Devops-Project2.git ```
``` git clone https://github.com/SamPro07/Kube_manifest.git ```

2️⃣ Let's install dependency to deploy the application

``` 
cd kube_terraform/ToDo-App/
terraform init
```

3️⃣ Edit the below file according to your configuration

`vim kube_terraform/ToDo-App/backend.tf`

add below code

```
terraform {
  backend "s3" {
    bucket = "S3-BUCKET-NAME"
    key    = "backend/TFSTATE-FILE.tfstate"
    region = "us-east-1"
    dynamodb_table = "DYNAMODB-TABLE-NAME"
  }
}
```

Let's set up the variable for our Infrastructure and create one file with the name of terraform.tfvars inside kube_terraform/ToDo-App/backend.tf and add the below conntent into that file.

```
REGION          = "us-east-1"
PROJECT_NAME    = "ToDo-App"
VPC_CIDR        = "10.0.0.0/16"
PUB_SUB1_CIDR   = "10.0.1.0/24"
PUB_SUB2_CIDR   = "10.0.2.0/24"
PRI_SUB3_CIDR   = "10.0.3.0/24"
PRI_SUB4_CIDR   = "10.0.4.0/24"
```

Please note that the above file is crucial for setting up the infrastructure, so pay close attention to the values you enter for each variable.

It's time to build the infrastructure

The below command will tell you what terraform is going to create.

`terraform plan`

Finally, HIT the below command to create the infrastructure...

`terraform apply`

type yes, and it will prompt you for permission or use --auto-approve in the command above.

Now make sure all the repo's youve cloned are now pushed to your own repo's and it is very vital that your github access token are configured to read and write. 

All you have to do now is integrate your circleCI account with your github account. 

next in the command line inside the the folder of Kube_manifest enter the code below:

`kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

`kubectl port-forward svc/argocd-server 8080:443 -n argocd`

`kubectl get secret argocd-initial-admin-secret -n argocd -o yaml`

`kubectl apply -f argo-manifest.yaml`

this will log you into ArgoCD and your project will automatically be deployed.

**This project contains Three GitHub repositories**

➡️ (https://github.com/SamPro07/terraformfiles.git)

➡️ (https://github.com/SamPro07/Kube_manifest.git)



