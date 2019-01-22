---
title: js的变量提升和函数提升
date: 2018-11-30 23:59:21
tags: js
summary:
---
### 作用域
作用域包括全局作用域，函数作用域，块级作用域

#### 全局作用域
最外层的变量和直接赋值的变量拥有全局作用域，任何函数都可以访问
#### 函数作用域
函数内部的全部变量可以在整个函数内使用，不能在全局作用域内使用
#### 块级作用域
es6新增块级作用域的概念。<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">一对花括号{}即为一个块级作用域，</span></span>声明的变量只在所在的代码块内有效

### 变量提升
<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">将变量声明提升到它所在作用域的最开始的部分</span></span>
```javascript
console.log(1, str) // 1, undefined
var str = 'abc';
console.log(2, str) // 2, abc
```

由于变量提升，以上代码实际上是这样执行的
```javascript
var str;
console.log(1, str) // 1, undefined
str = 'abc';
console.log(2, str) // 2, abc
```

变量声明被提升到作用域的最上面，否则的话第一个str输出应该是这样
<span data-type="color" style="color:#F5222D">Uncaught ReferenceError: str is not defined</span>

#### 用var、let、const 声明变量时的区别
var会使变量提升，而let和const不会
```javascript
console.log(a) // undefined
var a = 'A';

console.log(b) // Uncaught ReferenceError: b is not defined
let b = 'B';

console.log(c) // Uncaught ReferenceError: c is not defined
let c = 'C';
```

### 函数提升
js创建函数有两种方式：函数申声明式和函数字面量式，只有函数声明式才存在函数提升
```javascript
console.log(f1); // function f1() {}   
console.log(f2); // undefined  
function f1() {}
var f2 = function() {}
```

以上代码实际上是这样执行的
```javascript
// 函数提升
function f1() {}
console.log(f1); // function f1() {}   
console.log(f2); // undefined 
var f2 = function() {}
```