---
title: TypeScript—基础类型
date: 2019-03-11 10:06:16
tags: ['TypeScript', 'TS', '数据类型']
summary:
---
<a name="06e1ad91"></a>
### 布尔值
```typescript
const isDone: boolean = false;
```
<a name="55d4790c"></a>
### 数字
和JavaScript一样，TypeScript里的所有数字都是浮点数。 这些浮点数的类型是 `number`。 除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量
```typescript
const decLiteral: number = 6;
const hexLiteral: number = 0xf00d;
const binaryLiteral: number = 0b1010;
const octalLiteral: number = 0o744;
console.log(decLiteral, hexLiteral, binaryLiteral, octalLiteral) // 6 61453 10 484
```
<a name="cc4dd1da"></a>
### 字符串
```typescript
const name: string = "Lucy";
```
<a name="0e67d4b0"></a>
### 数组
有两种方式可以定义数组<br />第一种，可以在元素类型后面接上`[]`，表示由此类型元素组成的一个数组
```typescript
const list: number[] = [1, 2, 3];
```
第二种方式是使用数组泛型，`Array<元素类型>`
```typescript
const list: Array<number> = [1, 2, 3];
```
<a name="4b0d5348"></a>
### 元组(Tuple)
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为`string`和`number`类型的元组
```typescript
let x: [string, number];
x = ['hello', 10]; 
x = [10, 'hello']; // Error
```
<a name="56975625"></a>
### 枚举
`enum`类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字
```typescript
enum Color {Red, Green, Blue}
const c: Color = Color.Green;
console.log(c) // 1
```
默认情况下，从`0`开始为元素编号。 也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 `1`开始编号
```typescript
enum Color {Red = 1, Green, Blue}
const c: Color = Color.Green;
console.log(c) // 2
```
或者，全部都采用手动赋值
```typescript
enum Color {Red = 1, Green = 2, Blue = 4}
const c: Color = Color.Green; 
console.log(c) // 2
```
枚举类型提供的一个便利是可以由枚举的值得到它的名字。 例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，我们可以查找相应的名字
```typescript
enum Color {Red = 1, Green, Blue}
const colorName: string = Color[2];
console.log(colorName);  // Green
```
<a name="Any"></a>
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
