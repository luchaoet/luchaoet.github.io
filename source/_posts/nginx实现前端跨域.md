---
title: nginx实现前端跨域
date: 2018-11-11 16:09:45
tags: ['nginx', '前端跨域']
summary:
---
<a name="19e98bc8"></a>
### 1.Homebrew安装
终端执行
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

<a name="ce5107cb"></a>
### [](https://www.yuque.com/luchao/blog/pfym4o#rle4uz)2.下载nginx
window：[http://nginx.org/en/download.html](http://nginx.org/en/download.html)<br />mac 安装
```
brew install nginx
```

<a name="2d5c162b"></a>
#### [](https://www.yuque.com/luchao/blog/pfym4o#di9cat)nginx启动与关闭
启动nginx
```
brew services start nginx
```
终止nginx
```
brew services stop nginx
```

<a name="217b55ce"></a>
### [](https://www.yuque.com/luchao/blog/pfym4o#1dglth)3.launchrocket安装
帮助管理Homebrew安装的服务软件
```
brew cask install launchrocket
```

nginx文件路径`/usr/local/etc/nginx`<br />可以使用 `open /usr/...` 打开文件夹

<a name="a7ad56fd"></a>
### [](https://www.yuque.com/luchao/blog/pfym4o#d6kpmr)4.nginx.config文件
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