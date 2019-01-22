---
title: fetch
date: 2018-12-18 23:46:19
tags: fetch
summary:
---
### XMLHttpRequest
```javascript
// Just getting XHR is a mess!
if (window.XMLHttpRequest) { // Mozilla, Safari, ...
  request = new XMLHttpRequest();
} else if (window.ActiveXObject) { // IE
  try {
    request = new ActiveXObject('Msxml2.XMLHTTP');
  } 
  catch (e) {
    try {
      request = new ActiveXObject('Microsoft.XMLHTTP');
    } 
    catch (e) {}
  }
}

// Open, send.
request.open('GET', 'https://davidwalsh.name/ajax-endpoint', true);
request.send(null);
```
<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">XMLHttpRequest是一个浏览器接口，使得Javascript可以进行HTTP(S)通信，这就是我们熟悉的ajax</span></span>
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">实际开发中我们使用的是各种框架封装了的</span></span>`XMLHttpRequest`<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">对象，而不必关心具体实现</span></span>

### fetch基本用法
```javascript
// url (required), options (optional)
fetch('https://davidwalsh.name/some/url', {
	method: 'get'
}).then(function(response) {
	
}).catch(function(err) {
	// Error :(
});
```

与<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Battery API</span></span>类似, fetch API 也使用了 Promises<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)"> </span></span>来处理结果/回调
```javascript
// Simple response handling
fetch('https://davidwalsh.name/some/url').then(function(response) {
	
}).catch(function(err) {
	// Error :(
});

// 链式写法
fetch('https://davidwalsh.name/some/url').then(function(response) {
	return //...
}).then(function(returnedValue) {
	// ...
}).catch(function(err) {
	// Error :(
});
```

### Request Headers
在请求灵活性方面，设置请求头的能力非常重要。通过执行`new Headers()`操作处理请求头
```javascript
// 创建一个空的Headers对象
var headers = new Headers();

// 添加信息
headers.append('Content-Type', 'text/plain');
headers.append('X-My-Custom-Header', 'CustomValue');

// 判断(has), 获取(get), 以及设置(set)请求头的值
headers.has('Content-Type'); // true
headers.get('Content-Type'); // "text/plain"
headers.set('Content-Type', 'application/json');

// 删除信息
headers.delete('X-My-Custom-Header');

// 设置初始值
var headers = new Headers({
	'Content-Type': 'text/plain',
	'X-My-Custom-Header': 'CustomValue'
});
```

创建一个 `Request` 对象来包装请求头
```javascript
var request = new Request('https://davidwalsh.name/some-url', {
    headers: new Headers({
        'Content-Type': 'text/plain'
    })
});

fetch(request).then(function() { /* handle response */ });
```

### Request
Request 对象表示一次 fetch 调用的请求信息。传入 Request 参数来调用 fetch, 可以执行很多自定义请求的高级用法
* `method` - 支持 `GET`, `POST`, `PUT`, `DELETE`, `HEAD`
* `url` - 请求的 URL
* `headers` - 对应的 `Headers` 对象
* `referrer` - 请求的 referrer 信息
* `mode` - 可以设置 `cors`, `no-cors`, `same-origin`
* `credentials` - 设置 cookies 是否随请求一起发送。可以设置: `omit`, `same-origin`
* `redirect` - `follow`, `error`, `manual`
* `integrity` - subresource 完整性值(integrity value)
* `cache` - 设置 cache 模式 (`default`, `reload`, `no-cache`)

示例
```javascript
var request = new Request('https://davidwalsh.name/users.json', {
    method: 'POST', 
    mode: 'cors', 
    redirect: 'follow',
    headers: new Headers({
        'Content-Type': 'text/plain'
    })
});

// 使用
fetch(request).then(function() { /* handle response */ });
```
第一个参数 URL 是必需的。在 `Request` 对象创建完成之后, 所有的属性都变为只读属性. 请注意, `Request` 有一个很重要的 `clone`方法, 需要注意的是在 Service Worker API 中使用时，一个 Request 就代表一串流, 如果想要传递给另一个 `fetch` 方法,则需要进行克隆。

`fetch` 的方法签名(signature,可理解为配置参数), 和 `Request` 很像, 示例如下
```javascript
fetch('https://davidwalsh.name/users.json', {
	method: 'POST', 
	mode: 'cors', 
	redirect: 'follow',
	headers: new Headers({
		'Content-Type': 'text/plain'
	})
}).then(function() { /* handle response */ });
```

### Response
该`fetch`的`then`方法提供了一个`Response`实例，但是你也可以手动创建`Response`的对象
* `type`- `basic`，`cors`
* `url`
* `useFinalURL`- 布尔值，表示`url`是否是最终的URL
* `status`-状态代码（例如：`200`，` 404`等）
* `ok` - 成功响应的布尔值（状态范围为200-299）
* `statusText`-状态代码（例如：`OK`）
* `headers` - 与响应关联的请求头
```javascript
// new Response(BODY, OPTIONS)
var response = new Response('.....', {
	ok: false,
	status: 404,
	url: '/'
});

// then方法传入Response 对象
fetch('https://davidwalsh.name/')
.then(function(responseObj) {
	console.log('status: ', responseObj.status);
});
```

该`Response`还提供了以下方法
* `clone()` - 创建Response对象的克隆
* `error()` - 返回与网络错误关联的新Response对象
* `redirect()` - 使用不同的URL创建新响应
* `arrayBuffer()` - 返回使用ArrayBuffer解析的promise
* `blob()` - 返回使用Blob解析的promise
* `formData()` - 返回使用FormData对象解析的promise
* `json()` - 返回使用JSON对象解析的promise
* `text()` - 返回使用USVString（text）解析的promise

#### 处理JSON
发出JSON请求 - 生成的回调数据response有一个`json()`方法，将原始数据转换为JavaScript对象的方法
```javascript
fetch('https://davidwalsh.name/demo/arsenal.json').then(function(response) { 
	// 转换为json
	return response.json();
}).then(function(res) {
	// `res` is a JavaScript object
	console.log(res); 
});
```

#### 处理基本的Text / HTML
```javascript
fetch('/next/page')
  .then(function(response) {
    return response.text();
  }).then(function(text) { 
  	// <!DOCTYPE ....
  	console.log(text); 
  });
```

#### 处理Blob
<span data-type="color" style="color:rgba(0, 0, 0, 0.8)"><span data-type="background" style="background-color:rgb(255, 255, 255)">如果想通过fetch加载图像，那将会有所不同</span></span>
```javascript
fetch('https://davidwalsh.name/flowers.jpg')
	.then(function(response) {
	  return response.blob();
	})
	.then(function(imageBlob) {
	  document.querySelector('img').src = URL.createObjectURL(imageBlob);
	});
```
`blob()`方法接受一个Response流并将其读取完成

#### 提交表单数据
ajax的另一个常见用例是发送表单数据，这是`fetch`用于发布表单数据的方式
```javascript
fetch('https://davidwalsh.name/submit', {
	method: 'post',
	body: new FormData(document.getElementById('comment-form'))
});
```

#### 提交json到服务器
```javascript
fetch('https://davidwalsh.name/submit-json', {
	method: 'post',
	body: JSON.stringify({
		email: document.getElementById('email').value,
		answer: document.getElementById('answer').value
	})
});
```

### ajax、axios、fetch的区别与优缺点
#### JQuery ajax
```javascript
$.ajax({
   type: 'POST',
   url: url,
   data: data,
   dataType: dataType,
   success: function () {},
   error: function () {}
});
```
对原生XHR的封装，除此以外还增添了对JSONP的支持。JQuery ajax经过多年的更新维护，真的已经是非常的方便了，优点无需多言；如果是硬要举出几个缺点，那可能只有

* 本身是针对MVC的编程,不符合现在前端MVVM的浪潮
* 基于原生的XHR开发，XHR本身的架构不清晰，已经有了fetch的替代方案
* JQuery整个项目太大，单纯使用ajax却要引入整个JQuery非常的不合理（采取个性化打包的方案又不能享受CDN服务

尽管JQuery对我们前端的开发工作曾有着（现在也仍然有着）深远的影响，但是我们可以看到随着vue，react新一代框架的兴起，以及ES规范的完善，更多API的更新，JQuery这种大而全的JS库，未来的路会越走越窄

#### axios
```javascript
axios({
    method: 'post',
    url: '/user/info',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
```
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">本质上也是对原生XHR的封装，只不过它是Promise的实现版本，符合最新的ES规范</span></span>

* 从 node.js 创建 http 请求
* 支持 Promise API
* 客户端支持防止CSRF
* __提供了一些并发请求的接口__（重要，方便了很多的操作）

#### <span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">fetch</span></span>
```javascript
try {
  let response = await fetch(url);
  let data = response.json();
  console.log(data);
} catch(e) {
  console.log("Oops, error", e);
}
```

符合关注分离，没有将输入、输出和用事件来跟踪的状态混杂在一个对象里
更好更方便的写法
更加底层，提供的API丰富（request, response）
脱离了XHR，是ES规范里新的实现方式
* fetch只对网络请求报错，对400，500都当做成功的请求，需要封装去处理
* fetch默认不会带cookie，需要添加配置项
* fetch不支持abort，不支持超时控制，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了量的浪费
* fetch没有办法原生监测请求的进度，而XHR可以