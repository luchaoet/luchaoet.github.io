---
title: css中的,>~+
date: 2019-03-23 15:31:55
tags: css
summary:
---
<a name="4508967c"></a>
### a , b
分隔符，a元素与b元素
```html
<--! CSS -->
<style>
  .test-1, .test-2{
  	color: red;
  }
</style>

<--! HTML -->
<div class="test-1">test-1</div>
<div class="test-2">test-2</div>
```
结果<br />![屏幕快照 2019-03-27 15.08.25.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1553670614366-1d7f3869-6c9f-4c69-bb3f-79f0ce01d4a3.png#align=left&display=inline&height=51&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-27%2015.08.25.png&originHeight=51&originWidth=99&size=8159&status=done&width=99)
<a name="721a26d4"></a>
### a > b
a元素下的子元素b
```html
<!-- CSS -->
<style>
  .test > p{
  	color: red;
  }
</style>

<!-- HTML -->
<div class="test">
  <p>p-1</p>
  <div>
    <p>div-p-1</p>
    <p>div-p-2</p>
    <p>div-p-3</p>
  </div>
  <p>p-2</p>
</div>
```
结果<br />![屏幕快照 2019-03-27 15.17.27.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1553671063423-2ade6894-35f9-420a-84ee-3fb32d9c47c5.png#align=left&display=inline&height=185&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-27%2015.17.27.png&originHeight=185&originWidth=114&size=10437&status=done&width=114)<br />
<a name="a32a950d"></a>
### a ~ b
a元素`位置之后`的所有满足b条件的`兄弟元素`
```html
<!-- CSS -->
<style>
  .test~p{
  	color: red;
  }
</style>

<!-- HTML -->
<div class="test">div-1</div>
<p>p-1</p>
<p>p-2</p>
<div>
  <p>div-p-1</p>
  <p>div-p-2</p>
</div>
```
结果<br />![屏幕快照 2019-03-27 15.23.08.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1553671419825-494cb7bb-d57a-4d7c-83a0-c2aa90509324.png#align=left&display=inline&height=181&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-27%2015.23.08.png&originHeight=181&originWidth=109&size=10467&status=done&width=109)
<a name="4cc0d03c"></a>
### a + b
紧跟在a元素后面的兄弟元素b
```html
<!-- CSS -->
<style>
  .test+p{
  	color: red;
  }
</style>

<!-- HTML -->
<div class="test">div-1</div>
<p>p-1</p>
<p>p-2</p>
```
结果<br />![屏幕快照 2019-03-27 15.30.03.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1553671820826-0fc664c2-044d-4064-be3f-b3c9bb1a1557.png#align=left&display=inline&height=114&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-27%2015.30.03.png&originHeight=114&originWidth=121&size=8997&status=done&width=121)