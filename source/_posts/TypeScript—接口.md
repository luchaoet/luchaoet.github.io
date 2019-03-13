---
title: TypeScript—接口
date: 2019-03-13 11:31:12
tags: ['TypeScript', 'TS', '接口']
summary:
---
接口是对传入参数进行约束，或者对类里面的属性和方法进行声明和约束，实现这个接口的类必须实现该接口里面属性和方法；typescript中的接口用interface关键字定义<br />接口定义了某一批类所需要遵守的规范，接口不关心这些类的内部状态数据，也不关心这些类里方法的实现细节，它只规定这批类里必须提供某些方法，提供这些方法的类就可以满足实际需要。typescrip中的接口类似于java，同时还增加了更灵活的接口类型，包括属性、函数、可索引和类等
<a name="56de21c1"></a>
### 接口分类
<a name="67bfde7f"></a>
#### 属性接口
对方法传入参数进行约束
```typescript
function printLabel(labelInfo: {label:string}){
    return labelInfo
}
// printLabel({name:'Lucy'});  //错误的写法
printLabel({label: 'Lucy'})
```
要求传入的参数是个对象形式，并且label的属性值为字符串格式<br />下面为属性接口的例子，方法printFullName对传入参数FullName(对象)进行约束
```typescript
interface FullName{
    firstName: string; // 注意;结束
    secondName: string;
    age?: number // 接口的可选属性用?
}

function printFullName(name:FullName) {
    // 传入对象必须包含firstName和secondName，可传可不传age
    return name
}
var obj = {
    firstName:'小',
    secondName:'明',
    age: 20
}
printFullName(obj)
```
属性接口应用：原生js封装ajax
```typescript
interface Config{
    type: string;
    url: string;
    data?: string;
    dataType: string;
}
function ajax(config: Config) {
    var xhr = new XMLHttpRequest
    xhr.open(config.type, config.url, true)
    xhr.send(config.data)
    xhr.onreadystatechange = function() {
        if(xhr.readyState == 4 && xhr.status == 200) {
            if(config.dataType == 'json'){
                console.log(JSON.parse(xhr.responseText))
            }else{
                console.log(xhr.responseText)
            }
        }
    }
}

ajax({
    type: 'get',
    data: 'name=xiaoming',
    url: 'http://****/api/info',
    dataType: 'json'
})
```
<a name="b139c7de"></a>
#### 函数类型接口
为了使用接口表示函数类型，我们需要给接口定义一个调用签名。 它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型<br />这样便可以像使用其它接口一样使用这个函数类型的接口
```typescript
interface SearchFunc {
  (source: string, subString: string): boolean; // 传入的参数和返回值的类型
}
let mySearch: SearchFunc = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```
对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配
```typescript
interface SearchFunc {
  (source: string, subString: string): boolean; // 传入的参数和返回值的类型
}
let mySearch: SearchFunc = function(src: string, sub: string): boolean {
  let result = src.search(sub);
  return result > -1;
}
```
<a name="92df4011"></a>
#### 可索引接口
对索引和传入参数的约束（一般用于对数组、对象的约束）<br />ts中定义数组
```typescript
var arr1:number[] = [1,2]
var arr2:Array = ['1', '2']
```
现在用接口来实现
```typescript
// 对数组的约束
interface UserArr{
    // 索引为number，参数为string
    [index:number]: string
}
var userarr:UserArr = ['a', 'b']

// 对象的约束
interface UserObj{
    // 索引为string，参数为string
    [index:string]: string
}
var userobj:UserObj = { name: '小明', sex: '男' }
```
类类型接口<br />对类的约束，和抽象类抽象有点相似
```typescript
interface Animal{
    // 对类里面的属性和方法进行约束
    name:string;
    eat(str:string):void;
}
// 类实现接口要用implements关键字，必须实现接口里面声明的方法和属性
class Cat implements Animal{
    name:string;
    constructor(name:string){
        this.name = name
    }
    eat(food:string){
        console.log(this.name + '吃' + food)
    }
}
var cat = new Cat('小花')
cat.eat('老鼠')
```
<a name="86e26f66"></a>
### 继承接口
和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里
```typescript
interface Shape {
    color: string;
}

// 继承
interface Square extends Shape {
    sideLength: number;
}
let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
```
一个接口可以继承多个接口，创建出多个接口的合成接口
```typescript
interface Shape {
    color: string;
}
interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}
let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```