---
title: Python—操作列表
date: 2019-01-12 23:10:25
tags: ['Python', '列表']
summary:
---
### 遍历列表
#### for in 循环
注意别忘了冒号
```python
names = ['Lucy', 'Tom', 'Jack', 'Allen']
for name in names:
  print(name)
	
# Lucy
# Tom
# Jack
# Allen
```

### 避免缩进错误
Python根据缩进来判断代码行与前一代码行的关系

### 创建数值列表
#### range()
轻松生成一系列的数字
```python
for item in range(1,5):
  print(item)
# 1
# 2
# 3
# 4
```
打印范围不包括5

#### 使用list() 和 range() 生成数字列表
```python
list(range(1,5)) # [1, 2, 3, 4]
```

#### 对数字列表执行简单的统计计算
```python
num = [1, 2, 3, 4]
a = min(num) # 1
b = max(num) # 4
c = sum(num) # 10
```

### 使用列表的一部分
#### 切片[start:end]
包括start位置的元素，不包括end位置的元素<br />start 切片起点，默认0<br />end 切片终点，默认列表长度
```python
names = ['Lucy', 'Tom', 'Jack', 'Allen', 'Mark']
print(names[0:3]) # ['Lucy', 'Tom', 'Jack']
```

#### 切片复制列表
```python
names = ['Lucy', 'Tom', 'Jack', 'Allen', 'Mark']
print(names[:]) # ['Lucy', 'Tom', 'Jack', 'Allen', 'Mark']
```

### 元组
列表可以修改，适合用于存储在程序运行期间可能变化的数据集。<br />不可变的列表称为元组，存储一组在程序的整个生命周期内都不变的列表<br />元组使用()来标识，定义元组后，就可以使用索引访问其元素
```python
num = (100, 200, 300)
print(num[0]) # 100
```