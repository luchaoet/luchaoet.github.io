---
title: area标签
date: 2019-03-28 09:22:33
tags: ['map', 'area']
summary:
---
用图像做超链接只能让整张图片指向一个链接，那么能否在一张图片上创建多个超链接？这时就需要热区链接。所谓热区链接，就是在图片上划出若干个区域，让每个区域分别链接到不同的网页
<a name="1603327e"></a>
### 热区绘制工具
[http://www.mtmao.com/hot](http://www.mtmao.com/hot)<br />
<img width="400" src="https://cdn.nlark.com/yuque/0/2019/png/115449/1553689426652-836be868-4917-4c42-ac70-79d5e971a6a1.png" />
<a name="06e004ef"></a>
### 代码
img的`usemap`属性值与map的`name`属性值要相等
```html
<div style='width:500px;height:357px;overflow:hidden;font-size:0px;line-height:0px;'>
	<img src="http://www.cssn.cn/wh/zxqy/201505/W020150520612786572643.jpg" border='0' usemap='#map5754509108' alt=''/>
  <map name='map5754509108' id='map5754509108'>
    <area shape='rect' coords='162,41,290,168'  href='javascript:;' target='_blank' title='' alt='' />
    <area shape='rect' coords='324,100,418,194'  href='javascript:;' target='_blank' title='' alt='' />
  </map>
</div>
```
<a name="7e8b2fa2"></a>
### 效果
<img width="400" src="https://cdn.nlark.com/yuque/0/2019/png/115449/1553689250599-aab7828e-a4cc-4792-be6c-44508e663573.png" />
<br />当然林妹妹头部也有热区
<a name="shap"></a>
### shap
定义热区的形状<br />支持矩形`rect`，圆形`circle`以及多边形`poly`
<a name="coords"></a>
### coords
区域的坐标<br />形状为矩形时，`coords='162,41,290,168'`，四个值，两两一组，分别表示矩形左上角和右下角的坐标<br />形状为圆形时，`coords='200,50,50'`，三个值，前两个表示圆心的坐标，后一个则表示半径<br />形状为多边形时，coords的值两两一组为各个点的坐标
<a name="href"></a>
### href
热区的跳转链接
<a name="target"></a>
### target
与a标签的target属性相同
<a name="title"></a>
### title
与a标签的title属性相同
<a name="alt"></a>
### alt
与a标签的alt属性相同
<a name="d9cb8442"></a>
### 热区的局限
* 样式无法自定义，只有一个`outline`轮廓
* 使用场景少

<a name="42f49daf"></a>
### 其他作用
`a`链接中套链接<br />由于a标签中不能继续嵌套a标签，所以可以在a标签中套map标签