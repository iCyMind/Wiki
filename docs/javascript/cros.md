# 跨域方案

## 原生的跨域方案
需要后端设置相应的头部:

- Access-Control-Allow-Origin: 可以是通配符 *, 或者单个域名. 如果是多个域名都要跨域, 那么需要后端做逻辑判断, 返回请求的相应 Origin 即可.
- Access-Control-Allow-Methods: 通常是 POST,GET,OPTIONS
- Access-Control-Allow-Headers: 如果请求包含了一些头部(如 Content-Type), 响应时也要返回这些头部
- Access-Control-Allow-Credentials: 如果请求需要传 Cookie, 响应需要将这个头部设置为 true


