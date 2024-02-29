# Deployments

Create deployment and set replica number

```shell
kubectl create deployment $DEPLOYMENT_NAME --image=$IMAGE
kubectl scale deployment $DEPLOYMENT_NAME --replicas=2
```

Update image
```shell
kubectl set image deployment $DEPLOYMENT_NAME $CONTAINER_NAME=$IMAGE
```

```shell
kubectl rollout history deployments $DEPLOYMENT_NAME
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: $DEPLOYMENT_NAME
spec:
  replicas: 2
  selector:
    matchLabels:
      $LABEL: $LABEL_VAL
  template:
    metadata:
      labels:
        app: $LABEL_VAL
    spec:
      containers:
      - name: server
        image: $IMAGE
```

## Scaling deployments

Scale also allows users to specify one or more preconditions for the scale action. If `–current-replicas` or `–resource-version` is specified, it is validated before the scale is attempted

```shell
kubectl scale $DEPLOYMENT_NAME --replicas=4
```

For other values the deployment can be edited using a text editor:

```shell
kubectl edit deployment $DEPLOYMENT_NAME
```

## Rollbacks

A deployment change is stored and it can be rolled back

```shell
kubectl rollout status $DEPLOYMENT_NAME
kubectl rollout restart deployment/abc
kubectl rollout restart deployment --selector=$SELECTOR_KEY=$SELECTOR_VALUE
kubectl rollout history $DEPLOYMENT_NAME
kubectl rollout undo $DEPLOYMENT_NAME
```