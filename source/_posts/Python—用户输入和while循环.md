---
title: Python—用户输入和while循环
date: 2019-01-15 15:03:21
tags: ['Python', 'raw_input', 'while']
summary:
---
### input()函数
函数input()让程序暂停运行，等待用户输入一些文本。获取用户输入后，Python将其存在一个变量中，方便使用
```python
message = input('请输入：')
print(message) 

# 请输入：123
# 123
```

Python2.7也包含函数input()，但它将用户输入解读为Python代码，并尝试运行它们。因此最好的结果是出现错误，指出Python不明白输入的代码；而糟糕的结果是，将运行你原本无意运行的代码。因此使用raw_input()来获取输入
```python
message = raw_input('请输入：')
print(message) 
```

### while循环
```python
num = 1
while num <= 5:
    print(num)
    num += 1

# 1 2 3 4 5
```

### break退出循环
不再执行余下代码并退出循环
```python
num = 1
while num <= 5:
  	# 在num == 4时终止循环
    if num == 4:
        break
    print(num)
    num += 1
  
  # 1 2 3
```

### continue
跳出本次循环，进入下次循环
```python
num = 0
while num <= 5:
    num += 1
    if num % 2 == 0:
        continue
    print(num)

# 1 3 5
```