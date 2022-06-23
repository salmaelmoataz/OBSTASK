# OBSTASK

# ✨EKS Cluster✨

## Description
we are targeting to create EKS cluster that contain two nodes(master-worker), deploy deployment, and connect pods together with service, 
and connect this service with ingress to expose it throw internet, and connect this ingressto ngnix ingress controller that workes as load balancer and recive requests and deliver it to ingress.


## Requirements
- having access to AWS
- [Installing  AWS CLI] (https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [Installing Kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)


## Walking through
We can discuss our solution through 4 steps:

1. create VPC
2. building EKS cluster and its nodes
3. install nginx ingress
4. deploying container,serviceand ingress


## 1- Creating VPC 
we will create VPC with two subnet:
1. public subnet for ingress and nginx ingress controller to recieve requests
2. private subnet for pods

for high availability we create VPC in two AZ so we will have 4 subnet (2public-2private)

## 2- Creating EKS Cluster and its nodes
we will vreate the cluster in the VPC in the 4 subnets
we will attach to the cluster EKS Service role with **EKS** trust policy  **AmazonEKSClusterPolicy** managed policy
**EC2 Managed Nodes Group**: Creating Nodes Group with the following configs:
    - **Role**: Nodes Service role with **AmazonEKSWorkerNodePolicy** & **AmazonEC2ContainerRegistryReadOnly** managed policies.
    - **Subnets** Selecting only the 2 private subnets to place nodes
    - **Scaling Options**: Setting 2 nodes as the min, max & desired capacity
we will restric ip address by creating security group and apply it to nodes and specify on it range of ip

## 3- Install ngnix ingress
from link (https://github.com/kubernetes/ingress-nginx)
we will open the deploy then choose provider AWS and choose version 1.22 in this link (https://github.com/kubernetes/ingress-nginx/blob/main/deploy/static/provider/aws/1.22/deploy.yaml)

## 4- Deploying container,service and ingress
After cluster was added we need to deploy all of them with yaml file



## configuration

for the first and seconed point created from GUI and CLI with the help of the official documention(https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html#eks-configure-kubectl)

for third point from the link i save the file on my laptop and do some changes on it to be more suitable for the project will attach the final file in the repo

for forth point will attach the configuration file in the repo

apply every file with command(kubectl apply -f $filename)

after finishing, can delete every thing with command(kubectl delete all --all -n ingress-nginx)



## Testing Service
after creating every thing i got the ip of the ingress from command(kubectl get ingress -n nginx-ingress) and check this ip on google and the page of juice shop has appered
