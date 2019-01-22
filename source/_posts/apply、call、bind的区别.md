---
title: apply、call、bind的区别
date: 2018-12-07 21:53:27
tags: ['js', 'apply', 'call', 'bind']
summary:
---
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">call、apply、bind的作用是改变函数运行时this的指向</span></span>
### this指向
JS（ES5）里面有三种函数调用形式
```javascript
func(p1, p2) 
obj.child.method(p1, p2)
func.call(context, p1, p2) // 先不讲 apply
```

第三种调用形式，才是正常调用形式
```javascript
func.call(context, p1, p2)
```

其他两种都是语法糖，可以等价地变为 call 形式
```javascript
func(p1, p2) 
// 等价于
func.call(undefined, p1, p2)

obj.child.method(p1, p2) 
// 等价于
obj.child.method.call(obj.child, p1, p2)
```

所以函数调用其实只有一种形式
```javascript
func.call(context, p1, p2)
```

__this，就是context__
this就是call的第一个参数

回头看上面的函数调用
```javascript
function func(){
  console.log(this)
}
func()
// 等价于 
func.call(undefined) // 或者 func.call()
```
所以此时的this就是undefiend
浏览器有一个规则，如果传的context是 null 或者 undefined，那么 window 对象就是默认的 context（严格模式下默认 context 是 undefined）
__所以this指向的就是window__

```javascript
var obj = {
  foo: function(){
    console.log(this)
  }
}
obj.foo() 
// 等价于
obj.foo.call(obj)
```
所以this指向就是obj

```javascript
var obj = {
  foo: function(){
    console.log(this)
  }
}

var bar = obj.foo
obj.foo() // 转换为 obj.foo.call(obj)，this 就是 obj
bar()
// 转换为 bar.call()
// 由于没有传 context
// 所以 this 就是 undefined
// 最后浏览器给你一个默认的 this —— window 对象
```

<strong>所以</strong><span data-type="color" style="color:rgb(26, 26, 26)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><strong>this 就是你 call 一个函数时，传入的第一个参数</strong></span></span>

### apply
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">apply接受两个参数，第一个参数是要绑定给this的值，第二个参数是一个</span></span><span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>参数数组</code></span></span><span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。当第一个参数为null、undefined的时候，默认指向window</span></span>
```javascript
var obj = {
    message: '哈哈哈'
}
function getName(firstName, lastName) {
    console.log(this.message，firstName，lastName)
}
getName.apply(obj, ['Dot', 'Dolby'])// 哈哈哈，Dot，Dolby
```

### call
```javascript
var obj = {
    message: '哈哈哈'
}
function getName(firstName, lastName) {
    console.log(this.message，firstName，lastName)
}
getName.call(obj, 'Dot', 'Dolby')// 哈哈哈，Dot，Dolby
```
<span data-type="background" style="background-color:rgb(255, 255, 255)"><span data-type="color" style="color:rgb(47, 47, 47)">apply 和 call 的用法几乎相同, 唯一的差别在于：当函数需要传递多个变量时, apply 可以接受一个数组作为参数输入, call 则是接受一系列的单独变量</span></span>

call和apply可用来借用别的对象的方法，这里以call()为例
```javascript
var Chinese = function () {
    this.name = '小明';
}
var American = function () {
    this.getname = function () {
        console.log(this.name);
    }
    Chinese.call(this);
}
var person = new American();
person.getname();       // 小明
```
从上面我们看到， American实例化出来的对象 person 通过 getname 方法拿到了Chinese中的 name。因为American中，Chinese.call(this) 的作用就是使用 Chinese 对象代替 this 对象，那么American 就有了 Chinese 中的所有属性和方法了，相当于 American 继承了 Chinese 的属性和方法

### bind
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">和call很相似，第一个参数是this的指向，从第二个参数开始是接收的参数列表。区别在于bind方法返回值是函数以及bind接收的参数列表的使用</span></span>
```javascript
var obj = {
    name: '小明'
}
function getName(age) {
    console.log(this.name,age)
}

// 将getName（）函数中的this指向obj
var dot = getName.bind(obj)
console.log(dot) // function () { … }
dot(12)  // 小明,12
```
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">bind 方法不会立即执行，而是返回一个改变了上下文 this 后的函数。而原函数 getName 中的 this 并没有被改变，依旧指向全局对象 window</span></span>
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">bind返回对应函数, 便于稍后调用； apply, call则是立即调用</span></span>
