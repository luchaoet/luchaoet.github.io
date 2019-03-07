---
title: css3新属性
date: 2018-10-18 15:12:25
tags: css3
summary: 列举css3的新属性
---
<a name="e8dabc40"></a>
## 1. 边框
* `border-radius`：圆角边框

* `box-shadow`：边框阴影

* `border-image`：边框图片


<a name="822d38fc"></a>
## 2. 背景
* `background-size`：规定背景图片的尺寸

* `background-origin`：规定背景图片的定位区域

| padding-box | 背景图像相对于内边距框来定位。 |
| --- | --- |
| border-box | 背景图像相对于边框盒来定位。 |
| content-box | 背景图像相对于内容框来定位。 |

* CSS3 允许您为元素使用多个背景图像。

```css
background-image:url(bg_flower.gif),url(bg_flower_2.gif);
```


<a name="c0614d4d"></a>
## 3. 文本效果
* `text-shadow`：文本阴影，可以规定水平阴影、垂直阴影、模糊距离，以及阴影的颜色。

```css
text-shadow: 10px 10px 2px #FF0000;/*(水平偏移，垂直偏移，模糊程度，阴影颜色)*/
```
![](https://cdn.nlark.com/yuque/0/2018/png/115449/1539842792405-f46f9cae-eeae-426d-bb70-c44311950811.png#align=left&display=inline&height=96&originHeight=140&originWidth=320&status=done&width=220)
* `word-wrap`：允许文本进行换行

```css
word-wrap:break-word;
```



<a name="bf124277"></a>
## 4. 字体
* @font-face 规则可以自定义字体

```css
/* 自定义字体 */
@font-face{
  font-family: myFirstFont;
  src: url('Sansation_Light.ttf'),
       url('Sansation_Light.eot'); /* IE9+ */
}
/* 使用字体 */
div{
    font-family:myFirstFont;
}
```

<a name="bb9930ed"></a>
## 5. 2D转换（transform）
* `translate()`：元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数。 
```css
transform: translate(50px,100px);
```


* `rotate()`：元素顺时针旋转给定的角度。允许负值，元素将逆时针旋转。
```css
transform: rotate(30deg);
```


* `scale()`：元素的尺寸缩放，根据给定的宽度（X 轴）和高度（Y 轴）参数缩放相应的倍数。
```css
transform: scale(2,4);
```


* `skew()`：元素翻转给定的角度，根据给定的水平线（X 轴）和垂直线（Y 轴）参数。
```css
transform: skew(30deg,20deg);
```


* `matrix()`： 矩阵，把所有 2D  转换方法组合在一起，需要六个参数，包含数学函数，允许您：旋转、缩放、移动以及倾

```css
div{
    transform:rotate(7deg);
    -ms-transform:rotate(7deg); 	/* IE 9 */
    -moz-transform:rotate(7deg); 	/* Firefox */
    -webkit-transform:rotate(7deg); /* Safari 和 Chrome */
    -o-transform:rotate(7deg); 	/* Opera */
}
```

<a name="f17ca877"></a>
## 6. 3D转换
* `rotateX()`：元素围绕其 X 轴以给定的度数进行旋转。
```
transform: rotateX(120deg);
```


* `rotateY()`：元素围绕其 Y 轴以给定的度数进行旋转。
```
transform: rotateY(130deg);
```


<a name="e86a601e"></a>
## 7. transition 过渡效果，使页面变化更平滑
* `transition-property` ：执行动画对应的属性，例如 color，background 等，可以使用 all 来指定所有的属性。

* `transition-duration`：过渡动画的一个持续时间。

* `transition-timing-function`：在延续时间段，动画变化的速率，常见的有：ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier 。

* `transition-delay`：延迟多久后开始动画。


简写为：`transition: [<transition-property> || <transition-duration> || <transition-timing-function> || <transition-delay>];`

```css
div{
    width:100px;
    height:100px;
    background:blue;

    transition:width 2s;
    -moz-transition:width 2s; /* Firefox 4 */
    -webkit-transition:width 2s; /* Safari and Chrome */
    -o-transition:width 2s; /* Opera */
}
div:hover{
    width:300px;
}
```

<a name="9f4a4f25"></a>
## 8. animation动画
* `animation-name`: 定义动画名称

* `animation-duration`: 指定元素播放动画所持续的时间长

* `animation-timing-function:ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>)`： 指元素根据时间的推进来改变属性值的变换速率，说得简单点就是动画的播放方式。

* `animation-delay`: 指定元素动画开始时间

* `animation-iteration-count:infinite | <number>`：指定元素播放动画的循环次

* `animation-direction: normal | alternate`： 指定元素动画播放的方向，其只有两个值，默认值为normal，如果设置为normal时，动画的每次循环都是向前播放；另一个值是alternate，他的作用是，动画播放在第偶数次向前播放，第奇数次向反方向播放。

* `animation-play-state:running | paused` ：控制元素动画的播放状态。


简写为： `animation:[<animation-name> || <animation-duration> || <animation-timing-function> || <animation-delay> || <animation-iteration-count> || <animation-direction>]`
```css
div{
width:100px;
height:100px;
background:red;
position:relative;

animation:mymove 5s infinite;
-webkit-animation:mymove 5s infinite; /*Safari and Chrome*/
}

@keyframes mymove{
    from {left:0px;}
    to {left:200px;}
}
```

<a name="5a63081f"></a>
## 9. 多列
IE 9 以及更早的版本不支持多列属性

| 属性 | 描述 |
| --- | --- |
| column-count | 规定元素应该被分隔的列数。 |
| column-fill | 规定如何填充列。 |
| column-gap | 规定列之间的间隔。 |
| column-rule | 设置所有 column-rule-* 属性的简写属性。 |
| column-rule-color | 规定列之间规则的颜色。 |
| column-rule-style | 规定列之间规则的样式。 |
| column-rule-width | 规定列之间规则的宽度。 |
| column-span | 规定元素应该横跨的列数。 |
| column-width | 规定列的宽度。 |
| columns | 规定设置 column-width 和 column-count 的简写属性。 |

![](https://cdn.nlark.com/yuque/0/2018/png/115449/1539843730703-39bc2c60-839f-420b-be04-d1e3588c217b.png#align=left&display=inline&height=386&originHeight=726&originWidth=1406&status=done&width=747)

```css
div{
    column-count:3;
    column-gap:30px;
    column-rule:3px outset #ff0000;
}
```

<a name="de9bc607"></a>
## 10. resize
规定可以由用户调整 div 元素的大小<br />如果希望此属性生效，需要设置元素的 overflow 属性，值可以是 auto、hidden 或 scroll。

| 值 | 描述 |
| --- | --- |
| none | 用户无法调整元素的尺寸。 |
| both | 用户可调整元素的高度和宽度。 |
| horizontal | 用户可调整元素的宽度。 |
| vertical | 用户可调整元素的高度。 |

```css
div{
    resize:both;
    overflow:auto;
}
/* 禁止textarea框拉伸 */
textarea{
    resize:none;
}
```

<a name="a01a76be"></a>
## 11. box-sizing
box-sizing属性可以指定盒子模型种类，`content-box`指定盒子模型为W3C，`border-box`为IE盒子模型