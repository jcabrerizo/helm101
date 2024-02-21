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
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  initContainers:
    - name: initializer 
      image: busybox
      command: ['/bin/sh', '-c', 'wget -O- google.com']
```

Check the logs:
```shell
POD_NAME=composed
INIT_CONTAINER_NAME=initializer
kubectl logs --previous $POD_NAME
kubectl logs $POD_NAME -c $INIT_CONTAINER_NAME
```