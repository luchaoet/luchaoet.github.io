---
title: css盒模型
date: 2018-11-16 22:51:23
tags: ['css', '盒模型']
summary: 
---
盒模型<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">由里向外content, padding, border, margin</span></span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">标准模型 和 IE模型(怪异模式)</span></span>
<span data-type="color" style="color:#F5222D">标准模型</span>：盒模型的宽高只是<span data-type="color" style="color:#F5222D">内容（content）</span>的宽高
<span data-type="color" style="color:#F5222D">IE模型</span>：盒模型的宽高是<span data-type="color" style="color:#F5222D">内容(content)+填充(padding)+边框(border)</span>的宽高
设置：（box-sizing）
```css
/* 标准模型 */
box-sizing:content-box;(默认值)

 /*IE模型*/
box-sizing:border-box;
```