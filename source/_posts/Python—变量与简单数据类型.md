---
title: Python—变量与简单数据类型
date: 2019-01-07 21:00:27
tags: Python
summary:
---
### 变量
#### 命名规则
1. 变量名只能包含字母、数字、下划线。可以字母或下划线开头，但不能以数字开头
1. 变量名不能包含空格
1. 变量名应该简洁，通俗易懂

### 字符串
字符串使用引号包裹，可以是单引号，也可以是双引号，或者三引号
```python
a = '哈哈哈'
b = "呵呵呵"
c = '''嘿嘿嘿'''
```

#### title()
将每个单词的首字母大写
```python
a = 'hello world'
a.title() # Hello World
```

#### upper()
将字符串改为全部大写
```python
a = 'hello world'
a.upper() # HELLO WORLD
```

#### lower()
将字符串改为全部小写
```python
a = 'HELLO WORLD'
a.lower() # hello world
```

#### 拼接字符串
Python使用+加号拼接字符串

#### 空白与换行
空白是添加制表符`\t`
```python
a = 'HELLO WORLD\tHELLO WORLD\tHELLO WORLD'
print(a) # HELLO WORLD	HELLO WORLD	HELLO WORLD
```

换行符使用`\n`

#### lstrip()
去除字符串开头多余空白

```
print('1' + a.rstrip() + '2') # 1hello    1
```

### rstrip()
去除字符串尾部多余空白
```python
a = ' hello    '
print('1' + a.rstrip() + '2') # 1 hello2
```

### 数字
#### 整数
在Python中，可以对整数执行加（+）减（-）乘（*）除（／）运算<br />**两个乘号表示乘方运算
```python
a = 3 ** 2
print(a) # 9
```
空格不影响计算表达式的运算

#### 浮点数
在Python中，将带小数点的数字都成为浮点数<br />需要注意的是
```python
a = 0.2 + 0.1
print(a) # 0.30000000000000004
```
所有语言都存在这样的问题，没什么可担心的，鉴于计算机内部表示数字的方式。暂时忽略多余的小数位数即可

#### str()
数字类型转化为字符串类型<br />使用str()函数避免类型错误
```python
a = '111' + 222 + '333'
print(a)
# TypeError: must be str, not int
```