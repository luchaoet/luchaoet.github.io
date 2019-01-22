---
title: git报错
date: 2018-10-15 17:04:54
tags: git
summary: 收集git使用中的一些报错处理方法
---
## 1. fatal: Unable to create 'xxxx/.git/index.lock': File exists.


![屏幕快照 2018-10-15 下午4.54.11.png | center | 747x44](https://cdn.nlark.com/yuque/0/2018/png/115449/1539593800284-cceef2e9-d6d4-47ff-9e7c-e00193145bf0.png "")


解决方法：在文件夹中删除index.lock文件即可

## 2.xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun


![屏幕快照 2018-10-22 上午3.51.46.png | center | 747x60](https://cdn.nlark.com/yuque/0/2018/png/115449/1540151519831-d146fed4-4b07-45ce-8110-0752c181ec97.png "")


可能是由于xcode问题造成的，可在终端中执行如下命令：
xcode-select --install
系统弹出下载xcode相关插件，大概1分钟安装完毕

