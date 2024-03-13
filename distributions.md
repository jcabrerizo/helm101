# Kubernetes distributions

## Docker desktop

## MicroK8s

https://microk8s.io/

### Install MicroK8s

```shell
sudo snap install microk8s --classic --channel=1.28
```

Needing to run `newgrp microk8s` for load the rights

### Config `kubeclt`

```shell
microk8s config > ~/.kube/config
```
