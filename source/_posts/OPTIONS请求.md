---
title: OPTIONS请求
date: 2019-01-11 22:57:21
tags: OPTIONS
summary:
---
在项目中ajax发起get请求去请求mock数据，发现接口发出去了两次请求，一直在找我代码哪里写的有问题触发了两次请求<br />![15_32_35__01_09_2019.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1547046162927-feac96f1-eb1c-4834-86d5-fce8bbe38ad5.jpeg#align=left&display=inline&height=115&linkTarget=_blank&name=15_32_35__01_09_2019.jpg&originHeight=115&originWidth=241&size=15604&width=241)

点开查看发现第一个请求方式为`OPTIONS`，并且没有返回任何数据<br />![23_06_24__01_09_2019.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1547046448361-55460b9e-545b-4a51-8af8-a8eaffcf61bb.jpeg#align=left&display=inline&height=158&linkTarget=_blank&name=23_06_24__01_09_2019.jpg&originHeight=158&originWidth=610&size=69332&width=610)

第二个为`GET`，这才返回数据<br />![15_34_05__01_09_2019.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1547046150491-ccd4fdfe-f956-43cc-b218-a1dc41a927c4.jpeg#align=left&display=inline&height=162&linkTarget=_blank&name=15_34_05__01_09_2019.jpg&originHeight=162&originWidth=613&size=53568&width=613)<br />

我就很好奇`OPTIONS`，因为一直都没有见过这种请求方式

查找原因是浏览器对简单跨域请求和复杂跨域请求的处理区别<br />XMLHttpRequest会遵守同源策略(same-origin policy). 也即脚本只能访问相同协议/相同主机名/相同端口的资源, 如果要突破这个限制, 那就是所谓的跨域, 此时需要遵守CORS(Cross-Origin Resource Sharing)机制<br />那么, 允许跨域, 不就是服务端设置Access-Control-Allow-Origin: *就可以了吗? 普通的请求才是这样子的, 除此之外, 还一种叫请求叫preflighted request<br />preflighted request在发送真正的请求前, 会先发送一个方法为OPTIONS的预请求(preflight request), 用于试探服务端是否能接受真正的请求，如果options获得的回应是拒绝性质的，比如404\403\500等http状态，就会停止post、put等请求的发出

那么, 什么情况下请求会变成preflighted request呢?<br />1、请求方法不是GET/HEAD/POST<br />2、POST请求的Content-Type并非application/x-www-form-urlencoded, multipart/form-data, 或text/plain<br />3、请求设置了自定义的header字段