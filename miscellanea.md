# Miscellanea

## Util commands & scripts

### wget

Redirect response to stdout using flag `-O-`

```shell
wget 10.1.0.15 -O-
# Connecting to 10.1.0.150 (10.1.0.150:80)
# writing to stdout
# <!DOCTYPE html>
# <html>
# <head>
# ...
```

### Download a file with curl

```shell
curl -fsSL -o $OUTPUT_FILE_NAME $URL
```

### Find hooks

```shell
grep -Ril 'helm.sh/hook' | while read -r line ; do
    echo "Hook found in $line"
    sed -n '2p' $line   
done
```

### Execute command in a set of pods

```shell
for name in $(kubectl get pods --selector=app=appName --output name); \
    do kubectl exec $name -- touch /tmp/healthy; done
```

### Copy files to/from the pod

```shell
kubectl cp $POD_NAME:$REMOTE_PATH $LOCAL_PATH
```
