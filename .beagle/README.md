# osrg/gobgp

## repo

```bash
git remote add upstream git@github.com:osrg/gobgp.git

git fetch upstream

git merge v2.33.0
```

## build

```bash
# devops-go-arch
docker run -it --rm \
-w /go/src/github.com/osrg/gobgp/ \
-v $PWD/:/go/src/github.com/osrg/gobgp/ \
-v $PWD/_obj/:/go/src/github.com/osrg/gobgp/dist/ \
-e CI_WORKSPACE=/go/src/github.com/osrg/gobgp \
-e PLUGIN_BINARY=gobgp \
-e PLUGIN_MAIN=cmd/gobgp \
registry.cn-qingdao.aliyuncs.com/wod/devops-go-arch:1.19-alpine
```

## cache

```bash
# 构建缓存-->推送缓存至服务器
docker run -it --rm \
  -e PLUGIN_REBUILD=true \
  -e PLUGIN_ENDPOINT=$PLUGIN_ENDPOINT \
  -e PLUGIN_ACCESS_KEY=$PLUGIN_ACCESS_KEY \
  -e PLUGIN_SECRET_KEY=$PLUGIN_SECRET_KEY \
  -e DRONE_REPO_OWNER="open-beagle" \
  -e DRONE_REPO_NAME="gobgp" \
  -e PLUGIN_MOUNT=".git,./vendor" \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  registry.cn-qingdao.aliyuncs.com/wod/devops-s3-cache:1.0

# 读取缓存-->将缓存从服务器拉取到本地
docker run -it --rm \
  -e PLUGIN_RESTORE=true \
  -e PLUGIN_ENDPOINT=$PLUGIN_ENDPOINT \
  -e PLUGIN_ACCESS_KEY=$PLUGIN_ACCESS_KEY \
  -e PLUGIN_SECRET_KEY=$PLUGIN_SECRET_KEY \
  -e DRONE_REPO_OWNER="open-beagle" \
  -e DRONE_REPO_NAME="gobgp" \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  registry.cn-qingdao.aliyuncs.com/wod/devops-s3-cache:1.0
```
