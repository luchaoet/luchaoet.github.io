---
title: node-path
date: 2019-03-03 15:29:25
tags: ['node', 'path']
summary: 路径
---
`path` 模块提供用于处理文件路径和目录路径的实用工具

<a name="dcab3a1c"></a>
### path.basename(path, ?ext)
返回 `path` 的最后一部分<br />ext: 可选的文件扩展名
```javascript
const path = require('path');

const a = path.basename('/tmp/index.html'); // index.html
const b = path.basename('/tmp/index.html', '.html'); // index
```

<a name="ea703498"></a>
### path.dirname(path)
返回 `path` 的目录名
```javascript
const path = require('path');

const a = path.dirname('/tmp/index.html'); //  /tmp
```

<a name="ebae83b1"></a>
### path.extname(path)
返回 `path` 的扩展名
```javascript
path.extname('index.html'); // .html
```

<a name="f9f6c9a2"></a>
### path.isAbsolute(path)
检测path是否为绝对路径
```javascript
path.isAbsolute('/tmp/test'); // true
path.isAbsolute('./tmp/test'); // false
```

<a name="f9c9d28b"></a>
### path.join(path1, path2, ....)
将所有给定的 `path` 片段连接在一起，然后规范化生成的路径
```javascript
path.join('/foo', 'bar', 'baz/asdf'); //  /foo/bar/baz/asdf
```

<a name="409c96a8"></a>
### path.parse(path)
返回一个对象，其属性表示 `path` 的重要元素
```javascript
path.parse('/home/user/dir/file.txt');
/*
{ 
	root: '/',
  dir: '/home/user/dir',
  base: 'file.txt',
  ext: '.txt',
  name: 'file' 
}
*/
```

<a name="1341a6c6"></a>
### path.relative(from, to)
根据当前工作目录返回 `from` 到 `to` 的相对路径
```javascript
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
// '../../impl/bbb'
```

<a name="path.sep"></a>
### path.sep
提供平台特定的路径片段分隔符：
* Windows 上是 `\`
* POSIX 上是 `/`
```javascript
// 在 POSIX 上
'foo/bar/baz'.split(path.sep); // ['foo', 'bar', 'baz']

// 在 Windows 上
'foo\\bar\\baz'.split(path.sep); // ['foo', 'bar', 'baz']
```