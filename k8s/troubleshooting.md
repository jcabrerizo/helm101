# Troubleshooting

> [!NOTE]\
> Create a pod and ssh in to have access to clusterIP deployments 
> 
> ```shell
> kubectl create deployment troubleshoot --image=busybox
> kubectl exec -ti troubleshoot- -- /bin/sh
> ```

## Is the pod running?

```shell
kubectl describe pods $POD_NAME
kubectl logs $POD_NAME 
```
