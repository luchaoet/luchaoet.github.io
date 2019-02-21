---
title: js的内存空间
date: 2019-02-21 21:52:03
tags: ['js', '内存空间']
summary:
---
JavaScript中的变量分为基本类型和引用类型<br />基本类型就是保存在栈内存中的简单数据段，而引用类型指的是那些保存在堆内存中的对象
### 栈
栈是限定仅在表头进行插入和删除操作的线性表。特点是`后进先出`<br />允许进行插入和删除的一端成为栈顶，另一端称为栈底<br />插入称为进栈（PUSH），删除称为退栈（POP）<br />![IMG_4953.JPG](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1550656615569-c45cd741-1235-48b3-9617-5aeb16b1d651.jpeg#align=left&display=inline&height=348&linkTarget=_blank&name=IMG_4953.JPG&originHeight=1000&originWidth=750&size=210000&width=261)

### 堆
堆是一种比较特殊的数据结构，可以被看做一棵树的数组对象<br />![20190220181242.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550657572573-8227ea4c-bbd5-4403-a304-27d0ca822bb6.png#align=left&display=inline&height=216&linkTarget=_blank&name=20190220181242.png&originHeight=418&originWidth=963&size=91589&width=497)<br />![DINGTALK_IM_3057285245.JPG](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1550658085149-5f25c1a3-0017-4230-9fb1-62f8845630f6.jpeg#align=left&display=inline&height=88&linkTarget=_blank&name=DINGTALK_IM_3057285245.JPG&originHeight=993&originWidth=3787&size=252707&width=335)
### 队列
队列与栈一样，也是一种线性表，不同的是，队列可以在一端添加元素，在另一端取出元素，也就是：`先进先出`。从一端放入元素的操作称为入队，取出元素为出队<br />![DINGTALK_IM_44170428.JPG](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1550657477420-ab5e365f-8c16-4796-b651-293e61187e4b.jpeg#align=left&display=inline&height=106&linkTarget=_blank&name=DINGTALK_IM_44170428.JPG&originHeight=888&originWidth=4021&size=203294&width=480)
### 变量的存放
`基本类型`有Undefined、Null、Boolean、Number 、String和Symbol。这些类型在内存中分别占有固定大小的空间，他们的值保存在栈空间，我们通过按值来访问的

`引用类型`保存在堆内存中，因为这种值的大小不固定，因此不能把它们保存到栈内存中，但内存地址大小的固定的，因此保存在堆内存中，在栈内存中存放的只是该对象的访问地址。当查询引用类型的变量时， 先从栈中读取内存地址， 然后再通过地址找到堆中的值。对于这种，我们把它叫做按引用访问<br />![20190221100701.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550714834962-aade7228-85db-47ac-a2d4-63112d10b2aa.png#align=left&display=inline&height=259&linkTarget=_blank&name=20190221100701.png&originHeight=373&originWidth=681&size=84578&width=473)

```javascript
var a = 20;
var b = 'abc';
var c = true;
var d = {m: 20};
.....
```

### 几个例子
```javascript
var a = 1;
var b = a;
b = 2;
console.log(a) // ???
```
var a = 1;

| a | 1 |
| --- | --- |

var b = a;

| a | 1 |
| :---: | :---: |
| b | 1 |

b = 2;

| a | 1 |
| :---: | :---: |
| b | 2 |

b值被修改，并不会影响到a值，a为1

```javascript
var a = { name: 'Lucy'};
var b = a;
b.name = 'Tom';
console.log(a.name) // ???
```
a,b都是引用类型，栈内存中存放地址指向堆内存的对象，引用类型的复制会为新的变量自动分配一个新的值保存在变量的对象中，但只是引用类型的一个地址指针而已，实际指向的是同一个对象，所以修改`b.name`的值后，`a.name`的值也会被修改

```javascript
var a = { name: 'Lucy'};
var b = a;
a = null;
console.log(b) // ???
```
此时a,b的地址指针都指向`{ name: 'Lucy'}`这个对象，a被赋值修改为一个基本类型null，但不会影响到堆内存中的对象，所以b的值不会受影响

```javascript
var a = {n: 1};
var b = a;
a.x = a = {n: 2};
console.log(a.x) // ???
console.log(b.x) // ???
```
a,b都指向同一个对象`{n: 1}`

不好理解的就是这句
```javascript
a.x = a = {n: 2};
```

啥意思，js赋值运算顺序是从右向左，但是`.`是优先级最高的运算符<br />所以先运算`a.x`<br />拆解之后的执行步骤
```javascript
var a = {n: 1};
var b = a;
a.x
a = {n: 2};
a.x = a;
```

现在我们一步一步来分析
```javascript
var a = {n: 1};
var b = a;
```
![DINGTALK_IM_3096870398.JPG](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1550718849318-98478ce7-357e-43ea-b1bf-4d3aab4b62a2.jpeg#align=left&display=inline&height=275&linkTarget=_blank&name=DINGTALK_IM_3096870398.JPG&originHeight=2023&originWidth=3127&size=315206&width=425)

```javascript
a.x
```
![DINGTALK_IM_1976603065.JPG](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1550719017280-86f24d62-9d8f-4e5b-9021-c8b717d6e175.jpeg#align=left&display=inline&height=248&linkTarget=_blank&name=DINGTALK_IM_1976603065.JPG&originHeight=1343&originWidth=2434&size=207798&width=450)

```javascript
a = {n: 2};
```
![DINGTALK_IM_711758233.JPG](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1550719304042-87daad72-30c9-47ba-b0a4-e409f5c33a7d.jpeg#align=left&display=inline&height=274&linkTarget=_blank&name=DINGTALK_IM_711758233.JPG&originHeight=2010&originWidth=3481&size=422373&width=474)

```javascript
a.x = a;
```
由于一开始js已经先计算了a.x，便已经解析了这个a.x是`{n: 1,x: undefined}`的x，所以在同一条公式的情况下再回来给a.x赋值，也不会说重新解析这个a.x为`{n: 2}`的x<br />![DINGTALK_IM_1641065839.JPG](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1550719619319-d98010bb-f604-4741-998e-1de17a895fb5.jpeg#align=left&display=inline&height=280&linkTarget=_blank&name=DINGTALK_IM_1641065839.JPG&originHeight=2051&originWidth=3169&size=379034&width=432)

所以
```javascript
console.log(a.x) // undefined
console.log(b.x) // {n:2}
```

### const变量申明
const的作用是声明变量为常量，实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量
```javascript
const a = 1;
a = 2; // 报错 Uncaught TypeError: Assignment to constant variable.
```

但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，`const`只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了
```javascript
const a = {};
a.name = 'Lucy';
console.log(a) // {name: 'Lucy'}

const b = [1, 2];
b.push(3);
console.log(b) // [1, 2, 3]
```