---
title: 去除Chrome浏览器下input黄色背景
date: 2019-02-14 21:36:03
tags: input
summary:
---
### 现象
![屏幕快照 2019-02-14 16.27.42.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550132879654-22010917-6a43-44ef-896c-9dba31fe8418.png#align=left&display=inline&height=192&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-14%2016.27.42.png&originHeight=192&originWidth=355&size=10780&width=355)<br />input输入框自动填充时，会出现黄色背景

### 解决方法一
阴影覆盖，使用`box-shadow`属性，覆盖掉背景颜色
```css
input {
   box-shadow: 0 0 0px 1000px white inset;
}
```

### 解决方法二
控制台查看，发现存在这样一段默认样式，这就是黄色背景的原因
```css
input:-webkit-autofill, textarea:-webkit-autofill, select:-webkit-autofill {
    background-color: rgb(250, 255, 189) !important;
    background-image: none !important;
    color: rgb(0, 0, 0) !important;
}
```
也是使用阴影覆盖掉背景颜色
```css
input:-webkit-autofill, textarea:-webkit-autofill, select:-webkit-autofill {
    -webkit-box-shadow: 0 0 0 1000px #fff inset!important;
}
```

当然也可以使用其他方式