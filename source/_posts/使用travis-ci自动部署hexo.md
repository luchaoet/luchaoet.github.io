---
title: 使用travis-ci自动部署hexo
date: 2019-03-05 09:49:51
tags: ['travis', 'travis-ci', 'hexo']
summary:
---
<a name="hexo"></a>
### hexo
Hexo是一款基于Node.js的静态博客框架<br />今天我们并不是讨论如何使用hexo搭建自己的博客，如果你需要，可以查看我与之相关的[博客](https://lucy20209060.github.io/2017/09/03/hexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
<a name="a34c64fd"></a>
#### hexo发布流程
```
hexo clean // 清除上一次生成的静态页面

hexo generate 或 hexo g // 生成最新的静态页面

hexo deploy 或 hexo d // 将最新的静态页面发布到gitHub
```
因为在项目中已经你配置了自己的github帐号，所以发布之后，内容会提交到你的github，在相关仓库的master分支上，一会你的博客上就能看到最新的内容了
<a name="5dc99f6e"></a>
#### 问题
自己的电脑上是项目文件，提交的是生成后的静态文件<br />![WX20190301-174206@2x.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551433352809-2046baf0-c61b-4753-a362-28ac0f21a991.png#align=left&display=inline&height=1178&name=WX20190301-174206%402x.png&originHeight=1178&originWidth=518&size=88665&status=done&width=518)<br />这是整个本地开发时的项目文件，红框中的是被提交的静态文件，两份代码并不完全一样<br />提交的是`public`文件中的内容，但项目文件没有备份，如果不小心被删除了，或者出了什么问题，那就傻逼了<br />如果我经常用两台电脑，那么我怎么书写并发布自己的博客内容

<a name="de842a6c"></a>
#### 解决方案
所以，我有一个需求<br />在博客的仓库`dev`分支开发项目，然后把`public`文件中的静态文件发布到`master`分支<br />如果真的可以这样，那就解决所有的问题<br />确实可以
<a name="travis-ci"></a>
### travis-ci
用于持续集成完成自动化测试部署的开源项目
<a name="fe0e8bf7"></a>
#### 注册
官网[https://travis-ci.org/](https://travis-ci.org/)，使用github帐号登录<br />
![20190301175309.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551434004603-87d33c2a-3bda-4de5-a1d4-ec29df3e386b.png#align=left&display=inline&height=310&name=20190301175309.png&originHeight=858&originWidth=2062&size=175245&status=done&width=746)

<a name="4e72650f"></a>
#### 选择博客仓库
点击小加号进去选择，当然图中我已经选好了<br />![20190301175629.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551434228181-38016d69-af61-4e6e-a852-2bdeca10a80e.png#align=left&display=inline&height=348&name=20190301175629.png&originHeight=966&originWidth=2072&size=273594&status=done&width=746)<br />进去后选择自己的博客仓库，打开仓库后面的开关即可<br />![20190301180051.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551434465186-bcf480f5-b413-4cd5-a694-2d813139ca41.png#align=left&display=inline&height=50&name=20190301180051.png&originHeight=88&originWidth=1322&size=27690&status=done&width=746)<br />这个`settings`稍后再说

<a name="c9f2d224"></a>
#### github上Access Token
由于需要操作github的api接口，所以我们需要授权，获取`Token`<br />直接进入该链接[https://github.com/settings/tokens](https://github.com/settings/tokens)<br />点击`Personal access token`![20190301180807.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551434918078-c531b9c9-4ae4-4a7a-afdd-309e9d9ed141.png#align=left&display=inline&height=280&name=20190301180807.png&originHeight=762&originWidth=2032&size=217395&status=done&width=746)<br />点击`Generate new token`<br />![20190301180628.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551434959552-d796f3cb-1530-49ee-b5b6-dc8a81f28e16.png#align=left&display=inline&height=491&name=20190301180628.png&originHeight=1320&originWidth=2006&size=314336&status=done&width=746)

添加该token的描述并且选择权限<br />生成之后一定要保存好，因为只会出现一次，刷新页面就看不到了

<a name="53c9a42c"></a>
#### 配置token字段
进入上面说的`settings`<br />将上一步获取到的token添加到配置中，这将是后面说到的文件的全局变量<br />![20190301181457.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551435364600-be7a8b66-12e4-434f-b321-d1308d3ff968.png#align=left&display=inline&height=61&name=20190301181457.png&originHeight=128&originWidth=1568&size=26021&status=done&width=746)<br />变量名自己随便取个有意义的即可

<a name="6df5ad43"></a>
#### 博客项目中添加配置文件.travis.yml
在项目文件根目录添加`.travis.yml`文件，内容如下
```
language: node_js # 声明环境为node
node_js: 
  - "8"

# Travis-CI Caching
cache:
  directories:
    - node_modules # 缓存node_modules文件夹

# S: Build Lifecycle
install:
  - npm install # 下载依赖

script:
  - hexo c
  - hexo g

after_script: # 推送到github的部分
  - cd ./public
  - git init
  - git config user.name "github对应的帐号"
  - git config user.email "github对应的邮箱"
  - git add .
  - git commit -m "Update docs"
  - git push --force --quiet "https://${GIT_REPO_TOKEN}@${GH_REF}" master:master # 通过之前存在Travis-CI里的token以及github仓库的地址推送到相应的master分支
# E: Build LifeCycle

branches:
  only:
    - dev # 只对dev分支构建

env: # 环境变量
 global:
   - GH_REF: github.com/Lucy20209060/Lucy20209060.github.io # 我的仓库地址
```
`GIT_REPO_TOKEN`就是上面说到的token<br />`GH_REF`是下面的环境变量，就是你的仓库地址

<a name="73378aea"></a>
### 提交代码
在`dev`分支开发完成后，提交代码至`dev`分支<br />我们便可以在官网看到构建过程<br />![屏幕快照 2019-03-01 下午6.44.43.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551437095374-4b29b0d8-8e96-4391-9520-2e3b3e833bda.png#align=left&display=inline&height=396&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-01%20%E4%B8%8B%E5%8D%886.44.43.png&originHeight=1520&originWidth=2866&size=381235&status=done&width=746)

最后达到了我们要的结果<br />dev上是项目代码<br />![20190301184658.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551437248027-56b26811-f2d1-4efb-909c-a656320717c8.png#align=left&display=inline&height=300&name=20190301184658.png&originHeight=808&originWidth=2008&size=253482&status=done&width=746)<br />master上是生成后的静态文件<br />![20190301184559.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551437301803-1277ca65-f8d9-4483-ac5a-810301f6469f.png#align=left&display=inline&height=411&name=20190301184559.png&originHeight=1104&originWidth=2006&size=279233&status=done&width=746)