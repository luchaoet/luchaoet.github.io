---
title: TypeScript—高级类型
date: 2019-03-21 18:55:41
tags: ['TypeScript', 'TS', '高级类型']
summary:
---
<a name="d83bf966"></a>
### 交叉类型
交叉类型是将多个类型合并为一个类型。 这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性。 例如，`Person & Serializable & Loggable`同时是`Person`_和_`Serializable`_和_`Loggable`。 就是说这个类型的对象同时拥有了这三种类型的成员。<br />我们大多是在混入（mixins）或其它不适合典型面向对象模型的地方看到交叉类型的使用
```typescript
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};
    for (let id in first) {
        (<any>result)[id] = (<any>first)[id];
    }
    for (let id in second) {
        if (!result.hasOwnProperty(id)) {
            (<any>result)[id] = (<any>second)[id];
        }
    }
    return result;
}

class Person {
    constructor(public name: string) { }
}
interface Loggable {
    log(): void;
}
class ConsoleLogger implements Loggable {
    log() {
        // ...
    }
}
var jim = extend(new Person("Jim"), new ConsoleLogger());
var n = jim.name;
jim.log();
```
<a name="62d5de70"></a>
### 联合类型
联合类型与交叉类型很有关联，但是使用上却完全不同
```typescript
function padLeft(value: string, padding: any) {
    if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}

padLeft("Hello world", 4); // returns "    Hello world"
```
`padLeft`存在一个问题， `padding`参数的类型指定成了 `any`。 这就是说可以传入一个既不是 `number`也不是 `string`类型的参数，这就有点问题了<br />于是使用联合类型代替`any`
```typescript
function padLeft(value: string, padding: string | number) {
    // ...
}

let indentedString = padLeft("Hello world", true); // error
```
联合类型表示一个值可以是几种类型之一。 我们用竖线（ `|`）分隔每个类型，所以 `number | string | boolean`表示一个值可以是 `number`， `string`，或 `boolean`<br />如果一个值是联合类型，我们只能访问此联合类型的所有类型里`共有的成员`
```typescript
interface Bird {
    fly();
    layEggs();
}

interface Fish {
    swim();
    layEggs();
}

function getSmallPet(): Fish | Bird {
    // ...
}

let pet = getSmallPet();
pet.layEggs(); // okay 共有
pet.swim();    // errors
```
<a name="001dd03e"></a>
### 类型保护与区分类型
联合类型适合于那些值可以为不同类型的情况。 但当我们想确切地了解是否为 `Fish`时怎么办？ JavaScript里常用来区分2个可能值的方法是检查成员是否存在。 如之前提及的，我们只能访问联合类型中共同拥有的成员

```typescript
let pet = getSmallPet();

// 每一个成员访问都会报错
if (pet.swim) {
    pet.swim();
}
else if (pet.fly) {
    pet.fly();
}
```
为了让这段代码工作，我们要使用类型断言
```typescript
let pet = getSmallPet();

if ((<Fish>pet).swim) {
    (<Fish>pet).swim();
}
else {
    (<Bird>pet).fly();
}
```
<a name="0bf6d87a"></a>
#### 用户自定义的类型保护
这里可以注意到我们不得不多次使用类型断言。 假若我们一旦检查过类型，就能在之后的每个分支里清楚地知道`pet`的类型的话就好了<br />TypeScript里的类型保护机制让它成为了现实。 类型保护就是一些表达式，它们会在运行时检查以确保在某个作用域里的类型。 要定义一个类型保护，我们只要简单地定义一个函数，它的返回值是一个类型谓词
```typescript
function isFish(pet: Fish | Bird): pet is Fish {
    return (<Fish>pet).swim !== undefined;
}
```
在这个例子里， `pet is Fish`就是类型谓词。 谓词为 `parameterName is Type`这种形式，`parameterName`必须是来自于当前函数签名里的一个参数名<br />每当使用一些变量调用 `isFish`时，TypeScript会将变量缩减为那个具体的类型，只要这个类型与变量的原始类型是兼容的
```typescript
// 'swim' 和 'fly' 调用都没有问题了

if (isFish(pet)) {
    pet.swim();
}
else {
    pet.fly();
}
```
注意TypeScript不仅知道在 `if`分支里 `pet`是 `Fish`类型； 它还清楚在 `else`分支里，一定 _不是_ `Fish`类型，一定是 `Bird`类型
<a name="dabde683"></a>
#### `typeof`类型保护
现在我们回过头来看看怎么使用联合类型书写 `padLeft`代码。 我们可以像下面这样利用类型断言来写
```typescript
function isNumber(x: any): x is number {
    return typeof x === "number";
}

function isString(x: any): x is string {
    return typeof x === "string";
}

function padLeft(value: string, padding: string | number) {
    if (isNumber(padding)) {
        return Array(padding + 1).join(" ") + value;
    }
    if (isString(padding)) {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}
```
然而，必须要定义一个函数来判断类型是否是原始类型，现在我们不必将 `typeof x === "number"`抽象成一个函数，因为TypeScript可以将它识别为一个类型保护。 也就是说我们可以直接在代码里检查类型了
```typescript
function padLeft(value: string, padding: string | number) {
    if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}
```
<a name="0ea0b82d"></a>
#### `instanceof`类型保护
_`instanceof`类型保护_是通过构造函数来细化类型的一种方式
```typescript
interface Padder {
    getPaddingString(): string
}

class SpaceRepeatingPadder implements Padder {
    constructor(private numSpaces: number) { }
    getPaddingString() {
        return Array(this.numSpaces + 1).join(" ");
    }
}

class StringPadder implements Padder {
    constructor(private value: string) { }
    getPaddingString() {
        return this.value;
    }
}

function getRandomPadder() {
    return Math.random() < 0.5 ?
        new SpaceRepeatingPadder(4) :
        new StringPadder("  ");
}

// 类型为SpaceRepeatingPadder | StringPadder
let padder: Padder = getRandomPadder();

if (padder instanceof SpaceRepeatingPadder) {
    padder; // 类型细化为'SpaceRepeatingPadder'
}
if (padder instanceof StringPadder) {
    padder; // 类型细化为'StringPadder'
}
```
<a name="ddc72277"></a>
### 可以为null的类型
TypeScript具有两种特殊的类型， `null`和 `undefined`，它们分别具有值null和undefined<br />默认情况下，类型检查器认为 `null`与 `undefined`可以赋值给任何类型。 `null`与 `undefined`是所有其它类型的一个有效值。 这也意味着，你阻止不了将它们赋值给其它类型<br />`--strictNullChecks`标记可以解决此错误：当你声明一个变量时，它不会自动地包含 `null`或 `undefined`。 你可以使用联合类型明确的包含它们
```typescript
let s = "foo";
s = null; // 错误, 'null'不能赋值给'string'
let sn: string | null = "bar";
sn = null; // 可以

sn = undefined; // error, 'undefined'不能赋值给'string | null'
```
注意，按照JavaScript的语义，TypeScript会把 `null`和 `undefined`区别对待。 `string | null`， `string | undefined`和 `string | undefined | null`是不同的类型
<a name="a4eb6eba"></a>
#### 可选参数和可选属性
使用了 `--strictNullChecks`，可选参数会被自动地加上 `| undefined`
```typescript
function f(x: number, y?: number) {
    return x + (y || 0);
}
f(1, 2);
f(1);
f(1, undefined);
f(1, null); // error, 'null' is not assignable to 'number | undefined'
```
可选属性也会有同样的处理
```typescript
class C {
    a: number;
    b?: number;
}
let c = new C();
c.a = 12;
c.a = undefined; // error, 'undefined' is not assignable to 'number'
c.b = 13;
c.b = undefined; // ok
c.b = null; // error, 'null' is not assignable to 'number | undefined'
```
<a name="10ff2e60"></a>
#### 类型保护和类型断言
由于可以为null的类型是通过联合类型实现，那么你需要使用类型保护来去除 `null`。 幸运地是这与在JavaScript里写的代码一致
```typescript
function f(sn: string | null): string {
    if (sn == null) {
        return "default";
    }
    else {
        return sn;
    }
}
```
这里很明显地去除了 `null`，你也可以使用短路运算符
```typescript
function f(sn: string | null): string {
    return sn || "default";
}
```

<a name="51c4a473"></a>
### 类型别名
<a name="6b6bebc9"></a>
#### 接口 vs. 类型别名
<a name="f1191b5b"></a>
### 字符串字面量类型
<a name="c1c0a5ed"></a>
### 数字字面量类型
<a name="220bf910"></a>
### 枚举成员类型
<a name="8009aaf4"></a>
### 可辨识联合
<a name="947f56ea"></a>
#### 完整性检查
<a name="7ee130a6"></a>
### 多态的 `this`类型
<a name="6f58175e"></a>
### 索引类型
<a name="72ebb08e"></a>
#### 索引类型和字符串索引签名
<a name="27edce9a"></a>
### 映射类型
<a name="8af814bb"></a>
#### 由映射类型进行推断