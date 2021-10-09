# Replication Controllers & Replica Sets

## Replication Controller
Replication controller spawn extra pod/s if the exksting one/s were deleted. Replication controller can span into multiple nodes. Replication controller and replica sets execute the same purpose but actually, the replication controller is replaced by replicaSets, which is the recommended way of replicating.

ReplicationController has a template section which specifies the properties of replicating pods.

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: my-rc
  labels:
    app: front-end
spec:
  template:
    metadata:
      name: my-pod
      labels: 
        app: front-end
    spec:
      containers:
      - name: nginx-controller
        image: nginx
  replicas: 2
```

## Replica Sets
The replicaset has a `selector` field in addtion to the above, under spec.

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-rc
  labels:
    app: front-end
spec:
  template:
    metadata:
      name: my-pod
      labels: 
        app: front-end
    spec:
      containers:
      - name: nginx-controller
        image: nginx
  replicas: 2
  selector:
    matchLabels:
      app: frontend 
```

the selector specifies, which pods this replicaset needs to be applied for. The selector enables the replicaset to be applied for even the pods that has been created before the replicaset but, having the specified label.

- `kubectl replace -f file-name` can be used to update existing resource.
- `kubectl scale --replicas=6 -f replicaset-definition.yaml` or `kubectl scale --replicas=6 replicaset replicaset-name` can be used to update the replica count.

[Back](index.md)
