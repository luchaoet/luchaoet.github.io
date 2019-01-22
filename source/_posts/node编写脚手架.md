---
title: node编写脚手架
date: 2018-11-10 16:12:34
tags: ['node', '脚手架']
summary:
---
前端框架，比如vue，react，angular等，为了便于推广和使用，官方都会配套推出一套脚手架
只要输入简单的命令，就可以下载模板完成项目的前期准备，这对于框架入门非常的方便

现在我们就根据脚手架的原理，搭建一个简单脚手架，便于我们对脚手架的理解

### 依赖库
#### commander 
可以自动的解析命令和参数，用于处理用户输入的命令
#### download-git-repo
下载并提取 git 仓库，用于下载项目模板
#### Inquirer
通用的命令行用户界面集合，用于和用户进行交互
#### handlebars
模板引擎，handlebars语法处理，用户提交的信息动态填充到文件中
#### ora
下载过程久的话，可以用于显示下载中的动画效果
#### chalk
可以给终端的字体加上颜色
#### log-symbols
可以在终端上显示出 √ 或 × 等的图标

### 初始化
创建一个项目文件夹，新建一个`index.js`文件，`npm init`生成一个`package.json`文件

### 安装依赖
安装上面介绍的依赖
```plain
npm install commander download-git-repo inquirer handlebars ora chalk log-symbols -S
```

### 定义命令行
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">node.js 内置了对命令行操作的支持，在 package.json 中的 bin 字段可以定义命令名和关联的执行文件。所以现在 package.json 中加上 bin 的内容</span></span>
```json
{
  "name": "nuo-cli",
  "version": "1.0.0",
  "description": "node编写脚手架",
  "bin": {
    "nuo": "index.js"
  },
  ...
}
```

修改index.js文件，这是运行的主要文件
```javascript
const program = require('commander');
program.version('1.0.0', '-v, --version')
.command('init <name>')
.action((name) => {
    console.log(name);
});
program.parse(process.argv);
```
运行下看看效果 node index.js init template


![屏幕快照 2018-11-09 17.22.30.png | center | 352x76](https://cdn.nlark.com/yuque/0/2018/png/115449/1541755368958-6b094244-1ae9-4c3e-87bd-dc6fca094714.png "")

此时可以输出我们的参数

这时候我们既可以去下载模版了
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>download-git-repo</code></span></span><span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> 支持从 Github、Gitlab 和 Bitbucket 下载仓库</span></span>


![20181109173837.png | center | 712x366](https://cdn.nlark.com/yuque/0/2018/png/115449/1541756337761-503c4a16-75c4-43ad-b493-8fea6227b120.png "")


```javascript
const program = require('commander');
const download = require('download-git-repo');
program.version('1.0.0', '-v, --version')
       .command('init <name>')
       .action((name) => {
           download('github:Lucy20209060/Lucy20209060.github.io', name, (err) => {
                console.log(err ? 'Error' : 'Success')
           })
       });
program.parse(process.argv);
```

注意，此处有一个坑，虽然官网说的很清楚，但是还是很多人会出问题，看我的图就可以了


![17_28_55__11_09_2018.jpg | center | 567x439](https://cdn.nlark.com/yuque/0/2018/jpeg/115449/1541755771058-f9e1858f-a247-4367-b5a0-1386a558a1d9.jpeg "")

图已经表示的很清楚了
看到有些论坛说一大堆，不知所云

### 命令行交互
```javascript
const inquirer = require('inquirer');
inquirer.prompt([
    {type: 'input', name: 'author', message: '请输入作者名称',default: "lucy" },
	{type: 'input', name: 'description', message: '请输入项目描述', default: ''}
]).then((answers) => {
    console.log(answers);
})
```
运行下 看看效果


![屏幕快照 2018-11-09 17.44.54.png | center | 508x103](https://cdn.nlark.com/yuque/0/2018/png/115449/1541756732926-91bc6b76-ea06-4758-9220-f0e7f64d04fd.png "")

现在已经接收到了我们自己的参数了，有种打开新大门的感觉，小激动呢

### 渲染模板
接收到了参数，然后把参数写入刚下载好的项目模板
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">这里用 handlebars 的语法对 HTML5/H5Template 仓库的模板中的 package.json 文件做一些修改</span></span>
模板中的package.json文件
```javascript
{
    "name": "...",
    "version": "0.0.1",
    "description": "{{description}}",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "init": "node index.js"
    },
    "author": "{{author}}",
    ...
  }
```

将参数渲染到package.json
```javascript
program.version('1.0.0', '-v, --version')
.command('init <name>')
.action((name) => {
    inquirer.prompt([
        {type: 'input', name: 'author', message: '请输入作者名称',default: "lucy" },
    	{type: 'input', name: 'description', message: '请输入项目描述', default: ''}
    ]).then((answers) => {
        download('github:Lucy20209060/Lucy20209060.github.io',name,{clone: true},(err) => {
            const meta = {
                name,
                description: answers.description,
                author: answers.author
            }
            const fileName = `${name}/package.json`;
            const content = fs.readFileSync(fileName).toString();
            const result = handlebars.compile(content)(meta);
            fs.writeFileSync(fileName, result);
        })
    })
});
```

### 过程优化
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">使用 ora 来提示用户正在下载中</span></span>
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">通过 chalk 来为打印信息加上样式，比如成功信息为绿色，失败信息为红色</span></span>
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">这样子会让用户更加容易分辨，同时也让终端的显示更加的好看</span></span>
<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">除了给打印信息加上颜色之外，还可以使用 log-symbols 在信息前面加上 √ 或 × 等的图标</span></span>
```javascript
// 可以自动的解析命令和参数，用于处理用户输入的命令
const program = require('commander');
// 下载文件
const download = require('download-git-repo');
// 通用的命令行用户界面集合，用于和用户进行交互
const inquirer = require('inquirer');
const fs = require('fs');
// handlebars语法处理
const handlebars = require('handlebars');
const ora = require('ora');
// 图标
const symbols = require('log-symbols');
const chalk = require('chalk');

program.version('1.0.1', '-v, --version')
.command('init <name>')
.action((name) => {
	inquirer.prompt([
		{type: 'input', name: 'author', message: '请输入作者名称',default: "lucy" },
		{type: 'input', name: 'description', message: '请输入项目描述', default: ''}
	]).then((answers) => {
		const spinner = ora('正在下载模板...');
		spinner.start();
		const meta = {
			name,
			description: answers.description,
			author: answers.author
		};
		// 下载模版 第一个参数：github:owner/repoName 第二个参数：模板放置的文件夹
		download('github:Lucy20209060/Lucy20209060.github.io', name, function (err) {
			if(err){
				spinner.fail();
        console.log(symbols.error, chalk.red(err));
			}else{
				spinner.succeed();
				const fileName = `${name}/package.json`;
				const content = fs.readFileSync(fileName).toString();
				const result = handlebars.compile(content)(meta);
				fs.writeFileSync(fileName, result);
				console.log(symbols.success, chalk.green('项目初始化完成'));
			}
		})
	})
});
program.parse(process.argv);
```


![屏幕快照 2018-11-09 17.55.16.png | center | 403x146](https://cdn.nlark.com/yuque/0/2018/png/115449/1541757339668-df5252a9-4fee-4cc9-8ef7-1016499ff294.png "")


<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">完成之后，就可以把脚手架发布到 npm 上面，通过 -g 进行全局安装，就可以在自己本机上执行 </span></span><span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>nuo init  name</code></span></span><span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> 来初始化项目，这样便完成了一个简单的脚手架工具了</span></span>

以上源码，[点击查看](https://github.com/Lucy20209060/nuo-cli)

`vue-cli`原理与这个大同小异，具体分析，可以进入此处查看[地址](https://github.com/Lucy20209060/sourceCode-vue-cli)
