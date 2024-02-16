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
