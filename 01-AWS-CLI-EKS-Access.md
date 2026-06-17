# AWS CLI & EKS Access

## Configure AWS SSO

aws configure sso

Inputs:

SSO Start URL
SSO Region
AWS Account
Permission Set

## Verify Login

aws sts get-caller-identity \
--profile GSIS-AWSLimitedAdmins-933412994300

## Configure EKS Access

aws eks update-kubeconfig \
--region us-east-1 \
--name tek-sre-eks \
--profile GSIS-AWSLimitedAdmins-933412994300

## Verify Cluster

kubectl cluster-info

kubectl get nodes

## Validate EKS

aws eks describe-cluster \
--name tek-sre-eks \
--region us-east-1 \
--profile GSIS-AWSLimitedAdmins-933412994300

## Validate Nodegroups

aws eks list-nodegroups \
--cluster-name tek-sre-eks \
--region us-east-1 \
--profile GSIS-AWSLimitedAdmins-933412994300

aws eks describe-nodegroup \
--cluster-name tek-sre-eks \
--nodegroup-name tek-sre-eks-wrkr-nodes-20231203023458542300000001 \
--region us-east-1 \
--profile GSIS-AWSLimitedAdmins-933412994300
