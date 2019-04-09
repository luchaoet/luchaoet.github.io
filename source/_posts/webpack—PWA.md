---
title: webpack—PWA
date: 2019-04-09 17:54:58
tags: ['webpack', 'PWA']
summary:
---
渐进式网络应用程序(progressive web application - PWA)，是一种可以提供类似于native app(原生应用程序) 体验的 web app(网络应用程序)。PWA 可以用来做很多事。其中最重要的是，在**离线(offline)**时应用程序能够继续运行功能。这是通过使用名为 [Service Workers](https://developers.google.com/web/fundamentals/primers/service-workers/) 的 web 技术来实现的<br />如何为我们的应用程序添加离线体验。我们将使用名为 [Workbox](https://github.com/GoogleChrome/workbox) 的 Google 项目来实现此目的，该项目提供的工具可帮助我们更简单地为 web app 提供离线支持<br />到目前为止，我们一直是直接查看本地文件系统的输出结果。通常情况下，真正的用户是通过网络访问 web app；用户的浏览器会与一个提供所需资源（例如，`.html`, `.js` 和 `.css` 文件）的 **server** 通讯<br />我们通过搭建一个简易 server 下，测试下这种离线体验。这里使用 [http-server](https://www.npmjs.com/package/http-server) package
```
npm install http-server --save-dev
```
还要修改 `package.json` 的 `scripts` 部分，来添加一个 `start` script<br />**package.json**
```
{
  ...
  "scripts": {
    "build": "webpack",
    "start": "http-server dist"
  },
  ...
}
```
运行命令 `npm run build` 来构建你的项目。然后运行命令 `npm start`<br />![屏幕快照 2019-04-09 下午5.20.22.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1554801678603-98516a83-6769-4170-b2eb-eb77c4c5c0b1.png#align=left&display=inline&height=138&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8B%E5%8D%885.20.22.png&originHeight=238&originWidth=1282&size=385230&status=done&width=746)<br />打开浏览器访问 `http://localhost:8080` (即 `http://127.0.0.1`)，你应该会看到 webpack 应用程序被 serve 到 `dist` 目录。如果停止 server 然后刷新，则 webpack 应用程序不再可访问<br />这就是我们为实现离线体验所需要的改变。我们应该要实现的是，停止 server 然后刷新，仍然可以看到应用程序正常运行
<a name="ccdc576b"></a>
### 添加 Workbox [](https://webpack.docschina.org/guides/progressive-web-application/#%E6%B7%BB%E5%8A%A0-workbox)
添加 workbox-webpack-plugin 插件，然后调整 `webpack.config.js` 文件：
```
npm install workbox-webpack-plugin --save-dev
```
**webpack.config.js**
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const WorkboxPlugin = require('workbox-webpack-plugin');

module.exports = {
  entry: './main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      // 加载css文件
      {
        test: /\.css$/,
        use: ['style-loader','css-loader']
      },
      {
        test: /\.(png|svg|jpg|gif|jpeg)$/,
        use: ['file-loader']
      },
      {
        test: /\.(woff|woff2|eot|ttf|otf)$/,
        use: ['file-loader']
      }
    ]
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: 'demo-11-渐进式网络应用程序'
    }),
    new WorkboxPlugin.GenerateSW({
      // 这些选项帮助快速启用 ServiceWorkers
      // 不允许遗留任何“旧的” ServiceWorkers
      clientsClaim: true,
      skipWaiting: true
    })
  ] 
}
```
执行 `npm run build`<br />![屏幕快照 2019-04-09 下午5.34.00.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1554802599055-a4ec3bb3-1b19-4245-842a-39ed451c1183.png#align=left&display=inline&height=82&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8B%E5%8D%885.34.00.png&originHeight=178&originWidth=1624&size=397692&status=done&width=746)<br />可以看到，生成了两个额外的文件：`service-worker.js` 和名称冗长的 `precache-manifest.bb5c1bd566fe684ea1dba4c8dbebe307.js`<br />`service-worker.js` 是 Service Worker 文件<br />`precache-manifest.b5ca1c555e832d6fbf9462efd29d27eb.js` 是 `service-worker.js` 引用的文件，所以它也可以运行。你本地生成的文件可能会有所不同；但是应该会有一个 `service-worker.js` 文件。<br />所以，我们现在已经创建出一个 Service Worker。接下来该做什么？
<a name="25d5da68"></a>
### 注册 Service Worker
**main.js**
```javascript
if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
        navigator.serviceWorker.register('/service-worker.js').then(registration => {
            console.log('SW registered: ', registration);
        }).catch(registrationError => {
            console.log('SW registration failed: ', registrationError);
        });
    });
}

var div = document.createElement('div');
var text = document.createTextNode('demo-01');
div.appendChild(text)
document.querySelector('body').appendChild(div);

```
再次运行 `npm build build` 来构建包含注册代码版本的应用程序。然后用 `npm start` 将构建结果 serve 到服务下。导航至 `http://localhost:8080` 并查看 console 控制台。应该看到<br />![屏幕快照 2019-04-09 下午5.51.46.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1554803560079-ef8ca9dc-717e-4c83-9a6c-abe5500cc2bd.png#align=left&display=inline&height=50&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8B%E5%8D%885.51.46.png&originHeight=102&originWidth=1518&size=57632&status=done&width=746)现在停止 server 并刷新页面。如果浏览器能够支持 Service Worker，应该可以看到你的应用程序还在正常运行。然而，server 已经**停止** serve 整个 dist 文件夹，此刻是 Service Worker 在进行 serve<br /><br /><br />以上源码[查看](https://github.com/Lucy20209060/webpack-test/tree/master/demo-11)