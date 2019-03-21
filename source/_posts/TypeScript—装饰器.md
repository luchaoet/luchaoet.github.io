---
title: TypeScript—装饰器
date: 2019-03-18 16:21:01
tags: ['TypeScript', 'TS', '装饰器']
summary:
---
装饰器是一种特殊类型的声明，它能够被附加到类声明，方法， 访问符，属性或参数上。 装饰器使用`@expression`这种形式，`expression`求值后必须为一个函数，它会在运行时被调用，被装饰的声明信息做为参数传入
<a name="aa2924cd"></a>
### 装饰器组合
多个装饰器可以同时应用到一个声明上
```typescript
function f() {
    console.log("f(): evaluated");
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("f(): called");
    }
}

function g() {
    console.log("g(): evaluated");
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("g(): called");
    }
}

class C {
    @f()
    @g()
    func() {}
}

var c = new C();
c.func()
// f(): evaluated
// g(): evaluated
// g(): called
// f(): called
```
<a name="c57754c9"></a>
### 类装饰器
类装饰器在类声明之前被声明（紧靠着类声明）。 类装饰器应用于类构造函数，可以用来监视，修改或替换类定义<br />类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数
```typescript
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}
```
当`@sealed`被执行的时候，它将密封此类的构造函数和原型<br />`Object.seal()`方法封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。当前属性的值只要可写就可以改变<br />重载构造函数的例子
```typescript
function classDecorator<T extends {new(...args:any[]):{}}>(constructor:T) {
    return class extends constructor {
        newProperty = "new property";
        hello = "override";
    }
}

@classDecorator
class Greeter {
    property = "property";
    hello: string;
    constructor(m: string) {
        this.hello = m;
    }
}

console.log(new Greeter("world"));

hello: "override"
newProperty: "new property"
property: "property"
__proto__: Greeter
```
<a name="12bd6111"></a>
### 方法装饰器
方法装饰器声明在一个方法的声明之前（紧靠着方法声明）。 它会被应用到方法的属性描述符上，可以用来监视，修改或者替换方法定义
```typescript
function enumerable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.enumerable = value;
    };
}
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }

    @enumerable(false)
    greet() {
        return "Hello, " + this.greeting;
    }
}
```
这里的`@enumerable(false)`是一个装饰器工厂。 当装饰器 `@enumerable(false)`被调用时，它会修改属性描述符的`enumerable`属性
<a name="ee6f73f5"></a>
### 访问器装饰器
访问器装饰器声明在一个访问器的声明之前（紧靠着访问器声明）。 访问器装饰器应用于访问器的 属性描述符并且可以用来监视，修改或替换一个访问器的定义<br />TypeScript不允许同时装饰一个成员的`get`和`set`访问器。取而代之的是，一个成员的所有装饰的必须应用在文档顺序的第一个访问器上。这是因为，在装饰器应用于一个_属性描述符_时，它联合了`get`和`set`访问器，而不是分开声明的<br />如果访问器装饰器返回一个值，它会被用作方法的属性描述符
```typescript
class Point {
    private _x: number;
    private _y: number;
    constructor(x: number, y: number) {
        this._x = x;
        this._y = y;
    }

    @configurable(false)
    get x() { return this._x; }

    @configurable(false)
    get y() { return this._y; }
}

function configurable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.configurable = value;
    };
}

var point = new Point(4,5)
console.log(point.x)
```
<a name="d6008c91"></a>
### 属性装饰器
属性装饰器声明在一个属性声明之前（紧靠着属性声明）
<a name="87ffc060"></a>
### 参数装饰器
参数装饰器声明在一个参数声明之前（紧靠着参数声明）。 参数装饰器应用于类构造函数或方法声明