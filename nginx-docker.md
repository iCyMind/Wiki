# nginx docker

## 获取镜像: docker pull nginx
## 一个静态服务: docker run --name my-nginx -v ~/Projects/88meeting.com/dist:/usr/share/nginx/html:ro -d nginx
## 设置宿主端口: docker run --name my-nginx -v ... -p 8080:80 -d nginx
## 传入配置文件: docker run --name my-nginx -v ... -p ... -v ~/Projects/88meeting.com/dist:/etc/nginx/nginx.conf:ro -d nginx

## 也可以写成配置文件

```yaml
link-demo:
  image: nginx
  container_name: link_demo
  volumes:
    - ~/Projects/link/dist:/usr/share/nginx/link:ro
    - ~/Projects/link/nginx.conf:/etc/nginx/conf.d/link.conf:ro
  ports:
    - 80:80
```


## 按配置文件执行: docker-compose up -d

## 进入 docker debug: docker exec -it web_demo bash
