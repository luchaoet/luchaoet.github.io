---
title: React Hooks
date: 2018-12-03 23:08:53
tags: ['React', 'Hooks']
summary:
---
React Hooks已经在各大论坛刷屏好久了，公司项目一结束就赶紧学习下，不然感觉自己out了
本文翻译至[官网](https://reactjs.org/docs/hooks-intro.html)

### 钩子介绍
Hook是一项新功能提案，可让您在不编写类的情况下使用状态和其他React功能。它们目前处于React v16.7.0-alpha中，将在v.16.7发布

### State Hook
`useState`是react自带的一个hook函数，它的作用就是给组件声明状态变量。如果你编写一个组件并需要为它添加一些状态，那么之前你必须将它转换为一个类。现在，您可以在现有功能组件中使用Hook
```javascript
import { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

在此之前，我们是这样写的
```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

#### 申明状态变量
`useState`直接在组件内部调用Hook
```javascript
import { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);
}
```
通过数组的结构赋值，接收useState返回的两个值
第一项count是申明的变量，相当于`this.state.count`
第二项是这个变量的更新函数，相当于`this.setState`
useState接受一个参数，即状态的初始值

### 读取State
之前获取state中的值，使用`this.state`
```html
<p>{this.state.count}</p>
```
现在直接在函数中
```html
<p>{count}</p>
```

#### 更新state
之前调用`this.setState()`以更新`count`状态
```html
<button onClick={() => this.setState({ count: this.state.count + 1 })}>
    Click me
</button>
```
在函数中，我们已经拥有了`setCount`与`count`作为变量，所以我们并不需要`this`：
```html
  <button onClick={() => setCount(count + 1)}>
    Click me
  </button>
```

#### 使用多个状态变量
将状态变量声明为一对`[something, setSomething]`也很方便，如果我们想要使用多个状态变量，它可以让我们为不同的状态变量赋予不同的名称
```javascript
function ExampleWithManyStates() {
  // Declare multiple state variables!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
```

更新orange
```javascript
function handleOrangeClick() {
    // Similar to this.setState({ fruit: 'orange' })
    setFruit('orange');
}
```

### Effect Hook
Effect Hook在组件中执行的副作用（side effects）
```javascript
import { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
文档标题包含点击次数信息

使用类时，我们得这样写
```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }

  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```
需要在两个周期函数中去处理
通过Effect Hook可以在组件渲染后执行某些操作，包括第一次渲染
useEffect中定义的副作用函数的执行不会阻碍浏览器更新视图，也就是说这些函数是异步执行的，而之前的componentDidMount或componentDidUpdate中的代码则是同步执行的。这种安排对大多数副作用说都是合理的，但有的情况除外，比如我们有时候需要先根据DOM计算出某个元素的尺寸再重新渲染，这时候我们希望这次重新渲染是同步发生的，也就是说它会在浏览器真的去绘制这个页面前发生
```javascript
/**
 * 监控浏览器变化
 * @return {number} innerWidth 返回当前窗口宽度
 * **/
const useWindowWith = ()=>{
  const [innerWidth, setInnerWidth] = useState(window.innerWidth);
  const handleResize = () => setInnerWidth(window.innerWidth)
  useEffect(()=>{
      window.addEventListener('resize',handleResize)
      return ()=>{
        window.removeEventListener('resize',handleResize)
      }
  })
  return innerWidth
}
```

#### 通过跳过效果优化性能
在某些情况下，在每次渲染后清理或应用效果可能会产生性能问题。在类组件中，我们可以通过在内部`prevProps`或`prevState`内部编写额外的比较来解决这个问题
```javascript
componentDidUpdate(prevProps, prevState) {
  if (prevState.count !== this.state.count) {
    document.title = `You clicked ${this.state.count} times`;
  }
}
```

这个要求很常见，它内置于`useEffect`Hook API中。如果在重新渲染之间没有更改某些值，则可以告诉React跳过这步操作。为此，将数组作为可选的第二个参数传递给`useEffect`：
```javascript
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if count changes
```
作用是：当count没有变化时，跳过该useEffect
第二个参数为可选参数，将来可能会在构建时自动添加上去

### Hook规则
不要在循环，条件或嵌套函数中调用Hook
只从React组件或自定义钩子中调用Hooks

可安装`eslint-plugin-react-hooks@next`插件执行这两条规则
```
npm install eslint-plugin-react-hooks@next


// Your ESLint configuration
{
  "plugins": [
    // ...
    "react-hooks"
  ],
  "rules": {
    // ...
    "react-hooks/rules-of-hooks": "error"
  }
}
```
将来该插件会包含在脚手架中

### 编写自己的Hook
自定义Hook是一个JavaScript函数，名称以use开头。可以调用其他Hook
```javascript
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

使用自定义Hook
```javascript
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

### Hooks API
基础Hooks
* useState
* useEffect
* useContext

高级Hooks
* useReducer
* useCallback
* useMemo
* useRef
* useImperativeMethods
* useLayoutEffect

#### useReducer
useState 的替代方案。 接受类型为 (state, action) => newState 的reducer，并返回与 dispatch 方法配对的当前状态。 （如果你熟悉Redux，你已经知道它是如何工作的）
这是 useState 部分的计数器示例，重写为使用 reducer：
```javascript
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'reset':
      return initialState;
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      // A reducer must always return a valid state.
      // Alternatively you can throw an error if an invalid action is dispatched.
      return state;
  }
}

function Counter({initialCount}) {
  const [state, dispatch] = useReducer(reducer, {count: initialCount});
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'reset'})}>
        Reset
      </button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
    </>
  );
}
```

`useReducer`接受可选的第三个参数`initialAction`。如果提供，则在初始渲染期间应用初始操作。这对于计算包含通过props传递的值的初始状态非常有用
```javascript
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'reset':
      return {count: action.payload};
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      // A reducer must always return a valid state.
      // Alternatively you can throw an error if an invalid action is dispatched.
      return state;
  }
}

function Counter({initialCount}) {
  const [state, dispatch] = useReducer(
    reducer,
    initialState,
    {type: 'reset', payload: initialCount},
  );

  return (
    <>
      Count: {state.count}
      <button
        onClick={() => dispatch({type: 'reset', payload: initialCount})}>
        Reset
      </button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
    </>
  );
}
```

`useReducer`通常比`useState`具有涉及多个子值的复杂状态逻辑更可取。它还允许您优化触发深度更新的组件的性能，因为您可以传递dispatch而不是回调

### useRef
`useRef`<span data-type="color" style="color:rgb(0, 0, 0)"> </span>返回一个可变的 ref 对象，其 `.current`<span data-type="color" style="color:rgb(0, 0, 0)"> </span>属性被初始化为传递的参数（`initialValue`<span data-type="color" style="color:rgb(0, 0, 0)">）。返回的对象将存留在整个组件的生命周期中</span>
```javascript
function useRefHandle(initial){
  let ref = useRef(initial)
  let focusHandle = ()=>ref.current.focus();
  return [ref,focusHandle]
}
const App =(props)=> {
    let [inputEl,focusHandle] = useRefHandle(null);
    return (
      <React.Fragment>
        <input ref={inputEl} />
        <button onClick={focusHandle}>搜索</button>
      </React.Fragment>
    )
}
```


### Hook常见问题
此页面回答了一些Hook的常见问题[https://reactjs.org/docs/hooks-faq.html](https://reactjs.org/docs/hooks-faq.html)