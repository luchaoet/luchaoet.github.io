---
title: express框架实现热更新
date: 2019-03-08 09:26:57
tags: ['express', '热更新']
summary:
---
express项目每次修改代码都得`npm start`重新启动一下，非常的反人类，找了两个解决方法
<a name="efc6158f"></a>
### 方法一: nodemon
nodemon是一种工具，通过在检测到目录中的文件更改时自动重新启动节点应用程序来帮助开发基于node.js的应用程序
<a name="6e65a656"></a>
#### 安装nodemon
```
npm install -g nodemon
```
或者在项目中安装
```
npm install --save-dev nodemon
```
<a name="503db396"></a>
#### 创建nodemon.json
根目录下创建该文件
```json
{
    "restartable": "rs",
    "ignore": [
        ".git",
        ".svn",
        "node_modules/**/node_modules"
    ],
    "verbose": true,
    "execMap": {
        "js": "node --harmony"
    },
    "watch": [

    ],
    "env": {
        "NODE_ENV": "development"
    },
    "ext": "js json"
}
```
restartable: 设置重启模式<br />ignore: 设置忽略文件<br />verbose: 设置日志输出模式，true 详细模式<br />execMap: 设置运行服务的后缀名与对应的命令
```json
{ "js": "node –harmony"}
```
表示使用 nodemon 代替 node

watch: 监控的文件夹路径或者文件路径<br />env：运行环境 development 是开发环境，production 是生产环境<br />ext: 监控指定后缀名的文件，用空格间隔。默认监控的后缀文件：.js, .coffee, .litcoffee, .json
<a name="4c763bb6"></a>
#### 运行
安装完 nodemon 后，就可以用 nodemon 来代替 node 来启动应用
```
nodemon app.js
```
如果你有其他需求可查看
<a name="1c9565da"></a>
### 方法二: node-dev
<a name="dbaa7bb9"></a>
#### 安装node-dev
```
npm install -g node-dev
```
<a name="3729c4cd"></a>
#### package.json增加命令
```
"scripts": {
    "start": "node ./bin/www",
    "dev": "node-dev ./bin/www"

```
<a name="4c763bb6-1"></a>
#### 运行
```
npm run dev
```