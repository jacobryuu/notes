# 日志格式

Apache日志配置文件中默认定义了两种打印格式

* combined
  * LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
* common
  * LogFormat "%h %l %u %t \"%r\" %>s %b" 
* 自定义格式
  * LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %f %k %p %q %R %T %I %O" customized

## Apache日志配置文件中同时需要指定当前日志的打印格式、日志文件路径及名称

    CustomLog "/var/log/apache2/access_log" combined

## 字段说明

|字段格式|键名称|含义|
| ----| ---- | ---- |
|%a|client_addr|请求报文中的客户端IP地址。|
|%A|local_addr|本地私有IP地址。|
|%b|response_size_bytes|响应字节大小，空值时可能为"-"。|
|%B|response_bytes|响应字节大小，空值时为0。|
|%D|request_time_msec|请求时间，单位为毫秒。|
|%h|remote_addr|远端主机名。|
|%H|request_protocol_supple|请求协议。|
|%l|remote_ident|客户端日志名称，来自identd。|
|%m|request_method_supple|请求方法。|
|%p|remote_port|服务器端口号。|
|%P|child_process|子进程ID。|
|%q|request_query|查询字符串，如果不存在则为空字符串。|
|"%r"|request|请求内容，包括方法名、地址和http协议。|
|%s|status|响应的http状态码。|
|%>s|status|响应的http状态码的最终结果。|
|%f|filename|请求的文件名。|
|%k|keep_alive|keep-alive请求数。|
|%R|response_handler|服务端的处理程序类型。|
|%t|time_local|服务器时间。|
|%T|request_time_sec|请求时间，单位为秒。|
|%u|remote_user|客户端用户名。|
|%U|request_uri_supple|请求的URI路径，不带query。|
|%v|server_name|服务器名称。|
|%V|server_name_canonical|服务器权威规范名称。|
|%I|bytes_received|服务器接收得字节数，需要启用mod_logio模块。|
|%O|bytes_sent|服务器发送得字节数，需要启用mod_logio模块。|
|"%{User-Agent}i"|http_user_agent|客户端信息。|
|"%{Rererer}i"|http_referer|来源页|

[apache mod_log_config](https://httpd.apache.org/docs/2.4/mod/mod_log_config.html)