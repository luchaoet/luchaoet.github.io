---
title: TypeScript—函数
date: 2019-03-15 22:58:09
tags: ['TypeScript', 'TS', '函数']
summary:
---
<a name="25009be8"></a>
### 函数类型
为函数定义类型
```typescript
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { return x + y; };
```
函数的完整类型
```typescript
let add: (x: number,y: number) => number = function(x: number,y: number): number { 
  return x + y
};
```
没有返回值的函数
```typescript
function voidFnc(): void{
    console.log('没有返回值的方法用void')
}
```
<a name="dcf8ae46"></a>
### 可选参数
TypeScript里的每个函数参数都是必须的，传递给一个函数的参数个数必须与函数期望的参数个数一致
```typescript
function buildName(firstName: string, lastName: string) {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // error, too few parameters
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");
```
JavaScript里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是undefined。 在TypeScript里我们可以在参数名旁使用 `?`实现可选参数的功能
```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

let result1 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result2 = buildName("Bob");
let result3 = buildName("Bob", "Adams");
```
可选参数必须跟在必须参数后面
<a name="2171d1b0"></a>
### 默认参数
在TypeScript里，我们也可以为参数提供一个默认值当用户没有传递这个参数或传递的值是`undefined`时。 它们叫做有默认初始化值的参数
```typescript
function buildName(firstName: string, lastName = "Smith") {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // Bob Smith
let result2 = buildName("Bob", undefined);       // Bob Smith
let result3 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result4 = buildName("Bob", "Adams");
```
<a name="be276e0f"></a>
### 剩余参数
必要参数，默认参数和可选参数有个共同点：它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。 在JavaScript里，你可以使用 `arguments`来访问所有传入的参数<br />在TypeScript里，你可以把所有参数收集到一个变量里
```typescript
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}
```
<a name="9bbd2b0d"></a>
### 函数重载
同名函数，传入不同的参数，实现不同的功能，这就叫作函数重载<br />es5中同名函数，后面会覆盖前面的函数，ts中则不会
```typescript
function overloadingFn(x: number, y: number): number;
function overloadingFn(x: string, y: string): string;

// 上面定义函数的格式，下面定义函数的具体实现
function overloadingFn(x: any, y: any): any {
    return x + y;
}

overloadingFn(1, 2);
overloadingFn('a', 'b');
```

<a name="d86f8699"></a>
### 箭头函数
箭头函数和es6中一样
```typescript
setTimeout(() => {
    console.log('箭头函数')
}, 1000);
```