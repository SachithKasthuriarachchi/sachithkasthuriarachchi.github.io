# Monitoring and Debug Applications

You can have one metrics server per kubernetes cluster. Metrics server gets information from each nodes and stores them
in memory. Metrics server is an in-memory solution. Therefore, it doesn’t store the monitoring info on disk. Therefore,
you can’t see historical metrics.

on minikube try: `minikube addons enable metrics-server` or on a cluster, you can download the artifact.

Kubelet sends the metrics info to the metrics-server on each node. Kubelet has an application called `cAdvisor` (
container Advisor)

Cluster performance can then be viewed by running:

- `kubectl top node` (for nodes)
- `kubectl top pods` (for pods)

## Labels and Selectors

`kubectl get pods —selector app=frontend`

ReplicaSets view over the pods whose label(s) is(are) match/es with that is of ReplicaSets.

Services also uses the labels under selector to match with the right pods. Annotations are used to record details which
are relevant to informatory purposes

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: webapp
  labels:
    app: App1
    function: front-end
  annotations:
    buildVersion: 3.1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: App1
  template:
    metadata:
      labels:
        app: App1
        function: front-end
    spec:
      containers:
      - name: webapp
         image: webapp
```

---
[Rolling Out](rollout.md)

[Back](index.md)