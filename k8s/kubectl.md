# `kubectl` command

## Docs:

* https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands
* https://kubernetes.io/docs/reference/kubectl/quick-reference/

## Help

This command describes the fields associated with each supported API resource

```shell
kubectl explain $RESOURCE_NAME
```

Adding the ``--recursive` flag shows the a yaml example with all its fields

## Pod

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

Force delete:

```shell
kubectl delete pod $POD_NAME --grace-period=0 --force
```

## Pod logs

```shell
kubectl logs $POD_NAME
```

To look for errors in the logs of the previous pod that crashed:

```shell
kubectl logs --previous $POD_NAME
```

Logs by container inside a pod

```shell
kubectl logs $POD_NAME -c $CONTAINER_NAME
```

Events

```shell
kubectl get events -n $NAMESPACE
```

The flag `-f` (`--follow`)    Specify if the logs should be streamed.

## Manage context

List existing context

```shell
kubectl config get-contexts
```

Select context

```shell
kubectl config use-context $CONTEXT_NAME
```

Set context namespace

```shell
kubectl config set-context --current --namespace=$NAMESPACE
```

or for other context:

```shell
kubectl config set-context $CONTEXT_NAME --namespace=$NAMESPACE
```

It shows the current one and the default namespace for each one

```
CURRENT   NAME              CLUSTER            AUTHINFO         NAMESPACE
*         docker-desktop    docker-desktop     docker-desktop   $CONTEXT_NAME
          microk8s          microk8s-cluster   admin
```

for unset use `--namespace=''`

List supported API resources and abbreviations on the server

```shell
kubectl api-resources -o wide
```

List resources within a helm release:

```shell
kubectl api-resources --verbs=list -o name | xargs -n 1 kubectl get --show-kind -l app.kubernetes.io/instance=${$RELEASE_NAME} --ignore-not-found -o name
```

## Verbosity

Kubectl verbosity is controlled with the `-v` or `--v` flags followed by an integer representing the log level

* `--v=0`: Generally useful for this to always be visible to a cluster operator.
* `--v=1`: A reasonable default log level if you don't want verbosity.
* `--v=2`: Useful steady state information about the service and important log messages that may correlate to
  significant changes in the system. **Default**
* `--v=3`: Extended information about changes.
* `--v=4`: **Debug** level verbosity.
* `--v=5`: **Trace** level verbosity.
* `--v=6`: Display **requested resources**.
* `--v=7`: Display HTTP request headers.
* `--v=8`: Display HTTP request contents.
* `--v=9`: Display HTTP request contents without truncation of contents.
