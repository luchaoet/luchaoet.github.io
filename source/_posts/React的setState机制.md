---
title: React的setState机制
date: 2018-11-17 10:05:58
tags: ['react', 'setState']
summary:
---
当this.setState()被调用时，React会重新调用render方法来重新绘制UI

### 异步更新
setState通过一个队列机制实现state的更新
当执行setState时，会将需要更新的state合并后放入状态队列，而不是立刻更新
队列机制可以高效的批量更新state，如果不通过setState直接修改this.state的值，将不会放入状态队列中，当下次调用setState并对状态队列进行合并时，将会忽略之前的直接被修改的state，而造成无法预知的错误

setState流程还是很复杂的，设计也很精巧，避免了重复无谓的刷新组件。它的主要流程如下
1. enqueueSetState将state放入队列中，并调用enqueueUpdate处理要更新的Component
2. 如果组件当前正处于update事务中，则先将Component存入dirtyComponent中。否则调用batchedUpdates处理。
3. batchedUpdates发起一次transaction.perform()事务
4. 开始执行事务初始化，运行，结束三个阶段
    1. 初始化：事务初始化阶段没有注册方法，故无方法要执行
    2. 运行：执行setSate时传入的callback方法，一般不会传callback参数
    3. 结束：更新isBatchingUpdates为false，并执行FLUSH\_BATCHED\_UPDATES这个wrapper中的close方法
5. FLUSH\_BATCHED\_UPDATES在close阶段，会循环遍历所有的dirtyComponents，调用updateComponent刷新组件，并执行它的pendingCallbacks, 也就是setState中设置的callback。

### 循环调用风险
当调用setState时，实际上会执行enqueueSetState方法，并对partialState 以及\_pendingStateQueue更新队列进行合并操作，最终通过enqueueSetState执行state更新
而performUpdateIfNecessary方法会获取\_pendingElement、\_pendingStateQueue、\_pendingForceUpdate 并调用receiveComponent和updateComponent方法进行组件更新
如果在shouldComponentUpdate 或 componentWillUpdate方法中调用setSate
此时this.\_pendingStateQueue != null 则performUpdateIfNecessary方法就会调用updateComponent方法进行组件更新
```javascript
performUpdateIfNecessary: function (transaction) {
  if (this._pendingElement != null) {
    // receiveComponent会最终调用到updateComponent，从而刷新View
    ReactReconciler.receiveComponent(this, this._pendingElement, transaction, this._context);
  } else if (this._pendingStateQueue !== null || this._pendingForceUpdate) {
    // 执行updateComponent，从而刷新View。
    this.updateComponent(transaction, this._currentElement, this._currentElement, this._context, this._context);
  } else {
    this._updateBatchNumber = null;
  }
}
```
但updateComponent方法又会调用shouldComponentUpdate 和 componentWillUpdate方法
因此造成循环调用 使得浏览器内存占满奔溃


![20181116125308.png | center | 685x359](https://cdn.nlark.com/yuque/0/2018/png/115449/1542344018834-803180ad-1c4a-4c80-8a74-1d450bd06270.png "")


### 调用栈
```javascript
import React,{Component} from 'react';

class Example extends Component {
    constructor(){
        super();
        this.state = {
            val:0
        }
    }
    componentDidMount(){
        
        this.setState({
            val:this.state.val + 1
        })
        console.log(this.state.val) // 第1次输出 0

        this.setState({
            val:this.state.val + 1
        })
        console.log(this.state.val) // 第2次输出 0
        
        setTimeout(() => {
            this.setState({
                val:this.state.val + 1
            })
            console.log(this.state.val) // 第3次输出 2

            this.setState({
                val:this.state.val + 1
            })
            console.log(this.state.val) // 第4次输出 3
        })
    }

}  
```
```
                            this.setState()
                                ||
                                ||
                                \/
                        newState存入pending队列
                                ||
                                ||
                                \/
                        调用enqueueUpdate
                                ||
                                ||
                                \/
                        是否处于批量更新模式
                        ||              ||
                        || Y            || N
                        \/              \/
                   将组件保存到       遍历dirtyComponrnts
                dirtyComponents      调用updateComponent
                                     更新pending state or props

                            setState简化调用栈
```

<span data-type="color" style="color:rgb(36, 41, 46)"><span data-type="background" style="background-color:rgb(255, 255, 255)">enqueueUpdate 的代码如下</span></span>
```javascript
function enqueueUpdate(component){
    ensureInjected();
    // 如果不处于批量更新模式
    if(!batchingStrategy.isBatchingUpdates){
        batchingStrategy.batchedUpdates(enqueueUpdate,component);
        return;
    }
    // 如果处于批量更新模式 则将组件保存在 dirtyComponents中
    dirtyComponents.push(component);
}   
```
如果 isBatchingUpdates 为true，则对队列中的更新执行 batchedUpdates 方法
否则只把当前的组件（即调用了setState的组件）放入dirtyComponents数组中
例子中4次setState调用的实现之所以不同 这里逻辑判断起了关键作用
