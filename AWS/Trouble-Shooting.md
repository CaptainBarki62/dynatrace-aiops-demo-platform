# Troubleshooting

## AWS SSO

Error:
NoCredentials

Fix:
aws configure sso

------------------------------------------------

## Kubectl

Error:
kubectl not recognized

Fix:
Install kubectl

------------------------------------------------

## EKS

Error:
No nodes visible

Fix:
Validate nodegroup scaling

------------------------------------------------

## Dynakube

Error:
unknown field cloudNativeFullStack

Fix:
Use correct CRD version

------------------------------------------------

## Webhook

Error:
no endpoints available for service dynatrace-webhook

Fix:
Wait until webhook becomes healthy

------------------------------------------------

## Dynakube Conflict

Error:
Only one Agent per node is supported

Fix:
Remove duplicate Dynakube deployment

------------------------------------------------

## EasyTrade

Error:
No External IP

Fix:
Expose service using LoadBalancer
