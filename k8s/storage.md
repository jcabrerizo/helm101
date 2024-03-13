# Storage

## List storage resources

```shell
kubectl get pv, pvc, sc
```

---

Three modes:

* RWO: Read write once
* ROX read only many
* RWX read write many

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: busybox
spec:
    containers:
    - name: cont_one
      image: busybox
      volumeMounts:
        - mountPath: /alphadir
          name: sharevol
    - name: cont_tow
      image: busybox
      volumeMounts:
        - mountPath: /betadir
          name: sharevol
    volumes:
    - name: sharevol
      emptyDir: {}
```

## emptyDir and hostPath

`emptyDir` and `hostPath` volumes are easy to use. As mentioned, emptyDir **is an empty directory that gets erased when
the Pod dies**, but is recreated when the container restarts.

The `hostPath` volume mounts a resource from the host node filesystem. The resource could be a directory, file socket,
character, or block device. These resources must already exist on the host to be used.

There are two types, `DirectoryOrCreate` and `FileOrCreate`, which create the resources on the host, and use them if
they don't already exist.

## PersistentVolume and PersistentVolumeClaim

A `PersistentVolume` (`PV`) is a storage abstraction used to retain data longer than the Pod using it.

Pods define a volume of type `PersistentVolumeClaim` (`PVC`) with various parameters for size and possibly the type of
backend storage known as its `StorageClass`.

Persistent volumes are _cluster-scoped_, but persistent volume claims are _namespace-scoped_

```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
    name: 10Gpv01
    labels:
      type: local
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/somepath/data01"
```

```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: $CLAIM_NAME
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 8GI
```

In the pod:

```yaml
...
spec:
    volumes:
        - name: test-volume
          persistentVolumeClaim:
                claimName: $CLAIM_NAME
```

## Dynamic provisioning

The StorageClass API resource allows an administrator to define a persistent volume provisioner of a certain type,
passing storage-specific parameters.

With a StorageClass created, a user can request a claim, which the API Server fills via auto-provisioning

```yaml
apiVersion: storage.k8s.io/v1        
kind: StorageClass
metadata:
  name: $SC_NAME
provisioner: kubernetes.io/gce-pd # for Google Compute Engine
parameters:
  type: pd-ssd
```
