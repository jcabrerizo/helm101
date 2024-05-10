# Working with manifests and images

## Inspect manifest
```shell
MANIFEST=nginx
docker manifest inspect $MANIFEST
```

## List architectures for a multi-arch images

This fails if no `architecture` is present in the manifest

```shell
MANIFEST=nginx
docker manifest inspect $MANIFEST | jq 'if .manifests then .manifests | map(select(.platform.architecture != "unknown")) | map(.platform.architecture) else [] end'
```
