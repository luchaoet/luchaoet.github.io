---
title: jsonp的原理及Promise封装
date: 2019-03-22 10:37:35
tags: jsonp
summary:
---
<a name="2d14fedd"></a>
### jsonp原理
客户端利用`script`标签可以跨域请求资源的性质，向网页中动态插入一个JavaScript标签，其src由接口url、请求参数、callback函数名拼接而成，利用js标签没有跨域限制的特性实现跨域请求<br />服务端会解析请求的`url`,至少拿到一个回调函数(比如`callback=myCallback`)参数,之后将数据放入其中返回给客户端
<a name="f16908f7"></a>
### 注意点
callback函数要绑定在window对象上<br />不支持post，因为js标签本身就是一个get请求
<a name="6912abd3"></a>
### 封装
```typescript
const jsonp = function (url, data){
  return new Promise(function(resolve, reject) {
    var oScript = document.createElement('script');
    url += `?cb=callback`
    // 拼接data数据
    if(data){
      Object.keys(data).forEach(key=>{
        url += `&${key}=${data[key]}`
      })
    }
    oScript.src = url;
    document.body.appendChild(oScript);
  
    window['callback'] = function (cbData){
      if(data){
        resolve(cbData)
      }else{
        reject('没有数据')
      }
      delete window['callback'];
      document.body.removeChild(oScript);
    }
  });
}

jsonp('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su',{
  wd: '前端'
}).then(res => {
    console.log(res.s)
})
```
结果<br />![屏幕快照 2019-03-22 上午10.26.17.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1553221660672-5c958b1c-4945-4982-a045-69cdd67ba9d0.png#align=left&display=inline&height=211&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-22%20%E4%B8%8A%E5%8D%8810.26.17.png&originHeight=494&originWidth=1748&size=115727&status=done&width=746)