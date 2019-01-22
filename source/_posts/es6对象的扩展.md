---
title: es6对象的扩展
date: 2019-01-03 23:34:39
tags: ['es6', '对象']
summary:
---
### 属性的简洁表示法
es6允许直接写入变量和函数作为对象的属性和方法，这样书写更加简洁
```javascript
function(name, age){
    return {
        name,
        age
    }
}

// 等同于
function(name, age){
    return {
        name: name
        age: age
    }
}

var Person = {
    name: 'lucy',
    // 等同于 birth: birth
    birth,
    // 等同于 hello: function(){...}
    hello(){...}
}
```

### 属性名表达式
es6允许字面量定义对象时用表达式作为对象的属性名，即把表达式放在括号内
```javascript
let propKey = 'foo';
let say = 'sayHello';
let obj = {
    [propKey]: true,
    ['a'+'b']: 123,
    [say](){return 'hello'}
}
```

但是属性名表达式不能与简洁表示法同时使用，否则会报错
```javascript
let foo = 'foo';
// 错误
let obj = {
    [foo]
} 
// 正确
let obj = {
    [foo]: 123
}
```

### 方法的name属性
函数的name属性返回函数名，对象方法也是函数，因此也有name属性
```javascript
let Person = {
    sayNmame: function(){...}
}
Person.sayName.name // sayName
```

### Object.is()
用来比较两个值是否严格相等（===）
```javascript
Object.is('foo', 'foo') // true
Object.is({}, {}) // false
```
不同之处只有两个
* +0 不等于 -0
* NaN 等于 NaN

### Object.assign(target, source1, source2)
将源对象source的所有可枚举的属性复制到目标对象target
如果有同名属性，则后面的属性会覆盖掉前面的属性
```javascript
var target = {a: 1, b: 2};
var source1 = {b: 3, c: 4};
var source2 = {c: 5};

Object.assign(target, source1, source2)
target // {a: 1, b: 3, c: 5}
```
属性名为Symbol值的属性，也会被Object.assign()复制

### 属性的可枚举性
Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象
```javascript
let obj = {name: 'lucy'};

Object.getOwnPropertyDescriptor(obj, 'name')
// {
//     configurable: true
//     enumerable: true
//     value: "lucy"
//     writable: true
// }
```

### 属性的遍历
### \_\_proto\_\_属性，Object.setPrototypeOf()，Object.getPrototypeOf()
### 对象的扩展运算符