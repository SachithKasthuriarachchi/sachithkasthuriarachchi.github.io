# Taints and Toleration

Ex: Humans apply insect repellents(taint) so that the bugs will not come near them. However, some bugs has **
toleration** to such taints.

In kubernetes, humans are nodes and the bugs are pods. Taints and tolerations have nothing to do with security. It is
just a restriction which nodes apply on pod. It decides what pods can be placed on which nodes. By default, pods have no
toleration.

```
Taints: nodes
Tolerations: pod
```

## Add a taint to a node:

`kubectl taint nodes node-name key=value:taint-effect`

taint-effect = what happens to pods that do not tolerate this taint.

### Taint Effects:

```
  NoSchedule: pods will not be scheduled
  PreferNoSchedule: Scheduler will try not to schedule pods on the nodes
  NoExecute: Scheduler will evict(kill) the already existing pods that are not tolerating and it will not place pods on this node which don’t tolerate the taint.
```

ex: `kubectl taint nodes node1 app=blue:NoSchedule`

## Applying tolerations to pods.

```
apiVersion: v1
kind: Pod
metadata:
  name: sample-pod
spec:
  containers:
    - name: my-container
      image: alpine
  tolerations:
    - key: "app"
      value: "blue"
      operator: "Equal"
      effect: "NoSchedule"
```

All of the values inside tolerations needs to be **double quoted**. Taints and toleration **only** tells the node which
pods it can accept. It doesn’t tell the pod to schedule on this specific node. The Scheduler does not schedule any pod
on the master node.

## How to see the taints

`kubectl describe node kubemaster | grep Taint`

## Remove taint

`kubectl taint node controlplane node-role.kubernetes.io/master:NoSchedule-`

[<< back](index.md)
