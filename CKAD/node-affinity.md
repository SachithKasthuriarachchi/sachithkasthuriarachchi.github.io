# Node Affinity

This is to ensure that the pods are only placed on particular nodes

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: container
       image: alpine
  affinity:
    nodeAffinity:
      requireDuringSchedulingIgnoreDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
               - key: size
                  operator: In
                  values:
                    - Large
                    - Medium
```

### Possible values for operator

- In
- NotIn
- Exists (when using this, you don’t need to specify values. It just checks if the label(key) exists on the node)

### Possible types of node affinity

- requiredDuringSchedulingIgnoredDuringExecution
- preferredDuringSchedulingIgnoredDuringExecution
- requiredDuringSchedulingRequiredDuringExecution

The first one is used when the placement of the pod is crucial. If a suitable node could not be found, the pod will not
be scheduled.

The second one places the pod in any node if the scheduler could not find a matching node.

The `ignoredDuringExecution` ensures that the any change in the node affinity while the pod is running will be
neglected. That means the pod will continue to run on the node it was scheduled.

[<< back](index.md)