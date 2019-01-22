---
title: new一个对象的过程
date: 2018-12-08 22:02:58
tags: ['js', 'new']
summary:
---
![1536826651758-ea6fcc46-cf1b-4a41-9321-1809f5ee0649 下午4.34.22.png | center | 677x283](https://cdn.nlark.com/yuque/0/2018/png/115449/1541062307311-9ada2a77-ffe3-4121-a6d3-10fb3344dc3a.png "")

使用new关键字调用函数 `new ClassA()` 的过程
### 1.创建空对象
```javascript
var obj = {};
```

### 2.设置新对象的`constructor`属性为构造函数的名称，设置新对象的\_\_proto\_\_属性指向构造函数的prototype对象
```javascript
obj.__proto__ = ClassA.prototype;
```

### 3.使用新对象调用函数，函数中的this被指向实例化对象
```javascript
ClassA.call(obj)
```

### 4.初始化完毕，将新对象保存到申明的变量中

如果构造函数没有return 或者 return this或者return的值是基本类型，则返回this，即这个新实例化对象。如果return一个引用类型，则返回这个引用类型