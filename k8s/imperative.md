# Imperative commands

## Namespaces

### Create name space
```shell
kubectl create namespace $NAMESPACE
```

### Delete name space
It deletes the objects on the namespace too
```shell
kubectl delete namespace $NAMESPACE
```

## Pods

### Create pod
```shell
kubectl run $NAME --image $IMAGE:$TAG --port $PORT --namespace $NAMESPACE 
```

### From file:
```shell
kubectl create -f $MANIFEST_PATH
```

Notice the use of `create` instead of `run` and the absence of pod name as it's provided in the manifest

## Use run for creating a `yaml` manifest
Use `-o yaml` and `--dry-run=client` 
```shell
kubectl run $NAME --image=$IMAGE -o yaml --dry-run=client > $FILENAME.yaml
```

# Executing commands in Containers

## Temporal pods for running a command
```shell
kubectl run busybox --image=busybox --restart=Never --rm -it -n $NAMESPACE \
    [-- $COMMAND] 
```

- `--rm`: Deletes the pod after exit
- `-it`: Keeps `stdin` open and allocate TTY for the container

If no command is provided, it opens an interactive shell.

A common scenario is to make `wget` request:
```shell
kubectl run $NODE_NAME --image=busybox -it --rm --restart=Never -- wget -O- $TARGET_ENDPOINT
```


## Run command in a existing pod 
kubectl exec -it $POD_NAME -- /bin/sh