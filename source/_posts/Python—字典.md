---
title: Python—字典
date: 2019-01-14 20:07:20
tags: ['Python', '字典']
summary:
---
字典就是js中的对象
```python
Lucy = {'age': 20, 'sex': 'man'}
```

### 访问字典中的值
```python
Lucy = {'age': 20, 'sex': 'man'}
print(Lucy['age']) # 20
```

### 添加键值对
```python
Lucy = {'age': 20, 'sex': 'man'}
Lucy['add'] = '中国'
print(Lucy) # {'age': 20, 'sex': 'man', 'add': '中国'}
```

### 修改字典中的值
```python
Lucy = {'age': 20, 'sex': 'man'}
Lucy['age'] = 25
print(Lucy) # {'age': 25, 'sex': 'man'}
```

### 删除键值对
使用del语句将相应的键值对彻底删除，使用del语句时，必须指定字典名和要删除的键
```python
Lucy = {'age': 20, 'sex': 'man'}
del Lucy['age']
print(Lucy) # {'sex': 'man'}
```

### items()遍历字典
```python
Lucy = {'age': 20, 'sex': 'man', 'add': '中国'}
for key,value in Lucy.items():
    print(key,value)

# age 20
# sex man
# add 中国
```

### keys()遍历字典中的所有键
```python
Lucy = {'age': 20, 'sex': 'man', 'add': '中国'}
for key in Lucy.keys():
    print(key)

# age 
# sex 
# add
```

### valus()遍历字典中的所有值
```python
Lucy = {'age': 20, 'sex': 'man', 'add': '中国'}
for value in Lucy.values():
    print(value)

# 20
# man
# 中国
```