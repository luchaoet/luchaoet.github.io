---
title: webpack—管理资源
date: 2019-03-30 22:52:51
tags: webpack
summary:
---
<a name="b2300fec"></a>
### 加载css文件
安装 `style-loader` 和 `css-loader`
```
npm install --save-dev style-loader css-loader
```
webpack.config.js
```javascript
const path = require('path');

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
      }
    ]
  }
}
```
webpack 根据正则表达式，来确定应该查找哪些文件，并将其提供给指定的 loader。在这个示例中，所有以 `.css` 结尾的文件，都将被提供给 `style-loader` 和 `css-loader`<br />在js文件中`import "index.css"`，于是样式将被插入至html文件的`<head>`标签中
<br /><img width="500" src="https://cdn.nlark.com/yuque/0/2019/png/115449/1553956753134-81d0b4ca-6155-4db3-8b92-e3c1f867ab41.png" />
<a name="dcefd1c3"></a>
### 加载image图片
安装`file-loader`
```
npm install --save-dev file-loader
```
webpack.config.js
```javascript
const path = require('path');

module.exports = {
  entry: './main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.(png|svg|jpg|gif|jpeg)$/,
        use: ['file-loader']
      }
    ]
  }
}
```
在 `import MyImage from './my-image.png'` 时，此图像将被处理并添加到 `output` 目录，_并且_ `MyImage` 变量将包含该图像在处理后的最终 url。在使用 css-loader 时，如前所示，会使用类似过程处理你的 CSS 中的 `url('./my-image.png')`。loader 会识别这是一个本地文件，并将 `'./my-image.png'` 路径，替换为 `output` 目录中图像的最终路径。而 html-loader 以相同的方式处理 `<img src="./my-image.png" />`<br />
<img width="500" src="https://cdn.nlark.com/yuque/0/2019/png/115449/1553957001016-99805e7f-7106-4a77-9792-91b3155ff40e.png" />
<a name="c51e99cb"></a>
### 加载fonts字体
与加载图片相同
```javascript
const path = require('path');

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
  }
}
```
css
```css
@font-face {
  font-family: "iconfont";
  src: url('./utils/font/iconfont.eot?t=1553955901318'); /* IE9 */
  src: url('./utils/font/iconfont.eot?t=1553955901318#iefix') format('embedded-opentype'), /* IE6-IE8 */
  url('data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAAAAKQAAsAAAAABlQAAAJFAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHEIGVgCCcApgbgE2AiQDCAsGAAQgBYRtBy0bkQXIDiVNl4iA7gJAQATVfm89+zYEICMBNaBKRYaFO5ZA/oT5YMQZe0KxEe/n19zbMA/tIXEeEbN6d9veP/F/b9bQEEWjSUk0CBFCI5Ix4HL6E+hB/qsDym1PGnPgmICBpYHugVERFUnkDWMXuIT7BFpNSogdzi5vQ7PMWhaIZ1PXoDkXkmWWbRYaa/Zm8U2D5vQ+fQJfw+/Hf7loJmkoWHUntzNFGP+Fdl6xV7517RPCBHS4AQXWgUxc1KaO1AnG1WlN1ZsF+6oOfmFZ+k6xV5tgf51V2Q7GofyeFK7yVisTyPHkKDA56m2kjo4X3Xe1d/Wlntvq28p8sel7/WN1pPG88qE2OrKx89RdGah0+4H/ai1CQwz+jl2A8m2G4q1AcH5zoildm3/NbRn8/LgZgcp+Hpp35Q5+iaLnQNZVliNlVVHrSUZHLU1o1YoS9vX6GWseOH8uNBu4C0+TiRSFZvNkZtfRoM0GGjXbQas1c4fb9JHqRG7BqmOA0O0dSacfKHT7IjP7jQaDftGoO+rQ6ir6zmyzGGbrgSSNqYj5GpoF3xOOi1uj6k3SS44ms6qQ3yUZmVHMpjLF7Bx5JOfYEJX1HLNAIX0XZ8Fj5Dg+BtK3qMApgzkYTKdF3YtSBd+FVgckomGkCOXVIFOBzyP88ay19PlNRFfi0MiWlpr0LiJFzOlRVkqmBzmn93q13MszkTJdDmMCEiSfC82CWcTh8KGgfpaFFLAUY0QiMChtjxJ99an1je4HlLEOLEsKu6lQPJoaAAA=') format('woff2'),
  url('./utils/font/iconfont.woff?t=1553955901318') format('woff'),
  url('./utils/font/iconfont.ttf?t=1553955901318') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
  url('./utils/font/iconfont.svg?t=1553955901318#iconfont') format('svg'); /* iOS 4.1- */
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.iconjia:before {
  content: "\e620";
}
```
显示结果<br />
<img width="500" src="https://cdn.nlark.com/yuque/0/2019/png/115449/1553957255248-30b509bb-aeb9-47ca-83a3-86b2f1159eae.png" />

该文源码点击[查看](https://github.com/Lucy20209060/webpack-test/tree/master/demo-01)
