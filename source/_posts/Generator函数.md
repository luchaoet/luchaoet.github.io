---
title: Generator函数
date: 2018-12-13 20:32:53
tags: ['es6', 'Generator']
summary:
---
Generator函数是ES6提供的一种异步编程解决方案
申明方式`function*`，执行Generator函数会返回一个遍历器对象，可以依次遍历Generator函数内部的每一个状态

### 特征
* function命令与函数名之间有一个\*
* 函数体内使用yield语句定义不同的内部状态
```javascript
function* helloWorldGenerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
}

var hw = helloWorldGenerator();
hw.next(); // {value: 'hello', done: false}
hw.next(); // {value: 'world', done: false}
hw.next(); // {value: 'ending', done: true}
hw.next(); // {value: undefined, done: true}
```

Generator函数是分段执行的，yield语句是暂停执行的标志，next是恢复执行，done为false表示遍历还没有结束，true则表示结束

### yield
yield语句是暂停执行后面的操作，并将yield后面的表达式的值作为返回对象里的value属性值
下次调用next方法时，再继续往下执行，直到遇到yield语句
如果没有yield语句就一直运行到函数结束，直到遇到return为止，并将return后面的表达式的值作为返回对象里的value属性值
如果没有return，则返回对象的value属性值为undefined
Generator函数可以不使用yield语句
yield语句不能用于普通函数，否则会报错

### next方法参数
yield没有返回值，即返回的都是undefined，如果next方法带参数，该参数会被当做上一条yield语句的返回值
```javascript
function* showWords() {
  for(var i = 0; true; i++){
    var result = yield i;
    if(result) { i = -1};
  }
}

var show = showWords();

console.log(show.next()) // {value: 0, done: false}
console.log(show.next()) // {value: 1, done: false}
console.log(show.next()) // {value: 2, done: false}
console.log(show.next()) // {value: 3, done: false}
console.log(show.next(true)) // {value: 0, done: false} 导致上一步执行next语句时yield返回true,则i等于-1，到这步i则为0
console.log(show.next()) // {value: 1, done: false}
console.log(show.next()) // {value: 2, done: false}
```

### for...of循环
for...of循环可以自动遍历Generator函数，不需要调用next方法
```javascript
function* showWords() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for(let i of showWords()){
  console.log(i)
} // 1 2 3 4 5
```
上面需要注意的是，return语句时，done属性为true，for...of循环被终止，所以6不在for...of循环中

### Generator.prototype.return()
该方法可以返回给定的值，并终止Generator函数的遍历
```javascript
function* showWords() {
  yield 1;
  yield 2;
  yield 3;
}
var g = showWords();
g.next(); // {value: 1, done: false}
g.return('foo'); // {value: 'foo', done: true} 终止遍历，返回给定的值，若不给定则返回undefined
g.next(); // {value: undefined, done: true}
```

### yield\* 语句
使用yeild\*语句，在一个Generator函数中执行另一个Generator函数
