# ECR

## Helm login

### Public

```shell
aws ecr-public get-login-password \
  --region us-east-1 | helm registry login \
  --username AWS \
  --password-stdin public.ecr.aws
```

### Private

```shell
aws ecr get-login-password \
  --region $AWS_REGION | helm registry login \
  --username AWS \
  --password-stdin \
  ${ECR_HELM_REPOSITORY%%/*}
```

## Docker login

### Public

```shell
aws ecr-public get-login-password \
  --region us-east-1 | docker login \
  --username AWS \
  --password-stdin public.ecr.aws
```

### Private

```shell
aws ecr get-login-password \
  --region $AWS_REGION | docker login \
  --username AWS \
  --password-stdin \
  ${ECR_REPO%%/*}
```

## Push image to ECR

```shell
docker tag $DOCKER_IMAGE_ID ${ECR_REPO}/${IMAGE_NAME}:${IMAGE_NAME}
docker push $ECR_REPO/$IMAGE_NAME:${IMAGE_NAME}
```

## Push helm charts

```shell
helm push ${CHAR_NAME}-${CHAR_VERSION}.tgz oci://${ECR_REPO}
```
