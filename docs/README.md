# Kubernetes, Helm and other container-ish notes

* [AWS: ECR & EKS + `eksctl`](./aws/index.md)
* [containerImages](./containerImages/README)
* [Kubernetes & kubectl](./k8s/index.md)
* [miscellanea](./miscellanea/index.md)
* [tools](./tools/index.md)
* [Helm](./tools/helm.md)

## Training

1. [exercises](./exercices/index.md)
2. [Official Linux Foundation training LFD 259](https://training.linuxfoundation.org/training/kubernetes-for-developers/)
3. [dgkanatsios/CKAD-exercises repo](https://github.com/dgkanatsios/CKAD-exercises/tree/main) ⭐️⭐️⭐️⭐️⭐️

## TODO

- [ ] `yq` notes
- [ ] `jsonpath` notes
- [ ] `docker` and `podman` commands
- [ ] `Dockefile` examples:
  ```dockerfile
  FROM docker.io/httpd:2.4
  RUN echo "Hello, World!" > /usr/local/apache2/htdocs/index.html
  ```
- [x] Add in `helm repo login` commands examples of the repo url, as it's important
- [x] Generate better `README.md` files for the root of each dir.
- [ ] Automate ^
