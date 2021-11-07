# Node Selectors

We can define on which nodes the pod needs to be scheduled. This can be done by first labelling the nodes and then
referring to that label inside the pod definition file. See the below example for how to use `nodeSelector`.

## Pods

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
   - name: processor
      image: alpine
   nodeSelector:
     size: Large
```

In order to use the above `size: Large` label, we first need to label the node with that.

## Nodes

`kubectl label nodes <node-name> <label-key>=<label-value>`

Ex: `kubectl label nodes node01 size=Large`

However, nodeSelectors are for simple purposes only (for one label)

[<< back](index.md)