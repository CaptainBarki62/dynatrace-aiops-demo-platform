# AKS Cluster Creation

## Login

az login

## Set Subscription

az account set --subscription "<subscription-name>"

## Create Resource Group

az group create \
--name dt-demo-rg \
--location eastus

## Create AKS Cluster

az aks create \
--resource-group dt-demo-rg \
--name dt-demo-cluster \
--node-count 2 \
--node-vm-size Standard_D2s_v5 \
--generate-ssh-keys

## Connect to Cluster

az aks get-credentials \
--resource-group dt-demo-rg \
--name dt-demo-cluster

## Verify

kubectl get nodes
