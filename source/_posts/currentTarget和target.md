---
title: currentTarget和target
date: 2018-10-13 22:39:19
tags: js
summary: 一分钟搞定currentTarget和target
---
## 事件捕获

![image.png | left | 220x191.56462585034012](https://cdn.nlark.com/yuque/0/2018/png/115449/1536906266058-e8c8b45d-e842-4c8e-90a7-f160ea1688c4.png "")


## 事件冒泡


![image.png | left | 220x183.90243902439025](https://cdn.nlark.com/yuque/0/2018/png/115449/1536907184193-06b851fb-e6d7-48c2-96c2-ed178eb5501f.png "")






![image.png | left | 747x187](https://cdn.nlark.com/yuque/0/2018/png/115449/1536896647566-4e228078-ae57-4086-adb9-aa94aab47f45.png "")

情景： 点击元素c，触发c，冒泡到b,a
结论：
`event.target`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">指向</span></span><span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:#FFFB8F">引起触发事件</span></span><span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">的元素</span></span>
`event.currentTarget`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">则是</span></span><span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:#FFFB8F">事件绑定</span></span><span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">的元素 恒等于this</span></span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">只有被点击的那个目标元素的</span></span>`event.target`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">才会等于</span></span>`event.currentTarge`


