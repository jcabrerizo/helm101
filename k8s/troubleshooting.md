# Troubleshooting

```shell
kubectl create deployment troubleshoot --image=nginx
kubectl exec -ti troubleshoot- -- /bin/sh
```

is the pod running?

```shell
kubectl logs pod-name 
```
