---
title: css单位px、em、rem、vw、vh
date: 2019-03-12 07:07:53
tags: ['px', 'em', 'rem', 'vw', 'vh']
summary:
---
<a name="px"></a>
### px
px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的
<a name="em"></a>
### em
em是相对长度单位，em的值并不是固定的，em会继承父级元素的字体大小，如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸<br />任意浏览器的默认字体高都是16px。所有未经调整的浏览器都符合: 1em=16px<br />那么12px=0.75em,10px=0.625em。为了简化font-size的换算，需要在css中的body选择器中声明`font-size=62.5%`，这就使em值变为 
```
1em = 16px * 62.5% = 10px
==>
10px = 1em
12px = 1.2em
```
也就是说只需要将原来的px数值除以10，然后换上em作为单位就行了
<a name="rem"></a>
### rem
rem是CSS3新增的一个相对单位（root em，根em），相对是HTML根元素字体的大小。这个单位可谓集相对大小和绝对大小的优点于一身，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。目前，除了IE8及更早版本外，所有浏览器均已支持rem。对于不支持它的浏览器，应对方法也很简单，就是多写一个绝对单位的声明。这些浏览器会忽略用rem设定的字体大小<br />rem是相对于根元素<html>，这样就意味着，我们只需要在根元素确定一个参考值，这个参考值设置为多少，完全可以根据您自己的需求来定，一般浏览器默认的字体大小是16px，即默认情况下
```
16px = 1rem
```
若<html>设置`font-size：10px`，则
```
10px = 1rem
```
<html>font-size值的动态计算
```javascript
(function(doc,win){
  var docEl = doc.documentElement,
      resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
  		recalc = function(){
        var clientWidth = docEl.clientWidth;
        if(!clientWidth) return;
        if(clientWidth>=750){
          docEl.style.fontSize = '100px';
        }
        else{
          docEl.style.fontSize = 100 * (clientWidth / 750) + 'px';
        }
      };
  if (!doc.addEventListener) return;
	win.addEventListener(resizeEvt, recalc, false);
	doc.addEventListener('DOMContentLoaded', recalc, false);
})(document,window);
```
此时需要在`head`中添加
```html
<meta 
	name="viewport" 
	content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no" 
/>
```
<a name="a093a834"></a>
### rem进阶版
此时不需要手动设置viewport，该方案自动帮你设置，通过修改viewport  属性放大缩小 initial-scale
```javascript
!function(e){function t(a){if(i[a])return i[a].exports;var n=i[a]={exports:{},id:a,loaded:!1};return e[a].call(n.exports,n,n.exports,t),n.loaded=!0,n.exports}var i={};return t.m=e,t.c=i,t.p="",t(0)}([function(e,t){"use strict";Object.defineProperty(t,"__esModule",{value:!0});var i=window;t["default"]=i.flex=function(e,t){var a=e||100,n=t||1,r=i.document,o=navigator.userAgent,d=o.match(/Android[\S\s]+AppleWebkit\/(\d{3})/i),l=o.match(/U3\/((\d+|\.){5,})/i),c=l&&parseInt(l[1].split(".").join(""),10)>=80,p=navigator.appVersion.match(/(iphone|ipad|ipod)/gi),s=i.devicePixelRatio||1;p||d&&d[1]>534||c||(s=1);var u=1/s,m=r.querySelector('meta[name="viewport"]');m||(m=r.createElement("meta"),m.setAttribute("name","viewport"),r.head.appendChild(m)),m.setAttribute("content","width=device-width,user-scalable=no,initial-scale="+u+",maximum-scale="+u+",minimum-scale="+u),r.documentElement.style.fontSize=a/2*s*n+"px"},e.exports=t["default"]}]);  flex(100, 1);
```
这是阿里团队的高清方案布局代码，所谓高清方案就是根据设备屏幕的DPR（设备像素比，又称DPPX，比如dpr=2时，表示1个CSS像素由4个物理像素点组成） 动态设置 html 的font-size, 同时根据设备DPR调整页面的缩放值，进而达到高清效果<br />代码优势：
* **引用简单，布局简便**
* **根据设备屏幕的DPR,自动设置最合适的高清缩放。**
* **保证了不同设备下视觉体验的一致性。（老方案是，屏幕越大元素越大；此方案是，屏幕越大，看的越多）**
* **有效解决移动端真实1px问题（这里的1px 是设备屏幕上的物理像素）**

总结：<br />1)两个方案默认 1rem = 100px，所以你布局的时候，完全可以按照设计师给你的效果图写各种尺寸<br />2)不是每个地方都要用rem，rem只适用于固定尺寸
<a name="vw"></a>
### vw
viewpoint width，视窗宽度，1vw等于视窗宽度的1%<br />使用vm适配手机端尺寸
```css
@function pxToVw($px, $screen-width: 750) { 
  @return ($px / $screen-width) * 100vw;
}
```

<a name="vh"></a>
### vh
viewpoint height，视窗高度，1vh等于视窗高度的1%

相关属性兼容性，可查看[caniuse](https://caniuse.com/#search=rem)网站