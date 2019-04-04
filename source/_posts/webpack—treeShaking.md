---
title: webpack—tree shaking
date: 2019-04-04 14:05:06
tags: webpack
summary:
---
tree shaking通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。它依赖于 ES2015 模块语法的静态结构特性，例如 `import` 和 `export`。这个术语和概念实际上是由 ES2015 模块打包工具 rollup 普及起来的。<br />webpack 2 正式版本内置支持 ES2015 模块（也叫做 _harmony modules_）和未使用模块检测能力。新的 webpack 4 正式版本扩展了此检测能力，通过 `package.json` 的 `"sideEffects"` 属性作为标记，向 compiler 提供提示，表明项目中的哪些文件是 "pure(纯的 ES2015 模块)"，由此可以安全地删除文件中未使用的部分
<a name="19408700"></a>
### 添加一个通用模块
src/main.js
```javascript
export function square(x) {
  return x * x;
}

export function cube(x) {
  return x * x * x;
}
```
将 `mode` 配置选项设置为 `development` 以确保 bundle 是未压缩版本<br />webpack.config.js
```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
	},
 mode: 'development',
 optimization: {
   usedExports: true
 }
};
```
src/index.js
```javascript
import { cube } from './main.js';
import './style.css';

var a = cube(3);
console.log(a) // 27
```
只使用了`cube`方法，并没有引用`square`方法，这个函数就是所谓的“未引用代码(dead code)”
<a name="1d8ec8b4"></a>
### 将文件标记为 side-effect-free(无副作用)
在一个纯粹的 ESM 模块世界中，很容易识别出哪些文件有 side effect。然而，我们的项目无法达到这种纯度，所以，此时有必要提示 webpack compiler 哪些代码是“纯粹部分”<br />通过 package.json 的 `"sideEffects"` 属性，来实现这种方式
```json
{
  "name": "your-project",
  "sideEffects": false
}
```
如果所有代码都不包含 side effect，我们就可以简单地将该属性标记为 `false`，来告知 webpack，它可以安全地删除未用到的 export<br />"side effect(副作用)" 的定义是，在导入时会执行特殊行为的代码，而不是仅仅暴露一个 export 或多个 export。举例说明，例如 polyfill，它影响全局作用域，并且通常不提供 export<br />如果你的代码确实有一些副作用，可以改为提供一个数组
```json
{
  "name": "your-project",
  "sideEffects": [
    "./src/some-side-effectful-file.js"
  ]
}
```
注意，所有导入文件都会受到 tree shaking 的影响。这意味着，如果在项目中使用类似 css-loader 并 import 一个 CSS 文件，则需要将其添加到 side effect 列表中，以免在生产模式中无意中将它删除
```json
{
  "name": "your-project",
  "sideEffects": [
    "./src/some-side-effectful-file.js",
    "*.css"
  ]
}
```
<a name="e5fb04b2"></a>
### 压缩输出结果
webpack.config.js
```
mode: 'production'
```
`npm run build`，你发现 `dist/bundle.js` 中的差异了吗？显然，现在整个 bundle 都已经被 minify(压缩) 和 mangle(混淆破坏)，但是如果仔细观察，则不会看到引入 `square` 函数，但能看到 `cube` 函数的混淆破坏版本。现在，随着 minification(代码压缩) 和 tree shaking，我们的 bundle 减小几个字节！虽然，在这个特定示例中，可能看起来没有减少很多，但是，在有着复杂依赖树的大型应用程序上运行 tree shaking 时，会对 bundle 产生显著的体积优化