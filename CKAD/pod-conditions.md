# Readiness and Liveness Probes

## Pod Conditions:

- PodScheduled
- Initialized
- ContainersReady
- Ready

Even though the `get pods` command displays the pod is ready, the containers might not be fully warmed up so that it can
correctly serves the user traffic. However, the services rely on pod’s ready status and as soon as the pod is ready, the
services routes the user traffic to pods.

## Readiness Probe
We can bind the container’s ready(up and running) status to the pod’s ready state by using `ReadinessProbe`.

```
apiVersion: v1
kind: Pod
metadata:
  name: webapp
  labels:
    name: webapp
spec:
  containers   - name: webapp
     image: webapp
     ports:
     - containerPort: 8080
     readinessProbe:
       httpGet:
         path: /api/ready
         port: 8080
```

or, for TCP you can use: (this can be used to test if a database port is ready to serve)

```
readinessProbe:
  tcpSocket:
    port: 3306
```

or you can also use exec command to test the readiness.

```
readinessProbe:
  exec:
    command:
      - cat
      - /app/is_ready
```

you can also specify initial delay if you know your app will consume that time before warming up at the minimum,
using `initialDelaySeconds`. How often to probe is defined using `periodSeconds`. By default if the app is not ready
after 3 probes, the probe will stop. You can change this number by specifying `failureThreshold`.

Example:

```
readinessProbe:
  exec:
    command:
    - cat
    - app/is_ready
  initialDelaySeconds: 10
  failureThreshold: 8
  periodSeconds: 5
```

## Liveness Probe
Liveness probe is also similar to the readiness probe. However, liveness probe periodically tests the application
whether it is healthy. If the liveness probe failed, Kubernetes will restart the pod. Configuration is similar to the
above. Only difference is it needs to be begin with livenessProbe.

```
livenessProbe:
  exec:
    command:
    - cat
    - app/is_ready
  initialDelaySeconds: 10
  failureThreshold: 8
  periodSeconds: 5
```

Logs: `kubectl logs -f <pod name> <container name>`

---
[Multi-Container Pods](multicontainer-pods.md)

[<< Prev](index.md)