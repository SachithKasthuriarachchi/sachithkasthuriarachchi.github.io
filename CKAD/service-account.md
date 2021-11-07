# Service Accounts

There are two types of accounts.

- User account which are used by humans
- Service Accounts which are used by machines.

Service account token is what must be used by the applications when authenticating with the kubernetes API. When a
service account is created it first creates the service account object and then creates the service account token which
is stored as a kubernetes secret.

If the 3rd party service is hosted itself on the kubernetes cluster, we just have to mount our service account token as
a volume in the third party application pod.

Service account token will be mounted under `var/run/secrets/kubernetes.io/serviceaccount`. In a pod serviceAccount can
be specified as follows.

```
apiVersion: v1
kind: Pod
metadata:
  name: sa-pod
specs:
  containers:
    - name: container
      image: ubuntu
  serviceAccountName: dashboard-sa
```

The serviceAccountName field of a pod cannot be edited. If you need to edit that, you have to delete the pod and update
the service account. However, we can edit the deployment since it will trigger a new roll out and create the new pods.

If no service account name has been specified, kubernetes will automount the default service account. If a service
account should not be automounted, you need to specify the below config.

```
automountServiceAccountToken: false
```

[<< back](index.md)
