---
title: webpack—生产环境
date: 2019-04-02 21:44:05
tags: webpack
summary:
---
<a name="224e2ccd"></a>
### 配置
在开发环境中，我们需要：强大的 source map 和一个有着 live reloading(实时重新加载) 或 hot module replacement(热模块替换) 能力的 localhost server。而生产环境目标则转移至其他方面，关注点在于压缩 bundle、更轻量的 source map、资源优化等，通过这些优化方式改善加载时间。由于要遵循逻辑分离，我们通常建议为每个环境编写彼此独立的 webpack 配置<br />webpack.common.js
```javascript
const path = require('path');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: {
        main: './src/main.js'
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title: '生产环境'
        })
    ],
    module: {
        rules:[
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            }
        ]
    },
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
}
```
webpack.dev.js
```javascript
const merge = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'development',
  devtool: 'inline-source-map',
  devServer: {
    contentBase: './dist'
  }
});
```
webpack.prod.js
```javascript
const merge = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'production',
  devtool: 'source-map'
});
```
现在，在 `webpack.common.js` 中，我们设置了 `entry` 和 `output` 配置，并且在其中引入这两个环境公用的全部插件。在 `webpack.dev.js` 中，我们将 `mode` 设置为 `development`，并且为此环境添加了推荐的 `devtool`（强大的 source map）和简单的 `devServer` 配置。最后，在 `webpack.prod.js` 中，我们将 `mode` 设置为 `production`，其中会引入之前在 [tree shaking](https://webpack.docschina.org/guides/tree-shaking) 指南中介绍过的 `TerserPlugin`。<br />注意，在环境特定的配置中使用 `merge()` 功能，可以很方便地引用 `dev` 和 `prod` 中公用的 common 配置。`webpack-merge` 工具提供了各种 merge(合并) 高级功能，但是在我们的用例中，无需用到这些功能
<a name="77fcf03f"></a>
### npm scripts
现在，我们把 `scripts` 重新指向到新配置。让 `npm start` script 中 `webpack-dev-server` 使用 _development(开发环境)_ 配置文件，而让 `npm run build` script 使用 _production(生产环境)_ 配置文件：<br />package.json
```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --open --config webpack.dev.js",
    "build": "webpack --config webpack.prod.js"
}
```
<a name="27b7ce5e"></a>
### minification(压缩)
设置 [`production mode`](https://webpack.docschina.org/concepts/mode/#mode-production) 配置后，webpack v4+ 会默认压缩你的代码
<a name="fcb3d1ba"></a>
### source mapping(源码映射) 
在_生产环境_中使用 `source-map` 选项，而不是我们在_开发环境_中用到的 `inline-source-map`
```javascript
const merge = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'production',
  devtool: 'source-map'
});
```
避免在生产中使用 _`inline-***`_ 和 _`eval-***`_，因为它们会增加 bundle 体积大小，并降低整体性能