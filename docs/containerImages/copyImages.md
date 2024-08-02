# Copy multi-arch image from one registry to another

```shell
ORIGIN_REG=originalRegistry.docker.com
TARGET_REG=targetRegistry.aws.com
TAG_VERSION=latest
TAG_NAME=my-image
TARGET_TAG=$TARGET_REG/$TAG_NAME:$TAG_VERSION
ORIGIN_TAG=$ORIGIN_REG/$TAG_NAME:$TAG_VERSION
AWS_REGION=us-east-1

# Azure login
az login
az acr login --name $TARGET_REG

# AWS login
aws ecr get-login-password \
  --region $AWS_REGION | docker login \
  --username AWS \
  --password-stdin \
  $ORIGIN_REG
  
# login with docker in required registries
docker login -u $ORIGIN_USER -p ORIGIN_PASS $ORIGIN_REG
docker login -u $TARGET_USER -p TARGET_PASS $ORIGIN_REG

docker buildx imagetools create --tag "$ORIGIN_TAG" "$TARGET_TAG" 
```

For testing:

```shell
docker manifest inspect $TARGET_TAG
docker pull $TARGET_TA
docker run -p=8080:8080 $TARGET_TAG
```
