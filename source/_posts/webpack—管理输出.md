---
title: webpack—管理输出
date: 2019-03-31 19:39:31
tags: webpack
summary:
---
<a name="5d332aa6"></a>
### 多入口与出口
```javascript
const path = require('path');

module.exports = {
  entry: {
    app: './src/app.js',
    main: './src/main.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  }
}
```
执行`npm run build`，生成如下<br />![屏幕快照 2019-03-31 下午7.04.33.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1554030334072-e3e53501-ef22-4631-aed6-bb833991cd84.png#align=left&display=inline&height=120&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-31%20%E4%B8%8B%E5%8D%887.04.33.png&originHeight=208&originWidth=1292&size=380442&status=done&width=746)<br />我们可以看到，webpack 生成 `print.bundle.js` 和 `app.bundle.js` 文件<br /><br /><br />但是，如果更改了我们的一个入口起点的名称，甚至添加了一个新的入口，这是需要修改`index.html`文件引用新的文件。让我们用 `HtmlWebpackPlugin` 来解决这个问题
<a name="b785ac30"></a>
### 设置 HtmlWebpackPlugin
安装`html-webpack-plugin`
```
npm install --save-dev html-webpack-plugin
```
webpack.config.js
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    app: './src/app.js',
    main: './src/main.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  },
  plugins: [
    // new HtmlWebpackPlugin({ template: './src/index.html' }),
    new HtmlWebpackPlugin({
      title: '管理输出'
    })
  ]
}
```
执行`npm run build`<br />![屏幕快照 2019-03-31 下午7.11.22.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1554030703624-40bdcc87-7bba-47e4-bb55-80beaf36d252.png#align=left&display=inline&height=146&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-31%20%E4%B8%8B%E5%8D%887.11.22.png&originHeight=242&originWidth=1236&size=409194&status=done&width=746)<br />此时`html-webpack-plugin`在`dist`文件中新建了一个`index.html`文件，所有的 bundle 会自动添加到 html 中
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>管理输出</title>
  </head>
  <body>
  <script type="text/javascript" src="app.bundle.js"></script><script type="text/javascript" src="main.bundle.js"></script></body>
</html>
```
<a name="3cd3bcc6"></a>
### 清理 `dist` 文件夹
可能会发现，当我们修改了文件输出的名称时，打包后生成新的文件，之前的旧文件依旧存在，但是我们并不需要它们了<br />所以需要在每次构建前清除`dist`中的文件，保证里面的文件是最新生成的，`clean-webpack-plugin` 是一个流行的清理插件<br />安装`clean-webpack-plugin`
```
npm install --save-dev clean-webpack-plugin
```
webpack.config.js
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  entry: {
    app: './src/app.js',
    index: './src/main.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: '管理输出'
    })
  ]
}
```
现在，执行 `npm run build`，检查 `/dist` 文件夹。如果一切顺利，现在只会看到构建后生成的文件，而没有旧文件<br />该文源码点击[查看](https://github.com/Lucy20209060/webpack-test/tree/master/demo-02)