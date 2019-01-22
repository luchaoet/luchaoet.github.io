---
title: ES7新特性
date: 2018-11-07 20:31:20
tags: ES7
summary:
---
ES7，正式名称为ECMAScript 2016 ，于2016年6月完成
es7只是一个小版本，在es6的基础上只增加了2个特性
* 求幂运算符(\*\*)
* Array.prototype.includes()方法

### 求幂运算符(\*\*)
```javascript
3 ** 2 // 9  效果同： Math.pow(3,2)

let b = 3;
b **= 2; // b = b ** 2;
console.log(b) // 9
```

### Array.prototype.includes(ele, ?index)方法
查找元素是否存在数组中，存在则返回true，否则返回false
接收两个参数，要搜索的值和开始索引的位置（索引默认为0）
```javascript
['a', 'b', 'c'].includes('a') // true
['a', 'b', 'c'].includes('d') // false

['a', 'b', 'c'].includes('a', 1) // false
```