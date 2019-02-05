---
title: Python—内置函数
date: 2019-01-27 23:14:04
tags: ['Python', '内置函数']
summary:
---
[https://www.cnblogs.com/xiao1/p/5856890.html](https://www.cnblogs.com/xiao1/p/5856890.html)
### abs()
返回数字的绝对值

### all()
接受一个迭代器，如果迭代器的所有元素都为真，返回True，否则返回False
```python
all(['a', 1])
# True
```

### any()
接受一个迭代器，如果迭代器有一个元素为真，返回True，否则返回False

### ascii()
返回一个字符串形式的对象
```python
a = ascii([1,2])
print(a) # "[1, 2]"
print(type(a)) # <class 'str'>
```
<br />
### bin()、oct()、hex()
三个函数功能为：将十进制数分别转换为2/8/16进制

### bool()
转为布尔值

### bytearray()
### bytes()
### callable()
判断对象是否可以被调用，能被调用的对象就是一个callables对象，比如函数和带有__call__()的实例
```python
callable(max)
# True

callable([1,2])
# False

callable('abc')
# False

callable(None)
# False
```

### chr()、ord()
查看十进制数对应的ASCII字符、查看某个ASCII对应的十进制数<br /><br />
### classmethod()
用来指定一个方法为类的方法，由类直接调用执行
```python
class Animal:
    def __init__(self, name):
        self.name = name
      
    @classmethod
    def say_hello(clas):  # 类方法，由类调用，最少要有一个参数clas，调用的时候这个参数不用传值，自动将类名赋值给cls
        print(clas) 

Animal.say_hello() # __main__.Animal
```

### compile()
### complex()
### delattr()
删除指定对象的属性，与setattr相反<br />只能删除属性，不能删除方法

```python
class A:
    def __init__(self,name):
        self.name = name
    def sayHello(self):
        print('hello--'+self.name)

a = A('Lucy')
delattr(a,'name') # 删除name属性
delattr(a,'sayHello') # 报错 不存在sayHello属性 只能删除属性 不能删除方法
```
<br />
### dict()
创建字典

```python
dict(one=1, two=2) # {'two': 2, 'one': 1}
dict([('id', 'name'),(1, 'lucy')]) # {'id':1, 'name': 'lucy'}
```

### dir()
不带参数时返回当前范围内的变量，方法和定义的类型列表<br />带参数时返回参数的属性，方法列表
```
dir()
# __main__.Animal
# ['A', 'Animal', '__builtins__', '__doc__', '__file__', '__name__', '__package__', 'a']

dir(list)
# ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__setslice__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

### divmod()
返回一个包含商和余数的元组
```python
divmod(7,3)
# (2, 1)
```

### enumerate()
返回一个迭代器<br />next()方法返回一个元组，包括下标及对应的元素
```python
g = enumerate(['a', 'b', 'c', 'd'])
print g.next() # (0, 'a')
```

### eval()
将字符串当成有效的表达式来求值并返回计算结果
```python
r = '1 + 4'
print eval(r) # 5
```

### exec()
### filter()
### float()
将一个字符串或整数转化为浮点数
```python
print float(100) # 100.0
print float('100') # 100.0
print float('a') # 报错 不能转化
```

### format()
格式化输出字符串
```python
s = 'My name is {0}, I am from {1}!'.format('Lucy', 'China')
print s 
# My name is Lucy, I am from China!
```

### frozenset()
### getattr()
获取对象的属性

### globals()
返回一个包含所有全局变量的字典

### hasattr()
判断某个对象是否包含某个特性
```python
hasattr(list, 'append')
# True
```

### hash()
### help()
### hex()
### id()
### input()
### int()
### isinstance()
### issubclass()
### iter()
### len()
### list()
### locals()
### map()
### max()
### memoryview()
### min()
### next()
### object()
### open()
### pow()
### print()
### property()
### range()
### repr()
### reversed()
### round()
### set()
### setattr()
### slice()
### sorted()
### staticmethod()
### str()
### sum()
### super()
### tuple()
### type()
### vars()
### zip()
### __import__()

### 

