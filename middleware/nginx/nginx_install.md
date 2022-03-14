# install nginx

## os

* centos 7

```code
sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
yum install -y nginx
```

or
/etc/yum.repos.d/nginx.repo

```code
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```

On Debian/Ubuntu/SLES signatures are checked by default, but on RHEL/CentOS it is necessary to set
gpgcheck=1

Signatures
sudo rpm --import nginx_signing.key

* 注意
**yum安装默认会安装许多模块，但是如果你想以后安装第三方模块那就没办法了。**

## 编译安装

nginx大部分常用模块，编译时./configure --help以--without开头的都默认安装。

* --prefix=PATH ： 指定nginx的安装目录。默认 /usr/local/nginx
* --conf-path=PATH ： 设置nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为prefix/conf/nginx.conf
* --user=name： 设置nginx工作进程的用户。安装完成后，可以随时在nginx.conf配置文件更改user指令。默认的用户名是nobody。--group=name类似
* --with-pcre ： 设置PCRE库的源码路径，如果已通过yum方式安装，使用--with-pcre自动找到库文件。使用--with-pcre=PATH时，需要从PCRE网站下载pcre库的源码（版本4.4 - 8.30）并解压，剩下的就交给Nginx的./configure和make来完成。perl正则表达式使用在location指令和 ngx_http_rewrite_module模块中。
* --with-zlib=PATH ： 指定 zlib（版本1.1.3 - 1.2.5）的源码解压目录。在默认就启用的网络传输压缩模块ngx_http_gzip_module时需要使用zlib 。
* --with-http_ssl_module ： 使用https协议模块。默认情况下，该模块没有被构建。前提是openssl与openssl-devel已安装
* --with-http_stub_status_module ： 用来监控 Nginx 的当前状态
* --with-http_realip_module ： 通过这个模块允许我们改变客户端请求头中客户端IP地址值(例如X-Real-IP 或 X-Forwarded-For)，意义在于能够使得后台服务器记录原始客户端的IP地址
* --add-module=PATH ： 添加第三方外部模块，如nginx-sticky-module-ng或缓存模块。每次添加新的模块都要重新编译

```code
./configure \
> --prefix=/usr \
> --sbin-path=/usr/sbin/nginx \
> --conf-path=/etc/nginx/nginx.conf \
> --error-log-path=/var/log/nginx/error.log \
> --http-log-path=/var/log/nginx/access.log \
> --pid-path=/var/run/nginx/nginx.pid  \
> --lock-path=/var/lock/nginx.lock \   
> --user=nginx \
> --group=nginx \
> --with-http_ssl_module \
> --with-http_stub_status_module \
> --with-http_gzip_static_module \
> --http-client-body-temp-path=/var/tmp/nginx/client/ \
> --http-proxy-temp-path=/var/tmp/nginx/proxy/ \
> --http-fastcgi-temp-path=/var/tmp/nginx/fcgi/ \
> --http-uwsgi-temp-path=/var/tmp/nginx/uwsgi \
> --with-pcre=../pcre-7.8
> --with-zlib=../zlib-1.2.3
```