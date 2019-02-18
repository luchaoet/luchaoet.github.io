---
title: Vue—v-for属性
date: 2019-02-18 22:52:46
tags: ['vue', 'v-if']
summary:
---
### v-for 示例
```html
<body>
    <div id='app'>
        <p v-for='(item, index) in arr'>{{item.name}}-{{index}}</p>
    </div>
</body>
<script type="text/javascript">
    var vm = new Vue({
      el: '#app',
      data: {
        arr:[
            {id: 1, name: 'Lucy'},
            {id: 2, name: 'Jack'},
            {id: 3, name: 'Tom'}
        ]
      }
    })
  </script>
```
对于`v-for`指令，通过processFor方法来进行解析
### processFor函数
```javascript
function processFor (el) {
	var exp;
	if ((exp = getAndRemoveAttr(el, 'v-for'))) {
		var res = parseFor(exp);
		if (res) {
			extend(el, res);
		} else {
			warn$2(
				("Invalid v-for expression: " + exp),
				el.rawAttrsMap['v-for']
			);
		}
	}
}
```


```javascript
var forAliasRE = /([\s\S]*?)\s+(?:in|of)\s+([\s\S]*)/;
function parseFor (exp) {
	var inMatch = exp.match(forAliasRE);
	if (!inMatch) { return }
	var res = {};
	res.for = inMatch[2].trim();
	var alias = inMatch[1].trim().replace(stripParensRE, '');
	var iteratorMatch = alias.match(forIteratorRE);
	if (iteratorMatch) {
		res.alias = alias.replace(forIteratorRE, '').trim();
		res.iterator1 = iteratorMatch[1].trim();
		if (iteratorMatch[2]) {
			res.iterator2 = iteratorMatch[2].trim();
		}
	} else {
		res.alias = alias;
	}
	return res
}
```

从`forAliasRE`可知，`v-for`中使用`in`和`of`是一致的

此时`inMatch`的值为
```
[
	"(item, index) in arr", 
  "(item, index)", 
  "arr", 
  index: 0, 
  input: "(item, index) in arr", 
  groups: undefined
]
```