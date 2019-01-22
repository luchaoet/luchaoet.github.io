---
title: webpack4从零搭建react应用
date: 2018-10-26 11:16:02
tags: ["webpack4", "react"]
summary: 
---
### 创建一个项目文件夹，在项目中引导创建一个package.json文件
```plain
npm init -y
```

### 安装webpack
```plain
npm i webpack webpack-cli --save-dev
```

### package.json添加一条命令
```plain
"scripts": {
    "start": "webpack --mode development",
    "build": "webpack --mode production"
}
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">创建</span></span>`./src/index.js`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">文件，随便写点东西</span></span>
```javascript
console.log('hello world')
```

`production`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">模式下，默认对打包的文件压缩，打包更小。</span></span>


![屏幕快照 2018-10-26 上午10.15.09.png | center | 747x53](https://cdn.nlark.com/yuque/0/2018/png/115449/1540520128162-dd1d2569-d910-41f7-9cde-a96276a249a8.png "")


<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>development</code></span></span><span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">下，对打包的文件不压缩，同时打包的速度更快。</span></span>


![屏幕快照 2018-10-26 上午10.14.26.png | center | 747x725](https://cdn.nlark.com/yuque/0/2018/png/115449/1540520093779-9f52da5c-edb2-4580-9d24-2d7a0558d5fa.png "")


<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">如果没有指定任何模式，默认为</span></span>`production`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">模式。</span></span>

好了，现在正式开始安装React部分了
### 先安装一堆依赖
```plain
npm i babel-core babel-loader babel-preset-env react react-dom babel-preset-react --save-dev
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">新建</span></span>.babelrc<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">文件，进行相关配置</span></span>
```json
{
  "presets": ["env","react"]
}
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">新建</span></span>`webpack.config.js`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">文件，进行配置</span></span>
```javascript
module.exports = {
  module: {
    rules: [
			{
				test: /\.js$/,
				exclude: /node_modules/,
				use: {
					loader: "babel-loader"
				}
			}
    ]
  }
}
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">修改index.js</span></span>
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './app.js'

ReactDOM.render(
<App />,
document.getElementById("app")
);
```

### 新建./src/app.js文件
```javascript
import React from 'react';

export default class App extends React.Component{
	render(){ 
		return (
			<div>hello world</div>
		) 
	}
}
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">新建</span></span>`./src/index.html`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">文件</span></span>
```html
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>react webpack4</title>
	</head>
	<body>
		<div id="app"></div>
	</body>
</html>
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">安装依赖 </span></span>
`npm i --save-dev html-webpack-plugin`
`npm install html-loader --save-dev`
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">使用</span></span>`html-webpack-plugin`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">对html打包</span></span>

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">修改</span></span>`webpack.config.js`
```javascript
const path = require('path')
const HtmlWebPackPlugin = require("html-webpack-plugin");
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    })
  ]
}
```

### 安装`webpack-dev-server`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">插件</span></span>
`npm i webpack-dev-server --save-dev`
### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">修改</span></span>`package.json`
```javascript
"scripts": {
    "start": "webpack-dev-server --mode development --open",
    "build": "webpack --mode production"
}
```

### `webpack.config.js`添加
```javascript
devServer:{
		contentBase:path.join(__dirname,"dist"),
		compress:true,
		port:8083,
		host:'127.0.0.1',
}
```

### 安装`react-hot-loader`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">插件</span></span>
`npm i react-hot-loader --save-dev`

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">修改</span></span>`.babelrc`
```json
{
  "presets": ["env","react"],
  "plugins": ["react-hot-loader/babel"]
}
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">修改</span></span>`webpack.config.js`
```javascript
plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
	}),
	new webpack.NamedModulesPlugin(),
    new webpack.HotModuleReplacementPlugin()
],
```

### `npm run start `启动
我擦 报错了


![WechatIMG3160.jpeg | center | 747x530](https://cdn.nlark.com/yuque/0/2018/jpeg/115449/1540522500861-3bad927b-e472-432f-b107-87b7c3d6e257.jpeg "")


不慌 百度找了下原因 是babel-loader版本的原因
### 卸载babel-loader，安装@7.1.5版本
```plain
npm uninstall babel-loader
npm install babel-loader@7.1.5
```

启动 完美


![屏幕快照 2018-10-26 上午10.58.32.png | center | 747x323](https://cdn.nlark.com/yuque/0/2018/png/115449/1540522790817-62583c24-a10e-46a4-b841-7f5912ddd0d8.png "")
修改内容，可以实现热更新
