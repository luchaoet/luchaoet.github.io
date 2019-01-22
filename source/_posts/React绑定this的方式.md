---
title: React绑定this的方式
date: 2018-12-06 21:37:15
tags: React
summary:
---
<span data-type="color" style="color:rgb(34, 34, 34)"><span data-type="background" style="background-color:rgb(255, 255, 255)">在react组件中，每个方法的上下文都会指向该组件的实例，即自动绑定this为当前组件,而且react还会对这种引用进行缓存，以达到cpu和内存的最大化。在使用了es6 class或者纯函数时，这种自动绑定就不复存在了，我们需要手动实现this的绑定</span></span>
React组件类的方法没有默认绑定this到组件实例，需要手动绑定
以下是几种绑定的方法

### bind方法
<span data-type="color" style="color:rgb(34, 34, 34)"><span data-type="background" style="background-color:rgb(255, 255, 255)">直接绑定是bind（this）来绑定，但是这样带来的问题是每一次渲染是都会重新绑定一次bind</span></span>
```javascript
export default class Index extends Component {
  constructor(){
    super();
    this.state = {
      count: 0
    }
  }
  render() {
    const { count } = this.state;
    return (
	   <div>
        <div>{count}</div>
        <button onClick={this.handleClick.bind(this)}>点击</button>
      </div>
    );
  }
  handleClick() {
    this.setState({
      count: this.state.count + 1
    })
  }
}
```

### __构造函数内绑定__
<span data-type="color" style="color:rgb(34, 34, 34)"><span data-type="background" style="background-color:rgb(255, 255, 255)">在构造函数 constructor 内绑定this，好处是仅需要绑定一次，避免每次渲染时都要重新绑定，函数在别处复用时也无需再次绑定</span></span>
```javascript
export default class Index extends Component {
  constructor(){
    super();
    this.state = {
      count: 0
    }
    this.handleClick = this.handleClick.bind(this);
  }
  render() {
    const { count } = this.state;
    return (
	   <div>
        <div>{count}</div>
        <button onClick={this.handleClick}>点击</button>
      </div>
    );
  }
  handleClick() {
    this.setState({
      count: this.state.count + 1
    })
  }
}
```

### ::双冒号
不能传参
```javascript
export default class Index extends Component {
  constructor(){
    super();
    this.state = {
      count: 0
    }
  }
  render() {
    const { count } = this.state;
    return (
	   <div>
        <div>{count}</div>
        <button onClick={::this.handleClick}>点击</button>
      </div>
    );
  }
  handleClick() {
    this.setState({
      count: this.state.count + 1
    })
  }
}
```

### 箭头函数
<span data-type="color" style="color:rgb(34, 34, 34)"><span data-type="background" style="background-color:rgb(255, 255, 255)">自动绑定了定义此函数作用域的this</span></span>
```javascript
export default class Index extends Component {
  constructor(){
    super();
    this.state = {
      count: 0
    }
  }
  render() {
    const { count } = this.state;
    return (
	   <div>
        <div>{count}</div>
        <button onClick={this.handleClick}>点击</button>
      </div>
    );
  }
  handleClick = () => {
    this.setState({
      count: this.state.count + 1
    })
  }
}
```