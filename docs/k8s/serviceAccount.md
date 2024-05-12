# Service accounts

## Service account example

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: $SA_NAME
```

## Cluster role

Example that limit access only to get and list `secrets`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: $ROLE_NAME
rules:
  - apiGroups:
    - ""
    resources:
      - secrets
    verbs:
     - get
     - list
```

## Role binding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: $BINDING_NAME
subjects:
  - kind: ServiceAccount
  name: $SA_NAME
roleRef:
  kind: ClusterRole
  name: $ROLE_NAME
  apiGroup: rbac.authorization.k8s.io
```