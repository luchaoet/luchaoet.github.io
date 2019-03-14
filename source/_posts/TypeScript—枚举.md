---
title: TypeScript—枚举
date: 2019-03-14 09:30:34
tags: ['TypeScript', 'TS', '枚举']
summary:
---
<a name="abd98536"></a>
### 数字枚举
```typescript
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
```
如上定义了一个数字枚举， `Up`使用初始化为 `1`。 其余的成员会从 `1`开始自动增长<br />换句话说，`Direction.Up`的值为 `1`， `Down`为 `2`， `Left`为 `3`， `Right`为 `4`<br />还可以完全不使用初始化器
```typescript
enum Direction {
    Up,
    Down,
    Left,
    Right
}
```
现在， `Up`的值为 `0`， `Down`的值为 `1`，其他以此类推<br />使用枚举很简单：通过枚举的属性来访问枚举成员，和枚举的名字来访问枚举类型
```typescript
enum Response {
    No = 0,
    Yes = 1,
}

function respond(recipient: string, message: Response): void {
    // ...
}

respond("Princess Caroline", Response.Yes)
```
<a name="bd7a0d8d"></a>
### 字符串枚举
在一个字符串枚举里，每个成员都必须用字符串字面量，或另外一个字符串枚举成员进行初始化
```typescript
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}
```
<a name="85f6d789"></a>
### 异构枚举
枚举可以混合字符串和数字成员，但是似乎你并不会这么做
```typescript
enum BooleanLikeHeterogeneousEnum {
    No = 0,
    Yes = "YES",
}
```
不建议这么使用
<a name="2336e67c"></a>
### 计算出来的和常量成员
每个枚举成员都带有一个值，它可以是 _常量_或 _计算出来的_。 当满足如下条件时，枚举成员被当作是常量
1. 它是枚举的第一个成员且没有初始化器，这种情况下它被赋予值 `0`
```typescript
enum E { X }
```
<br />1. 它不带有初始化器且它之前的枚举成员是一个 _数字_常量。 这种情况下，当前枚举成员的值为它上一个枚举成员的值加1
```typescript
enum E1 { X, Y, Z }

enum E2 {
    A = 1, B, C
}
```

1. 枚举成员使用常量枚举表达式初始化。 常数枚举表达式是TypeScript表达式的子集，它可以在编译阶段求值。 当一个表达式满足下面条件之一时，它就是一个常量枚举表达式
* 一个枚举表达式字面量（主要是字符串字面量或数字字面量）
* 一个对之前定义的常量枚举成员的引用（可以是在不同的枚举类型中定义的）
* 带括号的常量枚举表达式
* 一元运算符 +, -, ~其中之一应用在了常量枚举表达式
* 常量枚举表达式做为二元运算符 +, -, *, /, %, <<, >>, >>>, &, |, ^的操作对象。 若常数枚举表达式求值后为 NaN或 Infinity，则会在编译阶段报错

所有其它情况的枚举成员被当作是需要计算得出的值
```typescript
enum FileAccess {
    // constant members
    None,
    Read    = 1 << 1,
    Write   = 1 << 2,
    ReadWrite  = Read | Write,
    // computed member
    G = "123".length
}
```
<a name="397e567c"></a>
### const枚举
大多数情况下，枚举是十分有效的方案。 然而在某些情况下需求很严格。 为了避免在额外生成的代码上的开销和额外的非直接的对枚举成员的访问，我们可以使用 `const`枚举。 常量枚举通过在枚举上使用 `const`修饰符来定义
```typescript
const enum Enum {
    A = 1,
    B = A * 2
}
```
常量枚举只能使用常量枚举表达式，并且不同于常规的枚举，它们在编译阶段会被删除。 常量枚举成员在使用的地方会被内联进来。 之所以可以这么做是因为，常量枚举不允许包含计算成员
```typescript
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right]
// ===>
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```
<a name="5d6d19c5"></a>
### 外部枚举
外部枚举用来描述已经存在的枚举类型的形状
```typescript
declare enum Enum {
    A = 1,
    B,
    C = 2
}
```
外部枚举和非外部枚举之间有一个重要的区别，在正常的枚举里，没有初始化方法的成员被当成常数成员。 对于非常数的外部枚举而言，没有初始化方法时被当做需要经过计算的