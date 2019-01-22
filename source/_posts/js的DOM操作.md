---
title: js的DOM操作
date: 2018-11-23 23:38:01
tags: ['js', 'DOM']
summary:
---
### DOM
<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">DOM（Document Object Model ，文档对象模型）一种独立于语言，用于操作xml，html文档的应用编程接口</span></span>

在 HTML DOM (Document Object Model) 中 , 每一个元素都是节点，dom使得html形成一棵dom树，类似于一颗家族树一样，一层接一层，子子孙孙
* 所有的HTML元素都是元素节点
* 所有 HTML 属性都是属性节点
* 文本插入到 HTML 元素是文本节点
* 注释是注释节点

对于JavaScript，为了能够使JavaScript操作Html，JavaScript就有了一套自己的dom编程接口

### 常用方法
#### 1.获取节点
```javascript
// 通过class属性获取元素，返回元素对象数组
document.getElementsByClassName(className)

// 通过id获取元素，返回一个元素对象
document.getElementById(id)

// 通过name属性获取元素，返回元素对象数组
document.getElementsByName(name)

// 通过html标签获取元素，返回元素对象数组
document.getElementsByTagName(tagName)
```

#### 2.获取/设置节点属性值
```javascript
// 根据属性名，获取属性值
ele.getAttribute('class')

// 设置属性值
ele.setAttribute('class','className')
```

#### 3.创建节点
```javascript
// 创建元素节点
document.createElement('div')

// 创建文本节点
document.createTextNode('abcd')

// 创建属性节点
document.createAttribute('class')
```

#### 4.增加节点
```javascript
// 向ele内部末尾插入Node节点
ele.appendChild(Node)

// 在ele内部的中在existingNode前面插入newNode
ele.insertBefore(newNode,existingNode); 
```

#### 5.删除节点
```javascript
// 删除ele的子节点Node
ele.removeChild(Node)
```

### 常用属性
#### 1.获取父节点
```javascript
// 返回当前父节点对象
ele.parentNode
```

#### 2.获取子节点
```javascript
// 获取所有子元素节点对象，只返回HTML节点
ele.children

// 获取所有子节点，包括文本，HTML,属性节点（回车也会当做一个节点）
ele.childNodes

// 返回第一个子节点对象
ele.firstChild

// 返回最后一个子节点对象
ele.lastChild
```

#### 3.获取同级元素
```javascript
// 获取元素的上一个同级元素，没有则返回null
ele.nextSibling

// 返回元素的上一个同级元素，没有则返回null
element.previousSibling  
```

#### 4.获取/设置元素的文本
```javascript
// 设置元素内部HTML代码，或者获取HTML代码字符串
ele.innerHTML

// 获取元素自身及子元素的所有文本值
ele.innerText
```

#### 5.获取节点类型
```javascript
// 数字形式（1-12）常见几个 1：元素节点，2：属性节点，3：文本节点
node.nodeType
```

#### 6.设置css样式
```javascript
// 设置元素的style
ele.style.color = "#333"
```