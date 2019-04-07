---
title: webpack—懒加载
date: 2019-04-07 23:17:08
tags: webpack
summary:
---
懒加载或者按需加载，是一种很好的优化网页或应用的方式。这种方式实际上是先把你的代码在一些逻辑断点处分离开，然后在一些代码块中完成某些操作后，立即引用或即将引用另外一些新的代码块。这样加快了应用的初始加载速度，减轻了它的总体体积，因为某些代码块可能永远不会被加载<br />src/index.js
```javascript
import _ from 'lodash';
function component() {
  var element = document.createElement('div');
  var button = document.createElement('button');
  var br = document.createElement('br');

  button.innerHTML = 'Click me';
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  element.appendChild(br);
  element.appendChild(button);
  button.onclick = e => import(/* webpackChunkName: "print" */ './print').then(module => {
    var print = module.default;
    print();
  });

  return element;
}
document.body.appendChild(component());
```
src/print.js
```javascript
console.log('print.js加载成功');

export default () => {
  console.log('Button Clicked');
};
```
`npm run dev`运行后发现，当你点击按钮时，`print.bundle.js`文件被立即加载进来，按钮点击事件执行
```
print.js加载成功  print.js:5
Button Clicked   print.js:5
```
<a name="ed99eaf8"></a>
### 框架 [](https://webpack.docschina.org/guides/lazy-loading/#%E6%A1%86%E6%9E%B6)
许多框架和类库对于如何用它们自己的方式来实现（懒加载）都有自己的建议。这里有一些例子：
* React: [Code Splitting and Lazy Loading](https://reacttraining.com/react-router/web/guides/code-splitting)
* Vue: [Lazy Load in Vue using Webpack's code splitting](https://alexjoverm.github.io/2017/07/16/Lazy-load-in-Vue-using-Webpack-s-code-splitting/)
* AngularJS: [AngularJS + Webpack = lazyLoad](https://medium.com/@var_bin/angularjs-webpack-lazyload-bb7977f390dd) by [@var_bincom](https://twitter.com/var_bincom)