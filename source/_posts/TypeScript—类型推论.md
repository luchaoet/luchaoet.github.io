---
title: TypeScript—类型推论
date: 2019-03-17 11:15:20
tags:
summary:
---
如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型
```typescript
let a = 'TypeInference';
a = true; // Type 'true' is not assignable to type 'string'
```
a初始化没有指定类型，ts会推论出a的类型为string，当a = true重新赋值的时候类型不匹配，编译提示错误<br />事实上，它等价于
```typescript
let a:string = 'TypeInference';
a = true; // Type 'true' is not assignable to type 'string'
```
TypeScript 会在没有明确的指定类型的时候推测出一个类型，这就是`类型推论`<br />如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 `any` 类型而完全不被类型检查
```typescript
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```
此时编译通过

ts也会根据上下文进行类型的推断，比如在事件函数中，函数的第一个参数会根据当前绑定的事件类型推断处理事件对象
```typescript
document.onkeydown = function(e) {
  	// KeyboardEvent类型上不存在属性"button"
   	console.log(e.button);  //<- Error Property 'button' does not exist on type 'KeyboardEvent'
};
```
这个例子会得到一个类型错误，ts类型检查器根据当前绑定的事件类onkeydown自动推导e的类型为KeyboardEvent，vscode编译器里鼠标放上去就有e推导出来的类型（e:KeyboardEvent）