# Kubernetes Scheduler

Kubernetes scheduler decides which node a pod goes to, based on the resources.

Define resource requests:

```
apiVersion: v1
kind: Pod
metadata:
  name: pod
spec:
  containers:
    - name: container
       image: ubuntu
       resources:
         requests:
           memory: “1Gi”
           cpu: 1
```

```
0.1 vCPU = 100m cpus
Lowest value for vCPU is ‘1m’

1 G = 1000 M
1 Gi = 1024 Mi
```

We can specify resource limits as follows with the    `limits` section.

```
apiVersion: v1
kind: Pod
metadata:
  name: pod
spec:
  containers:
    - name: container
       image: ubuntu
       resources:
         requests:
           memory: “1Gi”
           cpu: 1
         limits:
           memory: “2Gi”
           cpu: 2
```

When an application tries to exceed these limits, for the CPU, kubernetes throttles the requests so that it cannot go
beyond the limit

But for memory, a container can consume more memory than the limit. However, when a container uses more memory than
specified under limits constantly, kubernetes terminates the pod.

You can specify default values for resource limits by deploying a `LimitRange` kind in the namespace.

```
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
       memory: 512Mi
    defaultRequest:
       memory: 256Mi
    type: container 
```

status `OOMKilled` means the pod is failing due to out of memory

[<< back](index.md)
