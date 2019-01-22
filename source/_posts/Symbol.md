---
title: Symbol
date: 2018-11-28 20:41:50
tags: ['es6', 'Symbol']
summary: es6新增第七种基础类型——Symbol
---
### 基本数据类型
Number, String, Null, Undefined, Boolean, <span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Object, </span></span>Symbol

es6新增第七种基础类型——Symbol，表示独一无二的值，可以避免变量的冲突
```javascript
typeof Symbol() // symbol

// 表示独一无二的值,每一个Symbol都不同
const t1 = Symbol();
const t2 = Symbol();
console.log(t1 === t2) // false

// Symbol函数可以接受一个字符串作为参数，表示对Symbol实例的描述，
// 主要是为了在控制台显示，或者转为字符串时，比较容易区分。
const s1 = Symbol("foo");
const s2 = Symbol("foo");
console.log(s1 === s2); // false
```

`Symbol`<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">函数前不能使用</span></span>`new`<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">命令，否则会报错。这是因为Symbol是一个原始类型的值，不是对象</span></span>

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Symbol值不能与其他类型的值进行运算</span></span>
```javascript
const s1 = Symbol("foo");
console.log(t1 + 'abc') // Uncaught TypeError: Cannot convert a Symbol value to a string
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Symbol值可以显式转为字符串,布尔值，但不能转为数字</span></span>
```javascript
console.log(s1.toString()) // Symbol(foo)
console.log(Boolean(s1)) // true
console.log(Number(s1)) // 报错 Cannot convert a Symbol value to a number
```

### 作为属性名的Symbol，保证不会出现同名的属性
<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Symbol值作为对象的属性名时不能使用点运算符，同理，在对象的内部使用Symbol值时也必须放在方括号中</span></span>
```javascript
const s1 = Symbol("foo");
const s2 = Symbol("foo");
const f1 = Symbol('fun');
let obj = {
  name: "Jack",
  [f1](arg){console.log(1)}
};
obj[s1] = 'Lucy'
obj[s2] = 'Tom'

console.log(obj)
```


![屏幕快照 2018-11-28 16.27.47.png | center | 512x116](https://cdn.nlark.com/yuque/0/2018/png/115449/1543393697302-d31b5597-7222-4a55-b739-1f73ee8a5689.png "")


### 属性名遍历
<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Symbol值作为属性名时，该属性还是公开属性，不是私有属性</span></span>
<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Symbol在类外部也是可以访问的，只是不会出现在</span></span>`for...in`<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">、</span></span>`for...of`<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">循环中，也不会被</span></span>`Object.keys()`<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">、</span></span>`Object.getOwnPropertyNames()`<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">返回</span></span>
```javascript
console.log(Object.keys(obj)) // ["name"]
console.log(Object.getOwnPropertyNames(obj)) // ["name"]
```

<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">但有一个</span></span>`Object.getOwnPropertySymbols`<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">方法，可以获取指定对象的所有Symbol属性名</span></span>


![屏幕快照 2018-11-28 16.38.51.png | center | 365x98](https://cdn.nlark.com/yuque/0/2018/png/115449/1543394356856-e0f7654b-a3f9-4db6-9f15-a334d83280f9.png "")


### Symbol.for()
当我们希望使用同一个Symbol值，Symbol.for方法可以做到这一点，它会根据给定的键 key，来从 symbol 注册表中找到对应的 symbol，如果找到了，则返回它，否则，<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">否则就新建并返回一个以该字符串为名称的Symbol值</span></span>
```javascript
var s1 = Symbol.for('foo');
var s2 = Symbol.for('foo');
console.log(s1 === s2); // true
```
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">s1和s2都是Symbol值，但是它们都是同样参数的Symbol.for方法生成的，所以实际上是同一个值</span></span>

Symbol.for()与Symbol()这两种写法，都会生成新的Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会
Symbol.for()不会每次调用就返回一个新的Symbol类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值
比如，如果你调用Symbol.for("cat") 30次，每次都会返回同一个Symbol值，但是调用Symbol("cat") 30次，会返回30个不同的Symbol值
```javascript
Symbol.for("bar") === Symbol.for("bar")
// true
Symbol("bar") === Symbol("bar")
// false
```
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">由于Symbol()写法没有登记机制，所以每次调用都会返回一个不同的值</span></span>

### Symbol.keyFor()
为给定符号从全局符号注册表中检索一个共享符号键
Symbol.keyFor方法返回一个已登记的Symbol类型值的key

```javascript
var s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"
 
var s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```
s2属于未登记的Symbol值，所以返回undefined