---
title: React.Fragment
date: 2018-12-04 23:39:40
tags: ['React', 'Fragment']
summary:
---
React中return只能返回一个标签，为此有时候我们不得不写很多嵌套标签去包裹我们真正需要的标签，这无形中增加了许多无意义的标签，还加深了html代码的层级

Vue中就可以使用<template>空标签包裹其他标签，最终的渲染结果将不包含<template>标签

为此React16中引入了相同的概念Fragment

### 使用片段
```javascript
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <div>Hello</div>
        <div>World</div>
      </React.Fragment>
    );
  }
}
```
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">key 是唯一可以传递给 Fragment 的属性。在将来，我们可能增加额外的属性支持，比如事件处理</span></span>

### 简写语法
```javascript
class Columns extends React.Component {
  render() {
    return (
      <>
        <div>Hello</div>
        <div>World</div>
      </>
    );
  }
}
```
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">可以像使用其他元素一样使用</span></span>`<></>`<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">，只是它不支持 键(keys) 或 属性(attributes)</span></span>

同时，React 16开始， render支持返回数组
```javascript
class Columns extends React.Component {
  render() {
    return [
        <div>Hello</div>
        <div>World</div>
      ]
    );
  }
}
```