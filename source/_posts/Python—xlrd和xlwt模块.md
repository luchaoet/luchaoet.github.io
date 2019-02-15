---
title: Python—xlrd和xlwt模块
date: 2019-02-14 00:10:14
tags: ['Python', 'xlrd', 'xlwt']
summary:
---
python操作excel主要用到xlrd和xlwt这两个库，即`xlrd`是读excel，`xlwt`是写excel的库

### 安装
```
pip install xlrd
pip install xlwt
```

### 导入
```python
import xlrd # 读
import xlwt # 写
```

### 读取excel文件
xlrd.open_workbook(filename)
```python
# 文件名以及路径，如果路径或者文件名有中文给前面加一个r拜师原生字符
excel = xlrd.open_workbook(filename)
```

#### 获取所有工作表名称
sheet_names()
```python
# 打开excel表格
excel = xlrd.open_workbook('/Users/lc/Desktop/test.xlsx')
print (excel.sheet_names())  # ['工作表1', '工作表2']

# 检查某个sheet是否导入完毕，返回True或False
excel.sheet_loaded('table2') # sheet_name or indx
```

#### 获取工作表的三种方式
```python
table = excel.sheets()[0]                 # 通过索引顺序获取
table = excel.sheet_by_index(0))					 # 通过索引顺序获取
table = excel.sheet_by_name('table1')		 # 通过名称获取
```

#### 行获取
nrows 获取工作表有效行
```python
table1 = data.sheets()[0]
nrows = table1.nrows # 获取该工作表的有效行
```

row(indx) 获取indx行的数据<br />row_slice(indx, start_indx, end_indx) 截取indx行的一段数据包括start_indx，不包括end_indx
```python
# 获取某行的数据 返回由该行中所有的单元格对象组成的列表
rowx = table.row(1)
# 第二行数据
print(rowx) # [text:'2A', empty:'', text:'2C']

# 截取某行的一段数据
rowx = table.row_slice(0,0,2)
print(rowx) # [text:'1A', empty:'']
```
![20190213142712.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550039245681-75a081a9-9686-488a-8b78-ad2bc5b4ada8.png#align=left&display=inline&height=185&linkTarget=_blank&name=20190213142712.png&originHeight=185&originWidth=252&size=17598&width=252)

row_len(indx) 获取indx行的有效数据长度
```python
table.row_len(0) # 3
```

row_values(indx, start_indx, end_indx)获取indx行指定单元格范围内数据组成的列表```python
values = table.row_values(0,0,3)
print(values) # ['1A', '', '']
```

get_rows() 获取所有行的生成器
```python
for item in table.get_rows():
    print(item)

# [text:'1A', empty:'', empty:'']
# [text:'2A', empty:'', text:'2C']
# [text:'3A', empty:'', empty:'']
# [empty:'', empty:'', text:'4C']
# [empty:'', text:'5B', empty:'']
# [empty:'', empty:'', empty:'']
# [empty:'', empty:'', empty:'']
# [empty:'', empty:'', empty:'']
# [empty:'', text:'9B', empty:'']
```

row_types(indx, start_indx, end_indx) 获取indx行指定范围内单元格的类型

| 返回值 | 类型 |
| --- | --- |
| 0 | empty |
| 1 | string |
| 2 | number |
| 3 | date |
| 4 | boolean |
| 5 | error |


```python
types = table.row_types(1, 0, 3)
print(types) # [1, 0, 1]
```

#### 列获取
ncols 获取列数<br />col(indx) 获取indx列的数据<br />col_slice(indx, start_indx, end_indx) 截取indx列的一段数据包括start_indx，不包括end_indx
col_values(indx, start_indx, end_indx)获取indx列指定单元格范围内数据组成的列表col_types(indx, start_indx, end_indx) 获取indx列指定范围内单元格的类型
#### 单元格获取
cell(rowx, colx) 获取rowx行colx列的单元对象
```python
cell = table.cell(0,0)
print(cell) # text:'1A'
```

cell_value(rowx,colx) 获取rowx行colx列的值
```python
cell = table.cell_value(0,0)
print(cell) # '1A'
```

cell_type(rowx,colx) 获取rowx行colx列的类型
```python
cell = table.cell_type(0,0)
print(cell) # 1
```

### 操作excel文件
#### 新建表格
```python
import xlwt
# 创建excel
file = xlwt.Workbook()
# 创建工作表
table = file.add_sheet('工作表1',cell_overwrite_ok=True)
# 写数据
table.write(0,0,'test')
# 保存 默认保存在当前目录
file.save('/Users/lc/Desktop/demo.xls')
```

#### 设置字体、颜色与单元格背景
write_merge(x, x + m, y, y + n, string, style) 合并单元格 x表示行，y表示列，m表示跨行个数，n表示跨列个数，string表示要写入的单元格内容，style表示单元格样式
```python
for i in range(0x00,0xff):
  	# 初始化样式
    style = xlwt.XFStyle()   
    
    font = xlwt.Font()   # 创建一个文本格式，包括字体、字号和颜色样式特性    
    font.name = u'微软雅黑'  # 设置其字体为微软雅黑     
    font.colour_index = i+1  # 设置其字体颜色     
    font.bold = True  
    font.height = 400 
    style.font = font 

    pattern = xlwt.Pattern()   # 创建一个模式     
    pattern.pattern = xlwt.Pattern.SOLID_PATTERN  
		# 设置单元格背景
    pattern.pattern_fore_colour = i
    # 设置单元格背景颜色 0 = Black, 1 = White, 2 = Red, 3 = Green, 4 = Blue, 5 = Yellow, 6 = Magenta, the list goes on...
    style.pattern = pattern

    table.write_merge(i, i, 0, 1, str(i+1)+'-lucy', style)
```

![屏幕快照 2019-02-15 15.42.11.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550216575067-e194aa06-0d7d-4265-a008-0629f9737122.png#align=left&display=inline&height=719&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-15%2015.42.11.png&originHeight=719&originWidth=227&size=66462&width=227)