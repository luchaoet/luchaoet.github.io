---
title: Vue—v-if属性
date: 2019-02-17 19:09:14
tags: ['vue', 'v-if']
summary:
---
### v-if 示例
```html
<div id='app'>
  <p v-if='value === 1'>v-if</p>
  <p v-else-if='value === 2'>v-else-if</p>
  <p v-else-if='value === 3'>v-else-if</p>
  <p v-else>v-else</p>
</div>
<script type="text/javascript">
  var vm = new Vue({
    el: '#app',
    data: {
      value: 4
    }
  })
</script>
```

与其他指令相同，在`parse`函数中处理`v-if`指令，parse函数的作用是将HTML字符串转换为AST

### 什么是AST
Vue源码中虚拟DOM构建经历 template编译成AST语法树 -> 再转换为render函数，最终返回一个VNode(VNode就是Vue的虚拟DOM节点) <br />在Vue的mount过程中，template会被编译成AST语法树，AST是指抽象语法树（abstract syntax tree或者缩写为AST），或者语法树（syntax tree），是源代码的抽象语法结构的树状表现形式<br /><br /><br />比如，这是`<div id='app'>`的AST语法树<br />![屏幕快照 2019-02-15 17.13.46.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550222090191-779b993c-5e15-4cff-abfa-a42b79b56ecc.png#align=left&display=inline&height=164&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-15%2017.13.46.png&originHeight=164&originWidth=185&size=15515&width=185)<br />包含属性列表，子节点，父节点，标签名称，节点类型及其他信息

### parse函数
```javascript
 function parse(template) {
   var currentParent;    //当前父节点
   var root;            //最终返回最终的AST树根节点
   var stack = [];
   parseHTML(template, {
     start: function start(tag, attrs, unary) {
     	......
     	if (inVPre) {
        processRawAttrs(element);
      } else if (!element.processed) {
        // structural directives
        processFor(element);
        processIf(element);
        processOnce(element);
        // element-scope stuff
        processElement(element, options);
      }
      ......
     },
     end: function end() {
     ......
     },
     chars: function chars(text) {
     ......
     }
   })
   return root
}
```

再看`processIf`函数
### processIf函数
```javascript
function processIf (el) {
  var exp = getAndRemoveAttr(el, 'v-if');
  if (exp) {
    console.log(exp) // "value === 1"
    el.if = exp;
    addIfCondition(el, {
      exp: exp,
      block: el
    });
  } else {
    if (getAndRemoveAttr(el, 'v-else') != null) {
      console.log(getAndRemoveAttr(el, 'v-else')) // ""
      el.else = true;
    }
    var elseif = getAndRemoveAttr(el, 'v-else-if'); 
    if (elseif) {
      console.log(elseif) // "value === 2/3"
      el.elseif = elseif;
    }
  }
}
```

该函数的作用是<br />如果节点包含`v-if`属性
```javascript
var exp = getAndRemoveAttr(el, 'v-if'); // value === 1
if (exp) {
  console.log(el)
  el.if = exp;
  addIfCondition(el, {
    exp: exp,
    block: el
  });
}
```
在AST中添加一个`if`属性<br />![20190215174004.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550223668598-cbcf3c45-803c-408f-9691-5e073498734c.png#align=left&display=inline&height=215&linkTarget=_blank&name=20190215174004.png&originHeight=215&originWidth=446&size=30530&width=446)<br />addIfCondition函数会在AST中该el添加一个`ifConditions`属性来保存当前v-if相关元素

如果节点包含`v-else-if`属性，则在AST中添加一个`elseif`属性
```javascript
var elseif = getAndRemoveAttr(el, 'v-else-if');
if (elseif) {
	el.elseif = elseif;
}
```
![20190215174027.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550224120956-ef022d92-8287-489e-9f55-8d50526d9498.png#align=left&display=inline&height=178&linkTarget=_blank&name=20190215174027.png&originHeight=178&originWidth=456&size=25783&width=456)

接着走如下判断
```javascript
if (currentParent && !element.forbidden) {
  if (element.elseif || element.else) {
  	processIfConditions(element, currentParent);
  } else {
    if (element.slotScope) {
      // scoped slot
      // keep it in the children list so that v-else(-if) conditions can
      // find it as the prev node.
      var name = element.slotTarget || '"default"'
      ;(currentParent.scopedSlots || (currentParent.scopedSlots = {}))[name] = element;
    }
    currentParent.children.push(element);
    element.parent = currentParent;
  }
}
```
从其中的判断可知，父节点的`children`中会有`v-if`的标签

如果节点包含`v-else`属性，则在AST中添加一个`else: true`属性<br />![20190215174050.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550224168597-b045e85f-2687-43c6-9eac-4dea90438325.png#align=left&display=inline&height=179&linkTarget=_blank&name=20190215174050.png&originHeight=179&originWidth=473&size=24673&width=473)

接着也是同上操作

看下`processIfConditions`函数
```javascript
function processIfConditions (el, parent) {
  var prev = findPrevElement(parent.children);
  if (prev && prev.if) {
    addIfCondition(prev, {
      exp: el.elseif,
      block: el
    });
  } else {
    warn$2(
      "v-" + (el.elseif ? ('else-if="' + el.elseif + '"') : 'else') + " " +
      "used on element <" + (el.tag) + "> without corresponding v-if.",
      el.rawAttrsMap[el.elseif ? 'v-else-if' : 'v-else']
      );
	}
}

function findPrevElement (children) {
  var i = children.length;
  while (i--) {
    if (children[i].type === 1) {
    	return children[i]
    } else {
      if (children[i].text !== ' ') {
        warn$2(
        "text \"" + (children[i].text.trim()) + "\" between v-if and v-else(-if) " +
        "will be ignored.",
        children[i]
        );
      }
    	children.pop();
    }
  }
}
```
`findPrevElement`函数会先拿到el元素的前一个兄弟节点，然后从后往前寻找第一个节点，如果中间存在文本节点，会被删除并报错<br />如果`prev`元素存在且`prev.if`存在，则把当前节点和条件添加到`pre`的`ifConditions`中