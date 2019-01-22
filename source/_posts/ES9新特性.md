---
title: ES9新特性
date: 2018-11-08 23:54:36
tags: ES9
summary:
---
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">ES2018 是 ECMAScript 标准的最新版本</span></span>

新增一下新特性：
* Rest/Spread属性
* Asynchronous iteration （异步迭代）
* Promise.prototype.finally()
* 正则表达式改进

### Rest/Spread 属性
ES2015引入了Rest参数和扩展运算符。三个点（...）仅用于数组。

Rest参数语法允许我们将一个布丁数量的参数表示为一个数组
```javascript
function foo(x, y, ...z){
    x // 1
    y // 2
    z // [3, 4, 5]
}

foo(1,2,3,4,5)
```

spread运算符以相反的方式工作，并将数组转换为可以传递给函数的单独参数
```javascript

var arr = [1,2,3,4,5]
console.log(...arr) // 1 2 3 4 5
```

ES2018为对象解构提供了和数组一样的Rest参数和展开操作符
```javascript
const {a, d, ...y} = {
  a: 'A',
  b: 'B',
  c: 'C',
  d: 'D'
}
console.log(a); // A
console.log(d); // D
console.log(y); // {b:'B', c: 'C'}
```

扩展运算符在其他对象内使用
```javascript
const x = {
    a: 'A',
    b: 'B'
}
const y = {...x, c:'C'} // {a: 'A', b: 'B', c: 'C'}
```

### Asynchronous iteration （异步迭代）
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">新的 </span></span>`for-await-of`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>构造允许使用异步可迭代对象作为循环迭代
```javascript
for await (const line of readLines(filePath)) {
    console.log(line)
}
```
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">由于这使用 </span></span>`await`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>，你只能在异步函数中使用它，就像普通的 `await`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>一样（参见 async / await 章节）

### Promise.prototype.finally()
当一个 promise 得到满足（fulfilled）时，它会一个接一个地调用 then() 方法
如果在此期间发生错误，则跳过 then() 方法并执行 catch()方法
finally() 允许您运行一些代码，无论 promise 的执行成功或失败
```javascript
fetch('file.json')
.then(data => data.json()) // 成功后调用
.catch(error => console.error(error)) // 失败后调用
.finally(() => console.log('finished')) // 完成后调用 无论成功失败
```

### 正则表达式改进
#### 先行断言(lookahead) 和 后行断言(lookbehind)
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">先行断言(lookahead)</span></span>
`?=`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>匹配一个字符串，该字符串后面跟着一个特定的子字符串
```javascript
// 检测字符串component后面是否跟随Will字符串
/component(?= Will)/.test('This lifecycle was previously named component WillMount') // true
/component(?=Will)/.test('This lifecycle was previously named componentWillMount') // true
/component(?=Mount)/.test('This lifecycle was previously named component WillMount') // false
```

`?!`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>执行逆操作，匹配一个字符串，该字符串后面没有一个特定的子字符串
```javascript
/component(?!Will)/.test('This lifecycle was previously named componentWillMount') // false
```

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">后行断言(lookbehind)：根据前面的内容匹配字符串</span></span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">使用 </span></span>`?<=`
```javascript
// 检测 Will前面是否是component字符串
/(?<=component) Will/.test('componentWillMount') //false
/(?<=component)Will/.test('componentWillMount') //true
```

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">使用 </span></span>`?<!`，<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">逆操作</span></span>
```javascript
/(?<!component) Will/.test('componentWillMount') //true
/(?<!component)Will/.test('componentWillMount') //false
```

#### Unicode 属性转义 \p{…} 和 \P{…}
在正则表达式模式中，您可以使用 `\d` 匹配任何数字，`\s` 匹配任何不为空格的字符，`\w` 匹配任何字母数字字符，依此类推
这个新功能将扩展此概念到引入 `\p{}` 匹配所有 Unicode 字符，否定为 `\P{}`

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">任何 unicode 字符都有一组属性。 例如，</span></span>`Script`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>确定语言系列，`ASCII`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>是一个布尔值， 对于 ASCII 字符，值为 `true`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">，依此类推。 您可以将此属性放在花括号中，正则表达式将检查是否为真</span></span>
```javascript
/^\p{ASCII}+$/u.test('abc')   // true
/^\p{ASCII}+$/u.test('ABC@')  // true
/^\p{ASCII}+$/u.test('ABC🙃') // false
```

`ASCII_Hex_Digit`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>是另一个布尔属性，用于检查字符串是否仅包含有效的十六进制数字
```javascript
/^\p{ASCII_Hex_Digit}+$/u.test('0123456789ABCDEF') //true
/^\p{ASCII_Hex_Digit}+$/u.test('h')                //false
```

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">还有许多其他布尔属性，您只需通过在花括号中添加它们的名称来检查它们，包括 </span></span>`Uppercase`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">, </span></span>`Lowercase`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">, </span></span>`White_Space`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">, </span></span>`Alphabetic`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">, </span></span>`Emoji`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>等
```javascript
/^\p{Lowercase}$/u.test('h') // true
/^\p{Uppercase}$/u.test('H') // true
/^\p{Emoji}+$/u.test('H')   // false
/^\p{Emoji}+$/u.test('🙃🙃') // true
```

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">除了这些二进制属性之外，您还可以检查任何 unicode 字符属性以匹配特定值。在这个例子中，我检查字符串是用希腊语还是拉丁字母写的</span></span>
```javascript
/^\p{Script=Greek}+$/u.test('ελληνικά') // true
/^\p{Script=Latin}+$/u.test('hey') // false
```

#### 命名捕获组（Named capturing groups）
JavaScript正则表达式可以返回匹配对象 - 类似于数组的值，包含匹配的字符串。例如，要以YYYY-MM-DD格式解析日期
```javascript
const reDate = /([0-9]{4})-([0-9]{2})-([0-9]{2})/;
const match  = reDate.exec('2018-04-30');
console.log(match)
```



![屏幕快照 2018-11-08 17.00.56.png | center | 170x145](https://cdn.nlark.com/yuque/0/2018/png/115449/1541667684629-1aa986c0-6fbe-4e75-8c1a-9c1ae45ab34c.png "")


它很难阅读，并且更改正则表达式也可能会更改匹配对象索引，ES2018允许在开始捕获括号后立即使用符号命名组
```javascript
const reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/;  
const match  = reDate.exec('2018-04-30');

const year   = match.groups.year,  // 2018
      month  = match.groups.month, // 04
      day    = match.groups.day;   // 30
```

任何未匹配的命名组都将其属性设置为undefined
