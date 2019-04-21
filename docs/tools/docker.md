# Docker

### 常用命令

```bash
# 交互模式
docker run -it --rm --name demo -p 8989:8989/tcp -p 8989/udp -e HOST icymind.com -v /my-conf.json:/etc/ss/ss.json image-name command

# 后台模式
docker run -d --restart always --name demo -p 8989:8989/tcp -p 8989/udp -v /my-conf.json:/etc/ss/ss.json image-name command

docker iamges
docker ps
docker ps -a
```
### Dockerfile

```
# Shadowsocks Server/Client with kcptun support.

FROM ubuntu:16.10

ENV KCP_VER 20161102

RUN    apt-get update \
    && apt-get install -y wget supervisor shadowsocks-libev\
    && mkdir -p /opt/kcptun \
    && mkdir -p /etc/kcptun \
    && cd /opt/kcptun \
    && wget https://github.com/xtaci/kcptun/releases/download/v$KCP_VER/kcptun-linux-amd64-$KCP_VER.tar.gz -O kcptun.tar.gz \
    && tar xf kcptun.tar.gz \
    && rm kcptun.tar.gz \
    && rm -rf /var/lib/apt/lists/*

# kcptun-server port
EXPOSE 7979/tcp
EXPOSE 7979/udp

# shadowsocks-server port
EXPOSE 8989/tcp
EXPOSE 8989/udp

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord"]

```

### supervisor

```
[supervisord]
nodaemon=true

[program:kt-server]
command=/opt/kcptun/server_linux_amd64 -l ":7979" -t "127.0.0.1:8989"

[program:ss-server]
command=/usr/bin/ss-server -s "0.0.0.0" -p 8989 -k %(ENV_PASSWORD)s -m %(ENV_METHOD)s -t 300 -u
```
### docker-compose

running with: `docker-compose up` or `docker-compose up -d`
```yaml
ss-kt:
    image: icymind/ss-kt
    restart: always
    ports:
        - "8989:8998/tcp"
        - "8989:8998/udp"
    volumes:
        - /my-conf.json:/etc/ss.json
    links:
        - blog
    environment:
        - HOST=icymind.com
    command: echo "hello world"
```

### iptables within docker
running with --privileged option works ok everytime.  

`docker run -it --rm --privileged mcreations/openwrt-x64 sh `

### save/load images to/from file

You will need to save the docker image as a tar file:

`docker save -o <save image to path> <image name>`

Then copy your image to a new system with regular file transfer tools such as cp or scp. After that you will have to load the image into docker:

`docker load -i <path to image tar file>`

### bugs
docker for mac 有几个严重的 bug, 其一就是对挂载到容器的数据卷的读写性能很差, 对此 [论坛](https://forums.docker.com/t/file-access-in-mounted-volumes-extremely-slow-cpu-bound/8076) 和 [github](https://github.com/docker/for-mac/issues/77) 上都有讨论.
因为我在本地搭载了一个基于 gollum 的 wiki, 所以这个 bug 对我影响很大. 如果 gollum 运行在局域网的 linux 主机上, 打开速度基本都是1秒以内, 运行在 docker for mac  上时, 主页需要5秒才加载完.

暂时的解决方法: [d4m-nfs](https://github.com/IFSight/d4m-nfs). 按照官网的说法:

1. 在 docker-for-mac 应用中删除 tmp 之外的所有文件共享
2. 运行 d4m-nfs.sh
3. 修改 docker-compose.yaml 或者命令行中的数据卷路径. 比如之前是 `-v /Users/simon/Dropbox/Wiki:/wiki` 的, 改为 `-v /mnt/Dropbox/Wiki:/wiki`. 总之 $HOME 路径替换为 /mnt 就对了
4. 运行容器
