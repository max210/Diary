- 参考文章
http://seanlook.com/2015/05/17/nginx-install-and-config/
https://www.zybuluo.com/phper/note/89391

- 安装 apt-get install nginx
- 主配置目录 /etc/nginx/nginx.conf，其中可以修改`server_token off;` 来隐藏nginx版本号
- 自定义配置目录 /etc/nginx/conf.d/xxx.conf, 需要在/etc/nginx/nginx.conf 中指定 `include /etc/nginx/conf.d/*.conf`
- 启动 sudo nginx -s reload
- 停止 sudo nginx -s stop
- 检查 nginx -t 检查配置 nginx -t -c /etc/nginx/nginx.conf

- 配置（主要分为六个区域：main(全局设置)、events(nginx工作模式)、http(http设置)、sever(主机设置)、location(URL匹配)、upstream(负载均衡服务器设置)）
```
# main
user              www www; # 指定Nginx Worker进程运行用户以及用户组
worker_processes  1; # 指定Nginx要开启的子进程数，建议指定CPU的数量一样的进程数

error_log  /usr/local/var/log/nginx/error.log  notice; # 全局错误日志文件（notice级别）
pid        /usr/local/var/run/nginx/nginx.pid; # 指定进程id的存储文件位置

events {
  use                 epoll; # 指定Nginx的工作模式(epoll为linux平台)
  worker_connections  1024; # 定义Nginx每个进程的最大连接数，即接收前端的最大请求数
}

http {
  include       mime.types; # nginx识别文件类型
  default_type  application/octet-stream; # 设定默认的类型为二进制流

  sendfile        on; # 开启高效文件传输模式
  tcp_nopush      on; # 防止网络阻塞
  tcp_nodelay     on; # 防止网络阻塞

  # gzip压缩功能设置
  gzip on;
  gzip_min_length 1k;
  gzip_buffers    4 16k;
  gzip_http_version 1.0;
  gzip_comp_level 6;
  gzip_types text/html text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
  gzip_vary on;

  # http_proxy 设置 实现的是nginx作为反向代理服务器的功能
  client_max_body_size   10m;
  client_body_buffer_size   128k;
  proxy_connect_timeout   75;
  proxy_send_timeout   75;
  proxy_read_timeout   75;
  proxy_buffer_size   4k;
  proxy_buffers   4 32k;
  proxy_busy_buffers_size   64k;
  proxy_temp_file_write_size  64k;
  proxy_temp_path   /usr/local/nginx/proxy_temp 1 2;

  # 用来定一个虚拟主机
  server {
    listen       80; # 指定虚拟主机的服务端口
    server_name  192.168.12.10 www.max210.com; # 用来指定IP地址或者域名，多个域名之间用空格分开
    root         /Users/maximilian/www; # 表示在这整个server虚拟主机内，全部的root web根目录
    index        index.php index.html index.htm;  # 全局定义访问的默认首页地址
    charset      utf-8; # 设置网页的默认编码格式

    # 负载均衡啊、反向代理、虚拟域名相关(location / 表示匹配访问根目录, 开启正则匹配这样：location ~)
    location / {
      root              /Users/maximilian/www; # 指定访问根目录时，虚拟主机的web目录
      index             index.html; # 设定只输入域名后访问的默认首页地址
      try_files         $uri $uri/ /index.html; # 避免单页面刷新404

      # 如果是前后的一体项目，可以做代理
      proxy_set_header  X-Real-Ip $remote_addr;
      proxy_set_header  X-Forward-For $proxy_add_x_forwarded_for;
      proxy_set_header  Host $http_host;
      proxy_set_header  X-Nginx-Proxy true;
      proxy_pass        http://testname; # 代理到upstream为testname去
      proxy_redirect    off;
    }

    location /api {
      proxy_set_header  X-Real-Ip $remote_addr;
      proxy_set_header  X-Forward-For $proxy_add_x_forwarded_for;
      proxy_set_header  Host $http_host;
      proxy_set_header  X-Nginx-Proxy true;
      proxy_pass        http://testname; # 代理到upstream为testname去
      proxy_redirect    off;
    }

    location ~* ^.+\.(html|jpg|jpeg|gif|png|ico|css|js|pdf|txt) {
      root   /Users/maximilian/www
    }
  }

  # 指定一个负载均衡器的名称testname
  upstream testname {
    ip_hash; # 一种负载均衡调度算法
    server 192.168.12.1:80;
    server 192.168.12.2:80 down; # down表示当前的server暂时不参与负载均衡
    server 192.168.12.3:8080  max_fails=3  fail_timeout=20s;
    server 192.168.12.4:8080;
  }

}
```

- docker 开启 nginx
docker cp mynginx:/etc/nginx .  表示把mynginx容器的/etc/nginx拷贝到当前目录。不要漏掉最后那个点。执行完成后，当前目录应该多出一个nginx子目录。然后，把这个子目录改名为conf
`docker run -d -p 8080:80 --rm --name mynginx --v /maximilian/html:/usr/share/nginx/html --v "$PWD/conf":/etc/nginx nginx`
-d：在后台运行
-p ：容器的80端口映射到8080
--rm：容器停止运行后，自动删除容器文件
--name：容器的名字为mynginx
--v /maximilian/html:/usr/share/nginx/html：映射到容器的网页文件目录/usr/share/nginx/html
$PWD 当前目录
