# EKS add-ons

Some helm features are not supported within EKS
all helm commands here are currently ran with helm version 3.8.1
* All Capabilities objects are supported, with an exception for .APIVersions
* * i) .APIVersions is not supported for non-built in custom k8 APIs
* All Release objects (except .Name and .Namespace) are not supported
*  Helm hooks and the Lookup function are not supported
* All dependent charts must be located within the main helm chart (specified with repository path file://...)
* Helm chart must successfully pass Helm Lint and Helm Template with no errors

```shell
export CHART_PATH=<CHART PATH>

export CHART_K8N_VERSION=1.27
export CHART_NAMESPACE=test-template
export CHART_NAME=$(helm show chart $CHART_PATH |yq .name)
export CHART_VERSION=$(helm show chart $CHART_PATH |yq .version)

helm package $CHART_PATH

helm lint ${CHART_NAME}-${CHART_VERSION}.tgz

helm template ${CHART_NAME}-${CHART_VERSION}.tgz \
  --set k8version=Kubernetes-version \
  --kube-version ${CHART_K8N_VERSION} \
  --namespace ${CHART_NAMESPACE} \
  --no-hooks \
  --include-crds >| ${CHART_NAME}-${CHART_VERSION}.template.yaml
  
```

Create manifest interacting with the cluster
```shell
export CHART_RELEASE_NAME=test-release
helm install \
  --dry-run \
  --debug \
  --create-namespace \
  --namespace $CHART_NAMESPACE \
  $CHART_RELEASE_NAME $CHART_PATH >|  ${CHART_NAME}-${CHART_VERSION}.dryrun.yaml
```
