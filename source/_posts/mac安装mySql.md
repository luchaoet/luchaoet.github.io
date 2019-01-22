---
title: mac安装mySql
date: 2018-11-20 22:01:50
tags: ['mac', 'mySql']
summary:
---
### 下载安装mySql
1.使用brew安装数据库，可参考该博客[https://www.cnblogs.com/walkerr/p/7289388.html](https://www.cnblogs.com/walkerr/p/7289388.html)
2.或者直接下载安装，官网选择macOS版本下载[https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)


![20181119140956.png | center | 747x542](https://cdn.nlark.com/yuque/0/2018/png/115449/1542607861353-384c5378-6193-46c6-8334-4d9a26b835c5.png "")


### Mysql 的管理工具Sequel Pro
官网下载地址[http://www.sequelpro.com](http://www.sequelpro.com)
更新！！！注意！！！
上面下载使用时会有问题


![屏幕快照 2018-11-19 14.47.17.png | center | 747x494](https://cdn.nlark.com/yuque/0/2018/png/115449/1542610059270-0a072173-4d83-4388-be80-2226011aeabc.png "")


！！！！使用test build版本[https://sequelpro.com/test-builds](https://sequelpro.com/test-builds)

更换后，果然可以了，都快哭了


![20181119144943.png | center | 747x498](https://cdn.nlark.com/yuque/0/2018/png/115449/1542610214089-99c98ca7-6c8d-42bd-b06a-f77df3caddae.png "")


### Sequel Pro连接mySql
Sequel Pro是一款管理 Mysql 的工具，界面简洁易用，<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">你可以一次性连接多个数据库，允许快速访问那些你经常需要访问的数据库</span></span>

#### 连接出现错误
Sequel Pro连接本地数据库时，出现了错误MySQL said: Authentication plugin 'caching\_sha2\_password' cannot be loaded: dlopen(/usr/local/...


![11_48_51__11_19_2018.jpg | center | 747x498](https://cdn.nlark.com/yuque/0/2018/jpeg/115449/1542599653103-8aadef88-fc9b-468d-b124-984e7fbd1c0e.jpeg "")


错误显示，<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">链接数据库时不能加载‘caching_sha2_password&#x27;这个插件，也就是不能对身份验证</span></span>

#### 解决方法
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">打开系统偏好设置，找到mysql</span></span>


![20181119115129.png | center | 668x548](https://cdn.nlark.com/yuque/0/2018/png/115449/1542599850427-d0e2d942-856f-4cfb-8244-0f39bb46d3e2.png "")


<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">点击Initialize Database，输入数据库密码，选择 Use Legacy Password Encryption</span></span>


![20181119114941.png | center | 540x399](https://cdn.nlark.com/yuque/0/2018/png/115449/1542599830821-afbcf6d6-2d0e-4da7-9b2a-7d475c7f90bf.png "")


ok，连接成功


![20181119115025.png | center | 747x498](https://cdn.nlark.com/yuque/0/2018/png/115449/1542606940407-a3ce6970-a6ef-49ed-a049-de00f370c7ee.png "")


### Navicat Premiun连接mySql
与Sequel Pro一样，都是数据库可视化管理工具
连接方式与上述大同小异，不述