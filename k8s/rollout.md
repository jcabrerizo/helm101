# Rollout

It applies to multiple resources, including `deployment`, `daemonset` and `statefulset`

The flag `--dry-run` shows the template to be applied.

## Rollback to the previous deployment

```shell
kubectl rollout undo $NAME
```  

## Show history
```shell
kubectl rollout history deployment $NAME --revision=$REVISION
```
