# Notes from killer.sh exam

## Tips:

* Get familiar with the Kubernetes documentation and be able to use the search. Allowed links are:
  * https://kubernetes.io/docs
  * https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#label
  * https://kubernetes.io/blog
  * https://helm.sh/docs
* New interface https://itnext.io/cks-cka-ckad-changed-terminal-to-remote-desktop-157a26c1d5e
* Use Application->Accessories->Mousepad to write down notes for yourself during the exam. 
* Add namespaces to stored scripts or commands
* Scripts cab be executed as parameter of `sh`: `sh /path/script.sh`
* `-h` is faster to type than `--help` 🤫
* If no *name* is required in the command, probably it has a `--name` flag
* `curl` supports a flag to set the max time in seconds `-m 5`
## Questions doubts

Captured during the exam

* **q02**: pod status --> `k get pod...`
* **q03**: job history --> `k describe job` 
* **q05**: sa tokens
* **q08**: rollout
* **q09**: deployment syntax. spec.template.spec....
* **q10**: run curl on one line
  * deployment.namespace should be accessible
* **q11**: podman build
* **q11**: podman terminal
* **q15**: curl deployment --> get the ip using `k get pod -o wide` and curl that ip
* **q18**: find and fix connection problem
* **q19**: check port config and how to connect with
* **q21**: network policies, test dns access
* **q22**: label and annotate command
* **pq1**: study container spec, who owns what, probes, sa

## Main failures

From the detailed score

* **q10**: wrong service name
* **q13**: need to create the storage class
* **q14**: wrong name!
* **q18**: 
* **q19**: 
* **q20**: 

## Detailed correction

### Pods
* `readinessProbe` belong to the container
* `resources` belong to the container
* `securityContext` belong to the container
* `volumeMounts` belong to the container BUT `volumes` and `persistentVolumeClaim` to the **`.specs`**

### Deployments
* Define the number of `replicas`
* Require a selector
* `.spec.template` is the same structure as the pod `.metadata` and `.spec` sections

### Services:
* Redirection `3333:80` means `--port 3333 --target-port 80`
* `kubectl get ep` returns the endpoints
* The service plus the namespace should be DNS resolved: `curl my-service.namespace:$PORT`
* Selectors need to target the pods, not the deployments
* `nodePort` needs to be added when changing from ClusterIP to NodePort type, unless same port is desired 

### Jobs
Key words:
* completions (plural)
* parallelism

### Secrets
* `kubectl describe secret $SECRET` returns the unencoded secret value for some types, as `service-account-token`.
* Check injected secrets in pod:
  * `kubectl exec $POD_NAME -- find /$PATH`
  * `kubectl exec $POD_NAME -- env | grep $ENVVAR_NAME`

### Rollout
* `kubectl rollout history $TYPE $NAME`
* `kubectl undo history $TYPE $NAME`
* `kubectl rollout restart deploy $NAME` force pods to be updated

---

### Helm
* `helm repo update`
* `helm repo list`
* `helm upgrade $RELEASE $IMAGE`
* `helm rollback`
* `helm show values`
* `--set` does not allow `=` before the key to be defined: `helm install $RELEASE $CHART --set valueKey=Value`, instead of `--set=valueKey=Value`

