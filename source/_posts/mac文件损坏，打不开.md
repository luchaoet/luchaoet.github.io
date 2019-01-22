---
title: mac文件损坏，打不开
date: 2018-11-05 23:04:43
tags: mac
summary:
---
在网上下了一个破解版软件，安装时出现了这个问题


![屏幕快照 2018-11-05 13.00.46.png | center | 747x282](https://cdn.nlark.com/yuque/0/2018/png/115449/1541394082450-535e7390-fb42-4033-b94d-21ca1936ea7c.png "")


在安装破解版软件或不明来源软件时，mac给出的提示
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">软件经过了汉化或者破解，所以可能被Mac认为已损坏，</span></span>是一种安全机制，<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">苹果出于安全考虑，是只允许安装AppStore或者被认可的开发者出品的软件</span></span>

如何解决

系统偏好设置 -> 安全性与隐私


![11_31_46__11_05_2018.jpg | center | 668x266](https://cdn.nlark.com/yuque/0/2018/jpeg/115449/1541388730149-4698f98c-02ab-4906-be0c-79778626e1f7.jpeg "")


点开 权限选项 如下



![20181105113316.png | center | 668x543](https://cdn.nlark.com/yuque/0/2018/png/115449/1541389087405-879c9483-4f37-4d8d-9901-d7b0957039cd.png "")


在终端运行
```plain
sudo spctl --master-disable
```

发现出现了一个 任何来源



![20181105113734.png | center | 668x543](https://cdn.nlark.com/yuque/0/2018/png/115449/1541389100426-4692b2ff-3d0a-4fb2-81df-07526cd80463.png "")


这样就可以继续安装你的破解版软件了，不会出现上述的问题

---

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">所以你不要想着用盗版是对的</span></span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">就是说，你作为一个穷学生，用盗版，为了上进好学，用辞典的盗版，谁忍心责怪你呢</span></span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">但是你至少知道自己是不对的，有一天不缺钱了，就要用正版</span></span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">不要那么理直气壮，好多中国人用盗版特别理直气壮，大义凌然，是吧</span></span>
—— 罗永浩
