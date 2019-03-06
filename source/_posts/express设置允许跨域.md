---
title: express设置允许跨域
date: 2019-03-06 13:10:28
tags: ['express' ,'跨域', 'CORS', 'node']
summary: 
---
```javascript
var express = require('express');
var app = express();

//设置允许跨域访问该服务
app.all('*', function (req, res, next) {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  res.header('Access-Control-Allow-Methods', '*');
  res.header('Content-Type', 'application/json;charset=utf-8');
  next();
});

....

module.exports = app;
```