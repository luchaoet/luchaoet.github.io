---
title: js中!和!!
date: 2018-10-29 14:17:51
tags: ['js', '!', '!!']
summary: 
---
### !（取反）
可将变量转换成boolean类型，null、undefined、空字符串、0 、NaN 取反都为 true，其余都为false
```javascript
!null // true
!undefined // true
!"" // true
!0 // true
!NaN // true
!false // true
```

### !! （两次取反）
常常用来做类型判断，强制转化为boolean类型
```javascript
if(a!==null && a!==undefined && a!=="" && a !== NaN){
    // code
}

// 直接使用!!操作上面臃肿的操作
if(!!a){
    // code
}
```

这种操作在组件传值的过程中非常有用，有时没有设置初始值而需要去判断值是否存在的时候，避免了值的异常导致的js报错
