---
title: BFC
date: 2018-12-05 22:19:27
tags: ['css', 'BFC']
summary:
---
### FC
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">FC的全称是：Formatting Contexts，是W3C CSS2.1规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用</span></span>

### BFC
BFC(Block Formatting Contexts)直译为"块级格式化上下文"。Block Formatting Contexts就是页面上的一个隔离的渲染区域，容器里面的子元素不会在布局上影响到外面的元素，反之也是如此
#### 触发条件
* <span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">根元素，即HTML元素</span></span>
* float的值不为none
* overflow的值不为visible
* position的值不为relative和static
* <span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">display的值是inline-block、table-cell、table-caption、flex或者inline-flex</span></span>
#### 作用
__1.阻止元素被浮动元素覆盖__
红色浮动元素覆盖黑色正常元素


![屏幕快照 2018-12-05 11.53.26.png | center | 336x234](https://cdn.nlark.com/yuque/0/2018/png/115449/1543982023023-5231f61a-60c7-411e-a21e-9f5cecc12608.png "")

黑色元素添加`overflow: hidden`属性，设置为BFC，阻止被浮动元素覆盖


![屏幕快照 2018-12-05 11.54.47.png | center | 569x257](https://cdn.nlark.com/yuque/0/2018/png/115449/1543982222429-2b016f36-6247-431e-aadf-08c58df3f7b2.png "")

__2.可以包含浮动元素__
子元素均浮动后，父元素高度塌陷


![屏幕快照 2018-12-05 12.02.45.png | center | 543x161](https://cdn.nlark.com/yuque/0/2018/png/115449/1543982587468-4a1c3378-514f-4a3b-b840-1b19cd63d757.png "")

父元素添加`overflow: hidden`属性，设置为BFC，可以正常包含浮动的子元素


![屏幕快照 2018-12-05 12.03.28.png | center | 469x161](https://cdn.nlark.com/yuque/0/2018/png/115449/1543982623177-570c8165-f367-42a5-b218-db3662a8d6da.png "")

<strong>3.</strong><span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><strong>两个相邻块级子元素分属于不同的BFC时可以阻止margin重叠</strong></span></span>
<span data-type="color" style="color:rgb(68, 68, 68)"><span data-type="background" style="background-color:rgb(255, 255, 255)">属于同一个BFC的两个相邻块级子元素的上下margin会发生重叠，(设置writing-mode:tb-rl时，水平margin会发生重叠)</span></span>
没有发生margin重叠


![屏幕快照 2018-12-05 14.24.51.png | center | 244x310](https://cdn.nlark.com/yuque/0/2018/png/115449/1543991106820-7757d5fa-d62f-44f7-9962-3d6f54245357.png "")


### __IFC__
IFC(Inline Formatting Contexts)直译为"内联格式化上下文"，IFC的line box（线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的padding/margin影响)
IFC中的line box一般左右都贴紧整个IFC，但是会因为float元素而扰乱。float元素会位于IFC与与line box之间，使得line box宽度缩短。 同个IFC下的多个line box高度会不同。 IFC中时不可能有块级元素的，当插入块级元素时（如p中插入div）会产生两个匿名块与div分隔开，即产生两个IFC，每个IFC对外表现为块级元素，与div垂直排列。
__作用__
水平居中：当一个块要在环境中水平居中时，设置其为inline-block则会在外层产生IFC，通过text-align则可以使其水平居中
垂直居中：创建一个IFC，用其中一个元素撑开父元素的高度，然后设置其vertical-align:middle，其他行内元素则可以在此父元素下垂直居中

### GFC
GFC(GridLayout Formatting Contexts)直译为"网格布局格式化上下文"，当为一个元素设置display值为grid的时候，此元素将会获得一个独立的渲染区域，我们可以通过在网格容器（grid container）上定义网格定义行（grid definition rows）和网格定义列（grid definition columns）属性各在网格项目（grid item）上定义网格行（grid row）和网格列（grid columns）为每一个网格项目（grid item）定义位置和空间。

### FFC
FFC(Flex Formatting Contexts)直译为"自适应格式化上下文"，display值为flex或者inline-flex的元素将会生成自适应容器（flex container），可惜这个牛逼的属性只有谷歌和火狐支持，不过在移动端也足够了，至少safari和chrome还是OK的，毕竟这俩在移动端才是王道
Flex Box 由伸缩容器和伸缩项目组成。通过设置元素的 display 属性为 flex 或 inline-flex 可以得到一个伸缩容器。设置为 flex 的容器被渲染为一个块级元素，而设置为 inline-flex 的容器则渲染为一个行内元素
伸缩容器中的每一个子元素都是一个伸缩项目。伸缩项目可以是任意数量的。伸缩容器外和伸缩项目内的一切元素都不受影响。简单地说，Flexbox 定义了伸缩容器内伸缩项目该如何布局
