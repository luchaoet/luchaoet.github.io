---
title: webpack—缓存
date: 2019-04-08 22:13:24
tags: webpack
summary:
---
使用 webpack 来打包模块化的应用程序，webpack 会生成一个可部署的 `/dist` 目录，然后把打包后的内容放置在此目录中。只要 `/dist` 目录中的内容部署到 server 上，client（通常是浏览器）就能够访问网站此 server 的网站及其资源。而最后一步获取资源是比较耗费时间的，这就是为什么浏览器使用一种名为 [缓存](https://searchstorage.techtarget.com/definition/cache)的技术。可以通过命中缓存，以降低网络流量，使网站加载速度更快，然而，如果我们在部署新版本时不更改资源的文件名，浏览器可能会认为它没有被更新，就会使用它的缓存版本<br />通过必要的配置，以确保 webpack 编译生成的文件能够被客户端缓存，而在文件内容变化后，能够请求到新的文件
<a name="9c5f9a37"></a>
### 输出文件的文件名(output filename)
通过替换 `output.filename` 中的 [substitutions](https://webpack.docschina.org/configuration/output#output-filename) 设置，来定义输出文件的名称。webpack 提供了一种使用称为 substitution(可替换模板字符串) 的方式，通过带括号字符串来模板化文件名。其中，`[contenthash]` substitution 将根据资源内容创建出唯一 hash。当资源内容发生变化时，`[contenthash]` 也会发生变化<br />**webpack.config.js**
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
    // filename: '[name].bundle.js'
    filename: '[name].[contenthash].js'
  },
  plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
          title: 'demo-10-Caching'
      })
  ]
}
```
npm run build<br />![20190408210546.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554728790190-bd567c18-e262-4968-83db-651e8d159fca.jpeg#align=left&display=inline&height=82&name=20190408210546.jpg&originHeight=142&originWidth=1290&size=53907&status=done&width=746)<br />可以看到，bundle 的名称是它内容（通过 hash）的映射
<a name="5874592d"></a>
### 提取引导模板(extracting boilerplate) 
在代码分离中所学到的，[`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin/) 可以用于将模块分离到单独的 bundle 中。webpack 还提供了一个优化功能，可使用 [`optimization.runtimeChunk`](https://webpack.docschina.org/configuration/optimization/#optimization-runtimechunk) 选项将 runtime 代码拆分为一个单独的 chunk。将其设置为 `single` 来为所有 chunk 创建一个 runtime bundle：<br />**webpack.config.js**

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  devServer: {
    contentBase: './dist'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    // filename: '[name].bundle.js'
    filename: '[name].[contenthash].js'
  },
  plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
          title: 'demo-10-Caching'
      })
  ],
  optimization: {
    runtimeChunk: 'single'
  }
}
```
再次构建<br />![20190408211330.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554729319211-3dda77b4-6fd3-4c76-9d12-451e53d749df.jpeg#align=left&display=inline&height=81&name=20190408211330.jpg&originHeight=140&originWidth=1282&size=52181&status=done&width=746)<br />将第三方库(library)（例如 `lodash` 或 `react`）提取到单独的 `vendor` chunk 文件中，是比较推荐的做法，这是因为，它们很少像本地的源代码那样频繁修改。因此通过实现以上步骤，利用 client 的长效缓存机制，命中缓存来消除请求，并减少向 server 获取资源，同时还能保证 client 代码和 server 代码版本一致。 这可以通过使用 [SplitChunksPlugin 示例 2](https://webpack.docschina.org/plugins/split-chunks-plugin/#split-chunks-example-2) 中演示的 [`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin/) 插件的 [`cacheGroups`](https://webpack.docschina.org/plugins/split-chunks-plugin/#splitchunks-cachegroups) 选项来实现。我们在 `optimization.splitChunks` 添加如下 `cacheGroups` 参数并构建
```javascript
optimization: {
    runtimeChunk: 'single',
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        }
      }
    }
  }
```
再次构建<br />![20190408211817.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554729696229-d35abc48-c5c3-4a9c-b007-7e9f53f83126.jpeg#align=left&display=inline&height=103&name=20190408211817.jpg&originHeight=178&originWidth=1284&size=73613&status=done&width=746)<br /> `main` 不再含有来自 `node_modules` 目录的 `vendor` 代码，并且体积减少到 `208 bytes`
<a name="99a99c58"></a>
### 模块标识符(module identifier)
src/print.js
```javascript
export default function print(text) {
    console.log(text);
};
```
src/index.js
```javascript
import _ from 'lodash';
import Print from './print';

function component() {
  var element = document.createElement('div');
  // lodash 是由当前 script 脚本 import 进来的
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  element.onclick = Print.bind(null, 'Hello webpack!');
  return element;
}
document.body.appendChild(component());
```
构建<br />![a.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554730684241-7f87e415-2cf2-4c81-a6a3-43f6a2bca248.jpeg#align=left&display=inline&height=100&name=a.jpg&originHeight=172&originWidth=1284&size=66523&status=done&width=746)<br />可以看到这三个文件的 hash 都变化了。这是因为每个 [`module.id`](https://webpack.docschina.org/api/module-variables#module-id-commonjs-) 会默认地基于解析顺序(resolve order)进行增量。也就是说，当解析顺序发生变化，ID 也会随之改变。因此，简要概括
* `main` bundle 会随着自身的新增内容的修改，而发生变化
* `vendor` bundle 会随着自身的 `module.id` 的变化，而发生变化
* `manifest` bundle 会因为现在包含一个新模块的引用，而发生变化

第一个和最后一个都是符合预期的行为 - 而 `vendor` hash 发生变化是我们要修复的。幸运的是，可以使用两个插件来解决这个问题。第一个插件是 `NamedModulesPlugin`，将使用模块的路径，而不是一个数字 identifier。虽然此插件有助于在开发环境下产生更加可读的输出结果，然而其执行时间会有些长。第二个选择是使用 [`HashedModuleIdsPlugin`](https://webpack.docschina.org/plugins/hashed-module-ids-plugin)，推荐用于生产环境构建<br />**webpack.config.js**
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const webpack = require('webpack');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  devServer: {
    contentBase: './dist'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    // filename: '[name].bundle.js'
    filename: '[name].[contenthash].js'
  },
  plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
          title: 'demo-10-Caching'
      }),
      new webpack.HashedModuleIdsPlugin()
  ],
  optimization: {
    // runtimeChunk: 'single'
    runtimeChunk: 'single',
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        }
      }
    }
  }
}
```
npm run build<br />![b.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554731924655-42d9ad9d-0d75-42a2-bc99-e51b39c53bb2.jpeg#align=left&display=inline&height=99&name=b.jpg&originHeight=170&originWidth=1282&size=67835&status=done&width=746)<br />现在，不论是否添加任何新的本地依赖，对于前后两次构建，`vendor` hash 都应该保持一致<br />然后，修改 `src/index.js`，临时移除额外的依赖<br />src/index.js
```javascript
import _ from 'lodash';
// import Print from './print';

function component() {
  var element = document.createElement('div');

  // lodash 是由当前 script 脚本 import 进来的
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  // element.onclick = Print.bind(null, 'Hello webpack!');
  return element;
}
document.body.appendChild(component());
```
再次构建<br />![d.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1554732640523-cb521a79-b828-470a-bf7e-641d259433ad.jpeg#align=left&display=inline&height=100&name=d.jpg&originHeight=172&originWidth=1284&size=67694&status=done&width=746)<br />我们可以看到，这两次构建中，`vendor` bundle 文件名称，都是 `070a3e1b2e607073a75e`，这就是我们要达到的效果
<br />以上源码点击[查看](https://github.com/Lucy20209060/webpack-test/tree/master/demo-10)
