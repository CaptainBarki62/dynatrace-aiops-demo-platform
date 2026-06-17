# Synthetic Monitoring Configuration

## Objective

Create a Synthetic Availability Monitor for the EasyTrade application hosted on AWS EKS.

The monitor will be used to:

- Detect application outages
- Trigger Davis AI Problems
- Drive Workflow Automation
- Validate Auto-Remediation
- Measure Availability SLOs

---

# Prerequisites

Ensure:

- EasyTrade is deployed successfully
- Frontend service is accessible externally
- Dynatrace Operator is installed
- DynaKube is healthy
- Application appears in Dynatrace

Verify:

kubectl get svc -n easytrade

The frontend service should expose an external endpoint.

Example:

http://a1b2c3d4e5.us-east-1.elb.amazonaws.com

---

# Create Availability Monitor

Navigate to:

Dynatrace → Observe and Explore → Synthetic Monitoring

Click:

Create Monitor

Select:

HTTP Monitor

or

Browser Monitor

Recommended:

Browser Monitor

Reason:

Captures:
- Availability
- Response Time
- User Experience
- Frontend Failures

---

# Monitor Configuration

Monitor Name:

EasyTrade Frontend Availability

URL:

http://<AWS-LoadBalancer-URL>

Example:

http://a1b2c3d4e5.us-east-1.elb.amazonaws.com

Frequency:

Every 5 Minutes

Locations:

Choose:

- Mumbai
- Singapore
- Frankfurt

(At least one location is required)

Enable:

Problem Detection

Enable:

Alerting

Save Monitor

---

# Validation

Run the monitor manually.

Expected Result:

Passed

Verify:

Dynatrace → Synthetic Monitoring

Status should show:

Available

---

# Simulate Failure

Scale frontend deployment to zero.

Command:

kubectl scale deployment easytrade-frontendreverseproxy \
-n easytrade \
--replicas=0

Verify:

kubectl get pods -n easytrade

Frontend pod should disappear.

---

# Expected Outcome

Synthetic Monitor

↓

Monitor Failure

↓

Availability Event

↓

Davis AI Analysis

↓

Problem Card Generated

↓

Workflow Triggered

↓

Notification Sent

↓

Auto Remediation (Optional)

---

# Restore Application

Scale deployment back.

Command:

kubectl scale deployment easytrade-frontendreverseproxy \
-n easytrade \
--replicas=1

Verify:

kubectl get pods -n easytrade

Frontend pod should return.

---

# Expected Recovery

Synthetic Monitor

↓

Successful Execution

↓

Problem Auto Closed

↓

Availability Restored

---

# Davis AI Validation

Navigate:

Dynatrace → Problems

Verify:

Problem Title similar to:

Service unavailable

or

Synthetic monitor failed

Verify:

Root Cause

Affected Service

Timeline

Impact Analysis

---

# Dashboard Widgets

Recommended Widgets

1. Synthetic Availability %

2. Synthetic Response Time

3. Failed Executions

4. Problem Feed

5. Service Availability

6. User Experience Score

---

# Demo Flow

EasyTrade Running

↓

Synthetic Success

↓

Scale Frontend To Zero

↓

Synthetic Failure

↓

Davis AI Problem

↓

Workflow Trigger

↓

Auto Remediation

↓

Frontend Restored

↓

Synthetic Success

↓

Problem Closed

---

# Business Value

- Proactive Outage Detection
- Faster Root Cause Analysis
- Reduced Mean Time To Resolution (MTTR)
- Automated Incident Response
- Improved Customer Experience
- Demonstrates Self-Healing Operations
