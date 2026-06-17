# Workflow Auto-Remediation

This file contains the automation script, Dockerfile and remediation.yaml used for Dynatrace Workflow Auto-Remediation.

## Architecture

Synthetic Failure

↓

Davis AI Problem

↓

Workflow Trigger

↓

Remediation API

↓

Kubernetes Action

↓

Service Recovery

---

# Automation Script

## app.py

```python
from flask import Flask, request, jsonify
import subprocess
import json

app = Flask(__name__)

def run_command(cmd):
    try:
        result = subprocess.check_output(
            cmd,
            shell=True,
            stderr=subprocess.STDOUT
        )
        return result.decode()
    except subprocess.CalledProcessError as e:
        return e.output.decode()

def get_current_replicas():
    cmd = "kubectl get deployment easytrade-frontendreverseproxy -n easytrade -o json"
    output = run_command(cmd)
    data = json.loads(output)
    return data['spec']['replicas']

@app.route('/remediate', methods=['POST'])
def remediate():

    data = request.get_json(force=True)

    issue_type = data.get("issue_type", "unknown")

    current_replicas = get_current_replicas()

    if issue_type == "failure":

        if current_replicas > 4:

            run_command(
                "kubectl scale deployment easytrade-frontendreverseproxy -n easytrade --replicas=1"
            )

        run_command(
            "kubectl rollout restart deployment easytrade-frontendreverseproxy -n easytrade"
        )

        return jsonify({
            "status": "scaled_down_and_restarted",
            "previous_replicas": current_replicas
        })

    return jsonify({
        "status": "unknown_issue_type"
    }), 400

@app.route('/')
def home():
    return "Dynatrace Auto-Remediation API Running"

app.run(host='0.0.0.0', port=5000)
```

---

# Docker Configuration

## Dockerfile

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY app.py .

RUN apt-get update && apt-get install -y curl

RUN curl -LO "https://dl.k8s.io/release/v1.29.0/bin/linux/amd64/kubectl" \
    && chmod +x kubectl \
    && mv kubectl /usr/local/bin/

RUN pip install --user flask

CMD ["python", "app.py"]
```

---

# Kubernetes Deployment

## remediation.yaml

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: remediation-sa
  namespace: easytrade

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: remediation-role

rules:
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "list", "patch", "update"]

- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: remediation-binding

roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: remediation-role

subjects:
- kind: ServiceAccount
  name: remediation-sa
  namespace: easytrade

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: remediation-api
  namespace: easytrade

spec:
  replicas: 1

  selector:
    matchLabels:
      app: remediation-api

  template:
    metadata:
      labels:
        app: remediation-api

    spec:
      serviceAccountName: remediation-sa

      containers:
      - name: remediation-api

        image: demoremediation17756.azurecr.io/remediation-api:latest

        imagePullPolicy: Always

        ports:
        - containerPort: 5000

---
apiVersion: v1
kind: Service
metadata:
  name: remediation-service
  namespace: easytrade

spec:
  type: LoadBalancer

  selector:
    app: remediation-api

  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
```

---

# Deployment Steps

Create the Python application:

```bash
nano app.py
```

Create the Dockerfile:

```bash
nano Dockerfile
```

Create the Kubernetes deployment:

```bash
nano remediation.yaml
```

Deploy:

```bash
kubectl apply -f remediation.yaml
```

Verify:

```bash
kubectl get pods -n easytrade
kubectl get svc -n easytrade
```

---

# Failure Simulation

Scale frontend to zero:

```bash
kubectl scale deployment easytrade-frontendreverseproxy \
-n easytrade \
--replicas=0
```

Verify:

```bash
kubectl get pods -n easytrade
```

Expected Result:

* Synthetic monitor fails
* Davis AI creates a problem
* Workflow executes
* Remediation API restarts frontend
* Service recovers automatically

---

# Business Value

* Automated Incident Response
* Reduced MTTR
* Self-Healing Operations
* Davis AI Driven Remediation
* Kubernetes Auto Recovery
* Demonstrates Enterprise AIOps Capabilities
