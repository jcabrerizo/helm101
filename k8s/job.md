# Job

https://kubernetes.io/docs/concepts/workloads/controllers/job/

The use of Jobs and CronJobs can further assist with implementing decoupled and transient microservices.

There are three parameters we can use to affect how the job runs:
* backoffLimit
* completions
* parallelism

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: google-ping-every-two-min
spec:
  schedule: "*/2 * * * *"
  successfulJobsHistoryLimit: 7
  concurrencyPolicy: Forbid
  failedJobsHistoryLimit: 7
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: google-ping
            image: busybox
            command:
            - /bin/sh
            - -c
            - curl google.com 
          restartPolicy: OnFailure
```

If `activeDeadlineSeconds` is reached, details of the job shows it

```yaml
..
status:
  conditions:
  - lastProbeTime: "2024-03-07T07:28:35Z"
    lastTransitionTime: "2024-03-07T07:28:35Z"
    message: Job was active longer than specified deadline
    reason: DeadlineExceeded
    status: "True"
...
```