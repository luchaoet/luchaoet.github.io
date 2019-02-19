---
title: Vue—v-for属性
date: 2019-02-18 22:52:46
tags: ['vue', 'v-for']
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

`v-for`有一下几种形式
```
v-for="item in items"
v-for="(item, index) in items"
v-for="(value, key, index) in object"
```
value是属性值，key是属性名，index是索引值

### AST结构
经过静态内容处理后的p标签对应AST结构为<br />![屏幕快照 2019-02-19 13.06.45.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550552827363-4c2b5dec-4cdc-4295-b2cd-18b2a6317835.png#align=left&display=inline&height=411&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-19%2013.06.45.png&originHeight=436&originWidth=791&size=71832&width=746)

下面便是根据AST结果生成对应的render字符串
### genFor函数
```javascript
function genFor (
  el,
  state,
  altGen,
  altHelper
) {
  var exp = el.for;
  var alias = el.alias;
  var iterator1 = el.iterator1 ? ("," + (el.iterator1)) : '';
  var iterator2 = el.iterator2 ? ("," + (el.iterator2)) : '';

  if ("development" !== 'production' &&
    state.maybeComponent(el) &&
    el.tag !== 'slot' &&
    el.tag !== 'template' &&
    !el.key
  ) {
    state.warn(
      "<" + (el.tag) + " v-for=\"" + alias + " in " + exp + "\">: component lists rendered with " +
      "v-for should have explicit keys. " +
      "See https://vuejs.org/guide/list.html#key for more info.",
      true /* tip */
    );
  }

  el.forProcessed = true; // avoid recursion
  return (altHelper || '_l') + "((" + exp + ")," +
    "function(" + alias + iterator1 + iterator2 + "){" +
      "return " + ((altGen || genElement)(el, state)) +
    '})'
}
```

该函数返回了一个`_l`函数的字符串
```
_l((arr),function(item,index){return _c('p',[_v(_s(item.name)+"-"+_s(index))])})
```
`_c`是创建一个 vnode 对象<br />`_v`是创建一个 vnode文本节点<br />`_l`函数就是对应的`renderList`函数

### renderList函数
```javascript
function renderList (
  val,
  render
) {
  var ret, i, l, keys, key;
  if (Array.isArray(val) || typeof val === 'string') {
    ret = new Array(val.length);
    for (i = 0, l = val.length; i < l; i++) {
      ret[i] = render(val[i], i);
    }
  } else if (typeof val === 'number') {
    ret = new Array(val);
    for (i = 0; i < val; i++) {
      ret[i] = render(i + 1, i);
    }
  } else if (isObject(val)) {
    keys = Object.keys(val);
    ret = new Array(keys.length);
    for (i = 0, l = keys.length; i < l; i++) {
      key = keys[i];
      ret[i] = render(val[key], key, i);
    }
  }
  if (isDef(ret)) {
    (ret)._isVList = true;
  }
  return ret
}
```

返回的数据<br />![屏幕快照 2019-02-19 13.20.07.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550553632933-a3c03fc5-7a2a-4e94-a549-21bc3438ac05.png#align=left&display=inline&height=99&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-19%2013.20.07.png&originHeight=99&originWidth=652&size=32093&width=652)

我们这里传入的`val`就是`arr` ， render 就是生成 p 段落的 render 函数<br />代码中的三段 if 判断是因为我们`v-for`可以遍历的不止数组和对象，还有数组和字符串<br />最终返回的 ret 是一个 VNode 数组 每一个元素都是一个 p 便签对应的 VNode