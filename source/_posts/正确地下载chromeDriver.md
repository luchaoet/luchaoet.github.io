---
title: 正确地下载chromeDriver
date: 2019-01-29 11:31:22
tags: ['Python', 'selenium', 'chromeDriver']
summary:
---
讨论环境：Mac Chrome

### 安装 selenium
Python安装selenium包操作浏览器
```
pip install selenium
```

示例代码，打开百度
```python
from selenium import webdriver
 
browser = webdriver.Chrome()
browser.get('http://www.baidu.com/')
```

### 安装 ChromeDriver
代码运行的前提是需要安装Chrome驱动<br />首先查看自己Chrome浏览器的版本<br />![屏幕快照 2019-01-28 14.36.15.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1548657445937-611ab195-f281-40ce-9c40-db5954392cc8.png#align=left&display=inline&height=534&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-01-28%2014.36.15.png&originHeight=553&originWidth=773&size=103826&width=746)<br />![20190128143827.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1548657511974-1ab25e29-853b-4812-b1cd-19b917cda16c.png#align=left&display=inline&height=336&linkTarget=_blank&name=20190128143827.png&originHeight=336&originWidth=697&size=43424&width=697)

然后点击去[官网](https://sites.google.com/a/chromium.org/chromedriver/downloads)查看什么版本的chromeDriver支持你的Chrome浏览器<br />详细说明了各个版本的chromeDriver支持的Chrome浏览器<br />![20190128144058.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1548657686999-b0dfedca-1c4a-4c01-8746-127e49ea0d03.png#align=left&display=inline&height=946&linkTarget=_blank&name=20190128144058.png&originHeight=960&originWidth=757&size=198362&width=746)<br /><br /><br />现在已经知道了需要下载哪个版本的chromeDriver了<br />下载地址 [https://npm.taobao.org/mirrors/chromedriver/downloads](https://npm.taobao.org/mirrors/chromedriver/)

### 将ChromeDriver安装包加入到环境变量
下载后，解压，放到 /usr/bin目录即可<br />OK

如果启动后，没有任何反应<br />可能hosts没有添加下面一行，开发都会有修改hosts的操作，注意一下，我就是因为注释掉了导致踩了坑
```
127.0.0.1 localhost
```