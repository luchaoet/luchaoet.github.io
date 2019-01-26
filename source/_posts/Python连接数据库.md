---
title: Python连接数据库
date: 2019-01-26 22:53:51
tags: ['Python', '数据库']
summary:
---
Python2 与 Python3 访问数据库使用的模块不同，由于我用的是Python3，所以在此只讨论Python3<br />Python2用的是MySQLdb模块，而Python3用的是pymysql模块，这里我就踩了坑，不过操作流程都是一样的

### 数据库连接
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
import pymysql
config = {
  'host': '127.0.0.1',
  'port': 3306,
  'user': 'localhost', # 账号
  'passwd': '12345678', # 密码
  'db': 'python_test', # 数据库名称
  'charset': 'utf8'
}
db = pymysql.connect(**config)
cursor = db.cursor()
# 执行简单的SQL语句 从menu表中查找id,code,name列
cursor.execute("select name from menu;")
result = cursor.fetchall()
print(result)
db.commit()
cursor.close()
db.close()

# 打印的结果
# (
#   (10, 'homePage', '首页'), 
#   (11, 'clientMonitor', '客户端监控'), 
#   (12, 'taskManager', '计划任务管理'), 
#   (13, 'assetManager', '资产管理'), 
#   (14, 'userAuthManager', '用户与权限管理'), 
#   (15, 'marketManager', '企业应用市场管理'), 
#   (16, 'opLogQuery', '操作日志查询'), 
#   (17, 'lisenceManager', '授权许可管理'), 
#   (18, 'systemConfig', '系统设置')
# )
```

数据库menu表<br />![11_28_12__01_24_2019.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1548300540365-e670198e-2f34-4947-b2ac-71c2bd200f2c.jpeg#align=left&display=inline&height=233&linkTarget=_blank&name=11_28_12__01_24_2019.jpg&originHeight=233&originWidth=675&size=129558&width=675)

### 处理数据
把数据处理为前端需要的格式
```json
[
  {
    "code": "homePage", 
    "id": 10, 
    "name": "首页"
  }, 
  {
    "code": "clientMonitor", 
    "id": 11, 
    "name": "客户端监控"
  }, 
  {
    "code": "taskManager", 
    "id": 12, 
    "name": "计划任务管理"
  },
  ...
]
```

处理过程
```python
result = cursor.fetchall()
# 临时空列表 用来存放最终的数据
res = []
for item in result:
  keys = ['id', 'code', 'name']
  res.append(dict(zip(keys, item)))
```

### zip()函数
用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表
```python
zip(['id','code','name'],(10,'homePage','首页'))
# [('id', 10), ('code', 'homePage'), ('name', '首页')]
```

### dict()函数
用于创建字典
```python
dict([('id', 10), ('code', 'homePage'), ('name', '首页')])
# {'code': 'homePage', 'id': 10, 'name': '首页'}
```