---
title: Python—生成器(generator)
date: 2019-02-03 20:17:22
tags: ['Python', '生成式', '生成器', 'generator']
summary:
---
### 列表生成式
运用列表生成式，可以快速生成list，可以通过一个list推导出另一个list
```python
range(1, 11)
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

[x for x in range(1, 11)]
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

[x * x for x in range(1, 11)]
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

for循环后面还可以加上if判断语句
```python
[x * x for x in range(1, 11) if x > 2]

# [9, 16, 25, 36, 49, 64, 81, 100]
```

亦可使用多层循环
```python
[m + n for m in 'ABC' for n in 'XYZ']

# ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

### 生成器
通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了<br />所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器（Generator）<br />要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的`[]`改成`()`
```python
g = (x * x for x in range(11))
print g 
# <generator object <genexpr> at 0x1031f3960>
print g.next()
print g.next()
...
print g.next()
```
![屏幕快照 2019-02-03 下午7.51.16.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1549194700463-d29bb5ea-7774-4935-9f3f-4f208a144f5b.png#align=left&display=inline&height=263&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-03%20%E4%B8%8B%E5%8D%887.51.16.png&originHeight=508&originWidth=690&size=618751&width=357)

generator保存的是算法，每次调用`next()`，就计算出下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误

使用for循环迭代generator
```python
g = (x * x for x in range(11))

for x in g:
    print x

# 0
# 1
# 4
# 9
# 16
# 25
# 36
# 49
# 64
# 81
# 100
```

如果一个函数定义中包含`yield`关键字，那么这个函数就不再是一个普通函数，而是一个generator
```python
def foo():
    print 'step 1'
    yield 1
    print 'step 2'
    yield 2
    print 'step 3'
    yield 3

f = foo()
print f.next()
print f.next()

# step 1
# 1
# step 2
# 2
```