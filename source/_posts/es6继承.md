---
title: es6继承
date: 2018-11-02 17:34:20
tags: js
summary:
---
<span data-type="color" style="color:rgb(51, 51, 51)">Class 可以通过</span>`extends`<span data-type="color" style="color:rgb(51, 51, 51)">关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多</span>

### constructor()
是ES6对类的默认方法，相当于es5的构造函数，通过 new 命令生成对象实例时自动调用该方法。并且，该方法是类中必须有的，如果没有显示定义，则会默认添加空的constructor( )方法。

### super()
在class方法中，继承是使用 extends 关键字来实现的。子类 必须 在 constructor( )调用 super( )方法，否则新建实例时会报错。
子类是没有自己的 this 对象的，它只能继承自父类的 this 对象，然后对其进行加工，而super( )就是将父类中的this对象继承给子类的。没有 super，子类就得不到 this 对象。

super在子类中一般有三种作用
1. 作为父类的构造函数调用
2. 在普通方法中，作为父类的实例调用
3. 在静态方法中，作为父类调用

作为父类的构造函数调用

```javascript
class Animal{
  constructor(color){
    this.color = color;
  };
}
class Bear extends Animal{
  constructor(color){
    super();
    this.color = color;
  }
}

var dog = new Bear('block')
console.log(dog)
```

<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">在普通方法中调用，此时指向父类的实例</span></span>

```javascript
class Animal{
  constructor(color){
    this.color = color;
  };
  run() {
    return 'run'
  }
}
class Bear extends Animal{
  constructor(color){
    super();
    this.color = color;
    console.log(super.run())
  }
}
```