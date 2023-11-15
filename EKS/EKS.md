# EKS

# `eksclt`
https://eksctl.io/

https://eksctl.io/getting-started/

## Commands

> **WARNING**
> 
> The `eksctl create cluster` execution will overwrite the `~/.kube/config` file to configure access

Create default cluster with:
- exciting auto-generated name
- two m5.large worker nodes
- use the official AWS EKS AMI
- us-west-2 region
- a dedicated VPC (check your quotas)

```shell
eksctl create cluster
```

For forzing a spedific version on k8s:
```shell
eksctl create cluster --version=1.24
```

Or for changing default config a config file can be used:
> **TODO**: getting an error using this file, I think related with permissions:
> ```shell
> Error: getting availability zones: only 0 zones discovered [], at least 2 are required
> ```

[medium2nodesCluster.yaml](./medium2nodesCluster.yaml):
```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: basic-cluster
  region: eu-west-1

nodeGroups:
  - name: ng-1
    instanceType: m5.medium
    desiredCapacity: 2
```

````shell
eksctl create cluster -f medium2nodesCluster.yaml
````

