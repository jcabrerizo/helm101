# Network policy

The pods filtered by the selector will be affected

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: $POLICY_NAME
spec:
  podSelector:
    matchLabels:
      $AFFECTED_LABEL_NAME: $AFFECTED_LABEL_VALUE
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
            $TARGET_LABEL_NAME: $TARGET_LABEL_VALUE
        - ipBlock:
            ...
        - namespaceSelector:
            ...
```

The empty braces will match all Pods not selected by other NetworkPolicy and will not allow ingress traffic. Egress traffic would be unaffected by this policy.

[Default deny all ingress traffic](https://kubernetes.io/docs/concepts/services-networking/network-policies/#default-deny-all-ingress-traffic)
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyTypes:
  - Ingress​
```

Other default policies: https://kubernetes.io/docs/concepts/services-networking/network-policies/#default-policies