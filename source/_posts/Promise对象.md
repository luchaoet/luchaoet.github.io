---
title: Promise对象
date: 2018-12-12 21:23:46
tags: ['es6', 'Promise']
summary: Promise，异步编程解决方案
---
Promise是一个构造函数，是异步编程的一种解决方案
Promise对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`
```javascript
const promise = new Promise(function(resolve, reject) {
  setTimeout(function(){
    console.log('执行完成');
    resolve('返回数据');
  }, 2000);
});
promise.then(res => {
    console.log(res)
})
// 执行完成
// 返回数据
```

### resolve函数
将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去

### reject函数
将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去

一般我们将Promise包在一个函数中，需要时再去运行它
```javascript
function promiseTest(){
  return new Promise(function(resolve, reject) {
    setTimeout(function(){
      console.log('执行完成');
      resolve('返回数据');
    }, 2000);
    setTimeout(function(){
      reject('失败')
    }, 3000);
  });
}

promiseTest().then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
})
```
promiseTest函数上直接调用then方法，then接收一个函数，有一个参数，该参数即为resolve函数返回的数据
catch方法与then方法一样，它的参数是reject返回的数据
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">这就是Promise的作用了，简单来讲，就是能把原来的回调写法分离出来，在异步操作执行完后，用链式调用的方式执行回调函数</span></span>

### all()
Promise.all<span data-type="color" style="color:rgb(51, 51, 51)">方法用于将多个 Promise 实例，包装成一个新的 Promise 实例</span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">它提供了并行执行异步操作的能力，并且在所有异步操作执行完后才执行回调</span></span>
```javascript
function p1(){
      return new Promise(function(resolve, reject) {
        setTimeout(function(){
          console.log('promiseTest1执行完成');
          resolve('promiseTest1返回数据');
        }, 2000);
      });
    }
    function p2(){
      return new Promise(function(resolve, reject) {
        setTimeout(function(){
          console.log('promiseTest2执行完成');
          resolve('promiseTest2返回数据');
        }, 2000);
      });
    }

    const p = Promise.all([ p1(), p2() ]);

    p.then(res => {
      console.log(res) // ["promiseTest1返回数据", "promiseTest2返回数据"]
    })
    .catch(err => {
      console.log(err)
    })
// promiseTest1执行完成
// promiseTest2执行完成
// ["promiseTest1返回数据", "promiseTest2返回数据"]
```
Promise.all方法接收一个数组作为参数，数组元素都是Promise实例
两个异步操作并行执行，等它们执行完毕，才会进入到p的then里面，并且all会异步操作的结果放进数组中传给then

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">有了all，你就可以并行执行多个异步操作，并且在一个回调中处理所有的返回数据</span></span>

### race()
Promise.race<span data-type="color" style="color:rgb(51, 51, 51)">方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例</span>
all方法是所有的异步操作都完成则进入then方法，而race方法则是，当有一个操作完成则进入then方法，其他的异步操作虽然仍在执行，但不影响进程
```javascript
function promiseTest1(){
  return new Promise(function(resolve, reject) {
    setTimeout(function(){
      console.log('promiseTest1执行完成');
      resolve('promiseTest1返回数据');
    }, 2000);
  });
}
function promiseTest2(){
  return new Promise(function(resolve, reject) {
    setTimeout(function(){
      console.log('promiseTest2执行完成');
      resolve('promiseTest2返回数据');
    }, 1000);
  });
}

Promise.race([
  promiseTest1(), 
  promiseTest2()
])
.then(res => {
  console.log(res) // promiseTest2返回数据
})
.catch(err => {
  console.log(err)
})
// promiseTest2执行完成
// promiseTest2返回数据 -> 直接返回最快完成的异步操作返回的数据，不是数组形式
// promiseTest1执行完成 -> 其他异步操作仍在继续
```

### finally()
finally<span data-type="color" style="color:rgb(51, 51, 51)">方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的</span>
<span data-type="color" style="color:rgb(51, 51, 51)">不管</span>promise<span data-type="color" style="color:rgb(51, 51, 51)">最后的状态，在执行完</span>`then`<span data-type="color" style="color:rgb(51, 51, 51)">或</span>`catch`<span data-type="color" style="color:rgb(51, 51, 51)">指定的回调函数以后，都会执行</span>`finally`<span data-type="color" style="color:rgb(51, 51, 51)">方法指定的回调函数</span>
```javascript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```