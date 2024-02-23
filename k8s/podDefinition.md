# Pod definition

## Simple pod with initializer

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: composed
  name: composed
spec:
  containers:
  - image: nginx
    name: complex-pod
    ports:
    - containerPort: 80
  initContainers:
    - name: initializer 
      image: busybox
      command: ['/bin/sh', '-c', 'wget -O- google.com']
  dnsPolicy: ClusterFirst
  restartPolicy: Never
```

Check the logs:
```shell
POD_NAME=composed
INIT_CONTAINER_NAME=initializer
kubectl logs --previous $POD_NAME
kubectl logs $POD_NAME -c $INIT_CONTAINER_NAME
```

## Probes

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: web-server
  name: web-server
spec:
  containers:
  - image: nginx
    name: web-server
    ports:
    - containerPort: 80
      name: web
    startupProbe:
      httpGet:
        path: /
        port: web
    readinessProbe:
      httpGet:
        path: /
        port: web
      initialDelaySeconds: 5
    livenessProbe:
      httpGet:
        path: /
        port: web
      periodSeconds: 30
      initialDelaySeconds: 10
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

## Deployments

Create deployment and set replica number
```script
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