---
title: npm并行多个命令
date: 2018-10-20 00:58:45
tags: ['npm', 'npm-run-all']
summary: 优雅的操作 美滋滋
---
遇到一个这样的问题，使用`create-react-app`搭建项目时，加入了sass，这时需要实时监听sass文件，将其转化为css文件
于是`npm run start`启动服务后，需要在终端再次执行一条命令 `npm run watch-css`
```json
"watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive"
```
这是我忍受不了的

事实上，我希望一条命令可以同时执行下面两条命令
```json
"watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive",
"start": "react-scripts start"
```

## 解决方法
1.安装 <span data-type="color" style="color:#ce9178">npm-run-all</span>
2.添加一条新的命令
```json
"dev": "npm-run-all -p watch-css start" /* 简写run-p 或者 npm run start & npm run watch-css */
```

此时，`npm run dev`便同时会执行 `npm run watch-css` 和 `npm run start`
完美
## npm-run-all
* run-s 运行串联任务
* run-p 运行并行任务

