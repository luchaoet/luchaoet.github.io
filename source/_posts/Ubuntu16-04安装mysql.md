---
title: Ubuntu 16.04安装mysql
date: 2019-04-18 11:30:35
tags: ['Ubuntu', 'mysql']
summary:
---
<a name="e655a410"></a>
### 安装
依次执行下面三条命令
```
sudo apt-get install mysql-server
sudo apt install mysql-client
sudo apt install libmysqlclient-dev
```
输入命令查看是否安装
```
sudo netstat -tap | grep mysql
```
![WX20190418-110321@2x.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1555556680693-d44b5b83-8552-4242-953a-eaa5bb4400d8.png#align=left&display=inline&height=32&name=WX20190418-110321%402x.png&originHeight=72&originWidth=1678&size=24384&status=done&width=746)<br />
<a name="9a13e209"></a>
### 实现远程控制mysql
编辑 `/etc/mysql/mysql.conf.d/mysqld.cnf` 文件
```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

注释`bind-address = 127.0.0.1`![20190410135820.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1555556791520-09ed41e6-42b8-41ef-b1b4-24a8c79e07f2.jpeg#align=left&display=inline&height=335&name=20190410135820.jpg&originHeight=682&originWidth=1520&size=138883&status=done&width=746)<br />按`A`键进入编辑模式，编辑完成按`esc`退出编辑模式，再输入`:wq`退出
<a name="21903a07"></a>
### 进入mysql
```
mysql -uroot -p
```
提示需要输入密码，输入密码后回车键，成功进入<br />![WX20190418-111155@2x.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1555557181878-1a5b7486-9cf3-4afd-abe6-ae6c6b93ce71.png#align=left&display=inline&height=277&name=WX20190418-111155%402x.png&originHeight=546&originWidth=1472&size=97775&status=done&width=746)
<a name="6194d413"></a>
### 授权给远程任何电脑登录数据库
在mysql环境下执行授权命令<br />这步必不可少，因为我们要在自己的电脑上使用可视化工具操作这个远程数据库
```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '你的密码' WITH GRANT OPTION;
```
![20190410140340.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1555557718348-7f5542fc-f73a-463f-9436-752127041427.jpeg#align=left&display=inline&height=73&name=20190410140340.jpg&originHeight=172&originWidth=1760&size=51340&status=done&width=746)<br />继续输入以下命令，刷新配置信息
```
flush privileges;
```
最后输入`exit`命令退出数据库
<a name="7d41c434"></a>
### 重启数据库
```
service mysql restart
```
测试发现连接成功<br />![WX20190418-112643@2x.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1555558080109-4e2cd056-403c-4cbf-9750-ff6822bef3f5.png#align=left&display=inline&height=538&name=WX20190418-112643%402x.png&originHeight=840&originWidth=1164&size=110802&status=done&width=746)