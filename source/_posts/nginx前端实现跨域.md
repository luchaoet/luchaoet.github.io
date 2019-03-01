---
title: nginx前端实现跨域
date: 2018-11-11 17:37:09
tags: ['nginx', '前端跨域']
summary:
---
### 1.Homebrew安装
<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(246, 248, 250)">/usr/bin/ruby </span></span><span data-type="color" style="color:rgb(0, 0, 0)">-e</span><span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(246, 248, 250)"> </span></span><span data-type="color" style="color:rgb(0, 153, 0)">&quot;$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)</span>"

### 2.下载nginx
// http://nginx.org/en/download.html

mac 安装： <span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(246, 248, 250)">brew install nginx</span></span>

### 3.launchrocket安装
帮助管理Homebrew安装的服务软件
brew cask install launchrocket

nginx文件路径 /usr/local/etc/nginx
可以使用 open /usr/... 打开文件夹

### 4.nginx.config
```nginx
# user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


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
        server_name  localhost;

        location ~/api {
            proxy_pass   https://xxxx.taobao.com;
        }

        location ~\.(jpg|png)$ {
            proxy_pass   https://xxxx.taobao.com;
        }

        location / {
            proxy_pass http://127.0.0.1:3333;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    include servers/*;
}
```