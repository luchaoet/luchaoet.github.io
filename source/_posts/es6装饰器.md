---
title: es6装饰器
date: 2018-11-29 22:09:17
tags: ['es6', '装饰器']
summary:
---
装饰器是一个对类进行处理的函数，用来修饰类，比如添加静态变量，及类的实例，比如添加一个属性

### 修饰类
```javascript
@testDecorator
class List {
    // ...
}

function testDecorator(target) {
  target.addedParam = true;
}

console.log(List.addedParam); // true
```
testDecorator就是一个装饰器，它的作就是给类添加了一个静态的属性addedParam，装饰器的默认参数target就是List类本身

以上代码等同于
```javascript
class List{}
List = testDecorator(List) || List()

function testDecorator(target) {
  target.addedParam = true;
}
```

#### 装饰器传入其他参数
```javascript
function testDecorator(desc) {
  return function(target) {
    target.addedParam = desc;
  }
}

//调用testDecorator函数之后，其又返回一个匿名函数，利用闭包的特性使用了decs变量 
@testDecorator("operate  one") 
class List {
}
console.log(List.addedParam) // operate  one
```

#### 添加实例属性
```javascript
function paramDecorator(target) {
  target.prototype.isTestable = true;
}

@paramDecorator
class List{

}
let list = new List();
console.log(list.isTestable); // true
```

#### 修饰类的方法
```javascript
class Person{
  @readonly
  name() {
    console.log('My name is Lucy')
  }
  age() {
    console.log('I am 18 years old')
  }
}

function readonly(target, name, descriptor){
  console.log('target => ',target)
  console.log('name => ',name)
  console.log('descriptor => ',descriptor)
  descriptor.writable = false;
}

let person = new Person();
person.name(); // 'I am 18 years old'
```


![屏幕快照 2018-11-29 16.04.42.png | center | 647x204](https://cdn.nlark.com/yuque/0/2018/png/115449/1543478697144-46c20060-7096-4fb3-8eab-90d5fe37c33c.png "")


因为存在函数提升，所以装饰器不能用于函数
