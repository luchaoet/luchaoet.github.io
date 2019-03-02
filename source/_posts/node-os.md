---
title: node-os
date: 2019-03-02 19:25:12
tags: ['node', 'os']
summary: 操作系统
---
`os` 模块提供了操作系统相关的实用方法

<a name="os.EOL"></a>
### os.EOL
一个常量，返回当前操作系统的换行符（Windows系统是\r\n，其他系统是\n）

<a name="c816a6ea"></a>
### os.arch()
表明 Node.js 二进制编译所用的 操作系统CPU架构<br />现在可能的值有: `'arm'`, `'arm64'`, `'ia32'`, `'mips'`, `'mipsel'`, `'ppc'`, `'ppc64'`, `'s390'`, `'s390x'`, `'x32'`, `'x64'`

<a name="daf3fff1"></a>
### os.cpus()
返回一个对象数组, 包含每个逻辑 CPU 内核的信息
```json
[ 
	{ model: 'Intel(R) Core(TM) i5-7360U CPU @ 2.30GHz',
    speed: 2300,
    times: { user: 12652330, nice: 0, sys: 9778130, idle: 86080280, irq: 0 } 
	},
  { 
  	model: 'Intel(R) Core(TM) i5-7360U CPU @ 2.30GHz',
    speed: 2300,
    times: { user: 5736150, nice: 0, sys: 3347920, idle: 99411030, irq: 0 } 
	},
  { 
  	model: 'Intel(R) Core(TM) i5-7360U CPU @ 2.30GHz',
    speed: 2300,
    times: { user: 12150280, nice: 0, sys: 6856410, idle: 89488570, irq: 0 } 
	},
  { model: 'Intel(R) Core(TM) i5-7360U CPU @ 2.30GHz',
    speed: 2300,
    times: { user: 5117000, nice: 0, sys: 2678190, idle: 100699870, irq: 0 } 
	} 
]
```

<a name="0563c4f2"></a>
### os.freemem()
以整数的形式回空闲系统内存的字节数

<a name="966a9a17"></a>
### os.homedir()
以字符串的形式返回当前用户的home目录

<a name="698358e3"></a>
### os.hostname()
以字符串的形式返回操作系统的主机名

<a name="f7013d2b"></a>
### os.platform()
返回Node.js编译时的操作系统平台<br />当前可能的值有: `'aix'`，`'darwin'`，`'freebsd'`，`'linux'`，`'openbsd'`，`'sunos'`，`'win32'`

<a name="78448de8"></a>
### os.tmpdir()
返回操作系统的默认临时文件目录