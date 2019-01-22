---
title: Python—函数
date: 2019-01-16 22:05:18
tags: ['Python', '函数']
summary:
---
Python使用def关键字定义一个函数
```python
def abs(x):
	if x >= 0:
		return x
	else:
		return -x
```

### 函数默认值
```python
def printValue(x='哈哈哈'):
	print(x)

printValue() # 哈哈哈
```

### 等效的函数调用
```python
def dog(name='Tom', age=2):
	print('一条叫' + name + '的狗，年龄是' + str(age) + '岁')

# 以下三种函数调用的方式相同
dog('Jack',3) 
dog(name='Jack',age=3) 
dog(age=3,name='Jack')

# 一条叫Jack的狗，年龄是3岁
# 一条叫Jack的狗，年龄是3岁
# 一条叫Jack的狗，年龄是3岁
```

### 传递任意数量的实参
```python
def fun(*parameter_list):
	print(parameter_list)

fun(1,2,3,'a','b') # (1, 2, 3, 'a', 'b')
```
形参名*parameter_list中的*让Python创建一个名为parameter_list的空元组，并将接收到的所有的值放在这个元组中。Python将实参封装到一个元组中，即便函数只接收到一个值也是如此

### 使用任意数量的关键字实参

```python
def fun(first, *parameter_list):
  print(first)
	print(parameter_list)

fun(1,2,3,'a','b') 
# 1 
# (2, 3, 'a', 'b')
```

### 将函数存储在模块中
通过将函数存储在独立的文件中，可隐藏程序代码的细节，将重点放在高层逻辑上。这还能让你在不同的程序中复用你的代码
#### 导入整个模块
tools.py
```python
def func1():
  pass

def func2(x, y):
  pass
```

make.py
```python
# 导入整个模块
import tools

# 使用模块中的函数
tools.func1()
tools.func2(1, 2)
```

Python读取这些文件时，代码行`import tools`让Python打开文件tools.py，并将其中的所有函数都复制到这个程序中。你看不到复制的代码，因为这个程序运行时，Python在幕后复制这些代码。你只需知道在make.py中可以使用tools.py中定义的所有函数

#### 导入特定的函数
make.py
```python
# 导入特定的函数
from tools import fun1

# 使用模块中的函数
func1()
```

#### 使用as给函数指定别名
make.py
```python
from tools import fun1 as mp

mp()
```

#### 使用as给模块指定别名
```python
from tools as t

t.func1()
```

#### 导入模块中的所有函数
使用 `*` 运算符可以导入模块中的所有函数
```python
from tools import *

func1()
```
 
### 函数编写指南
1.函数名只能使用小写字母和下划线<br />2.多个函数可以使用两个空行将相邻的函数分开，这样更容易知道函数从哪里开始和结束<br />3.所有的import语句都放在文件开头，唯一例外的是，文件开头使用了注释来描述整个程序<br />4.给形参指定默认值时，等号两边不要有空格
```python
def dog(name='Tom', age=2):
	pass
```