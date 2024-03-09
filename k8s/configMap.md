# ConfigMap

They must exist before the pods unless are optional

Can be injected as environment var or volume

```yaml
...
env:
- name: $ENV_VAR_NAME
  valueFrom:
    configMapKeyRef:
      name: $CONFIG_MAP_NAME
      key: $CONFIG_MAP_KEY

---
...
volumes:
  - name: $VOLUME_NAME
    configMap:
      name: $CONFIG_MAP_NAME

```