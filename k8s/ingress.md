# Ingress resources

List
```shell
kubectl get ingress 
```

Test nginx cafe demo
```shell
export IC_HTTPS_PORT=443
export IC_IP=127.0.0.1
curl --resolve cafe.example.com:$IC_HTTPS_PORT:$IC_IP https://cafe.example.com:$IC_HTTPS_PORT/coffee --insecure
```

Test web ingress https
```shell
export IC_IP=127.0.0.1
export IC_HTTPS_PORT=443
export IC_HTTP_PORT=80
curl --resolve www.example.com:$IC_HTTPS_PORT:$IC_IP https://www.example.com:$IC_HTTPS_PORT/app --insecure
curl --resolve www.example.com:$IC_HTTP_PORT:$IC_IP http://www.example.com:$IC_HTTP_PORT/app
```
