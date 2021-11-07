# Jobs and CronJobs

When we deploy an application in kubernetes which runs a short term task, it will show the pod is in completed state.
Since the default behaviour of kubernetes is to run an application forever, it will show us number of restarts as well.
This can be configured using the `restartPolicy` in pod.

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: conatiner
     image: my-image
  restartPolicy: Always
```

Possible values for `restartPolicy`:

- Never
- OnFailure

`Jobs` are the manifests which are meant to run only for a short period of time to completion.

```
apiVersion: batch/v1 
kind: Job 
metadata:
  name: math-job 
spec:
  completions: 3 template:
  spec:
    containers:
    - name: math-container 
      image: alpine 
      command:
      - ‘expr’
      - ‘5’
      - ‘2’ 
    restartPolicy: Never

```

The output of the above job can be found by running the `kubectl log` command.

In jobs there is a property called `Completions` which is some way similar to `Replicas` in deployment. The job make
sure it meets the number of successful completions specified in the definition. These jobs run one after the other. To
run them in parallel, we can specify the `parallelism` property, where we denote number of parallel jobs.

```
apiVersion: batch/v1 
kind: Job 
metadata:
  name: math-job 
spec:
  completions: 3 
  parallelism: 3 #here 3 parallel jobs will start initially template:
  spec:
    containers:
    - name: math-container 
      image: alpine 
      command:
      - 'expr'
      - ‘5'
      - ‘2’ 
    restartPolicy: Never

```

CronJob is a scheduled job which contains all of the `spec` section in the above job template

```

apiVersion: batch/v1beta1 
kind: CronJob 
metadata:
  name: mycron 
spec:
  schedule: “*/1 * * * *” 
  jobTemplate:
    spec:
      completions: 3 
      parallelism: 3 #here 3 parallel jobs will start initially 
      template:
        spec:
          containers:
          - name: math-container 
            image: alpine 
            command:
            - ‘expr’
            - ‘5’
            - ‘2’ 
          restartPolicy: Never

```

`kubectl describe jobs` command can be used to determine number of attempts in the job. We can get that by looking at
the `Pod Statuses` field and add up both succeeded and failed attempts.

To educate yourself about jobs you can use,

```
kubectl explain job —recursive | less
```

and then to find something in the output, you can enter `\<whatever-you-want>` and hit enter.

You can also use

```
kubectl create cronjob cron-name —image alpine --schedule “*/1 * * * *” —dry-run=client > out.yaml
```

___
[Service](services.md)

[Back](index.md)