---
title: flex-basis、flex-grow、flex-shrink
date: 2018-12-27 21:29:13
tags: ['flex', 'flex-basis', 'flex-grow', 'flex-shrink']
summary: 告别蒙蔽
---
三个属性都是在子元素上设置

| flex-basis  | 设置宽度，可以覆盖掉width |
| --- | --- |
| flex-grow | 当父元素宽度大于子元素宽度之和时，子元素通过该属性分配多余空间 |
| flex-shrink | 当父元素宽度小于子元素宽度之和时，子元素通过该属性按比例收缩 |

### flex-basis 
```css
.higherFun-homePage{
    display: flex;
    height: 200px;
    width: 1200px;
    background: #333;
    .box1{
        height: 200px;
        width: 200px;
        flex-basis: 300px;
        background: red;
    }
}
```


![屏幕快照 2018-12-26 14.52.43.png | center | 747x186](https://cdn.nlark.com/yuque/0/2018/png/115449/1545807186216-2a250f24-50b4-46f7-acf9-48a8f1541dad.png "")

宽度为300px，flex-basis把width覆盖掉了

### flex-grow
当父元素宽度有剩余时，子元素通过flex-grow属性分配剩余宽度，flex-grow默认值为0，表示不分配，值越大分配越多
```css
.higherFun-homePage{
    display: flex;
    height: 200px;
    width: 1200px;
    background: #333;
    .box1{
        height: 200px;
        flex-basis: 100px;
        flex-grow: 1;
        background: red;
    }
    .box2{
        height: 200px;
        flex-basis: 200px;
        background: #3678;
    }
}
```
box1设置flex-grow:1 ,box2不设置则默认为零，所以将父元素的剩余空间（1200px - 100px - 200px = 900px）分成 1 + 0 = 1份
box1分配1份，box2分配0份，即box1的宽度为 100px + 900px = 1000px, box2的宽度不变，为200px


![20181226151237.png | center | 747x144](https://cdn.nlark.com/yuque/0/2018/png/115449/1545808372208-d8b2890a-b066-4286-a1ea-fc3fdf9214b3.png "")


<strong>所以flex-grow的分配原则是，将父元素的剩余宽度按子元素指定比例</strong><code><strong>flex-grow</strong></code><strong>分配给子元素，与子元素本身的宽度无关</strong>

### flex-shrink
```css
.higherFun-homePage{
    display: flex;
    height: 200px;
    width: 1200px;
    background: #333;
    flex-direction: row;
    .box1{
        height: 200px;
        flex-basis: 600px;
        flex-shrink: 0;
        background: red;
    }
    .box2{
        height: 200px;
        flex-basis: 800px;
        flex-shrink: 0;
        background: #3678;
    }
}
```

box1与box2均设置`flex-shrink: 0`，此时子元素将超去父元素


![20181226155921.png | center | 747x121](https://cdn.nlark.com/yuque/0/2018/png/115449/1545811173675-29a88746-e8d3-4e3f-9b4c-7d3ac30a647c.png "")


若box1设置`flex-shrink: 1`，box2设置`flex-shrink: 2`，则计算方式为


![IMG_4788.JPG | center | 747x560](https://cdn.nlark.com/yuque/0/2018/jpeg/115449/1545811994620-ea3c839f-4943-4c2f-b11b-52a211c0d735.jpeg "")


所以flex-shrink的分配原则是，子元素按照权重比例削减宽度

### <span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">flex</span></span>
flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选
```css
.item {flex: none;}
/*等同于*/
.item {
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: auto;
}
```

```css
.item {flex: auto;}
/*等同于*/
.item {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: auto;
}
```