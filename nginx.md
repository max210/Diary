* 启动 sudo nginx -s reload
* 停止 sudo nginx -s stop
* 检查 nginx -t 检查配置 nginx -t -c /etc/nginx/nginx.conf

开启gzip /etc/nginx/conf/nginx.conf
```
gzip on;
gzip_min_length  1k;
gzip_buffers     4 8k;
gzip_http_version 1.1;
gzip_types text/plain application/javascript application/x-javascript text/javascript text/xml text/css;
```
