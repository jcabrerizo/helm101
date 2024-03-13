# Useful alias

```shell
alias k='kubectl'
alias kkill='kubectl delete --grace-period=0 --force'
alias kn='kubectl config set-context --current --namespace '
```

## Partial commands:

```shell
export do="--dry-run=client -o yaml"
export now="--force --grace-period 0"

kubectl run $NAME --image=$IMAGE $do
kubectl delete pod $NAME $now
```
