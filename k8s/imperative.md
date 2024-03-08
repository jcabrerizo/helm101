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

## Secrets

```shell
kubectl create secret generic $SECRET_NAME --from-literal=${SECRET_KEY}=${SECRET_VALUE} -n $NAMESPACE
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

# Annotations

## Annotate existing resource
```script
kubectl annotate [--overwrite] (-f $FILENAME | $TYPE $NAME) KEY_1=VAL_1 ... KEY_N=VAL_N [--resource-version=$VERSION]
```

# Labels

```script
kubectl label pod $POD_NAME $LABEL_KEY=u$LABEL_VALUE
```

# Resource selection

Use the flag `-l` / `--selector`

```script
kubectl get $RESOURCE_TYPE -l $LABEL_NAME_1=$LABEL_VALUE -l '$LABEL_NAME_2 in ($VALUE_1, $OR_VALUE_2)' --show-labels
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

Open a interactive shell
```shell
kubectl exec -it $POD_NAME -- /bin/sh
```

Run an isolated commandF
```shell
kubectl exec --stdin --tty $POD_NAME -- $COMMAND  
```

For multi-container pods, the flag `-c $CONTAINER_NAME` can be used for target an specific container within the pod
