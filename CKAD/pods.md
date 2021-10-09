# Pod
Pod is the smallest thing in a kubernetes cluster. Pod encapsulates containers(Kubernetes does not allow deploying containers directly in the node. It encapsualte each container with a pod). A pod usually has a single container inside it. If the number of users accessing the running application inside the container increases, we need to deploy another pod/s with same appllication. We don't deploy another container with the same application in an existing pod.

Pod can have multiple containers inside it which are not same. That means we can deploy helper containers for the container running our application in the same pod. The containers running in the same pod can access each other via localhost since both of them are in the same network space. They can also easily share storages as well. If we want to scale up our application we have to deploye another pod running our application inside a container.  

- `kubectl run <application-name> --image <image-name>`: The image will be pulled online upon entering this command. The command first creates the pod running the specified application container.

- `kubectl get pods -o wide`: IP address, nodes placed on, nominated nodes, readiness gates information will be displayed

- `kubectl run redis --image=redis123 --dry-run=client -o yaml > pod.yaml`: Creates the yaml definition of the pod
