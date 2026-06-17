# Dynatrace Operator Installation

## Create Namespace

kubectl create namespace dynatrace

## Install Operator

helm install dynatrace-operator \
oci://public.ecr.aws/dynatrace/dynatrace-operator \
--create-namespace \
--namespace dynatrace

## Verify

kubectl get pods -n dynatrace

kubectl get deployment -n dynatrace

kubectl get daemonset -n dynatrace

## Verify CRDs

kubectl get crd | findstr dynatrace
