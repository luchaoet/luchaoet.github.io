---
title: TypeScript—基础类型
date: 2019-03-11 10:06:16
tags: ['TypeScript', 'TS', '数据类型']
summary:
---
### Any
有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any`类型来标记这些变量
```typescript
let notSure: any;
notSure = "maybe a string instead";
notSure = false;
```
或者，有一个数组，它包含了不同的类型的数据
```typescript
const list: any[] = [1, true, "free"];
list[1] = 100;
console.log(list) // [1, 100, "free"]
```
<a name="Void"></a>
### Void
某种程度上来说，`void`类型像是与`any`类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`
```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
```
声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`
```typescript
let unusable: void = undefined;
```
<a name="7ef2308a"></a>
### Null 和 Undefined
TypeScript里，`undefined`和`null`两者各自有自己的类型分别叫做`undefined`和`null`。 和 `void`相似，它们的本身的类型用处不是很大
```typescript
const u: undefined = undefined;
const n: null = null;
```
默认情况下`null`和`undefined`是所有类型的子类型。 就是说你可以把 `null`和`undefined`赋值给`number`类型的变量
<a name="Never"></a>
### Never
`never`类型表示的是那些永不存在的值的类型。 例如， `never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 `never`类型，当它们被永不为真的类型保护所约束时<br />`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，_没有_类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使 `any`也不可以赋值给`never`<br />下面是一些返回`never`类型的函数
```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```
<a name="Object"></a>
### Object
`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型<br />使用`object`类型，就可以更好的表示像`Object.create`这样的API
```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```
<a name="89a8faee"></a>
### 类型断言
类型断言有两种形式。 其一是“尖括号”语法
```typescript
const someValue: any = "this is a string";
const strLength: number = (<string>someValue).length;
console.log(strLength) // 16
```
另一个为`as`语法
```typescript
const someValue: any = "this is a string";
const strLength: number = (someValue as string).length;
console.log(strLength) // 16
```
两种形式是等价的，测试发现，当你在TypeScript里使用JSX时，只有 `as`语法断言是被允许的