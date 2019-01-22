---
title: Python—if语句
date: 2019-01-13 19:01:19
tags: ['Python', 'if']
summary:
---
每条if语句的核心都是一个值为True或False的表达式，这种表达式称为条件测试。Python根据条件测试的值为True还是False来决定是否执行if语句中的代码
```python
if 1+2 == 3:
    print('true')
else:
    print('false')

# true
```

### ==
检查是否相等，考虑大小写

### !=
检查是否不想等

### <
小于

### <=
小于等于

### >
大于

### >=
大于等于

### and
检查多个条件，并且同时满足时返回True

### or
检查多个条件，满足一个条件即返回True

### 检查特定值是否包含在列表中
```python
names = ['Lucy', 'Tom', 'Jack', 'Allen']
print('Lucy' in names) # True
```

### 检查特定值是否不包含在列表中
```python
names = ['Lucy', 'Tom', 'Jack', 'Allen']
print('Tony' not in names) # True
```

### if语句
```python
if True:
  print('True')
```

### if-else语句
```python
age = 18
if age >= 18:
  print('成年了！')
 else:
  print('小屁孩！')
```

### if-elif-else语句
```python
age = 18
if age > 18:
  print('小屁孩')
 elif age = 18:
  print('要成年了')
 else:
  print('大人了')
```

### 多个elif代码块

```python
age = 18
if age = 16:
  print('16岁的娃')
 elif age = 17:
  print('17岁的娃')
 elif age = 18:
  print('18岁的娃')
 else:
  print('大人了')
```
此时可以省略else代码块