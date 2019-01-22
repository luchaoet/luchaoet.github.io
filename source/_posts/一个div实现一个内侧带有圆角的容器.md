---
title: 一个div实现一个内侧带有圆角的容器
date: 2018-12-09 00:17:54
tags: css
summary:
---
效果：


![image.png | center | 132x79](https://cdn.nlark.com/yuque/0/2018/png/115449/1544283738301-4a7677d2-79d9-4d44-8e32-606a85a43dc5.png "")

一般实现是使用一个div套一个圆角的div
这中实现在此不述

现在我们使用一个div来实现
首先来一个div


![粘贴图片.png | center | 115x62](https://cdn.nlark.com/yuque/0/2018/png/115449/1544285160105-84d52be9-dbfa-494f-8207-a770c70e1e26.png "")

```css
{
    width: 100px;
    height: 50px;
    border-radio: 10px;
    border: 1px solid #000; /* 为了展示效果加上边框 */
}
```

使用轮廓代替边框


![粘贴图片2.png | center | 148x86](https://cdn.nlark.com/yuque/0/2018/png/115449/1544285367742-974675b0-4ee7-4c35-8076-6192ba4a2ae3.png "")

```css
{
    width: 100px;
    height: 50px;
    border-radio: 10px;
    border: 1px solid #000; /* 为了展示效果加上边框 */
    outline: 10px solid #000;
}
```
因为border-radio的原因，此时轮廓与内容区之间存在四个角的空白区

我们用阴影来填充


![粘贴图片3.png | center | 137x86](https://cdn.nlark.com/yuque/0/2018/png/115449/1544285567588-9f6b0acd-b21e-4438-bbb5-690cb3da45dd.png "")

```css
{
    width: 100px;
    height: 50px;
    border-radio: 10px;
    border: 1px solid #000; /* 为了展示效果加上边框 */
    outline: 10px solid #000;
    box-shadow: 0 0 0 5px #000;
}
```

唯一的问题就是外围的轮廓outline不占据空间
