---
title: hexo搭建个人博客
date: 2017-09-03 21:08:16
tags: hexo
summary: 不断踩坑后的泣血总结
---

首先你走到这步，说明你已经有了自己的gitHub帐号，并且一切环境已经准备OK了，下面我们就来一步一步搭建自己的博客吧

### 在gitHub上创建博客项目
1. 创建一个新的项目
命名格式好像只能这样（xxxx.github.io），看别人这么说的，也没试过，这个不重要


![屏幕快照 2018-10-04 下午10.30.27.png | center | 800x568.6534216335541](https://cdn.nlark.com/yuque/0/2018/png/115449/1538663489485-b7c8c002-f263-4a11-8265-d1b7faddc6d3.png "")

2. 下一步本地搭建博客项目完成后，一定要把这个项目的github地址填写到本地项目的\_config.yml如图所示位置，这样是为了连接本地与你的github帐号，本地发布成功后会自动帮你更新github项目


![屏幕快照 2018-10-04 下午10.35.30.png | center | 827x257](https://cdn.nlark.com/yuque/0/2018/png/115449/1538663877896-385aec49-d886-4ab9-b07d-d58cc980644f.png "")


### 本地搭建博客项目
1. 创建好文件夹，全局安装hexo
    ```javascript
    npm install -g hexo
    ```
2. 初始化
    ```javascript
    hexo init
    npm install
    
    ```
3. 本地启动服务
    ```javascript
    npm install 
    hexo-deployer-git --save 
    // 生成页面
    hexo g 
    hexo s
    ```
4. 打开[localhost:4000](localhost:4000) 即可本地查看
5. 发布 
```javascript
hexo clean
// 生成页面
hexo g
// 发布到gitHub
hexo d
```

### 查看线上地址
发布成功后，便可以直接在线上浏览你的博客了，至此博客从搭建到上线就完成了
[https://zhangSan.github.io/](https://zhangSan.github.io/)

### 更换主题
以后有时间再总结啦