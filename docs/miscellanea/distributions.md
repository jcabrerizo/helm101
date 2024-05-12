# Kubernetes distributions

## Docker desktop

[https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)

## MicroK8s

[https://microk8s.io/](https://microk8s.io/)

### Install MicroK8s

```shell
sudo snap install microk8s --classic --channel=1.28
```

Needing to run `newgrp microk8s` for load the rights

### Config `kubeclt`

```shell
microk8s config > ~/.kube/config
```
