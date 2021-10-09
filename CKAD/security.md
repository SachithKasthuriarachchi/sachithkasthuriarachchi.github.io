# Kubernetes Security

If you specify security settings both on container and pod levels, the container level security specifications will override that of pod level.
```
apiVersion: v1
kind: Pod
metadata:
  name: my-ubuntu-pod
spec:
  containers:
  - name: ub-container
    image: ubuntu
    command:
      - "sleep"
      - "3600"
    securityContext:
      runAsUser: 1000
      capabilities:
        add: 
          - "MAC_ADMIN"
```
Capabilities are only allowed in container level and not in pod level.

[<< back](index.md)
