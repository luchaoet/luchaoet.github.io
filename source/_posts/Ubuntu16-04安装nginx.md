---
title: Ubuntu 16.04安装nginx
date: 2019-04-15 23:47:21
tags: ['Ubuntu', 'nginx']
summary:
---
<a name="e88566ff"></a>
### 基于APT安装
```
sudo apt-get install nginx
```
<a name="d4d2a668"></a>
### 位置
```
/usr/sbin/nginx 主程序
/etc/nginx 存放配置文件
/var/www 静态文件存放地址
```
<a name="nginx.config"></a>
### nginx.config
```nginx
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    client_max_body_size 5m;
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    #access_log  logs/access.log  main;
    sendfile        on;
    #tcp_nopush     on;
    #keepalive_timeout  0;
    keepalive_timeout  65;
    #gzip  on;
    server {
        listen       80;
        server_name  localhost; # 远程机器请把localhost换成ip或域名
        location / {
            root /var/www/test; # test是随意取的前端文件夹名称
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
    include servers/*;
}
```
<a name="ddf7d2a5"></a>
### 命令
```
sudo service nginx reload // 重启nginx
sudo service nginx start // 启动nginx
sudo service nginx stop // 停止nginx
sudo vi nginx.config // 修改nginx
```
或者使用
```
sudo /etc/init.d/nginx start
```