---
title: 利用localStorage存储数据(可设置有效期)
date: 2018-10-17 17:11:03
tags: ['js', 'localStorage']
summary: 设置有限期，合理管理localStorage存储的数据，数据没有加密，谨慎使用
---
在HTML5中，新加入了一个localStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k)，localStorage中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。

### localStorage的优势

1、localStorage拓展了cookie的4K限制
2、localStorage会可以将第一次请求的数据直接存储到本地，这个相当于一个5M大小的针对于前端页面的数据库，相比于cookie可以节约带宽，但是这个却是只有在高版本的浏览器中才支持的

### localStorage的局限

1、浏览器的大小不统一，并且在IE8以上的IE版本才支持localStorage这个属性
2、目前所有的浏览器中都会把localStorage的值类型限定为string类型，这个在对我们日常比较常见的JSON对象类型需要一些转换
3、localStorage在浏览器的隐私模式下面是不可读取的
4、localStorage本质上是对字符串的读取，如果存储内容多的话会消耗内存空间，会导致页面变卡
5、localStorage不能被爬虫抓取到

localStorage与sessionStorage的唯一一点区别就是localStorage属于永久性存储，而sessionStorage属于当会话结束的时候，sessionStorage中的键值对会被清空。

```javascript
/**
 * 使用localStorage完成对缓存管理
 * 通过cache.get(key)获取key对应的缓存数据 没有则返回null 或自定义返回值defaultValue
 * 通过cache.set(key, value, time)设置缓存的key, value, 和有效时间（s）（-1表示永久有效）
 * 通过cache.clear(key) 清空key对应的缓存
 */
const cache = {
	/*
		数据结构示例
		expire: {
			"name":{   							// key
				"value":lucy,					// value
				"time":1509948640834  // 过期时间
			},
			"name2":{
				"value":lucy2,
				"time":1509948701341
			}
		}
	*/

	// localStorage的名称
	EXPIRE_KEY: 'CACHE_EXPIRE',
	// localStorage的值（object形式）最终要转为字符串存储
	expire: {},

	// 获取localStorage
	get (key, defaultValue = null) {
		this.initExpire();
		// 已经被删除
		if(!this.expire[key]){
			return null; 
		}
		// 已经过期
		if (this.expire[key].time !== -1 && this.expire[key].time < new Date().getTime()) {
			this.clear(key)
			return null
		}
		// 存在 返回数据
		return this.expire[key].value || defaultValue
	},

	// 设置localStorage
  set (key, value, expire = 60) {
    this.initExpire()
    this.expire[key] = {
    	value:value,
    	time:expire === -1 ? expire : new Date().getTime() + expire * 1000
    }
    this.setExpire()
  },

  // 删除 localStorage => this.expire => 删除key => localStorage
  clear (key) {
    this.initExpire()
    delete this.expire[key]
    this.setExpire()
  },

	// 初始化expire localStorage => this.expire
	initExpire () {
		let expire = localStorage.getItem(this.EXPIRE_KEY) || null
		this.expire = expire !== null ? JSON.parse(expire) : {}
	},

	// 存储localStorage this.expire => localStorage
	setExpire () {
		localStorage.setItem(this.EXPIRE_KEY, JSON.stringify(this.expire))
	}
 }

 export default cache;
```