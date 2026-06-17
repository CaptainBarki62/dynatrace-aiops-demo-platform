# AWS EKS + Dynatrace Kubernetes Monitoring Setup

## Objective

Build a Dynatrace-monitored AWS EKS environment capable of supporting:
- Kubernetes monitoring
- Davis AI problem detection
- Synthetic monitoring
- SLO management
- Workflow automation
- Auto-remediation
- OpenTelemetry future integrations

## Step 0 - Initial Access Issue
Attempted to access AWS EKS from the AWS Console.

Error:
User was not authorized to perform eks:ListClusters.

Resolution:
Connected through Global Protect VPN and retried access.

## Step 1 - Configure AWS CLI Authentication

aws configure sso

Created profile:
GSIS-AWSLimitedAdmins-933412994300

Validation:

aws sts get-caller-identity --profile GSIS-AWSLimitedAdmins-933412994300

## Step 2 - Connect Kubectl To EKS Cluster

aws eks update-kubeconfig --region us-east-1 --name tek-sre-eks --profile GSIS-AWSLimitedAdmins-933412994300

## Step 3 - Kubectl Installation

Error:
'kubectl' is not recognized as an internal or external command

Resolution:
Installed kubectl and verified installation.

## Step 4 - Cluster Investigation

Verified:
- Cluster ACTIVE
- Nodegroup ACTIVE

Issue:
Worker nodes were initially unavailable.

Resolution:
Validated nodegroup scaling configuration and ensured nodes were provisioned.

## Step 5 - Install Helm

helm version

Verified Helm installation.

## Step 6 - Install Dynatrace Operator

kubectl create namespace dynatrace

Installed Dynatrace Operator using Helm.

## Step 7 - Dynakube Deployment Troubleshooting

Error:
unknown field spec.cloudNativeFullStack

Cause:
Operator / CRD version mismatch.

Resolution:
Updated configuration to match v1beta6 schema.

## Step 8 - Webhook Validation Failure

Error:
no endpoints available for service dynatrace-webhook

Resolution:
Waited for webhook pods to become healthy and retried deployment.

## Step 9 - Dynakube Conflict

Error:
Only one Agent per node is supported

Resolution:
Removed duplicate Dynakube configuration.

## Step 10 - Dynatrace Token Configuration

Configured:
- API Token
- Data Ingest Token

Created Kubernetes secret and applied Dynakube successfully.

## Step 11 - Verify Dynatrace Components

Verified:
- Operator
- Webhook
- OneAgent CSI Driver

## Step 12 - EasyTrade Deployment

git clone https://github.com/Dynatrace/easytrade.git

kubectl create namespace easytrade

helm install easytrade .\\helm\\easytrade -n easytrade

## Step 13 - Service Exposure

kubectl expose deployment easytrade-frontendreverseproxy -n easytrade --type=LoadBalancer --port=80 --target-port=8080

## Next Planned Activities

1. Synthetic Monitoring
2. Davis AI Failure Detection
3. SLO Configuration
4. Workflow Automation
5. Auto Remediation
6. ServiceNow Integration
7. OpenTelemetry Collector

## Key Lessons Learned

- AWS SSO is mandatory for CLI access.
- VPN access may be required.
- EKS can be ACTIVE while worker nodes are unavailable.
- Dynatrace CRD version must match Operator version.
- Webhook health is required before Dynakube deployment.
- Multiple OneAgents on the same node are not supported.
- Helm deployment is the easiest way to deploy EasyTrade.
