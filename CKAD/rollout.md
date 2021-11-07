# Rollouts

The status of rolling out can be obtained by following command.

```
kubectl rollout status deployment myapp-deployment
```

History and revisions of the rolling out can be visualized using,

```
kubectl rollout history deployment myapp-deployment
```

You can check the status of each revision individually by using the --revision flag:

```
kubectl rollout history deployment nginx --revision=1
```

```
kubectl set image deployment/<deployment name> <container name>=nginx:1.17 --record
``` 

This enables the change cause under `kubectl rollout history deployment` command.

There are 2 deployment strategies.

1. Delete all the replicas and replace with new ones (this one incurs production down time) - Recreate strategy
2. Upgrade replicas one by one - Rolling Update Strategy (this is the default one)

For updating the image, we can do this in commandline using the `set image` as below.

```
kubectl set image deployment <deployment name> nginx=nginx:1.9.1
```

The problem is, this approach does not update the deployment definition file. So, when you use the same definition file
for future updates you need to pay attention to the updated image

In recreate strategy if we use `kubectl describe deployment my-deployment` command, you will see in the events section
the replicasets have first been scaled down to zero and then scaled up. In the rolling update strategy, you will see the
replicaSets have been scaled down and up one by one.

When using deployments, kubernetes creates new replicaset per update. Then it will start deploying new pods one by one
in the new replicaSet while deleting the pods one by one in the old replicaset. This old replicaset will not be deleted
even it has zero pods.

To undo a rollout deployment, we can use

```
kubectl rollout undo deployment <deployment name>
```

When we do a rollout upgrade, say we are in our 3rd revision now and we need to rollback to the 2nd revision. So, if we
undo the rollout it will go to the 2nd revision. However, when we execute `kubectl rollout history` command after the
rollback, we will not see the 2nd revision. Instead of that there will be another revision with number 4 whose change
cause is set to that of 2nd one.

---
[Back](index.md)