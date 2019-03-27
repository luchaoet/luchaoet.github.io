---
title: querySelector, querySelectorAll, matchesSelector
date: 2019-03-27 14:57:01
tags: ['querySelector', 'querySelectorAll', 'matchesSelector']
summary: querySelector(), querySelectorAll(), matchesSelector()
---
<a name="b2301229"></a>
### querySelector()
`querySelector()`方法接收一个CSS选择符，返回与该模式匹配的第一个元素，如果没有找到匹配的元素，返回`null`。该方法既可用于文档document类型，也可用于元素element类型
```javascript
// 通过class获取
document.querySelector('.className');

// 通过id获取
document.querySelector('#idName');

// 通过标签获取
document.querySelector('body');

// 通过元素结合选择器获取
document.querySelector('ul.list');

// 通过属性选择器获取
document.querySelector("[class='className']")
```
<a name="74652b48"></a>
### querySelectorAll()
`querySelectorAll()`接收一个CSS选择符，返回一个类数组对象NodeList的实例<br />![屏幕快照 2019-03-27 14.26.25.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1553668008434-b8bebf5a-82a6-422c-9af4-e1dd1235f75d.png#align=left&display=inline&height=111&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-27%2014.26.25.png&originHeight=111&originWidth=490&size=24194&status=done&width=490)<br />没有匹配元素时，返回空的类数组对象，而不是null
<a name="e8e7faab"></a>
### matchesSelector()
`matchesSelector()`方法接收一个CSS选择符参数，如果调用元素与该选择符相匹配，返回true；否则返回false<br />一般地，使用`matches()`方法来替代`matchesSelector()`方法<br />由于兼容性问题，现在各个浏览器都只支持加前缀的方法。IE9+浏览器支持msMatchesSelector()方法，firefox支持mozMatchesSelector()方法，safari和chrome支持webkitMatchesSelector()方法
```javascript
function matchesSelector(element,selector){
    if(element.matchesSelector){
        return element.matchesSelector(selector);
    }
    if(element.msMatchesSelector){
        return element.msMatchesSelector(selector);
    }
    if(element.mozMatchesSelector){
        return element.mozMatchesSelector(selector);
    }
    if(element.webkitMatchesSelector){
        return element.webkitMatchesSelector(selector);
    }            
}
matchesSelector(document.getElementsByClassName('test')[0], 'div') // true
```