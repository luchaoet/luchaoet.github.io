---
title: CSS3之mask
date: 2019-03-26 16:08:42
tags: ['css3', 'mask']
summary:
---
<a name="mask"></a>
### mask
css属性 `mask` 允许使用者通过部分或者完全隐藏一个元素的可见区域。这种效果可以通过遮罩或者裁切特定区域的图片
```css
/* Keyword values */
mask: none;

/* Image values */
mask: url(mask.png);                       /* 使用位图来做遮罩 */
mask: url(masks.svg#star);                 /* 使用 SVG 图形中的形状来做遮罩 */

/* Combined values */
mask: url(masks.svg#star) luminance;       /* Element within SVG graphic used as luminance mask */
mask: url(masks.svg#star) 40px 20px;       /* 使用 SVG 图形中的形状来做遮罩并设定它的位置：离上边缘40px，离左边缘20px */
mask: url(masks.svg#star) 0 0/50px 50px;   /* 使用 SVG 图形中的形状来做遮罩并设定它的位置和大小：长宽都是50px */
mask: url(masks.svg#star) repeat-x;        /* Element within SVG graphic used as horizontally repeated mask */
mask: url(masks.svg#star) stroke-box;      /* Element within SVG graphic used as mask extending to the box enclosed by the stroke */
mask: url(masks.svg#star) exclude;         /* Element within SVG graphic used as mask and combined with background using non-overlapping parts */

/* Global values */
mask: inherit;
mask: initial;
mask: unset;
```
mask是以下值的缩写
<a name="mask-image"></a>
### mask-image
设置元素上遮罩层的图像
```css
/* Keyword value */
mask-image: none;

/* <mask-source> value */
mask-image: url(masks.svg#mask1);

/* <image< values */
mask-image: linear-gradient(rgba(0, 0, 0, 1.0), transparent);
mask-image: image(url(mask.png), skyblue);

/* Multiple values */
mask-image: image(url(mask.png), skyblue), linear-gradient(rgba(0, 0, 0, 1.0), transparent);

/* Global values */
mask-image: inherit;
mask-image: initial;
mask-image: unset;
```
<a name="mask-mode"></a>
### mask-mode
`mask-mode`属性的默认值是`match-source`，意思是根据资源的类型自动采用合适的遮罩模式。<br />例如，如果我们的遮罩使用的是SVG中的`<defs>`中的`<mask>`元素，则此时的`mask-mode`属性的计算值是`luminance`，表示基于亮度遮罩。如果是其他场景，则计算值是`alpha`，表示基于透明度遮罩
```css
/* Keyword values */
mask-mode: alpha;
mask-mode: luminance;
mask-mode: match-source;

/* Multiple values */
mask-mode: alpha, match-source;

/* Global values */
mask-mode: inherit;
mask-mode: initial;
mask-mode: unset;
```
<a name="mask-repeat"></a>
### mask-repeat
`mask-repeat`属性的默认值是`repeat`，行为类似于`background-repeat`属性
```css
/* One-value syntax */
mask-repeat: repeat-x;
mask-repeat: repeat-y;
mask-repeat: repeat;
mask-repeat: space;
mask-repeat: round;
mask-repeat: no-repeat;
/* Two-value syntax: horizontal | vertical */
mask-repeat: repeat space;
mask-repeat: repeat repeat;
mask-repeat: round space;
mask-repeat: no-repeat round;
/* Multiple values */
mask-repeat: space round, no-repeat;
mask-repeat: round repeat, space, repeat-x;
/* Global values */
mask-repeat: inherit;
mask-repeat: initial;
mask-repeat: unset;
```
<a name="mask-position"></a>
### mask-position
设置`mask`图层的初始位置<br />`mask-position`和`background-position`支持的属性值和表现基本上都是一模一样的
```css
/* Keyword values */
mask-position: top;
mask-position: bottom;
mask-position: left;
mask-position: right;
mask-position: center;
/* <position> values */
mask-position: 25% 75%;
mask-position: 0px 0px;
mask-position: 10% 8em;
/* Multiple values */
mask-position: top right;
mask-position: 1rem 1rem, center;
/* Global values */
mask-position: inherit;
mask-position: initial;
mask-position: unset;
```
<a name="mask-clip"></a>
### mask-clip
限制元素的绘制区域<br />`mask-clip`属性性质上和`background-clip`类似
```css
/* <geometry-box> values */
mask-clip: content-box;
mask-clip: padding-box;
mask-clip: border-box;
mask-clip: margin-box;
mask-clip: fill-box;
mask-clip: stroke-box;
mask-clip: view-box;
/* Keyword values */
mask-clip: no-clip;
/* Non-standard keyword values */
-webkit-mask-clip: border;
-webkit-mask-clip: padding;
-webkit-mask-clip: content;
-webkit-mask-clip: text;
/* Multiple values */
mask-clip: padding-box, no-clip;
mask-clip: view-box, fill-box, border-box;
/* Global values */
mask-clip: inherit;
mask-clip: initial;
mask-clip: unset;
```
<a name="mask-origin"></a>
### mask-origin
设置mask的原点<br />`mask-origin`属性性质上和`background-origin`类似
```
/* Keyword values */
mask-origin: content-box;
mask-origin: padding-box;
mask-origin: border-box;
mask-origin: margin-box;
mask-origin: fill-box;
mask-origin: stroke-box;
mask-origin: view-box;
/* Multiple values */
mask-origin: padding-box, content-box;
mask-origin: view-box, fill-box, border-box;
/* Non-standard keyword values */
-webkit-mask-origin: content;
-webkit-mask-origin: padding;
-webkit-mask-origin: border;
/* Global values */
mask-origin: inherit;
mask-origin: initial;
mask-origin: unset;
```
<a name="mask-size"></a>
### mask-size
设置mask图像的大小<br />`mask-size`属性性质上和`background-size`类似
```css
/* Keywords syntax */
mask-size: cover;
mask-size: contain;
/* One-value syntax */
/* the width of the image (height set to 'auto') */
mask-size: 50%;
mask-size: 3em;
mask-size: 12px;
mask-size: auto;
/* Two-value syntax */
/* first value: width of the image, second value: height */
mask-size: 50% auto;
mask-size: 3em 25%;
mask-size: auto 6px;
mask-size: auto auto;
/* Multiple values */
/* Do not confuse this with mask-size: auto auto */
mask-size: auto, auto;
mask-size: 50%, 25%, 25%;
mask-size: 6px, auto, contain;
/* Global values */
mask-size: inherit;
mask-size: initial;
mask-size: unset;
```
<a name="mask-type"></a>
### mask-type
设置SVG <mask>元素是用作亮度还是alpha蒙版。它适用于<mask>元素本身
```css
/* Keyword values */
mask-type: luminance;
mask-type: alpha;
/* Global values */
mask-type: inherit;
mask-type: initial;
mask-type: unset;
```
<a name="mask-composite"></a>
### mask-composite
`mask-composite`表示当同时使用多个图片进行遮罩时候的混合方式
```css
/* Keyword values */
mask-composite: add; /* 遮罩累加 */
mask-composite: subtract; /* 遮罩相减。也就是遮罩图片重合的地方不显示。意味着遮罩图片越多，遮罩区域越小 */
mask-composite: intersect; /* 遮罩相交。也就是遮罩图片重合的地方才显示遮罩 */
mask-composite: exclude; /* 遮罩排除。也就是后面遮罩图片重合的地方排除，当作透明处理 */
/* Global values */
mask-composite: inherit;
mask-composite: initial;
mask-composite: unset;
```

详情可至张鑫旭博客，内容详细[客栈说书：CSS遮罩CSS3 mask/masks详细介绍](https://www.zhangxinxu.com/wordpress/2017/11/css-css3-mask-masks/)