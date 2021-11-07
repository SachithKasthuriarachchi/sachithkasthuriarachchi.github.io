# Service

K8s services helps our application to connect with the other applications or with users.

Nodeport services: Services listen on a port on the node which redirect incoming traffic to a port on the pod inside the
node.

Here, we have three ports involved in the service’s viewpoint.

- TargetPort: which is the port on the pod that service is pointing to
- Port: port on the service itself
- NodePort: Port on the node which service listens to (Default range for port is 30000 - 32767)

```
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport
spec:
  type: NodePort
  ports:
    - targetPort: 80 #port on the pod. (not mandatory. similar to the port if not specified)
       port: 80 #mandatory
       nodePort: 30008 #Not mandatory. It will automatically assign a free port if not specified.
  selector:
    app: myapp
    type: front-end #These labels should match with the pod’s label (that is how service identifies the pod)
```

In this way, the service can match multiple pods with same labels to distribute the traffic in a random manner. Thus,
the service automatically acts as a loadbalanecer to the matching pods. If the pods span across multiple nodes, then the
kubernetes automatically creates the service to span across the nodes on the same node port on each node.

ClusterIP: Service creates a virtual IP inside the cluster to enable communication between different services such as
from front end servers to backend servers. Assume we have multiple pods for frontend, backend and database. also assume
the frontend needs to communicate with backend and the backend needs to communicate with the databases. For this
purpose, we can not rely on pods’ IP address since those are not static. Pods get destroyed and new ones created with
new IPs. To address this, we can group each kind of pods with the help of a service and the service will handle the IP
changes in the relevant pods. All we have to do then is that point the frontend pods to the backend service port and the
backend pods to the db service ports. This type of service which we can refer to using its name and port is known
as `ClusterIP` service.

```
apiVersion: v1
kind: Service
metadata:
  name: cluster-mine
spec:
  type: ClusterIP # This is the default service type if you didn’t specify
  ports:
    - targetPort: 80
       port: 80
  selector:
    app: myapp
    type: frontend # these should match with the pod’s labels
```

Then we can access the service using its name or its cluster ip.

LoadBalancer: This provides load balancing for our application in supported cloud providers. (ex: Distribute load across
applications)

---
[Back](index.md)