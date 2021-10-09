# Basics

Node is a physical/virtual machiene. Nodes have kubernetes installed. It is nodes where the containers relies. Node is called 'Minion' as well. One node is not enough since if it failed we still need to access the application. For that we need multiple nodes.

Cluster is a collection of multiple nodes. Even if one node fails we have other nodes to point our  traffic to. The real ochestration is handled by a node called 'master node' which is configured to be master. It monitors all other nodes in the cluster and decide how the traffic should happen etc.

Installing kubernetes actually installs the following components:
	
	1. API Server
	2. etcd
	3. Kubelet
	4. Controller	
	5. Scheduler
	6. Container Runtime

Users, command line interfaces all interact with the kubernetes cluster via the api-server.
etcd is the key values store which stores all the information related to cluster such as status of a node.
Scheduler distributes the work among containers on different nodes. It identifies which are new containers and assign work for them.

Controller is the brain of container orchestration. It can identify when traffic fails to a certain container. It can decide when new container is needed.
Container runtime is the software which runs our containers.
Kubelet is installed on every node in the cluster and it is responsible to check if the container is running as expected.

The master node contains kube api server, conrtroller and scheduler. The etcd lies on the master which stores all the information regarding the status of nodes.
The worker node is where the containers run. So the worker node has container runtime installed. Also it has kubelet which is responsible for sending health information of worker node to the master node. Kubelet is also responsible for carrying out actions on the node which master is instructing.

Kubectl is the command line tool to interact with clusters and nodes.

	 kubectl run <application-name>: Runs an application on a k8s cluster
	 kubectl get nodes: Lists all the nodes in a cluster
	 kubectl cluster-info: Returns cluster info
	 kubectl get pods: List the pods
         kubectl describe pod myapp-pod: Get more details about the pods (creation time, labels assigned, what docker containers are part of it, events associated with pod)
	 kubectl create -f pod-definition.yaml: Creates the resource from pod-definition.yaml
	 
## K8S Manifests
Kubernetes definitions in yaml always have following 4 manodotory properties in the yaml file. They are,

- apiVersion
- kind
- metadata
- spec


[Part 2](basics-part2.md)
