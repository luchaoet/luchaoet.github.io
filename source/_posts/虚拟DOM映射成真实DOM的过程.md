---
title: 虚拟DOM映射成真实DOM的过程
date: 2018-10-31 16:59:16
tags: ['virtual dom']
summary:
---
我们要实现如下真实DOM的渲染过程
```html
<div id="vdom">
    <div>vdom-div</div>
    <img src="https://avatars1.githubusercontent.com/u/22017372">
    <ul>
        <li class="item">li-01</li>
        <li class="item">li-02</li>
        <li class="item">li-03</li>
    </ul>
</div>
```

## 实现原理：
### 用JS对象模拟DOM树
```javascript
var element = {
  tagName: 'div',
  props: {
    id: 'vdom'
  },
  children: [
    {
        tagName: 'div', 
        props: {},
        children: ['vdom-div']
    },
    {
        tagName: 'img', 
        props: {src: 'https://avatars1.githubusercontent.com/u/22017372'}, 
        children: []
    },
    {
        tagName: 'ul', 
        props: {}, 
        children: [
            {tagName: 'li',props: {class:"item"},children: ['li-01']},
            {tagName: 'li',props: {class:"item"},children: ['li-02']},
            {tagName: 'li',props: {class:"item"},children: ['li-03']}
        ]
    },
  ]
}
```

<span data-type="color" style="color:rgb(36, 41, 46)"><span data-type="background" style="background-color:rgb(255, 255, 255)">用 JavaScript 来表示一个 DOM 节点,只需要记录它的节点类型、属性，还有子节点</span></span>
```javascript
function Element(tagName, props, children) {
    if (!(this instanceof Element)) {
        return new Element(tagName, props, children);
    }

    this.tagName = tagName;
    this.props = props || {};
    this.children = children || [];
    this.key = props ? props.key : undefined;

    let count = 0;
    this.children.forEach((child) => {
        if (child instanceof Element) {
            count += child.count;
        }
        count++;
    });
    this.count = count;
}
```

于是DOM结构便可以这样表示
```javascript
var tree = Element('div', { id: 'vdom' }, [
    Element('div', {}, ['vdom-div']),
    Element('img', {src:'https://avatars1.githubusercontent.com/u/22017372'}, ['']),
    Element('ul', {}, [
        Element('li', { class: 'item' }, ['li-01']),
        Element('li', { class: 'item' }, ['li-02']),
        Element('li', { class: 'item' }, ['li-03']),
    ]),
]);
```

`render`函数
```javascript
Element.prototype.render = function() {
    // 创建标签
    const el = document.createElement(this.tagName);
    const props = this.props;
    // 设置属性
    for (const propName in props) {
        setAttr(el, propName, props[propName]);
    }
    // 处理子节点
    this.children.forEach((child) => {
        /*
            子节点是元素 继续render
            子节点是文本 直接创建
        */
        const childEl = (child instanceof Element) ? child.render() : document.createTextNode(child);
        // 子节点插入至父节点
        el.appendChild(childEl);
    });
    return el;
};
```

`setAttr`函数，调用原生setAttribute方法设置标签的属性
```javascript
function setAttr(node, key, value){
    switch (key) {
        case 'style':
            node.style.cssText = value;
            break;
        case 'value': {
            const tagName = node.tagName.toLowerCase() || '';
            if (tagName === 'input' || tagName === 'textarea') {
                node.value = value;
            } else {
                node.setAttribute(key, value);
            }
            break;
        }
        default:
            node.setAttribute(key, value);
            break;
    }
};
```

最后一步 挂载到一个真实DOM节点上
```javascript
var root = tree.render();
document.getElementById('demo').appendChild(root);
```

完毕

[代码地址](https://github.com/Lucy20209060/sourceCode-React/tree/master/virtualDOM/demo)
