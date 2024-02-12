# `kubectl` command

Get all pods
```shell
kubectl get pods -A
```

Describe pod
```shell
kubectl describe pod $POD_NAME
```

Delete pod
```shell
kubectl delete pod $POD_NAME
```
or by referencing the manifest
```shell
kubectl delete pod -f $MANIFEST.yaml
```



See pod logs
```shell
kubectl logs $POD_NAME
```

To look for errors in the logs of the previous pod that crashed:
```shell
kubectl logs --previous YOUR-POD_NAME
```

##  Manage context

List existing context
```shell
kubectl config get-contexts
```

Select context
```shell
kubectl config use-context $CONTEXT_NAME
```

List supported API resources and abbreviations on the server
```shell
kubectl api-resources -o wide
```

List resources within a helm release:
```shell
kubectl api-resources --verbs=list -o name | xargs -n 1 kubectl get --show-kind -l app.kubernetes.io/instance=${$RELEASE_NAME} --ignore-not-found -o name
```
