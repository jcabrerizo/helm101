# Job

https://kubernetes.io/docs/concepts/workloads/controllers/job/

The use of Jobs and CronJobs can further assist with implementing decoupled and transient microservices.

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
