---
title: Python—列表
date: 2019-01-09 21:49:58
tags: ['Python', '列表']
summary:
---
列表就是数组
### 增
#### append(ele)在尾部添加
```python
arr = ['Lucy', 'Tom', 'Jack']
arr.append('Tony')
print(arr) # ['Lucy', 'Tom', 'Jack', 'Tony']
```

#### insert(index, ele)在列表中插入元素
在列表中的任何位置插入新元素，需要指定新元素的索引和值
```python
arr = ['Lucy', 'Tom', 'Jack']
arr.insert(1,'Tony')
print(arr) # ['Lucy', 'Tony', 'Tom', 'Jack']
```

### 删
#### del删除指定位置元素
```python
arr = ['Lucy', 'Tom', 'Jack']
del arr[1]
print(arr) # ['Lucy', 'Jack']
```

#### pop(?index)删除指定位置的元素
当不指定索引时，默认删除末尾的那一个元素，原列表被修改，返回删除的元素

#### remove()删除指定的值
```python
arr = ['Lucy', 'Tom', 'Jack']
arr.remove('Tom')
print(arr) # ['Lucy', 'Jack']
```

### 改
```python
arr = ['Lucy', 'Tom', 'Jack']
arr[0] = 'YUMI'
print(arr) # ['YUMI', 'Tom', 'Jack']
```

### sort()排序
原列表直接被修改，这与下面的sorted()方法不同
```python
cars = ['bmw', 'audi', 'toyota', 'subaru']
cars.sort()
print(cars) # ['audi', 'bmw', 'subaru', 'toyota']
```
列表按照字母顺序排列

通过向sort()方法传递参数reverse=True，可以按照字母顺序相反的顺序排列
```python
cars = ['bmw', 'audi', 'toyota', 'subaru']
cars.sort(reverse=True)
print(cars) # ['toyota', 'subaru', 'bmw', 'audi']
```

### sorted()临时排序
与sort()的区别是，sorted()返回修改后的列表，原列表不会被修改，所以称为临时排序

### reverse()颠倒列表
原列表直接被修改，要返回到原列表，可以再次调用reverse()方法即可

### len()返回列表长度
```python
cars = ['bmw', 'audi', 'toyota', 'subaru']
print(len(cars)) # 4
```