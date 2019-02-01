---
title: Python—装饰器(decorator)
date: 2019-02-01 20:34:50
tags: ['Python', '装饰器', 'decorator']
summary:
---
装饰器可以对一个函数、方法或者类进行加工<br />什么时候需要使用装饰器？当很多方法都需要执行一段相同的代码时，我们可以把这段相同的代码封装成公共函数，然后去装饰其他函数，这个公共函数就是装饰器。只不过有它特定的书写规范。它的目的就是为了减少和优化代码的书写<br />也可以在不改动函数的情况下，为函数添加额外的功能<br />当使用Python编写Web API的时候，每一个数据库数据查询的过程都需要连接数据库和查询后断开数据库，这是一段公共的代码段，这个时候就可以使用装饰器

### 示例，目标函数不带参数的装饰器
```python
# 数据库操作
def sql_operation(fun):
  def foo():
    # 连接数据库
    res = fun()
    # 断开数据库
    return res
  return foo

@sql_operation
def query_list():
  # sql语句查询数据库列表信息并返回
  return data

@sql_operation
def query_menu():
  # sql语句查询数据库menu信息并返回
  return menu

# 执行query_list函数和query_menu函数
# 它们都经历了连接数据库，执行自己的操作，断开数据库
list = query_list()
menu = query_menu()
```
这是一个很简单的装饰器，在函数定义前加一个@符号，接收一个函数作为参数，在装饰器内部执行，返回值也是一个函数

### 目标函数带固定参数的装饰器
```python
def decorator(func):
    def wrapper(name):
        print '妈妈来了'
        func(name)
        print '开始叠被子'
    return wrapper
    
@decorator
def foo(name):
    print name + '起床了'

foo('Lucy')
# 妈妈来了
# Lucy准备起床
# 开始叠被子
```

### 目标函数带不固定参数的装饰器
```python
def decorator(func):
  	# 第一个参数 args 包含所有参数的元组
    def wrapper(*args, **kwargs):
        print '妈妈来了'
        func(*args, **kwargs)
        print '开始叠被子'
    return wrapper
    
@decorator
def foo(name):
    print name + '起床了'

@decorator
def bar(name1, name2):
    print name1 + '和' + name2 + '起床了'

foo('Lucy')
# 妈妈来了
# Lucy起床了
# 开始叠被子

bar('Lucy', 'YUMI')
# 妈妈来了
# Lucy和YUMI起床了
# 开始叠被子
```

### 装饰器带参数
```python
def decorator(parent):
    def _decorator(func):
        def wrapper(*args, **kwargs):
            print parent + '来了'
            func(*args, **kwargs)
            print '开始叠被子'
        return wrapper
    return _decorator

@decorator('爸爸')
def baz(name):
    print name + '起床了'

baz('Lucy')
# 爸爸来了
# Lucy起床了
# 开始叠被子
```

多个装饰器装饰一个函数时，执行顺序时从下到上

### 类装饰器
装饰器不仅可以是函数，也可以是类，相比函数装饰器，类装饰器具有灵活度大、高内聚、封装性等优点。使用类装饰器主要依靠类的__call__方法，当使用 @ 形式将装饰器附加到函数上时，就会调用此方法
```python
class Foo(object):
    def __init__(self, func):
        self._func = func

    def __call__(self):
        print('class decorator runing')
        self._func()
        print('class decorator ending')


@Foo
def bar():
    print('bar')

bar()
# class decorator runing
# bar
# class decorator ending
```