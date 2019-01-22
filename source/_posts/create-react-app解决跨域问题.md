---
title: create-react-app解决跨域问题
date: 2018-10-30 09:51:47
tags: ['react', 'create-react-app', '跨域']
summary: 一步搞定，就是这么快
---
在`package.json`文件中配置proxy即可

例如：
```json
"proxy": "http://pc.b2b.xxxxx.com"
```

![屏幕快照 2018-10-29 下午5.12.47.png | center | 747x254](https://cdn.nlark.com/yuque/0/2018/png/115449/1540804428642-7420457a-dbb7-4ade-b4b9-f6401d6c9239.png "")

```javascript
// http://pc.bcb/xxx/api.php?s=api/xxxx/searchcategory
fetch('/api.php?s=api/xxxx/searchcategory')
.then(response => response.json())  
.then(res => {  
    console.log(res)
})  
.catch((error) => {  
    console.log("error");
}); 
```