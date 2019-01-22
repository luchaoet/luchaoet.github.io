---
title: ES8新特性
date: 2018-11-07 20:34:36
tags: ES8
summary:
---
ECMAScript2017，ECMA-262标准版本的第8版（通常称为ES2017或ES8），于 2017年6月完成

新增特性：
* 字符串填充（padStart 和 padEnd）
* Object.values
* Object.entries
* Object.getOwnPropertyDescriptors()
* 函数参数列表和调用中的尾随逗号
* Async Functions (异步函数)
* 共享内存 和 Atomics

### 字符串填充（padStart 和 padEnd）
向目标字符串前后填充字符，使字符串达到指定的长度
第二个参数默认为空格
```javascript
'test'.padStart(8,'123') // 1231test
'test'.padEnd(8,'123') // test1231

// 异常处理
'test'.padEnd(8, {}) // test[obj
'test'.padEnd(8, true) // testtrue
'test'.padEnd(8, NaN) // testNaNN
```

稍后单独写篇文章，探究下这两个方法在开发中使用方式

### Object.values
返回一个包含对象所有自身属性的数组
```javascript
const obj = {
  name: 'lucy',
  age: 20,
  sex: 'man'
}
Object.values(obj) //  ["lucy", 20, "man"]
```

同样适用于数组
```javascript
const arr = ['lucy', 'Tom', 'Jack'];
Object.values(arr) // ["lucy", "Tom", "Jack"]
```

### Object.entries()
```javascript
const obj = {
  name: 'lucy',
  age: 20,
  sex: 'man'
}
Object.entries(obj) // [["name": "lucy"], ["age", 20], ["sex", "man"]]
```

### <span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Object.getOwnPropertyDescriptors()</span></span>
返回属性的描述符
```javascript
const obj = {
  name: 'lucy',
  age: 20,
  sex: 'man'
}
Object.getOwnPropertyDescriptors(obj)
```

返回的是各属性的描述


![屏幕快照 2018-11-07 14.47.54.png | center | 587x80](https://cdn.nlark.com/yuque/0/2018/png/115449/1541573303193-84c891bf-42f8-4894-bfed-ed75b7ef0e21.png "")


<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Object.getOwnPropertyDescriptors 方法返回一个对象，所有原来的对象的属性名都是该对象的属性名，对应的属性值就是该属性的描述对象</span></span>

### <span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">函数参数列表和调用中的尾随逗号</span></span>
允许在函数申明和函数调用中使用尾逗号
```javascript
foo = (name, age, ) => {
    // code
}
foo('lucy', 20,)
```
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">这个功能同样支持对象字面量和数组字面量后面跟的逗号</span></span>

### Async Functions (异步函数)
```javascript
function fun1() {
  return new Promise((resolve)=>{
    setTimeout(() => resolve('哈哈哈'), 4000)
  })
}
async function fun2() {
  const something = await fun1()
  return something + '吼吼吼'
}
async function fun3() {
  const something = await fun2()
  return something + '哟哟哟'
}

fun3().then((res) => {
  console.log(res) // 哈哈哈吼吼吼哟哟哟
})
```

### 共享内存 和 Atomics
略