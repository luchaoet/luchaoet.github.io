---
title: dva引入sass
date: 2018-10-24 13:05:10
tags: ['dva', 'sass']
summary: dva快速引入sass
---
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">dva-cli 默认开启了css-modules</span></span>
用惯了sass的同学就有点不爽了
下面介绍一下，如何快速的引入sass

## 1.安装相关依赖
```javascript
npm install node-sass sass-loader --save
```

下面就很简单不用解释了 


![粘贴图片.png | center | 665x367](https://cdn.nlark.com/yuque/0/2018/png/115449/1540357129958-ec49fece-023b-49bf-989b-930f529e2337.png "")


<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">sacc与css Modules相结合 避免了全局污染 更快乐了</span></span>
