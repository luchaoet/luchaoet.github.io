---
title: node-fs
date: 2019-02-28 14:24:57
tags: ['node', 'fs']
summary: 文件系统
---
fs模块用于对系统文件及目录进行读写操作

<a name="0b7ece20"></a>
### 同步与异步
所有文件系统操作都具有同步和异步的形式<br />异步的形式总是将完成回调作为其最后一个参数。 传给完成回调的参数取决于具体方法，但第一个参数始终预留用于异常。 如果操作成功完成，则第一个参数将为 `null` 或 `undefined`
```javascript
const fs = require('fs');

fs.unlink('/tmp/hello', (err) => {
  if (err) throw err;
  console.log('已成功删除 /tmp/hello');
});
```
使用同步的操作发生的异常会立即抛出，可以使用 `try`/`catch` 处理，也可以允许冒泡
```javascript
const fs = require('fs');

try {
  fs.unlinkSync('/tmp/hello');
  console.log('已成功删除 /tmp/hello');
} catch (err) {
  // 处理错误
}
```
同步方法执行完并返回结果后，才能执行后续的代码。而异步方法采用回调函数接收返回结果，可以立即执行后续代码

<a name="de52bec6"></a>
### readFile读取文件
有以下文件 tmp/test.js
```javascript
console.log(1)
```
读取文件
```javascript
fs.readFile('./tmp/test.js', function(err, data) {
    if (err) {
        throw err;
    }
    // 读取文件成功
    console.log(data); # <Buffer 63 6f 6e 73 6f 6c 65 2e 6c 6f 67 28 31 29>
});
```
这是原始二进制数据在缓冲区中的内容，要显示文件内容可以使用`toString()`或者设置输出编码
```javascript
fs.readFile('./tmp/test.js', function(err, data) {
    if (err) {
        throw err;
    }
    console.log(data.toString()); # console.log(1)
});
```
或者
```javascript
fs.readFile('./tmp/test.js', 'utf-8', function(err, data) {
    if (err) {
        throw err;
    }
    console.log(data); # console.log(1)
});
```

<a name="1dbf2734"></a>
### WriteFile写入文件
直接清空文件，再写入
```javascript
fs.writeFile('./tmp/test.js', 'console.log(123)', function(err) {
    // ...
});
```
默认flag='w'是写，会清空文件，想要追加，可以传递一个flag参数<br />flag传值，r代表读取文件，w代表写文件，a代表追加<br /><br />
<a name="c42bbf2f"></a>
### fs.read和fs.write读写文件
fs.read和fs.write功能类似fs.readFile和fs.writeFile()，但提供更底层的操作，实际应用中多用fs.readFile和fs.writeFile<br />使用fs.read和fs.write读写文件需要使用fs.open打开文件和fs.close关闭文件
<a name="5e50153f"></a>
#### fs.open(path, flags, ?mode, callback)
path: 文件路径或者Buffer<br />flags: 文件系统标志，具体设置可参考[官网](http://nodejs.cn/api/fs.html#fs_file_system_flags)
<a name="4d2400ca"></a>
#### fs.read()
```javascript
// 打开文件
fs.open('./tmp/test.js', 'r', function(err, fd) {
    if (err) {
        throw err;
    }
    console.log('open file success.');
    var buffer = Buffer.alloc(255);
    // 读取文件
    fs.read(fd, buffer, 0, 16, 0, function(err, bytesRead, buffer) {
        if (err) {
            throw err;
        }
        // 打印出buffer中存入的数据
        console.log(bytesRead, buffer.slice(0, bytesRead).toString());

        // 关闭文件
        fs.close(fd, (err) => {
            console.log(err)
        });
    });
});
```
<a name="9c37c440"></a>
#### fs.write()
```javascript
fs.open('./tmp/test.js', `w`, function(err, fd) {
    if (err) {
        throw err;
    }
    console.log('open file success.');
    var buffer = Buffer.alloc(17, 'console.log(2333)');
    // 读取文件
    fs.write(fd, buffer, 0, 17, 0, function(err, bytesWritten, buffer) {
        if (err) {
            throw err;
        }

        console.log('write success.');
        // 打印出buffer中存入的数据
        console.log(bytesWritten, buffer.slice(0, bytesWritten).toString());

        // 关闭文件
        fs.close(fd, () => {});
    });
});
```

<a name="72befded"></a>
### readdir读取目录
使用`fs.readdir(path,callback)`读取文件目录
```javascript
fs.readdir('./tmp', function(err, files) {
    if (err) {
        throw err;
    }
    // files是一个数组 包含此目录下所有的文件和文件夹名称
    console.log(files); // [ 'file_test', 'test.js' ] 
});
```

<a name="f06b40ef"></a>
### mkdir创建目录
使用`fs.mkdir(path, ?mode, callback)`创建目录<br />path是需要创建的目录<br />mode是目录的权限（默认是0777）<br />callback是回调函数
```python
fs.mkdir('./tmp', function(err) {
    // ...
});
```