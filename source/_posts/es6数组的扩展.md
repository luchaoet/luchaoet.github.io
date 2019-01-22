---
title: es6数组的扩展
date: 2018-12-25 22:51:51
tags: ['es6', '数组']
summary:
---
### Array.from()
将类似数组的对象和可遍历对象（包括es6新增的Set和Map数据结构）转为真正的数组

### Array.of()
将一组值转换为数组
```javascript
Array.of(1, 2, 3); // [1, 2, 3]
Array.of(); // []
```
返回参数组成的数组，没有参数则返回空数组

### copyWithin()
在数组中复制指定位置的元素，从指定位置开始覆盖掉原数组的元素，返回修改后的数组
```javascript
// 将3号位置的元素复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]
```

### find()
找到数组中第一个符合条件的元素，并返回
```javascript
[1, 5, 10, 15].find((value, index, arr)=>{
    return value >= 10
})
// 10
```

### findIndex()
找到第一个符合条件的元素的索引，否则返回 -1
```javascript
[1, 5, 10, 15].find((value, index, arr)=>{
    return value >= 10
})
// 2
```

这两个方法都可以发现NaN，弥补了indexOf方法的不足
```javascript
[NaN].indexOf(NaN) // -1
```

findIndex()可以借助 Object.is()方法做到
```javascript
[NaN].findIndex(value => Object.is(NaN, value)) // 0
```

### fill()
用给定的值，填充掉数组指定位置的元素
```javascript
['a', 'b', 'c'].fill(7); // [7, 7, 7]

['a', 'b', 'c'].fill(7,1,2); // ['a', 7, 'c']
```

### entries(), keys(), values()
三者都返回遍历器对象，entries()包含数组的键值对，keys()包含数组的键名，values()包含数组的键值，都可用for...of循环遍历
```javascript
for(let index of ['a', 'b'].keys()){
    console.log(index)
} // 0, 1

for(let index of ['a', 'b'].values()){
    console.log(index)
} // a, b

for(let [index,ele] of ['a', 'b'].entries()){
    console.log(index, ele)
} 
// 0, a
// 1, b
```

### includes(ele, ?index)
<span data-type="color" style="color:#F5222D">该方法属于es7的内容</span>
查找元素是否存在数组中，存在则返回true，否则返回false
接收两个参数，要搜索的值和开始索引的位置（索引默认为0）
```javascript
['a', 'b', 'c'].includes('a') // true
['a', 'b', 'c'].includes('d') // false

['a', 'b', 'c'].includes('a', 1) // false
```

### 数组的空位
数组的空位是指数组的某个位置没有任何值，比如Array构造函数返回的数组都是空位
```javascript
Array(3) // [,,,]
```
数组的各个遍历方法对空位的处理规格非常不统一，建议避免出现空位