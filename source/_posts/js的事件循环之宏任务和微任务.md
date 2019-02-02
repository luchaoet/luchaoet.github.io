---
title: js的事件循环之宏任务和微任务
date: 2019-02-02 22:57:52
tags: ['js', '事件循环', '宏任务' , '微任务']
summary:
---
看了一篇关于事件循环机制的文章<br />[https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/?utm_source=html5weekly](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/?utm_source=html5weekly)

### js事件循环
同步事件进入主线程，异步事件进入Event Table并注册函数。当指定的事件完成时，Event Table会将这个函数移入Event Queue。主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。上述过程会不断重复，即事件循环Event Loop

![屏幕快照 2019-02-02 下午9.02.38.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1549112579495-e5676849-6c4c-4254-9ab9-78daa288c225.png#align=left&display=inline&height=375&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-02%20%E4%B8%8B%E5%8D%889.02.38.png&originHeight=932&originWidth=994&size=240644&width=400)<br />但是，JS异步还有一个机制，就是遇到宏任务，先执行宏任务，将宏任务放入event queue，然后再执行微任务，将微任务放入eventqueue，但是，这两个queue不是一个queue。当你往外拿的时候先从微任务里拿这个回调函数，然后再从宏任务的queue拿宏任务的回调函数<br />![屏幕快照 2019-02-02 下午8.58.20.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1549112420844-effb74b9-a914-42fe-8c14-d29d572faa99.png#align=left&display=inline&height=376&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-02%20%E4%B8%8B%E5%8D%888.58.20.png&originHeight=932&originWidth=992&size=220714&width=400)

### setTimeout
```javascript
setTimeout(() => {
    task();
},5000)
console.log('执行console');
```

* 首先遇到setTimeout，task()进入Event Table并注册,计时开始
* 执行`console.log('执行console')`
* 5s到了，计时事件`timeout`完成，`task()`进入Event Queue
* 前面已经没有什么任务了，`task()`从Event Queue进入了主线程执行

<br /><br />因为js是单线程，任务只能一个一个地执行，如果前面的任务消耗的时间太久，task()会等待主线程的任务完成再执行，这时等待的时间可能大于5s<br /><br /><br />所以说，定时器的时间意义是该时间段后将setTimeout中的事件放入Event Queue，但是必须等待主线程任务完成才会将setTimeout中的事件从Event Queue进入了主线程执行<br /><br />
### setTimeout(fn,0)
这个之前写过一篇博客讨论过<br />立即将回调函数放入任务队列，这会比其他任务更早放入（先进意味着先出），当主线程任务执行完毕，回调函数fn随即便放入主线程执行

### setInterval
每隔指定的时间将回调函数置入Event Queue，如果前面的任务耗时太久，那么同样需要等待

### Promise与process.nextTick(callback)
process.nextTick(callback)类似node.js版的"setTimeout"，在事件循环的下一次循环中调用 callback 回调函数<br /><br />
### 宏任务/微任务
宏任务包括整体代码script，setTimeout，setInterval<br />微任务包括Promise，process.nextTick<br />不同类型的任务会进入对应的Event Queue

```javascript
setTimeout(function() {
    console.log('setTimeout');
})

new Promise(function(resolve) {
    console.log('promise');
  	resolve();
}).then(function() {
    console.log('then');
})

console.log('console');
```
* 第一轮，先执行宏任务后执行微任务，整段代码作为宏任务进入主线程
* 从上到下，先遇到`setTimeout`，那么将其回调函数注册后分发到宏任务Event Queue
* 接下来遇到了`Promise`，`new Promise`立即执行，`then`函数分发到微任务Event Queue
* 遇到`console.log`，立即执行
* 宏任务运行结束，然后执行微任务，`then`执行，第一轮事件循环结束

* 第二轮，同理，先执行宏任务后执行微任务
* `setTimeout`在宏任务队列，执行
* 没啥事了，结束了

<br /><br />所以结果是
```javascript
// promise
// console
// then
// setTimeout
```

找一段复杂的例子
```javascript
// 1 打印 A
console.log('A');

// 2 将该定时器分发到宏任务Event Queue中
setTimeout(function() {
  	// 8 打印 B
    console.log('B');
  	// 9 将回调函数分发到微任务Event Queue中
    process.nextTick(function() {
      	// 11 打印 C
        console.log('C');
    })
  	// 10 new Promise直接执行，打印 D，then回调函数分发到微任务Event Queue中
    new Promise(function(resolve) {
        console.log('D');
        resolve();
    }).then(function() {
      	// 12 打印 E
        console.log('E')
    })
})

// 3 将回调函数分发到微任务Event Queue中
process.nextTick(function() {
  	// 6 打印 F
    console.log('F');
})

// 4 new Promise直接执行，打印 G，then回调函数分发到微任务Event Queue中
new Promise(function(resolve) {
    console.log('G');
    resolve();
}).then(function() {
  	// 7 打印 H
    console.log('H')
})

//  5 将该定时器分发到宏任务Event Queue中
setTimeout(function() {
  	// 13 打印 I
    console.log('I');
  	// 14 将回调函数分发到微任务Event Queue中
    process.nextTick(function() {
      	// 16 打印 J
        console.log('J');
    })
  	// 15 new Promise直接执行，打印 K，then回调函数分发到微任务Event Queue中
    new Promise(function(resolve) {
        console.log('K');
        resolve();
    }).then(function() {
      	// 17 打印 L
        console.log('L')
    })
})
```
第一轮循环<br />整段代码作为宏任务进入主线程<br />执行 1 2 3 4 5 步骤，宏任务执行完毕<br />执行微任务<br />执行 6 7 步骤

第二轮循环<br />先执行宏任务，第一个定时器<br />执行 8 9 10 步骤<br />执行微任务<br />执行 11 12 步骤

第三轮循环<br />先执行宏任务，第二个定时器<br />执行 13 14 15 步骤<br />执行微任务<br />执行 16 17 步骤

结束 打印顺序 A G F H B D C E I K J L 