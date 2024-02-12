# Imperative commands

## Create name space
```shell
kubectl create namespace $NAMESPACE
```

## Delete name space
It deletes the objects on the namespace too
```shell
kubectl delete namespace $NAMESPACE
```


## Create pod
```shell
kubectl run $NAME --image $IMAGE:$TAG --port $PORT --namespace $NAMESPACE 
```

## Interactive shell
```shell
kubectl run busybox --image=busybox --restart=Never --rm -it -n $NAMESPACE -- /bin/sh 
```

## Temporal pods for running a command
```shell
kubectl run busybox --image=busybox --restart=Never --rm -it -n $NAMESPACE \
    -- bash 
```

## Use run for creating a `yaml` manifest
Use `-o yaml` and `--dry-run=client` 
```shell
kubectl run $NAME --image=$IMAGE -o yaml --dry-run=client > $FILENAME.yaml
```