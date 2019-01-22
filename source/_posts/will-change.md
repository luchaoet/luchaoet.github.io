---
title: will-change
date: 2018-03-10 21:29:50
tags: css
summary: 一个好玩的css属性
---

<span data-type="color" style="color:rgb(0, 0, 0)">will-change属性可以提前通知浏览器我们要对元素做什么动画，这样浏览器可以提前准备合适的优化设置。这样可以避免对页面响应速度有重要影响的昂贵成本。元素可以更快的被改变，渲染的也更快，这样页面可以快速更新，表现的更加流畅。</span>
使用will-change提示浏览器关于即将发生的变形十分简单，添加个CSS属性就行

```
will-change: transform;
```

也可以告诉浏览器要改变元素的滚动条位置，或者多个要变化的属性，写下属性的名字就行，也可以写多个，逗号隔开

```
will-change: transform, opacity;
```
<span data-type="color" style="color:rgb(0, 0, 0)">声明了元素即将进行的变化会让浏览器在渲染页面时做更好的决定，这明显比之前说的3D hacks要好。</span>

## will-change属性的值

1. auto 表示没有明确的意图; 无论是启发式和最优化，用户代理应该应用都和正常情况相同
2. scroll-position 表示开发者期望去在接下来去改变或者有动画应用元素的滚动位置
3. contents 表示开发者期望去在接下来去改变或者有动画应用元素的内容
4. 用来排除关键字 will-change, none, all, auto, scroll-position, and contents, 从之外增加一些通用的关键字
    will-change： transform：
    will-change： opacity：
    will-change： top, left, bottom, right：

如果一个属性无最初的值,在这个元素上这个属性将创建一个堆栈的内容, 明确规定在will-change的属性必须在这个元素上创建一个堆栈的内容.

如果一个属性无最初的值, 这个属性将造成这个元素产生一个包含区块的固定定位的元素, 明确规定在 will-change的属性必须造成这个元素产生一个包含区块的固定定位的元素

# 浏览器兼容性

这个目前[不乐观](http://caniuse.com/#feat=will-change)，相信以后会好
