---
title: js循环遍历对比
date: 2018-10-28 00:14:47
tags: js
summary: 上次面试就问到过for-in/for-of的区别，一时竟然不知道，尴尬
---
### map
```javascript
const array = ['A','B','C','D'];
var arrayOfSquares = array.map((value,index)=>{
    return `${index}:${value}`
})
console.log(arrayOfSquares) // ["0:A", "1:B", "2:C", "3:D"]
```

### for
1. 遍历数组通常使用for循环

### forEach(遍历数组)
```javascript
const arr = ['a', 'b', 'c', 'd'];
arr.forEach(function (value, index) {
    console.log('index:'+index, "value:"+value);
});
```


![屏幕快照 2018-10-27 下午11.07.49.png | center | 408x162](https://cdn.nlark.com/yuque/0/2018/png/115449/1540652893184-028e26dd-62e6-426e-a8ec-755d2fe17a7e.png "")


1. 只能遍历数组
2. forEach不能使用`break` 和 `return`

```javascript
Object.prototype.objCustom = function () {}; 
Array.prototype.arrCustom = function () {};

let iterable = [3, 5, 7];
iterable.foo = "hello";

for (let index in iterable) {
	console.log(index); //  0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let value of iterable) {
	console.log(value); // 3, 5, 7
}
```

### for-in(遍历对象/字符串)可遍历数组但不推荐
```javascript
for(let index in obj){
    console.log(index)
}
```

下面这个例子，原型上的test方法被遍历出来了
```javascript
Object.prototype.test=function(){}
var json= {
	0: 'A',
	1: 'B',
	'name': 'lucy'
}
for(var i in json){
    console.log(i,json[i],json.hasOwnProperty(i));
}
```

test()是原型上的方法，不是实例上的方法


![屏幕快照 2018-11-02 下午2.42.08.png | center | 326x152](https://cdn.nlark.com/yuque/0/2018/png/115449/1541140965446-c4b15d57-6e90-4df4-8e9e-30a2bc537a38.png "")


1. <span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">ES5标准，遍历key</span></span>
2. <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>index</code></span></span><span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">索引为</span></span><span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>字符串</code></span></span><span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">型数字</span></span>
3. <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">使用for-in会遍历数组所有的可枚举属性，包括原型，所以for-in更适合遍历对象，简而言之，for-in是为普通对象设计的，</span></span><span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>不要使用for-in遍历数组</code></span></span>
4. 上面的例子可以看出，原型上的方法被遍历出来了，<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">如果不想遍历原型方法和属性的话，可以在循环内部判断一下,hasOwnPropery方法可以判断某属性是否是该对象的实例属性</span></span>

### for-of(遍历数组/字符串)
```javascript
for(let value of obj){
    console.log(value)
}
```

1. ES6标准，遍历value
2. for-of适用遍历数/数组对象/字符串/map/set等拥有迭代器对象的集合.但是不能遍历对象,因为没有迭代器对象.与forEach()不同的是，它可以正确响应break、continue和return语句
3. <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">for-of不支持普通对象，但如果你想迭代一个对象的属性，可以使用for-of遍历Object.keys()，曲线救国</span></span>