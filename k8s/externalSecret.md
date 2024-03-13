# External Secrets

https://external-secrets.io/

Set AWS access creds:

```shell
kubectl create secret generic awssm-secret --from-file=./access-key --from-file=./secret-access-key -n $NAMESPACE
```

```shell 
AWS_REGION=<replace>
CLUSTER_SECRET_STORE_NAME=<replace>
EXTERNAL_SECRET_NAME=<replace>
NAMESPACE=<replace>
REMOTE_SECRET_KEY=<replace>
REMOTE_SECRET_PROPERTY=<replace>
SECRET_STORE_NAME=<replace>
TARGET_K8S_SECRET_KEY=<replace>
TARGET_K8S_SECRET_NAME=<replace>
```

[Secret store example](https://external-secrets.io/latest/api/secretstore/)
The SecretStore is **namespaced** and specifies how to access the external API. The SecretStore maps to exactly one
instance of an external API.

```yaml
apiVersion: external-secrets.io/v1beta1
kind: SecretStore
metadata:
  name: $SECRET_STORE_NAME
  namespace: $NAMESPACE
spec:
  provider:
    aws:
      service: SecretsManager
      region: $AWS_REGION
      auth:
        secretRef:
          accessKeyIDSecretRef:
            name: awssm-secret
            key: access-key
#            namespace: $NAMESPACE # not needed as it must be in the same namespace as the `SecretStore`
          secretAccessKeySecretRef:
            name: awssm-secret
            key: secret-access-key
#            namespace: $NAMESPACE # not needed as it must be in the same namespace as the `SecretStore`
```

[External secret](https://external-secrets.io/latest/api/externalsecret/)
The `ExternalSecret` describes what data should be fetched, how the data should be transformed and saved as
a `kind: Secret`

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: $EXTERNAL_SECRET_NAME
  namespace: $NAMESPACE
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: $SECRET_STORE_NAME
    kind: SecretStore
  target:
    name: $TARGET_K8S_SECRET_NAME
    creationPolicy: Owner
  data:
    - secretKey: $TARGET_K8S_SECRET_KEY
      remoteRef:
        key: $REMOTE_SECRET_KEY
        property: $REMOTE_SECRET_PROPERTY
#        decodingStrategy: Base64 # optional
```

[Cluster secret store](https://external-secrets.io/latest/api/clustersecretstore/)
The `ClusterSecretStore` is a cluster scoped SecretStore that can be referenced by all `ExternalSecrets` from all
namespaces. Use it to offer a central gateway to your secret backend.

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ClusterSecretStore
metadata:
  name: $CLUSTER_SECRET_STORE_NAME
  namespace: $NAMESPACE
spec:
  provider:
    aws:
      service: SecretsManager
      region: $AWS_REGION
      auth:
        secretRef:
          accessKeyIDSecretRef:
            name: awssm-secret
            key: access-key
            namespace: $NAMESPACE
          secretAccessKeySecretRef:
            name: awssm-secret
            key: secret-access-key
            namespace: $NAMESPACE
```

[External secret](https://external-secrets.io/latest/api/externalsecret/) using `ClusterSecretStore` as origin

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: amp-external-secret
  namespace: amp # add-on namespace
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: $CLUSTER_SECRET_STORE_NAME
    kind: ClusterSecretStore
  target:
    name: $TARGET_K8S_SECRET_NAME
    creationPolicy: Owner
  data:
    - secretKey: $TARGET_K8S_SECRET_KEY
      remoteRef:
        key: $REMOTE_SECRET_KEY
        property: $REMOTE_SECRET_PROPERTY
#        decodingStrategy: Base64 # optional
```

## Tricks:

### Trigger an update

From the FAQ

```shell
kubectl annotate externalsecret $EXTERNAL_SECRET_NAME force-sync=$(date +%s) --overwrite -n $NAMESPACE
```
