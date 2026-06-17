# Troubleshooting

## Dynatrace Operator Not Starting

Verify:

kubectl get pods -n dynatrace

## Dynakube Not Created

Verify:

kubectl get dynakube -n dynatrace

## EasyTrade Pods Not Starting

Verify:

kubectl get pods -n easytrade

kubectl describe pod <podname>

## Synthetic Monitor Not Failing

Verify:

Frontend is actually scaled to zero

## Workflow Not Triggering

Verify:

Problem was created by Davis AI

Workflow trigger matches event type
