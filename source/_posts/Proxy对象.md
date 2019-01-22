---
title: Proxy对象
date: 2018-12-23 00:06:59
tags: ['es6', 'Proxy']
summary:
---
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Proxy(代理) 是 ES6 中新增的一个特性。Proxy 让我们能够以简洁易懂的方式控制外部对对象的访问，可以对外界的访问进行拦截和改写</span></span>

```javascript
var proxy = new Proxy(target, handler)
```

<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">target 表示所要拦截的目标</span></span>
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">handler 也是一个对象，其声明了代理 target 的一些操作</span></span>

hander常用拦截操作
### get(target, propKey, ?receiver)
拦截读取属性的操作
```javascript
var target = {
  name: 'lucy'
};

var proxy = new Proxy(target, {
  get: function(target, key) {
    console.log(`读取${key}`);
    return target[key];
  }
});

proxy.name; // 读取name
```

### set(target, propKey, value, ?receiver)
拦截属性的赋值操作
```javascript
var target = {
  name: 'lucy'
};

var proxy = new Proxy(target, {
  set: function(target, key, value) {
    console.log(`${key}被设置为${value}`);
    return target[key] = value;
  }
});

proxy.name = 'YUMI'; // name 被设置为 others
console.log(target); // {name: "YUMI"}
```

### has(target, propKey)
has方法可以隐藏某些属性，不被in操作符发现
```javascript
var target = {
  name: 'lucy',
  _age: 20
};

var proxy = new Proxy(target, {
  has: function(target, key){
    // 属性带下划线_,直接则返回false，实现了私有属性
    if(key[0] === '_')return false;
    return key in target
  }
});
console.log('_age' in proxy) // false
```
