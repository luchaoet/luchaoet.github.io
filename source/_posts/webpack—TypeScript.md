---
title: webpack—TypeScript
date: 2019-04-10 14:45:23
tags: ['webpack', 'TypeScript']
summary:
---
TypeScript 是 JavaScript 的超集，为其增加了类型系统，可以编译为普通 JavaScript 代码
<a name="b6453aea"></a>
### 基础配置[](https://webpack.docschina.org/guides/typescript/#%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE)
首先，执行以下命令安装 TypeScript compiler 和 loader
```
npm install --save-dev typescript ts-loader
```
现在，我们将修改目录结构和配置文件<br />添加tsconfig.json<br />设置一个基本的配置，来支持 JSX，并将 TypeScript 编译到 ES5
```
{
    "compilerOptions": {
        "outDir": "./dist/",
        "sourceMap": true,
        "noImplicitAny": true,
        "module": "es6",
        "target": "es5",
        "jsx": "react",
        "allowJs": true
    }
}
```
查看 [TypeScript 官方文档](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)了解更多关于 `tsconfig.json` 的配置选项<br />webpack.config.js
```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  entry: './src/index.ts',
  devtool: 'inline-source-map',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/
      }
    ]
  },
  resolve: {
    extensions: [ '.tsx', '.ts', '.js' ]
  },
  plugins: [
    new CleanWebpackPlugin(),
    // new HtmlWebpackPlugin({ template: './src/index.html' }),
    new HtmlWebpackPlugin({
      title: 'TypeScript'
    })
  ]
}
```
这会直接将 webpack 的入口起点指定为 `./index.ts`，然后通过 `ts-loader` 加载所有的 `.ts` 和 `.tsx` 文件，并且在当前目录输出到 `bundle.js` 文件

以上源码[查看](https://github.com/Lucy20209060/webpack-test/tree/master/demo-12)