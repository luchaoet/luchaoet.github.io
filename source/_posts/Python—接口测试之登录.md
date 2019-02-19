---
title: Python—接口测试之登录
date: 2019-02-19 22:12:58
tags: ['Python', '接口测试']
summary:
---
测试妹子需要编写后台接口测试的python代码<br />由于我们的后台登录涉及到密码加密，过程过于复杂，使用python很难实现<br />所以想了另外一种方式，直接操作js方法
### 需要的依赖
```python
# pip install PyExecJS 执行js文件
import execjs
import requests
# 序列化数据
import json
# 操作本地文件
import io
```

### 获取js文件
下一步需要它执行其中的js方法
```python
def get_js():
  	# 将需要的文件保存到本地
    f = io.open("./js/RSA.js", 'r', encoding='UTF-8')
    line = f.readline()
    htmlstr = ''
    while line:
        htmlstr = htmlstr + line
        line = f.readline()
    # 返回js文件字符串
    return htmlstr
```

### 密码加密
js的操作步骤，在python中实现，这个就不过多解释了，涉及到我们网站的密码加密，返回的是密码加密后的字符串
```python
def password_encry(password):
    global session
    pas = password[::-1]
    req = session.get('http://r*a.a***n.net/***/user/sso/***/public/hex')
    data = req.json()['data']
    jsstr = get_js()
    ctx = execjs.compile(jsstr)
    return (ctx.call('returnPas', data['publicExp'], '', data['publicKey'], pas))
```

### 登录
requests库的session对象能够帮我们跨请求保持某些参数，也会在同一个session实例发出的所有请求之间保持cookies
```python
acounts = 'test_acounts' # 账号
password = 'test_password' # 密码
# 密码加密
password =  password_encry(password)
# 登录需要的数据
data = {
    'userName': acounts,
    'password': password
}
session = requests.session()
# 登录接口
req = session.post('http://r*a.a****n.net/***/user/sso/login/***', data=data)
print(req.json())
# {'success': True, 'code': 0, 'msg': 'SUCCESS', 'data': None, 'pager': None}
# 从返回的结果来看 已经登录成功

# 接口获取用户基本信息
res = session.get('http://r*a.a****n.net/rpa/***/baseinfo')
print(res.json())
```

### 返回用户信息
返回success，得到了我们需要的数据，说明已经登录了
```json
{
  'success': True, 
  'code': 0, 'msg': 
  'SUCCESS', 
  'data': {
    				'id': 27, 
    				'uuid': 'ea****d6-2**c-4**f-9**7-f**1****3c2c', 
            'userName': '***', 
            'nickName': '***', 
            'groupId': 1, 
            'groupName': '测试企业', 
            'expireDate': '2099-12-31'
          }, 
  'pager': None
}
```

下面便可以在登录的情况下，继续测试其他接口了<br />666