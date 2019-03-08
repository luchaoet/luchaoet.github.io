---
title: MySQL常用语法
date: 2019-03-04 11:08:51
tags: MySQL
summary: MySQL常用语法
---
```sql
#判断是否存在数据库db_test,有则删除
drop database if exists db_test;

#创建数据库
create database db_test;

#删除数据库
drop database db_test;

#显示数据库中的表
show tables;

#删除表
drop table menu;

#创建表
create table student(
id int auto_increment primary key,
name varchar(50),
sex varchar(20),
date varchar(50),
content varchar(100)
)default charset=utf8;

#删除表
drop table student;

#查看表结构
describe menu;

#查询表中的数据
select * from menu;
select id,code,name from menu;

#插入数据
insert into menu values(13,'assetManager','资产管理');

#修改某一条数据
update menu set name='资产Manager' where id=13;

#删除数据
delete from menu where id=10;

#and 并
select * from menu where id=12 and name='资产管理';

#or 或
select * from menu where id=12 and id=13;

#between 查询范围 不包括 10
select * from menu where id between 10 and 13;

#in 查询指定集合内的数据
select * from menu where id in (1,3,5);

#排序 asc 升序  desc 降序
select * from menu order by id asc;
select * from menu order by id desc;

#查询第2条数据之后的5条数据，不包括第2条
select * from menu limit 2,5;
```