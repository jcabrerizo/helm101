# Util commands & scripts

# Find hooks
```shell
grep -Ril 'helm.sh/hook' | while read -r line ; do
    echo "Hook found in $line"
    sed -n '2p' $line   
done
```
