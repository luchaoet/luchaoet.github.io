---
title: 'position:sticky'
date: 2019-01-08 21:00:20
tags: ['position', 'sticky']
summary: position的几个属性，特别介绍一下css3新增属性sticky，这个属性很早之前就见过，可以说是相对定位relative和固定定位fixed的结合
---
position的几个属性，特别介绍一下css3新增属性sticky，这个属性很早之前就见过，可以说是相对定位relative和固定定位fixed的结合
### static
默认值，没有定位，通常用来覆盖掉全局的position设置

### inherit
继承父元素的position属性，但需要注意的是IE8及IE8以下都不支持inherit属性

### relative
相对定位，是指给元素设置了相对于原本位置的定位，元素并不脱离文档流，因此元素原本的位置会被保留，其他的元素位置不会受到影响<br /><br />
### absolute
绝对定位，是指给元素设置了绝对的定位，相对定位的对象是最靠近的（设置了position属性为relative或者absolute）祖先元素，如果祖先元素都没有设置position的属性，那么则相对于document，看过的所以文章说的都是body，这是错误的，这一点特别指出

### fixed
固定定位，直接相对于document定位，是特殊情况下的absolute，通常的使用场景是，页面中的浮层广告位、返回页面顶部按钮等<br /><br />
### sticky
粘性定位，相对定位relative和固定定位fixed的结合<br />使用场景是，导航栏的吸顶，<br />![ayo27-znsak.gif](https://cdn.nlark.com/yuque/0/2019/gif/115449/1546939344420-3e2aca6f-ecf6-410e-b694-69d44765be93.gif#align=left&display=inline&height=434&linkTarget=_blank&name=ayo27-znsak.gif&originHeight=434&originWidth=496&size=1064697&width=496)<br /><br /><br />在浏览器窗口中，或者以下表现为relative，当滚动到浏览器窗口之上以后，则表现为fixed

粘性定位生效条件
1. 必须设置top值，否则行为表现与相对定位相同
1. 父元素一定需要在滚动状态，即不能设置`overflow:hidden`
1. 如果 `position:sticky` 元素的任意父节点定位设置为 `position:relative | absolute | fixed`，则元素相对父元素进行定位，而不会相对 viewprot 定位

<br /><br />兼容性不乐观 [caniuse](https://caniuse.com/#feat=css-sticky)<br />![20190108174814.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1546940919510-3cac90c7-7c4d-431d-b8d0-0f80beba001d.png#align=left&display=inline&height=177&linkTarget=_blank&name=20190108174814.png&originHeight=300&originWidth=1266&size=57441&width=746)

可以使用开源库实现兼容[fixed-sticky](https://github.com/filamentgroup/fixed-sticky)