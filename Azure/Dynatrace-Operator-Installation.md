# Dynatrace Operator Installation
## Navigate to dynatrace platform and search for Kubernetes. 
Choose AKS and select the correct requirement of pods.
Follow the steps for helm installation of dt-operator.


helm install dynatrace-operator oci://public.ecr.aws/dynatrace/dynatrace-operator --create-namespace --namespace dynatrace --atomic

## Make sure helm is already installed in azure.
