## Docker Containers
container runs as long as the process inside it runs. Once the process stops, the container exits. Conatiners are meant to run some processes inside them instead of hosting operating systems.

When specifying the CMD argument in a json array format, the first element should be the executable as follows.

ex: `CMD ["sleep", "5"]`

```
FROM Ubuntu
ENTRYPOINT ["sleep"]
```
here, we can specify the number of seconds to sleep as follows when the image is run.

`docker build -t ubuntu-sleeper .`
`docker run ubuntu-sleeper 10` (This will sleep for 10s)

So, basically in the CMD, whatever we specify when docker run is executed will replace whatever in the CMD. In the entrypoint, whatever we specify will be appended to the entrypoint when run.

We can specify default value for entrypoint by combining the entrypoint and cmd together.

```
FROM Ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
```

In the above example, the `sleep 5` will be executed by default if we didn't specify an argument.
We can also override the docker entrypoint at run time using the entrypoint argument.


`docker run --entrypoint <any-command> <arguments>`

In kubernetes, we can override the entrypoint and command fields with command and args respectively as follows.

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: ubuntu-sleeper
    command: ["sleep2"]
    args: ["5"]
```

We cannot edit all the fields of a running pod. We can only change container image, init container image, spec.activeDeadlineSeconds, spec.tolerations. We can't edit other fields. However, if we tried to edit those forbidden fileds, the new edited yaml will be saved to a temporary location where we can later create our pod from.

However, we can edit any field of our deployment. Then the deployment will delete existing pod and creates new pods for each change we introduce.

```
env:
  - name: APP_COLOR
    value: pink
  - name: APP2_COLOR
    valueFrom:
      configMapKeyRef:
        name: cm-name
        key: APP_COLOR
  - name: APP3_COLOR
    valueFrom:
      secretKeyRef:

envFrom:
- configMapRef:
    name: my-cmap

volumes:
- name: app-config-volume
  configMap:
    name: app-config
```

`kubectl create configmap <cmap name> --from-literal=FOO=bar --from-literal=X=y`
`kubectl create configmap <cmap-name> --from-file=<path to file>`

## Configmap kind

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-cm
data:
  FOO: bar
```

[<prev](basics-part2.md)
