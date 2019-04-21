# alpine

alpine 是个很小的 linux 发行版, 只有不到 10M 的大小, 很适合做 docker 镜像的 base image.

## 安装软件

```bash
# update list
apk update

# search package
apk search taskd

# install package
apk add taskd

# install multiple package and group them
apk add --virtual .build-deps g++ gcc make cmake

# uninstall package
apk del taskd

# del group
apk del --purge .build-deps

# remove cache
rm -rf /var/cache/apk/*
```
