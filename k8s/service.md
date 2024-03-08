# Services

## Exposure

Expose a resource as a new Kubernetes service.

Having labels in the pods is required to use `expose`

```shell
kubectl expose -f $FILENAME
```

```shell
kubectl expose deployment/$DEPLOYMENT --port=$PORT --type=[NodePort | ClusterIp | LoadBalancer | ExternalName]
kubectl get svc
# NAME         CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE 
# kubernetes   10.0.0.1     <none>        443/TCP        18h
# nginx        10.0.0.112   <none>        80:31230/TCP   5s
```

## Ingress Controller

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /path
        pathType: Prefix
        backend:
          service:
            name: $SERVICE_NAME
            port:
              number: 80
```

## Port configuration

* `port` exposes the Kubernetes service on the specified port within the cluster. Other pods within the cluster can communicate with this server on the specified port.
* `targetPort` is the port on which the service will send requests to, that your **pod will be listening on**. Your application in the container will need to be listening on this port also.
* `nodePort` **exposes a service externally to the cluster** by means of the target nodes IP address and the NodePort.

## Service types
### ClusterIP

The `clusterIP` service type is the **default**, and only provides access internally  withing the cluster (except if manually creating an external endpoint). The range of `clusterIP` used is defined via an API server startup option.

The `kubectl proxy` command creates a local service to access a ClusterIP. This can be useful for troubleshooting or development work.

```shell
kubectl proxy
```

### NodePort

The `nodePort` type is great for debugging, or when a static IP address is necessary, such as opening a particular address through a firewall. The NodePort range is defined in the cluster configuration.

The NodePort is accessible via calls to `<NodeIP>:<NodePort>`.

### LoadBalancer

The `loadBalancer` service was created to pass requests to a cloud provider like GKE or AWS. Private cloud solutions also may implement this service type if there is a cloud provider plugin, such as with CloudStack and OpenStack. Even without a cloud provider, the address is made available to public traffic, and packets are spread among the Pods in the deployment automatically.

### ExternalName

A newer service is `externalName`, which is a bit different. It has no selectors, nor does it define ports or endpoints. It allows the return of an alias to an external service. The redirection happens at the DNS level, not via a proxy or forward. This object can be useful for services not yet brought into the Kubernetes cluster. A simple change of the type in the future would redirect traffic to the internal objects. As CoreDNS has become more stable, this service is not used as much.
