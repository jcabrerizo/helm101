# Helm

Helm learning notes and POCs

> **TODO**
>
> - [x] What is `appVersion`:  Version of the deployed application using helm, eg: WordPress, independent of the helm
    version. [Docs](https://helm.sh/docs/topics/charts/#the-appversion-field)
> - [x] What is `apiVersions`: [Docs](https://helm.sh/docs/chart_template_guide/builtin_objects/)
    >
    >   Capabilities: This provides information about what capabilities the Kubernetes cluster supports.
    >
    >   Capabilities.APIVersions is a set of versions.
    >
    >   Capabilities.APIVersions.Has $version indicates whether a version (e.g., batch/v1) or resource (e.g.,
    apps/v1/Deployment) is available on the cluster.
> - [ ] Alternatives to `lookup`. [Docs](https://helm.sh/docs/chart_template_guide/function_list/#lookup)

## Links

* Play ground: https://helm-playground.com/
* [Template function list](https://helm.sh/docs/chart_template_guide/function_list/)

## Resources

https://www.youtube.com/watch?v=DQk8HOVlumI&t=741s

## Setup environment

### Install Helm

https://helm.sh/docs/intro/install/
https://github.com/helm/helm/releases/tag/v3.8.1

### Install `kubectl`

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

## Helm 101

### Create the charm

```shell
helm create helloworld
```

```shell
└── helloworld
 ├── charts
 ├── Chart.yaml
 ├── templates
 │   ├── deployment.yaml
 │   ├── _helpers.tpl
 │   ├── hpa.yaml
 │   ├── ingress.yaml
 │   ├── NOTES.txt
 │   ├── serviceaccount.yaml
 │   ├── service.yaml
 │   └── tests
 │       └── test-connection.yaml
 └── values.yaml
```

### Create release

```shell
helm install releaseName helloworld
```

```shell
helm list -a
```

```shell
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/juan/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/juan/.kube/config
NAME    NAMESPACE    REVISION    UPDATED                              STATUS   CHART        APP VERSION
helloworld    default   1     2023-11-12 18:17:39.662551547 +0000 UTC    deployed    helloworld-0.1.01.16.0
```

```shell
kubectl get service
```

```shell
NAME      TYPE     CLUSTER-IP    EXTERNAL-IP   PORT(S)     AGE
kubernetes   ClusterIP   10.152.183.1  <none>     443/TCP     69m
helloworld   NodePort 10.152.183.123   <none>     80:30325/TCP   11m
```

Validate deployment, notice the port

```shell
curl http://localhost:30325
```

## Helm commands

List all

```shell
helm list -a
```

or only with `kubctl`

```shell
kubectl api-resources --verbs=list -o name | xargs -n 1 kubectl get --show-kind -l app.kubernetes.io/instance=$RELEASE_NAME --ignore-not-found -o name
```

Uninstall release

```shell
helm uninstall $RELEASE_NAME
```

Get release notes of a

```shell
helm get notes $RELEASE_NAME
```

Upgrade release, after modifying the chart code

```shell
helm upgrade example $HELM_DIR
helm upgrade myrelease repo/foo --set=image.tag=1.2.2
```

List release story

```shell
helm history $RELEASE_NAME
```

### Validating helm chart

Interaction with the actual k8s cluster

```shell
helm install releaseName –-debug -–dry-run $HELM_DIR
```

Validating only the YAMLs

```shell
helm template
```

Run linter

```shell
helm lint
```

Show helm environment variables

```shell
helm env
```
