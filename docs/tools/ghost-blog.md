# Ghost Blog

## Ghost Docker

- 官方的docker images 包含了最新的 ghost 版本，通过 `docker pull ghost` 即可获取。
- 请先将 blog 的内容放到某个文件夹如 `/home/simon/blog/` 内，目录结构为：

    ```
    ├── apps
    ├── config.js
    ├── data
    ├── images
    └── themes
    ```

- 将 blog 文件夹内的 config.js 先移除，待会我们让 ghost 自己生成一个
- 以 dev 模式运行，让 ghost docker images 里的脚本生成一些文件：`docker run --name blog -v /home/simon/blog:/var/lib/ghost -p 2368:2368 -t --rm ghost`
- blog 文件夹内生成了一个 config.js 文件，修改 production 部分，在 server 部分后添加

    ```javascript
    paths: {
        contentPath: path.join(process.env.GHOST_CONTENT, '/')
    }
    ```

- 最后以 production 模式运行容器即可：`docker run --name blog -v /home/simon/blog:/var/lib/ghost -p 2368:2368 -d ghost`

## nginx

- 首先获取 nginx 官方的 docker 镜像到本地：`docker pull nginx`
- 运行容器即可使用 nginx ：`docker run -d --name nginx -p 80:80 nginx`, 在浏览器中打开 http://server-ip 即可验证
- 为了 nginx 能够反向代理 ghost ，添加必要的配置文件如 icymind.com.conf:

    ```
    server {
        listen  80;
        server_name icymind.com www.icymind.com;
        access_log /var/log/nginx/icymind.com.log;

        location / {
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Host $http_host;
            # blog 为 ghost 容器的别称，在 docker run nginx 的 --link 参数中指定
            proxy_pass http://blog:2368;
        }
    }
    ```

- 运行 nginx 容器：

    ```bash
    docker run -it --rm -p 80:80 \
        -v /path/to/icymind.com.conf:/etc/nginx/conf.d/icymind.com.conf \
        --link ghost:blog \
        nginx bash
    ```

    启动 nginx 服务即可进行反代： `service nginx start`

- 以交互方式测试成功后，即可后台运行容器：

    ```bash
    docker run --name nginx -d -p 80:80 \
        -v /path/to/icymind.com.conf:/etc/nginx/conf.d/icymind.com.conf \
        --link ghost:blog nginx
    ```

- 如果 conf.d 里有多个配置，添加文件夹更优雅：

    ```bash
    mkdir -p ~/nginx/{conf.d, certs, logs}
    cp ~/icymind.com.conf ~/nginx/conf.d/
    docker run --name nginx -d -p 80:80 \
        -v /home/simon/nginx/logs:/var/log/nginx \
        -v /home/simon/nginx/certs:/etc/nginx/certs \
        -v /home/simon/nginx/conf.d:/etc/nginx/conf.d \
        --link ghost:blog nginx
    ```

## SSL

为ghost博客添加ssl支持，可以用https-portal镜像一步到位。
- 首先安装docker-compose
- 新建一个文件夹如 https-ghost , 编写 docker-compose.yml :

    ```yaml
    https-portal:
      image: steveltn/https-portal
      restart: always
      ports:
        - '80:80'
        - '443:443'
      links:
        - ghost-blog
      environment:
        DOMAINS: 'www.icymind.com -> http://ghost-blog:2368, icymind.com -> http://ghost-blog:2368'
        STAGE: 'production'
    ghost-blog:
      image: ghost
      restart: always
      expose:
        - '2368'
      volumes:
        - /home/simon/blog:/var/lib/ghost
      environment:
        NODE_ENV: 'production'
    ```

- 观察执行情况：`docker-compose up`
- 后台运行: `docker-compose up -d`

## code highlight

- 下载 [prismjs](http://prismjs.com/download.html)
- 按说明使用

## 参考

- [docker-library](https://github.com/docker-library/ghost/issues/27)
- [carloscheddar](https://carloscheddar.com/)
