---
title: Parcel搭建React项目
date: 2018-11-21 23:09:02
tags: ['Parcel', 'React']
summary:
---
以前鉴于parcel快速零配置的便捷性，经常用来快速搭建各种测试学习的demo，而不会没想过用来做正式的项目
今天我就来尝试一下，用parcel搭建一个可以用于生产的基本框架

首先，我觉得一个前端项目应该具备以下基本条件：
* 路由
* sass/less
* 热更新

### 创建项目文件
```plain
mkdir parcel-react-example
cd parcel-react-example
```

### 初始化npm（默认使用npm）
```plain
npm init -y
```

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">此时会创建要给 package.json 文件，文件内容：</span></span>
```json
{
  "name": "parcel-react-example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

### 添加React相关依赖
```plain
npm install react react-dom --save
```

### 添加parcel相关依赖
```plain
npm install parcel-bundler babel-preset-react --save-dev
```

### 支持sass
```plain
npm install node-sass
```


### 根据项目文件夹创建以下文件


![屏幕快照 2018-11-19 15.37.44.png | center | 232x265](https://cdn.nlark.com/yuque/0/2018/png/115449/1542613459624-dcc50811-f914-460a-8a79-2818c4d28068.png "")


#### index.html
```html
<html>
  <head>
    <title>Parcel React Example</title>
    <meta charset="UTF-8"/>
  </head>
  <body>
    <div id="app"></div>

    <script src="main.js"></script>
  </body>
</html>
```

#### main.js
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App/>, document.getElementById('app'));
```

#### App.js
```javascript
import React, { Component } from 'react';
import './App.scss';

export default class App extends Component {
  render() {
    return <div>
      <h1>Hello World📦 🚀</h1>
    </div>;
  }
}
```

#### App.scss
```css
html,
body {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}
```

#### .gitignore
```plain
.DS_Store
.cache
node_modules/
dist/
npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

### 添加打包命令
```json
"scripts": {
    "start": "parcel src/index.html --open"
  }
```

`npm run start`运行，则可自动打开页面，并且自带热更新功能，贴心


![20181119154240.png | center | 747x260](https://cdn.nlark.com/yuque/0/2018/png/115449/1542613373989-5774e288-ad3f-40c9-b83b-ea3d921a1bc3.png "")


那么，我们便初步完成项目的搭建

接下来，引入路由

### 创建src/pages
文件中随便创建两个页面文件 Index和About


![屏幕快照 2018-11-19 16.51.40.png | center | 266x182](https://cdn.nlark.com/yuque/0/2018/png/115449/1542617516235-18b3059e-f039-4f46-8112-9520c8eabcf1.png "")


页面随便写点内容
```javascript
import React, { Component } from 'react';
import './index.scss';

export default class Index extends Component {
  render() {
    return (
			<div>Index page</div>
    );
  }
}
```

### 修改App.js
```javascript
import React, { Component } from 'react';
import './App.scss';
import { BrowserRouter as Router, Route,Redirect } from 'react-router-dom';
// 引入页面
import About from './pages/About/index'
import Index from './pages/Index/index'

export default class App extends Component {
  render() {
    return (
      <Router>
          <div className="app_pages_wrap">
            // 重定向
            <Route exact path="/" render={ () => ( <Redirect to="/index" /> ) } />
            // 页面路由
            <Route path="/index" component={Index}/>
            <Route path="/about" component={About}/>
          </div>
      </Router>
    )
  }
}
```

访问 /index 和 /about 页面即可显示对应的页面内容
路由部分初步完成
其实到此，差不多已经可以扛起一个中小型项目了
以上源码查看[地址](https://github.com/Lucy20209060/parcelTest/tree/master/parcel-react)
