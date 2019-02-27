---
title: node-assert断言
date: 2019-02-27 16:15:57
tags: ['node', 'assert']
summary:
---
`assert` 模块提供了一组简单的断言测试，可用于测试不变量<br />存在严格模式（`strict`）和遗留模式（`legacy`），但建议仅使用严格模式

<a name="90e7a36c"></a>
### assert.AssertionError 类
`Error` 的子类，表明断言的失败。 `assert` 模块抛出的所有错误都是 `AssertionError` 类的实例

<a name="08a6d5e2"></a>
### assert(value, ?message)
assert.ok()的别名<br />value: 检查是否为真的输入<br />message: value为false时的提示信息
```javascript
const assert = require('assert');
assert(1===2, 'this is an error');
```
![20190227102447.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551234310836-78a5e86e-bec6-4364-bfde-5aa34049cf77.png#align=left&display=inline&height=316&linkTarget=_blank&name=20190227102447.png&originHeight=548&originWidth=1292&size=928697&status=done&width=746)

<a name="3f5f5a04"></a>
### assert.deepStrictEqual(actual, expected, ?message)
测试 actual 参数与 expected 参数是否深度相等
```javascript
const obj1 = {
    a: {b: 1}
};
const obj2 = {
    a: {b: 2}
};
const obj3 = {
    a: {b: 1}
};
const obj4 = Object.create(obj1); // {}
assert.deepEqual(obj1, obj2); # AssertionError [ERR_ASSERTION]: { a: { b: 1 } } deepEqual { a: { b: 2 } }
assert.deepEqual(obj1, obj4); # AssertionError [ERR_ASSERTION]: { a: { b: 1 } } deepEqual {}
```
如果值不相等，则抛出 `AssertionError`，并将 `message` 属性设置为等于 `message` 参数的值。 如果未定义 `message` 参数，则会分配默认错误消息。 如果 `message` 参数是 `Error` 的实例，那么它将被抛出而不是 `AssertionError`
<a name="7387576e"></a>
#### 比较运算的详细说明
* 使用 SameValue比较（使用 `Object.is()`）来比较原始值。
* 对象的类型标签应该相同
* 使用严格相等比较来比较对象的原型
* 只考虑可枚举的自身属性
* 始终比较 `Error` 的名称和消息，即使这些不是可枚举的属性
* 可枚举的自身 `Symbol` 属性也会比较
* 对象封装器作为对象和解封装后的值都进行比较
* `Object` 属性的比较是无序的
* `Map` 键名与 `Set` 子项的比较是无序的
* 当两边的值不相同或遇到循环引用时，递归停止
* `WeakMap` 和 `WeakSet` 的比较不依赖于它们的值

<a name="c6d9a356"></a>
### assert.doesNotReject(asyncFn, ?error, ?message)
等待 `asyncFn` Promise，或者，如果 `asyncFn` 是一个函数，则立即调用该函数并等待返回的 Promise 完成。 然后它将检查 Promise 是否被拒绝，如果Promise被reject则抛出异常
```javascript
const promise = new Promise(function(resolve, reject) {
    setTimeout(function(){
        reject('失败，Promise-rejected');
    }, 2000);
});
assert.doesNotReject(promise, SyntaxError);
```
![20190227115124.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1551239500212-e65fe1fd-0e22-46f5-b11c-c1527b8e52ca.png#align=left&display=inline&height=174&linkTarget=_blank&name=20190227115124.png&originHeight=304&originWidth=1300&size=564953&status=done&width=746)

<a name="0b080e70"></a>
### assert.doesNotThrow(fn[, error][, message])
断言 `fn` 函数不会抛出错误<br />当 assert.doesNotThrow() 被调用时，它会立即调用 block 函数<br />如果抛出错误且错误类型与 error 参数指定的相同，则抛出 AssertionError。 如果错误类型不相同，或 error 参数为 undefined，则抛出错误<br />error 可以是 Class、RegExp 或校验函数

<a name="663b0f52"></a>
### assert.fail(?message)
使用提供的错误消息或默认错误消息抛出 `AssertionError`。 如果 `message` 参数是 `Error` 的实例，则它将被抛出而不是 `AssertionError`
```javascript
const assert = require('assert');
assert.fail();
// AssertionError [ERR_ASSERTION]: Failed

assert.fail('失败');
// AssertionError [ERR_ASSERTION]: 失败

assert.fail(new TypeError('需要数组'));
// TypeError: 需要数组
```

<a name="0cfb49e8"></a>
### assert.ifError(value)
如果 `value` 不为 `undefined` 或 `null`，则抛出 `value`。 在回调中测试 `error` 参数时，这很有用。 堆栈跟踪包含传递给 `ifError()` 的错误的所有帧，包括 `ifError()` 本身的潜在新帧
```javascript
const assert = require('assert');

assert.ifError(null);
// 通过
assert.ifError(0);
// AssertionError [ERR_ASSERTION]: ifError got unwanted exception: 0
assert.ifError('错误');
// AssertionError [ERR_ASSERTION]: ifError got unwanted exception: '错误'
assert.ifError(new Error());
// AssertionError [ERR_ASSERTION]: ifError got unwanted exception: Error
```

<a name="b2ff685f"></a>
### assert.notDeepStrictEqual(actual, expected, ?message)
测试深度严格的不相等。 与 `assert.deepStrictEqual()` 相反
```javascript
const assert = require('assert');
assert.notDeepStrictEqual({ a: 1 }, { a: '1' });
// 通过
```
如果值深度且严格相等，则抛出 `AssertionError`，并将 `message` 属性设置为等于 `message` 参数的值。 如果未定义 `message` 参数，则会分配默认错误消息。 如果 `message` 参数是 [`Error`](http://nodejs.cn/s/FLTm19) 的实例，则它将被抛出而不是 `AssertionError`

<a name="38c21712"></a>
### assert.notStrictEqual(actual, expected, ?message)
测试 `actual` 参数和 `expected` 参数之间的严格不相等
```javascript
const assert = require('assert');

assert.notStrictEqual(1, 2);
// 通过

assert.notStrictEqual(1, 1);
// AssertionError [ERR_ASSERTION]: Identical input passed to notStrictEqual: 1

assert.notStrictEqual(1, '1');
// 通过
```
如果值严格相等，则抛出 `AssertionError`，并将 `message` 属性设置为等于 `message` 参数的值。 如果未定义 `message` 参数，则会分配默认错误消息。 如果 `message` 参数是 [`Error`](http://nodejs.cn/s/FLTm19) 的实例，则它将被抛出而不是 `AssertionError`

<a name="dec0ff04"></a>
### assert.rejects(asyncFn, ?error, ?message)
等待 `asyncFn` Promise，或者，如果 `asyncFn` 是一个函数，则立即调用该函数并等待返回的 Promise 完成。 然后它将检查 Promise 是否被拒绝<br />如果 `asyncFn` 是一个函数并且它同步抛出一个错误，则 `assert.rejects()` 将返回一个带有该错误的被拒绝的 `Promise`。 如果函数未返回 Promise，则 `assert.rejects()` 将返回一个被拒绝的 `Promise`，其中包含 `ERR_INVALID_RETURN_VALUE` 错误。 在这两种情况下都会跳过错误处理函数<br />除了等待的异步性质之外，完成行为与 `assert.throws()` 完全相同<br />如果指定，则 `error` 可以是 `Class`、`RegExp`、验证函数、将测试每个属性的对象、或者将测试每个属性的错误实例（包括不可枚举的 `message` 和 `name` 属性）<br />如果指定 `message`，则当 `asyncFn` 无法拒绝时 `message` 将是 `AssertionError` 提供的消息

<a name="3514a9c1"></a>
### assert.strictEqual(actual, expected, ?message)
测试 `actual` 参数和 `expected` 参数之间的严格相等性
```javascript
assert.strictEqual(1, 1);
// OK

assert.strictEqual(1, '1');
// AssertionError [ERR_ASSERTION]: Expected values to be strictly equal: 1 !== '1'
```
如果值不严格相等，则抛出 `AssertionError`，并将 `message` 属性设置为等于 `message` 参数的值。 如果未定义 `message` 参数，则会分配默认错误消息。 如果 `message` 参数是 `Error` 的实例，则它将被抛出而不是 `AssertionError`

<a name="af9357cd"></a>
### assert.throws(fn, ?error, ?message)
期望 `fn` 函数抛出错误<br />如果指定，则 `error` 可以是 `Class`、`RegExp`、验证函数，每个属性将被测试严格的深度相等的验证对象、或每个属性（包括不可枚举的 `message` 和 `name` 属性）将被测试严格的深度相等的错误实例。 使用对象时，还可以在对字符串属性进行验证时使用正则表达式<br />如果指定 `message`，则当 `fn` 调用无法抛出或错误验证失败时，`message` 将附加到 `AssertionError` 提供的消息