# nginx

## 同域名下部署不同的项目

要达到的目标: 访问 ir.88meeting.com, 导向 landing-page 项目; 访问 ir.88meeting.com/app 访问 link 项目

```
server {
        listen  443 ssl;
        server_name  ir.88meeting-qa.com;
        ssl_certificate /root/nginx/88meeting_qa_com/ir_88meeing_qa_com.pem;
        ssl_certificate_key /root/nginx/88meeting_qa_com/ir_88meeing_qa_com.key;

        # desktop
        location /desktop {
                alias /var/www/ir-site/desktop/;
                autoindex on;
        }

        # app
        location /app {
                alias /var/www/ir-site/app/dist/;
                index index.html;
                try_files $uri index.html =404;

                location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
                        expires 1y;
                        access_log off;
                        add_header Cache-Control "public";
                }

                location ~* \.(?:css|js)$ {
                        expires 1y;
                        add_header Cache-Control "public";
                }
        }

        # site
        location / {
                alias /var/www/ir-site/site/dist/;
                index index.html;
                try_files $uri index.html =404;

                location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
                        expires 1y;
                        access_log off;
                        add_header Cache-Control "public";
                }

                location ~* \.(?:css|js)$ {
                        expires 1y;
                        add_header Cache-Control "public";
                }
        }
}
```
