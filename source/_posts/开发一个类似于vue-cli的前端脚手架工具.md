---
title: 开发一个类似于vue-cli的前端脚手架工具
date: 2019-02-25 15:00:37
tags: ['脚手架', 'cli']
summary:
---
现在的前端框架都配套了安装脚手架，比如vue
```
npm install --global vue-cli
```
当我们全局安装`vue-cli`后，我们便可以使用`vue`命令安装我们的vue项目的模板，它通过询问式的流程，引导开发者安装自己需要的模板<br />![20190225103705.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551062392228-7be8bc92-58e6-4c74-ad72-aec1314420b6.png#align=left&display=inline&height=462&linkTarget=_blank&name=20190225103705.png&originHeight=800&originWidth=1292&size=688987&status=done&width=746)<br />这一切是怎么做到的？
<a name="a04f5e4b"></a>
### node命令行工具
<a name="package.json"></a>
#### package.json
初始化项目，产生package.json
```
npm init -y
```
<a name="9e8818a6"></a>
#### 添加命令
在package.json中添加一个命令
```json
"bin": {
	"lv": "index.js"
}
```
<a name="3aa88397"></a>
#### 增加index.js文件
`#!/usr/bin/env node`必不可少，告诉操作系统，使用node解析该文件
```javascript
#!/usr/bin/env node
console.log('lv-cli test')
```
此时执行`node index.js`命令可以执行`index.js`文件<br />那么，如何执行`lv`命令就能执行`index.js`文件呢？<br />首先添加bin之后就可以了，但是我们现在在本地，我们的文件还没有发布至npm，本地也未安装，便没有这个命令，但是我们可以模拟
<a name="0f7e3aee"></a>
#### npm link
在本地开发npm模块的时候，我们可以使用`npm link`命令，将npm模块链接到对应的运行项目中去，方便地对模块进行调试和测试<br />![20190225110643.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551064016494-2ba02e0d-2ba5-413f-8fcf-5404b2a09dd5.png#align=left&display=inline&height=68&linkTarget=_blank&name=20190225110643.png&originHeight=68&originWidth=402&size=17957&status=done&width=402)<br />显然，运行`lv`命令执行了index.js文件<br />这意味着当我们使用npm安装该脚手架后，便能使用lv命令了<br />这是第一步

<a name="af23807f"></a>
### 脚手架相关依赖
<a name="commander"></a>
#### commander
```javascript
#!/usr/bin/env node
const program = require('commander');

program
    .version('1.0.0', '-v, --version')
    .option('-i, --init', 'init something')
    .option('-g, --generate', 'generate something')
    .option('-r, --remove', 'remove something')
    .on('--help', function(){
        console.log('  Examples:');
        console.log('');
        console.log('    this is an example');
        console.log('');
    });
program.parse(process.argv);
```
最后一句别忘了<br />具体可查看该[文章](https://segmentfault.com/a/1190000002918295#articleHeader11)，具体在此不述
<a name="download-git-repo"></a>
#### download-git-repo
从github下载仓库文件
```javascript
const download = require('download-git-repo');
download('github:owner/name', 'test/tmp', function (err) {
  console.log(err ? 'Error' : 'Success')
})
```
第一个参数：模板地址。以`github`为例，`owner`是github帐号名称，`name`是仓库名称<br />第二个参数：下载后保存的文件地址<br />该插件详情点击[查看](https://github.com/flipxfx/download-git-repo#readme)
<a name="inquirer"></a>
#### inquirer
终端交互式命令行，通过提出自定义的问题来获取终用户输入的参数
```javascript
inquirer.prompt([
    {type: 'input', name: 'author', message: '请输入作者名称',default: "lucy" },
    {type: 'input', name: 'description', message: '请输入项目描述', default: 'a vue`s project'}
]).then((answers) => {
    const meta = {
        description: answers.description,
        author: answers.author
    }
    console.log(meta)
})
```

![屏幕快照 2019-02-25 下午1.03.52.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551071043764-64780f6f-f322-4d60-ada2-fe28d31ba0e9.png#align=left&display=inline&height=127&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-25%20%E4%B8%8B%E5%8D%881.03.52.png&originHeight=148&originWidth=872&size=184047&status=done&width=746)<br />该插件详情点击[查看](https://github.com/SBoudrias/Inquirer.js#readme)
<a name="fs"></a>
#### fs
node对系统文件及目录进行读写操作的模块。详情可查看[官网](http://nodejs.cn/api/fs.html)
<a name="handlebars"></a>
#### handlebars
handlebars语法处理
```javascript
const handlebars = require('handlebars');

var source = `
	<div class="entry">
    <h1>{{title}}</h1>
    <div class="body">
      {{body}}
    </div>
  </div>
`;
var context = {title: "标题", body: "我是字符串"}
const result = handlebars.compile(source)(context);
console.log(result)
```
处理结果
```html
<div class="entry">
  <h1>标题</h1>
    <div class="body">
      我是字符串
    </div>
</div>
```
<a name="ora"></a>
#### ora
终端loading效果
```javascript
const ora = pequire('ora');

const spinner = ora('稍候...');
spinner.start();
setTimeout(()=>{
    spinner.succeed();
},5000)
```
`spinner.fail()`与`spinner.succeed()`的区别在于前面的图标不同<br />![20190225143308.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551076403950-0bfba2e6-5acc-46aa-8bb1-c6e3bd645a22.png#align=left&display=inline&height=134&linkTarget=_blank&name=20190225143308.png&originHeight=134&originWidth=556&size=115617&status=done&width=556)
<a name="log-symbols"></a>
#### log-symbols
终端log图标
```javascript
const symbols = require('log-symbols');

console.log(symbols.error,'error');
console.log(symbols.success,'success');
```
![20190225143823.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551076729826-a04cd7b9-ce5f-4843-9be9-5e28d8ac6d28.png#align=left&display=inline&height=70&linkTarget=_blank&name=20190225143823.png&originHeight=70&originWidth=188&size=22036&status=done&width=188)
<a name="chalk"></a>
#### chalk
终端日志字体颜色
```javascript
console.log(chalk.red('error'));
console.log(chalk.green('success'));
```
![20190225144224.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551076979448-f9212423-16fc-44ea-a16c-2ea348bf472d.png#align=left&display=inline&height=66&linkTarget=_blank&name=20190225144224.png&originHeight=66&originWidth=146&size=17040&status=done&width=146)

ok 相关依赖介绍完毕

<a name="1ef4aca3"></a>
### 脚手架代码

```javascript
#!/usr/bin/env node
// 可以自动的解析命令和参数，用于处理用户输入的命令
const program = require('commander');
// 下载文件
const download = require('download-git-repo');
// 通用的命令行用户界面集合，用于和用户进行交互
const inquirer = require('inquirer');
// 文件操作
const fs = require('fs');
// handlebars语法处理
const handlebars = require('handlebars');
// loading效果
const ora = require('ora');
// 图标
const symbols = require('log-symbols');
// 颜色插件
const chalk = require('chalk');

program.version('1.0.1', '-v, --version')
.command('init <name>')
.action((name) => {
	inquirer.prompt([
		{type: 'input', name: 'author', message: '请输入作者名称',default: "lucy" },
		{type: 'input', name: 'description', message: '请输入项目描述', default: 'a vue`s project'}
	]).then((answers) => {
		const spinner = ora('正在下载模板...');
		spinner.start();
		const meta = {
			name,
			description: answers.description,
			author: answers.author
		};
		// 下载模版 第一个参数：github:owner/repoName 第二个参数：模板放置的文件夹
		download('github:Lucy20209060/vue-template', name, function (err) {
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

<a name="32f91adf"></a>
### 发布至npm
关于如何发布npm包，请查看我的下一篇文章