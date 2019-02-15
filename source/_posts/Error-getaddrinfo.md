---
title: 'Error: getaddrinfo ENOTFOUND localhost'
date: 2019-02-15 19:25:06
tags:
summary:
---
运行本地项目时出现的错误

![20190215114231.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550202189846-a5a8d815-d3fa-415d-aad1-2b085b623a3d.png#align=left&display=inline&height=484&linkTarget=_blank&name=20190215114231.png&originHeight=484&originWidth=650&size=412133&width=650)

```
Error: getaddrinfo ENOTFOUND localhost
    at errnoException (dns.js:50:10)
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:92:26)
```

这个问题是系统hosts的问题

### 解决方法
修改hosts，`localhost`绑定`127.0.0.1`
```
# local
127.0.0.1 localhost
```