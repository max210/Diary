- 参考文章
http://seanlook.com/2015/05/17/nginx-install-and-config/
https://www.zybuluo.com/phper/note/89391

- 安装 apt-get install nginx
- 主配置目录 /etc/nginx/nginx.conf，其中可以修改`server_token off;` 来隐藏nginx版本号
- 自定义配置目录 /etc/nginx/conf.d/xxx.conf, 需要在/etc/nginx/nginx.conf 中指定 `include /etc/nginx/conf.d/*.conf`
- 启动 sudo nginx -s reload
- 停止 sudo nginx -s stop
- 检查 nginx -t 检查配置 nginx -t -c /etc/nginx/nginx.conf
- /etc/nginx/nginx.conf 主要配置文件；/etc/nginx/conf.d/default.conf 默认配置文件
- 配置（主要分为六个区域：main(全局设置)、events(nginx工作模式)、http(http设置)、sever(主机设置)、location(URL匹配)、upstream(负载均衡服务器设置)）
```
# 用来定一个虚拟主机
server {
  listen       80; # 指定虚拟主机的服务端口
  server_name  192.168.12.10 www.max210.com; # 用来指定IP地址或者域名，多个域名之间用空格分开

  # 负载均衡啊、反向代理、虚拟域名相关(location / 表示匹配访问根目录, 开启正则匹配这样：location ~)
  location / {
    root              /Users/maximilian/www; # 指定访问根目录时，虚拟主机的web目录
    index             index.html; # 设定只输入域名后访问的默认首页地址
    try_files         $uri $uri/ /index.html; # 避免单页面刷新404
  }

  #error_page  404              /404.html;
  #error_page   500 502 503 504  /50x.html;
  #location = /50x.html {
    #root   /usr/share/nginx/html;
  #}

  location /api {
    proxy_pass  http://xxx.xx.xx.xx:3000;
    proxy_set_header  Host $host;
    proxy_set_header  X-Real-IP  $remote_addr;
    proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
    proxy_set_header  X-Nginx-Proxy true;
    proxy_redirect    off;
  }

  location ~* ^.+\.(html|jpg|jpeg|gif|png|ico|css|js|pdf|txt) {
    root   /Users/maximilian/www
  }
}

# https配置
server {
  listen  443 ssl http2;
  server_name  your-site.com www.your-site.com;
  ssl on;
  ssl_certificate cert/xxxx.pem;
  ssl_certificate_key cert/xxxx.key;
  ssl_session_timeout 5m;
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;

  # 负载均衡啊、反向代理、虚拟域名相关(location / 表示匹配访问根目录, 开启正则匹配这样：location ~)
  location / {
    root              /Users/maximilian/www; # 指定访问根目录时，虚拟主机的web目录
    index             index.html; # 设定只输入域名后访问的默认首页地址
    try_files         $uri $uri/ /index.html; # 避免单页面刷新404
  }

  #error_page  404              /404.html;
  #error_page   500 502 503 504  /50x.html;
  #location = /50x.html {
    #root   /usr/share/nginx/html;
  #}

  location /api {
    proxy_pass  http://xxx.xx.xx.xx:3000;
    proxy_set_header  Host $host;
    proxy_set_header  X-Real-IP  $remote_addr;
    proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
    proxy_set_header  X-Nginx-Proxy true;
    proxy_redirect    off;
  }

  location ~* ^.+\.(html|jpg|jpeg|gif|png|ico|css|js|pdf|txt) {
    root   /Users/maximilian/www
  }
}
```

- docker 开启 nginx
docker cp mynginx:/etc/nginx .  表示把mynginx容器的/etc/nginx拷贝到当前目录。不要漏掉最后那个点。执行完成后，当前目录应该多出一个nginx子目录。然后，把这个子目录改名为conf
`docker run -d -p 8080:80 -p 443:443 --rm --name mynginx -v /root/html:/usr/share/nginx/html -v /root/nginx/conf.d:/etc/nginx/conf.d nginx`
-d：在后台运行
-p ：容器的80端口映射到8080
-p 443:443 https
--rm：容器停止运行后，自动删除容器文件
--name：容器的名字为mynginx
--v /root/html:/usr/share/nginx/html：映射到容器的网页文件目录/usr/share/nginx/html
$PWD 当前目录

- `docker exec -it mynignx nginx -t`，是进入名字为mynignx的 Nginx 容器，并执行 nginx -t 这个命令
`docker exec -it mynignx nginx -s reload`，则是进入名字为mynignx的 Nginx 容器，并执行 nginx -s reload 命令
