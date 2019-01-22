---
title: Set和Map数据结构
date: 2018-12-12 21:20:19
tags: ['es6', 'Set', 'Map']
summary:
---
### Set
Set数据结构类似于数组，但是成员没有重复的元素（包括元素的数据类型）
<span data-type="color" style="color:rgb(51, 51, 51)">Set 本身是一个构造函数，用来生成 Set 数据结构</span>
```javascript
const s = new Set();
[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
for (let i of s) {
  console.log(i);
} // 2 3 5 4
```


![屏幕快照 2018-12-12 10.33.51.png | center | 207x147](https://cdn.nlark.com/yuque/0/2018/png/115449/1544582045519-d45054da-e495-4a00-920b-431cf66e9f4a.png "")

<span data-type="color" style="color:rgb(51, 51, 51)">向 Set 结构加入成员，结果表明 Set 结构不会添加重复的值</span>

<span data-type="color" style="color:rgb(51, 51, 51)">主要的区别是</span>`NaN`<span data-type="color" style="color:rgb(51, 51, 51)">等于自身，而精确相等运算符认为</span>`NaN`<span data-type="color" style="color:rgb(51, 51, 51)">不等于自身</span>
```javascript
NaN === NaN // false
[...newSet([NaN, NaN])] // [NaN]
```

### 数组去重
利用Set数据结构的特性，用于数据去重
```javascript
// ... 和 Array.from方法可以将Set结构转为数组
[...new Set([1, 1, 2, 3, 3])] // [1, 2, 3]
Array.from(new Set([1, 1, 2, 3, 3]) // [1, 2, 3]
```

### Set属性
Set.size 返回Set实例的成员个数

### Set方法
* `add(value)`：添加某个值，返回 Set 结构本身
* `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功
* `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员
* `clear()`：清除所有成员，没有返回值

### Set遍历操作
* `keys()`：返回键名的遍历器
* `values()`：返回键值的遍历器
* `entries()`：返回键值对的遍历器
* `forEach()`：使用回调函数遍历每个成员
<span data-type="color" style="color:rgb(51, 51, 51)">由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以</span>`keys`<span data-type="color" style="color:rgb(51, 51, 51)">方法和</span>`values`<span data-type="color" style="color:rgb(51, 51, 51)">方法的行为完全一致</span>
```javascript
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

__forEach(）__
```javascript
let set = new Set([1, 4, 9]);
set.forEach((value, key) => console.log(key + ' : ' + value))
// 1 : 1
// 4 : 4
// 9 : 9
```

### WeakSet
<span data-type="color" style="color:rgb(51, 51, 51)">WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别</span>
WeakSet的成员只能是对象，而不能是其他类型的值
WeakSet中的对象都是弱引用，即垃圾回收机制不考虑WeakSet对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于WeakSet之中
这是因为垃圾回收机制依赖引用计数，如果一个值的引用次数不为`0`，垃圾回收机制就不会释放这块内存。结束使用该值之后，有时会忘记取消引用，导致内存无法释放，进而可能会引发内存泄漏。WeakSet 里面的引用，都不计入垃圾回收机制，所以就不存在这个问题。因此，WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。
由于上面这个特点，WeakSet 的成员是不适合引用的，因为它会随时消失。另外，由于 WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 ES6 规定 WeakSet 不可遍历
<span data-type="color" style="color:rgb(51, 51, 51)">这些特点同样适用于WeakMap 结构</span>

### Map
Map结构提供了“值—值”的对应，是一种更完善的Hash结构实现。如果你需要“键值对”的数据结构，Map比Object更合适。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键
```javascript
var m = new Map();
var o = {p: "Hello World"};

m.set(o, "content")
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```
注意，只有对同一个对象的引用，Map结构才将其视为同一个键。这一点要非常小心。
```javascript
var map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
// 上面代码的set和get方法，表面是针对同一个键，但实际上这是两个值，内存地址是不一样的，因此get方法无法读取该键，返回undefined。
```
如果Map的键是一个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map将其视为一个键，包括0和-0。另外，虽然NaN不严格相等于自身，但Map将其视为同一个键

实例属性和方法：size、set、get、has、delete、clear

### Map遍历方法
* `keys()`：返回键名的遍历器。
* `values()`：返回键值的遍历器。
* `entries()`：返回所有成员的遍历器。
* `forEach()`：遍历 Map 的所有成员。