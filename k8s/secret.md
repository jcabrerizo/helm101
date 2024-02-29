# Secret

By default, secret are `base64` encoded, but real encryption can be configured.

```shell
SECRET_VALUE=$(echo $VALUE|base64)
kubectl create secret generic $SECRET_NAME --from-literal=$SECRET_KEY=$SECRET_VALUE

kubectl get secrets
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: $SECRET_NAME
data:
  $SECRET_KEY: $SECRET_VALUE
```

Secrets can be injected from environment variables into a container:

```yaml
...
spec:
    containers:
    - image: busybox
      name: busybox
      env:
      - name: $SECRET_NAME # Env var nae
        valueFrom:
          secretKeyRef:
            name: $SECRET_NAME
            key: $SECRET_KEY
```

Or mounted as volumes

```yaml
spec:
    containers:
    - image: busybox
      name: busybox
      command:
        - sleep
        - "3600"
      volumeMounts:
      - mountPath: $VOLUME_PATH
        name: $VOLUME_NAME
    volumes:
    - name: $VOLUME_NAME
      secret:
        secretName: $SECRET_NAME
```

```shell
kubectl exec -ty busybox -- cat $VOLUME_PATH/$SECRET_KEY
# $SECRET_VALUE
```