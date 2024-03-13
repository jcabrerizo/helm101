# Secret

By default, secret are `base64` encoded, but real encryption can be configured.

```shell
kubectl create secret generic $SECRET_NAME --from-literal=$SECRET_KEY=$SECRET_VALUE

kubectl get secrets
# NAME          TYPE     DATA   AGE
# test          Opaque   1      29m
# $SECRET_NAME  Opaque   1      30m

kubectl get secret $SECRET_NAME
# apiVersion: v1
# kind: Secret
# metadata:
#   name: $SECRET_NAME
# data:
#   $SECRET_KEY: $SECRET_VALUE
```

> [!NOTE]\
> **Example** notice the secret value is passed _plain_, but stored as `base64`
>
> ```shell
> kubectl create secret generic test --from-literal=secretName=secretValue
> # kubectl get secret test -o=yaml 
> # apiVersion: v1
> # data:
> #   secretName: c2VjcmV0VmFsdWU=
> # kind: Secret
> # metadata:
> #   creationTimestamp: "2024-03-09T11:35:02Z"
> #   name: test
> #   namespace: default
> #   resourceVersion: "122029"
> #   uid: c40ae44d-fc1f-4668-a451-f51628f1a187
> # type: Opaque
> 
> secret=$(kubectl get secret test -o=jsonpath='{.data.secretName}')  
> # $secret=c2VjcmV0VmFsdWU= 
> 
> base64 -d <<< $secret
> # secretValue%
> ```

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
