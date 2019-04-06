---
title: webpack—代码分离
date: 2019-04-06 21:56:31
tags: webpack
summary:
---
代码分离是 webpack 中最引人注目的特性之一。此特性能够把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间<br />常用的代码分离方法有三种：
* 入口起点：使用 [`entry`](https://webpack.docschina.org/configuration/entry-context) 配置手动地分离代码
* 防止重复：使用 [`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin/) 去重和分离 chunk
* 动态导入：通过模块中的内联函数调用来分离代码
<a name="fc6412ce"></a>
### 入口起点(entry points)
这是迄今为止最简单、最直观的分离代码的方式。不过，这种方式手动配置较多，并有一些隐患<br />src/index.js
```javascript
import _ from 'lodash';

console.log('index.js');
console.log(
  _.join(['index', 'module', 'loaded!'], ' ')
);
```
src/another.js
```javascript
import _ from 'lodash';

console.log('main.js');
console.log(
  _.join(['Another', 'module', 'loaded!'], ' ')
);
```
webpack.config.js
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    another: './src/another.js'
  },
  devServer: {
    contentBase: './dist'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  },
  plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
          title: 'demo-07-代码分离'
      })
  ]
}
```
构建结果<br />![20190406210825.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554556181316-89bfcb99-e3ce-4ae5-8064-7dde8c369e70.jpeg#align=left&display=inline&height=171&name=20190406210825.jpg&originHeight=440&originWidth=1288&size=115519&status=done&width=500)<br />正如前面提到的，这种方式存在一些隐患
* 如果入口 chunk 之间包含一些重复的模块，那些重复模块都会被引入到各个 bundle 中
* 这种方法不够灵活，并且不能动态地将核心应用程序逻辑中的代码拆分出来

这两点中的第一点，对我们的示例来说毫无疑问是个严重问题，因为我们在 `./src/index.js` 中也引入过 `lodash`，这样就造成在两个 bundle 中重复引用。我们可以通过使用 [`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin/) 插件来移除重复模块
<a name="4c0a6d59"></a>
### 防止重复(prevent duplication) 
[`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin/) 插件可以将公共的依赖模块提取到已有的 entry chunk 中，或者提取到一个新生成的 chunk。让我们使用这个插件，将前面示例中重复的 `lodash` 模块去除<br />`CommonsChunkPlugin`_ 已经从 webpack v4（代号 legato）中移除。想要了解最新版本是如何处理 chunk，请查看 _[`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin/)<br />webpack.config.js
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    another: './src/another.js'
  },
  devServer: {
    contentBase: './dist'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js'
  },
  plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
          title: 'demo-07-代码分离'
      })
  ],
  optimization: {
    splitChunks: {
      chunks: 'all'
    }
  }
}
```
使用 [`optimization.splitChunks`](https://webpack.docschina.org/plugins/split-chunks-plugin/#optimization-splitchunks) 配置选项，现在可以看到已经从 `index.bundle.js` 和 `another.bundle.js` 中删除了重复的依赖项。需要注意的是，此插件将 `lodash` 这个沉重负担从主 bundle 中移除，然后分离到一个单独的 chunk 中。执行 `npm run build` 查看效果<br />![20190406210851.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554556361908-c3edbb70-f545-4e8d-98f5-ec6b566993ac.jpeg#align=left&display=inline&height=199&name=20190406210851.jpg&originHeight=512&originWidth=1288&size=154273&status=done&width=500)<br /><br />以下是由社区提供，一些对于代码分离很有帮助的 plugin 和 loader
* [`mini-css-extract-plugin`](https://webpack.docschina.org/plugins/mini-css-extract-plugin)：用于将 CSS 从主应用程序中分离
* [`bundle-loader`](https://webpack.docschina.org/loaders/bundle-loader)：用于分离代码和延迟加载生成的 bundle
* [`promise-loader`](https://github.com/gaearon/promise-loader)：类似于 `bundle-loader` ，但是使用了 promise API

以上源码点击[查看](https://github.com/Lucy20209060/webpack-test/tree/master/demo-07)
<a name="05411f79"></a>
### 动态导入(dynamic imports) 
当涉及到动态代码拆分时，webpack 提供了两个类似的技术。第一种，也是推荐选择的方式是，使用符合 [ECMAScript 提案](https://github.com/tc39/proposal-dynamic-import) 的 [`import()` 语法](https://webpack.docschina.org/api/module-methods#import-) 来实现动态导入。第二种，则是 webpack 的遗留功能，使用 webpack 特定的 [`require.ensure`](https://webpack.docschina.org/api/module-methods#require-ensure)。让我们先尝试使用第一种<br />webpack.config.js
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js'
  },
  devServer: {
    contentBase: './dist'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js',
    chunkFilename: '[name].bundle.js'
  },
  plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
          title: 'demo-08-动态导入'
      })
  ],
  optimization: {
    splitChunks: {
      chunks: 'all'
    }
  }
}
```
注意，这里使用了 `chunkFilename`，它决定 non-entry chunk(非入口 chunk) 的名称。关于 `chunkFilename`更多信息，请查看 [输出](https://webpack.docschina.org/configuration/output/#output-chunkfilename) 文档<br />现在不再使用 statically import(静态导入) `lodash`，而是通过 dynamic import(动态导入) 来分离出一个 chunk<br />src/index.js
```javascript
function getComponent() {
  return import(/* webpackChunkName: "lodash" */ 'lodash').then(({ default: _ }) => {
    var element = document.createElement('div');
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
    return element;
  }).catch(error => 'An error occurred while loading the component');
}
getComponent().then(component => {
  document.body.appendChild(component);
})
```
这里我们需要使用 `default` 的原因是，从 webpack v4 开始，在 import CommonJS 模块时，不会再将导入模块解析为 `module.exports` 的值，而是为 CommonJS 模块创建一个 artificial namespace object(人工命名空间对象)，关于其背后原因的更多信息，请阅读 [webpack 4: import() 和 CommonJs](https://medium.com/webpack/webpack-4-import-and-commonjs-d619d626b655)。<br />注意，在注释中我们提供了 `webpackChunkName`。这样会将拆分出来的 bundle 命名为 `lodash.bundle.js`，而不是 `[id].bundle.js`。想了解更多关于 `webpackChunkName` 和其他可用选项，请查看 [`import()`](https://webpack.docschina.org/api/module-methods/#import-) 文档。让我们执行 webpack，看到 `lodash` 分离出一个单独的 bundle<br />![20190406213904.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554557995061-a0a6ef79-3bf7-48d3-b583-2283474daf90.jpeg#align=left&display=inline&height=146&name=20190406213904.jpg&originHeight=378&originWidth=1296&size=97341&status=done&width=501)<br />由于 `import()` 会返回一个 promise，因此它可以和 [`async` 函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)一起使用。但是，需要使用像 Babel 这样的预处理器和 [Syntax Dynamic Import Babel Plugin](https://babel.docschina.org/docs/en/babel-plugin-syntax-dynamic-import/#installation)。下面是如何通过 async 函数简化代码<br />src/index.js
```javascript
async function getComponent() {
  var element = document.createElement('div');
  const { default: _ } = await import(/* webpackChunkName: "lodash" */ 'lodash');
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  return element;
}
getComponent().then(component => {
  document.body.appendChild(component);
})
```
<a name="f6a36429"></a>
### 预取/预加载模块(prefetch/preload module)
webpack v4.6.0+ 添加了预取和预加载的支持<br />在声明 import 时，使用下面这些内置指令，可以让 webpack 输出 "resource hint(资源提示)"，来告知浏览器
* prefetch(预取)：将来某些导航下可能需要的资源
* preload(预加载)：当前导航下可能需要资源

下面这个 prefetch 的简单示例中，有一个 `HomePage` 组件，其内部渲染一个 `LoginButton` 组件，然后在点击后按需加载 `LoginModal` 组件<br />LoginButton.js

```javascript
//...
import(/* webpackPrefetch: true */ 'LoginModal');
```
这会生成 `<link rel="prefetch" href="login-modal-chunk.js">` 并追加到页面头部，指示着浏览器在闲置时间预取 `login-modal-chunk.js` 文件<br />与 prefetch 指令相比，preload 指令有许多不同之处
* preload chunk 会在父 chunk 加载时，以并行方式开始加载。prefetch chunk 会在父 chunk 加载结束后开始加载
* preload chunk 具有中等优先级，并立即下载。prefetch chunk 在浏览器闲置时下载
* preload chunk 会在父 chunk 中立即请求，用于当下时刻。prefetch chunk 会用于未来的某个时刻
* 浏览器支持程度不同

下面这个简单的 preload 示例中，有一个 `Component`，依赖于一个较大的 library，所以应该将其分离到一个独立的 chunk 中<br />我们假想这里的图表组件 `ChartComponent` 组件需要依赖体积巨大的 `ChartingLibrary` 库。它会在渲染时显示一个 `LoadingIndicator(加载进度条)` 组件，然后立即按需导入 `ChartingLibrary`<br />ChartComponent.js
```javascript
//...
import(/* webpackPreload: true */ 'ChartingLibrary');
```
在页面中使用 `ChartComponent` 时，在请求 ChartComponent.js 的同时，还会通过 `<link rel="preload">`请求 charting-library-chunk。假定 page-chunk 体积很小，很快就被加载好，页面此时就会显示 `LoadingIndicator(加载进度条)` ，等到 `charting-library-chunk` 请求完成，LoadingIndicator 组件才消失。启动仅需要很少的加载时间，因为只进行单次往返，而不是两次往返。尤其是在高延迟环境下。<br />不正确地使用 webpackPreload 会有损性能，请谨慎使用
<a name="206c76a3"></a>
### bundle 分析(bundle analysis) [](https://webpack.docschina.org/guides/code-splitting/#bundle-%E5%88%86%E6%9E%90-bundle-analysis-)
如果我们以分离代码作为开始，那么就应该以检查模块的输出结果作为结束，对其进行分析是很有用处的。[官方提供分析工具](https://github.com/webpack/analyse) 是一个好的初始选择。下面是一些可选择的社区支持(community-supported)工具
* [webpack-chart](https://alexkuz.github.io/webpack-chart/)：webpack stats 可交互饼图
* [webpack-visualizer](https://chrisbateman.github.io/webpack-visualizer/)：可视化并分析你的 bundle，检查哪些模块占用空间，哪些可能是重复使用的
* [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer)：一个 plugin 和 CLI 工具，它将 bundle 内容展示为便捷的、交互式、可缩放的树状图形式
* [webpack bundle optimize helper](https://webpack.jakoblind.no/optimize)：此工具会分析你的 bundle，并为你提供可操作的改进措施建议，以减少 bundle 体积大小

以上源码点击[查看](https://github.com/Lucy20209060/webpack-test/tree/master/demo-08)