---
title: mac更新node版本
date: 2019-03-01 15:21:00
tags: ['node', 'mac']
summary:
---
<a name="75d5c53b"></a>
### 查看node版本
```
node -v
```
如果不是最新版，需要更新node版本
<a name="3f35e874"></a>
### 清除node缓存
```
sudo npm cache clean -f
```
<a name="4a195dd8"></a>
### 安装n工具
`n`是node版本管理工具
```
sudo npm install -g n
```
<a name="a99fad6b"></a>
### 安装最新版node
```
sudo n stable
```
<a name="17d77453"></a>
### 验证node版本
```
node -v
```
<a name="7bc106f8"></a>
### 更新npm版本
```
sudo npm install npm@latest -g
```
<a name="193f4810"></a>
### 验证npm版本
```
npm -v
```