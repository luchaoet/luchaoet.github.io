---
title: $(document).ready()与window.onload()的区别
date: 2018-11-04 22:04:49
tags: ['js', 'jQuery']
summary:
---
### window.onload()
* 必须等到页面内容，包括图片等所有元素加载完成之后才执行
* 不能写多个，编写多个只能执行一个

### $(document).ready()
* DOM结构绘制完毕即可执行，不需要等到所有资源加载完毕
* 可以同时写多个，都会执行