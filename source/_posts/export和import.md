---
title: export和import
date: 2018-12-19 22:17:10
tags: ['es6', 'export', 'import']
summary:
---
<span data-type="color" style="color:rgb(51, 51, 51)">模块功能主要由两个命令构成：</span>`export`<span data-type="color" style="color:rgb(51, 51, 51)">和</span>`import`
`export`<span data-type="color" style="color:rgb(51, 51, 51)">命令用于规定模块的对外接口</span>
`import`<span data-type="color" style="color:rgb(51, 51, 51)">命令用于引入其他模块提供的功能</span>

### export命令
<span data-type="color" style="color:rgb(51, 51, 51)">外部需要读取模块内部的某个变量，就必须使用</span>`export`<span data-type="color" style="color:rgb(51, 51, 51)">关键字输出该变量</span>
tools.js
```javascript
export const funcA = function(){...}
export const funcB = function(){...}
```

或者
```javascript
const funcA = function(){...}
const funcB = function(){...}

export {funcA, funcB }
```

其他文件引入
```javascript
import { funcA, funcB } from 'tools'
console.log(funcA, funcB)
```

`export`<span data-type="color" style="color:rgb(51, 51, 51)">输出的变量就是本来的名字，但是可以使用</span>`as`<span data-type="color" style="color:rgb(51, 51, 51)">关键字重命名一次或多次</span>
```javascript
function funcA() { ... }
function funcB() { ... }

export {
  funcA as renameA,
  funcB as renameB,
  funcB as renamec
};
```

总结这三种写法
```javascript
// 写法一
export const funA = function(){...}

// 写法二
const funA = function(){...}
export { funA }

// 写法三
const funA = function(){...}
export { funA as renameA }
```

另外，`export`语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值
```javascript
export var foo = 'bar';
setTimeout(() => foo = 'baz', 500);
```

上面代码输出变量`foo`，值为`bar`，500 毫秒之后变成`baz`

`export`命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错，下一节的`import`命令也是如此。这是因为处于条件代码块之中，就没法做静态优化了，违背了 ES6 模块的设计初衷

```javascript
function foo() {
  export default 'bar' // SyntaxError
}
foo()
```

上面代码中，`export`语句放在函数之中，结果报错

### import命令
<span data-type="color" style="color:rgb(51, 51, 51)">使用</span>`export`<span data-type="color" style="color:rgb(51, 51, 51)">命令定义了模块的对外接口以后，其他 JS 文件就可以通过</span>`import`<span data-type="color" style="color:rgb(51, 51, 51)">命令加载这个模块</span>
`import`命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块对外接口的名称相同

如果想为输入的变量重新取一个名字，`import`命令要使用`as`关键字，将输入的变量重命名
```javascript
import { funcA as renameA } from 'tools';
```

`import`<span data-type="color" style="color:rgb(51, 51, 51)">后面的</span>`from`<span data-type="color" style="color:rgb(51, 51, 51)">指定模块文件的位置，可以是相对路径，也可以是绝对路径，</span>`.js`<span data-type="color" style="color:rgb(51, 51, 51)">后缀可以省略。如果只是模块名，不带有路径，那么必须有配置文件，告诉 JavaScript 引擎该模块的位置</span>

注意，`import`命令具有提升效果，会提升到整个模块的头部，首先执行
```javascript
funcA();
import { funcA } from 'tools';
```

### 模块整体加载
<span data-type="color" style="color:rgb(51, 51, 51)">除了指定加载某个输出值，还可以使用整体加载，即用星号（</span>`*`<span data-type="color" style="color:rgb(51, 51, 51)">）指定一个对象，所有输出值都加载在这个对象上面</span>
```javascript
// tools.js
var funcA = function(){}
var funcB = function(){}
export {funcA , funcB}
```

引入
```javascript
import * as obj from 'tools'

// obj对象包含tools.js文件导出的所有变量
```

### export default命令
<span data-type="color" style="color:rgb(51, 51, 51)">用到</span>`export default`<span data-type="color" style="color:rgb(51, 51, 51)">命令，为模块指定默认输出</span>
```javascript
// 模块默认输出一个函数
tools.js
export default function () {
  console.log('foo');
}
```
等同于
```javascript
function foo() {
  console.log('foo');
}

export default foo;
```
<span data-type="color" style="color:rgb(51, 51, 51)">上面代码中，</span>`foo`<span data-type="color" style="color:rgb(51, 51, 51)">函数的函数名</span>`foo`<span data-type="color" style="color:rgb(51, 51, 51)">，在模块外部是无效的。加载的时候，视同匿名函数加载</span>

此时，引入时可以随意为该匿名函数命名
```javascript
import customName from 'tools';
customName(); // foo
```

对比一下默认与正常的导入导出
```javascript
// export default
export default function funcA() {...}
  // ...
}
import funcA from 'tools';

// export
export function funcA() {...};
import { funcA } from 'tools';
```
`export default`<span data-type="color" style="color:rgb(51, 51, 51)">时，对应的</span>`import`<span data-type="color" style="color:rgb(51, 51, 51)">语句不需要使用大括</span>
<span data-type="color" style="color:rgb(51, 51, 51)">使用</span>`export`<span data-type="color" style="color:rgb(51, 51, 51)">时，对应的</span>`import`<span data-type="color" style="color:rgb(51, 51, 51)">语句需要使用大括号</span>

本质上，`export default`就是输出一个叫做`default`的变量或方法，然后系统允许你为它取任意名字
所以，下面的写法是有效的
```javascript
// tools.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as foo } from 'tools';
// 等同于
// import foo from 'tools';
```

正是因为`export default`命令其实只是输出一个叫做`default`的变量，所以它后面不能跟变量声明语句
```javascript
// 正确
export var a = 1;

// 正确
var a = 1;
export default a;

// 错误
export default var a = 1;
```
<span data-type="color" style="color:rgb(51, 51, 51)">上面代码中，</span>`export default a`<span data-type="color" style="color:rgb(51, 51, 51)">的含义是将变量</span>`a`<span data-type="color" style="color:rgb(51, 51, 51)">的值赋给变量</span>`default`<span data-type="color" style="color:rgb(51, 51, 51)">。所以，最后一种写法会报错</span>

如果想在一条`import`语句中，同时输入默认方法和其他接口，可以写成下面这样
```
import _, { each, forEach } from 'lodash';
```

对应上面代码的`export`语句如下
```
export default function (obj) {...}

export function each(obj, iterator, context) {...}

export { each as forEach };
```

`export default`也可以用来输出类
```javascript
// MyClass.js
export default class { ... }

// main.js
import MyClass from 'MyClass';
let o = new MyClass();
```

