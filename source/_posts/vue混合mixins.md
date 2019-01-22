---
title: vue混合mixins
date: 2018-11-06 23:57:04
tags: ['vue', 'mixins']
summary: 
---
一种分发vue组件中可复用功能的非常灵活的方式，混合对象可以包含任意组件选项，组件使用混合对象时，所有混合对象的选项将被混入该组件本身的选项
```javascript
import miXins from './mixins';

export default {
    mixins:[miXins],
...
```
mixins.vue组件中的钩子函数会被调用，且mixins.vue组件中的methods中的方法可以直接在本组件中调用

当组件和混合对象含有同名选项时，这些选项将以恰当的方式混合，同名钩子函数将混为一个数组，因此都将会被调用
另外混合对象的钩子函数将在组件自身钩子函数之前调用

methods, components和directives将会混合合并，若存在键名冲突同名，组件中的键值对将会覆盖掉混合对象中的键值对