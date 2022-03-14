# nginx Config

## Nginx配置文件主要分成四部分：

* main（全局设置）
* server（主机设置）
* upstream（上游服务器设置，主要为反向代理、负载均衡相关配置）
* location（URL匹配特定位置后的设置），

## 每部分包含若干个指令

* main部分设置的指令将影响其它所有部分的设置；
* server部分的指令主要用于指定虚拟主机域名、IP和端口；
* upstream的指令用于设置一系列的后端服务器，设置反向代理及后端服务器的负载均衡；
* location部分用于匹配网页位置（比如，根目录“/”,“/images”,等等）。

## 他们之间的关系式

* server继承main，location继承server；upstream既不会继承指令也不会被继承。它有自己的特殊指令，不需要在其他地方的应用。

## 通用config

```code
user  www www;
worker_processes  auto;

error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

pid        logs/nginx.pid;


events {
    use epoll;
    worker_connections  2048;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    # tcp_nopush     on;

    keepalive_timeout  65;

  # gzip压缩功能设置
    gzip on;
    gzip_min_length 1k;
    gzip_buffers    4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 6;
    gzip_types text/html text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
    gzip_vary on;

  # http_proxy 设置
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

  # 设定负载均衡后台服务器列表
    upstream  backend  {
              #ip_hash;
              server   192.168.10.100:8080 max_fails=2 fail_timeout=30s ;
              server   192.168.10.101:8080 max_fails=2 fail_timeout=30s ;
    }

  # 很重要的虚拟主机配置
    server {
        listen       80;
        server_name  itoatest.example.com;
        root   /apps/oaapp;

        charset utf-8;
        access_log  logs/host.access.log  main;

        #对 / 所有做负载均衡+反向代理
        location / {
            root   /apps/oaapp;
            index  index.jsp index.html index.htm;

            proxy_pass        http://backend;
            proxy_redirect off;
            # 后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
            proxy_set_header  Host  $host;
            proxy_set_header  X-Real-IP  $remote_addr;
            proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        }

        #静态文件，nginx自己处理，不去backend请求tomcat
        location  ~* /download/ {
            root /apps/oa/fs;
        }
        location ~ .*\.(gif|jpg|jpeg|bmp|png|ico|txt|js|css)$
        {
            root /apps/oaapp;
            expires      7d;
        }
        location /nginx_status {
            stub_status on;
            access_log off;
            allow 192.168.10.0/24;
            deny all;
        }

        location ~ ^/(WEB-INF)/ {
            deny all;
        }
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

  ## 其它虚拟主机，server 指令开始
}
```

## 常用指令说明

* main全局配置
  * woker_processes
    * worker角色的工作进程的个数，master进程是接收并分配请求给worker处理。这个数值简单一点可以设置为cpu的核数(也是auto值)，如果开启了ssl和gzip更应该设置成与逻辑CPU数量一样甚至为2倍，可以减少I/O操作。
  * worker_cpu_affinity
    * 在高并发情况下，通过设置cpu粘性来降低由于多CPU核切换造成的寄存器等现场重建带来的性能损耗。如worker_cpu_affinity 0001 0010 0100 1000; （四核）。
  * worker_connections
    * 写在events部分。每一个worker进程能并发处理（发起）的最大连接数（包含与客户端或后端被代理服务器间等所有连接数）。
    * nginx作为反向代理服务器，计算公式 最大连接数 = worker_processes * worker_connections/4，这个可以增到到8192都没关系，看情况而定，但不能超过后面的worker_rlimit_nofile。当nginx作为http服务器时，计算公式里面是除以2
  * worker_rlimit_nofile
    * 写在main部分。默认是没有设置，可以限制为操作系统最大的限制65535。
  * use epoll
    * 写在events部分。在Linux操作系统下，nginx默认使用epoll事件模型，得益于此，nginx在Linux操作系统下效率相当高。同时Nginx在OpenBSD或FreeBSD操作系统上采用类似于epoll的高效事件模型kqueue。在操作系统不支持这些高效模型时才使用select。

  [其他设置](http://seanlook.com/2015/05/17/nginx-install-and-config/)
