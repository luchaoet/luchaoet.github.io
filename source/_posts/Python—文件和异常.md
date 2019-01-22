---
title: Python—文件和异常
date: 2019-01-18 22:41:54
tags: ['Python', '文件', '异常']
summary:
---
### 读取整个文件
test.txt
```
tank n. 坦克; 油[水]箱; 贮水池; 酒量大的人; vt. 把…贮放在柜内; 打败;
tanks n. 坦克; 油[水]箱，罐，槽( tank的名词复数 ); 水罐;
tanker n. 油轮; 油罐车; 一辆坦克乘员中的一名;
tank top 油罐顶盖; 背心装;
```

read.py
```python
with open('test.txt') as file_obj:
    contents = file_obj.read()
    print(contents)
```

![屏幕快照 2019-01-16 18.01.37.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1547632927992-918f5168-1a43-4e6b-9829-981c83dacf9a.png#align=left&display=inline&height=93&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-01-16%2018.01.37.png&originHeight=93&originWidth=556&size=88298&width=556)

关键字with在不再需要访问文件后将其关闭<br />open()函数接受一个参数，要打开的文件名称<br />read()读取文件的全部内容，并将其作为一个长长的字符串存储在变量contents中

#### 文件路径
```python
with open('/Users/lc/Desktop/demo/pythonDemo/file/test.txt') as file_obj:
    contents = file_obj.read()
    print(contents)
```

而在window系统中
```python
with open('C:\Users\lc\Desktop\demo\pythonDemo\file\test.txt') as file_obj:
    contents = file_obj.read()
    print(contents)
```

### 写入文件
```python
with open('test.txt', 'a') as file_obj:
    file_obj.write('I Love You')
```

open()第二个参数

| 参数 | 功能 | 描述 |
| --- | --- | --- |
| r | 读取 | 读取文件内容 |
| w | 写入 | 覆盖原内容 |
| a | 附加 | 紧接原内容后面添加 |
| r+ | 读取和写入 | 可读取也可写入 |

#### 写入多行
write()函数不会在你写入的文本末尾添加换行符，因此换行需要在write()中包含换行符
```python
with open('test.txt', 'w') as file_obj:
    file_obj.write('I Love You\n')
    file_obj.write('I Love You\n')
```

#### 附加到文件
添加文件，而不是覆盖原有文件，则使用附加模式打开文件。如果指定的文件不存在，则会新建文件
```python
with open('test.txt', 'a') as file_obj:
    file_obj.write('I Love You\n')
    file_obj.write('I Love You\n')
```

### json.dump()和json.load()
json.fump()接受两个参数：要存储的数据以及可用于存储数据的文件对象
```python
import json

numbers = [1,2,3,4,5,6,7]
file_path = 'num.json'
with open(file_path, 'w') as file_obj:
    json.dump(numbers, file_obj)
```

打开 num.json 发现内容数据为 `[1,2,3,4,5,6,7]`

```python
import json

file_path = 'num.json'
with open(file_path) as file_obj:
    numbers = json.load(file_obj)

print(numbers) # [1,2,3,4,5,6,7]
```

### 异常
Python使用异常的特殊对象来管理程序执行期间发生的错误。每当Python发生错误时，它都会创建一个异常对象。如果你编写了处理异常的代码，程序将继续运行。如果未对异常进行处理，程序将停止，显示异常的报告

#### 使用try-except处理ZeroDivisionError异常
```python
try:
    print(5/0)
except ZeroDivisionError:
    print('You can`t divide by zero')
```

#### else
将可能引发错误的代码放在try-except中，可提高这个程序抵御错误的能力。该示例包含else代码块，依赖于try代码块成功执行的代码都应该放在else中
```python
try:
    print(5/1)
except ZeroDivisionError:
    print('You can`t divide by zero')
else:
    print('没有遇到错误')
```