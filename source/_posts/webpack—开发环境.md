---
title: webpack—开发环境
date: 2019-04-01 23:53:49
tags: webpack
summary:
---
先将 `mode` 设置为 `'development'`
<a name="webpack.config.js"></a>
### webpack.config.js
```javascript
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    main: './main.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  },
  plugins: [
    // new CleanWebpackPlugin()
  ]
}
```
<a name="aebef980"></a>
### 使用 source map
当 webpack 打包源代码时，可能会很难追踪到 error(错误) 和 warning(警告) 在源代码中的原始位置。例如，如果将三个源文件（`a.js`, `b.js` 和 `c.js`）打包到一个 bundle（`bundle.js`）中，而其中一个源文件包含一个错误，那么堆栈跟踪就会直接指向到 `bundle.js`。你可能需要准确地知道错误来自于哪个源文件，所以这种提示这通常不会提供太多帮助。<br />为了更容易地追踪 error 和 warning，JavaScript 提供了 [source map](http://blog.teamtreehouse.com/introduction-source-maps) 功能，可以将编译后的代码映射回原始源代码。如果一个错误来自于 `b.js`，source map 就会明确的告诉你。<br />source map 有许多 [可用选项](https://webpack.docschina.org/configuration/devtool)，请务必仔细阅读它们，以便可以根据需要进行配置

src/main.js
```javascript
console.log(1)
```
src/index.js
```javascript
import './main'
consol.log(2)
```
webpack.config.js
```javascript
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js'
  },
  devtool: 'inline-source-map',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  }
}
```
在浏览器查看效果<br />![屏幕快照 2019-04-01 下午11.27.58.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1554132524538-d00c4f2e-1e5a-4643-88f9-c0989b8306d3.png#align=left&display=inline&height=120&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-01%20%E4%B8%8B%E5%8D%8811.27.58.png&originHeight=228&originWidth=762&size=46950&status=done&width=400)<br />虽然`main.js`和`index.js`被打包到了`index.bundle.js`文件，但是依旧显示错误来自`index.js`文件的第二行
<a name="a6670e39"></a>
### 使用 watch mode(观察模式) 
你可以指示 webpack "watch" 依赖图中所有文件的更改。如果其中一个文件被更新，代码将被重新编译，所以必再去手动运行整个构建<br />在`package.json`添加一个用于启动
```
"watch": "webpack --watch"
```
在命令行中运行 `npm run watch`，然后就会看到 webpack 是如何编译代码。 然而，你会发现并没有退出命令行。这是因为此 script 当前还在 watch 文件<br />唯一的缺点是，为了看到修改后的实际效果，需要重新刷新浏览器。如果能够自动刷新浏览器就更好了，因此接下来尝试通过 `webpack-dev-server` 实现此功能
<a name="2d01ff51"></a>
### 使用 webpack-dev-server 
安装`webpack-dev-server`
```
npm install --save-dev webpack-dev-server
```
修改配置文件，告诉 dev server，从什么位置查找文件<br />webpack.config.js
```javascript
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js'
  },
  devtool: 'inline-source-map',
  devServer: {
    contentBase: './dist'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  }
}
```
以上配置告知 `webpack-dev-server`，将 `dist` 目录下的文件 serve 到 `localhost:8080` 下<br />在package.json配置中添加运行命令行
```
"dev": "webpack-dev-server --open"
```
现在，在命令行中运行 `npm run dev`，我们会看到浏览器自动加载页面。如果你更改任何源文件并保存它们，web server 将在编译代码后自动重新加载<br />`webpack-dev-server`还有[其他配置](https://webpack.docschina.org/configuration/dev-server)<br />该文源码点击[查看](https://github.com/Lucy20209060/webpack-test/tree/master/demo-03)