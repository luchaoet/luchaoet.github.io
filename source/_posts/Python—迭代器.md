---
title: Python—迭代器
date: 2019-02-04 21:05:56
tags: ['Python', '迭代器']
summary:
---
### 迭代器
可以被`next()`函数调用并不断返回下一个值的对象称为迭代器：`Iterator`
```python
g = (x for x in range(1,11))
print g.next() # 1
```

凡是可作用于`for`循环的对象都是可迭代对象<br />凡是可作用于`next()`函数的对象都是迭代器，它们表示一个惰性计算的序列<br />集合数据类型如`list`、`dict`、`str`等是可迭代对象但不是迭代器<br />可以通过`iter()`函数获得一个`Iterator`对象
```python
g = iter([1,2,3])
print g.next()
print g.next()
print g.next()
print g.next()

1
2
3
Traceback (most recent call last):
  File "iter.py", line 32, in <module>
    print g.next()
StopIteration
```

### 判断是迭代器或可迭代对象
isinstance判断是否为指定类型
```python
from collections import Iterator  # 迭代器
from collections import Iterable	# 可迭代对象

g = (x for x in range(1,11))
l = [1,2,3,4,5]

print isinstance(g,Iterator) # True 判断是不是迭代器
print isinstance(l,Iterable) # True 判断是不是可迭代对象
```