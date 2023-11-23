# `kubectl` command

Get all pods
```shell
kubectl get pods -A
```

Describe pod
```shell
kubectl describe pod <pod_name>
```

See pod logs
```shell
kubectl logs <pod_name>
```

##  Manage context

List existing context
```shell
kubectl config get-contexts
```

Select context
```shell
kubectl config use-context <context name>
```