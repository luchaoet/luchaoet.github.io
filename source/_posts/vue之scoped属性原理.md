---
title: vue之scoped属性原理
date: 2018-10-30 17:09:53
tags: ['vue', 'scoped']
summary: 添加“scoped”属性，将css作用域限制在该组件，避免了全局污染
---
添加“scoped”属性，将css作用域限制在该组件，避免了全局污染
```html
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
    /* css */
</style>
```

### 渲染前
html
```html
<div class="lu_pagination"></div>
```
css
```html
<style lang="scss" scoped>
    .lu_pagination{
        display: flex;
        align-items: center;
        user-select:none;
    }
</style>
```

### 渲染后
html
```html
<div data-v-0c4b1b06 class="lu_pagination"></div>
```
css
```css
.lu_pagination[data-v-0c4b1b06] {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
```

唯一的区别
就是在DOM结构以及css样式上加上唯一的标记，`data-v-0c4b1b06`的属性
利用css选择器的原理作用于html，由于id是唯一的，所以实现了组件样式的私有化

优缺点在此不论

scoped属性的效果其实主要是通过PostCSS转译实现
看看`vue-loader`是怎么处理的


![17_04_00__10_30_2018.jpg | center | 747x579](https://cdn.nlark.com/yuque/0/2018/jpeg/115449/1540890286881-90d86ab6-4022-4d9e-adcd-e76f24392b60.jpeg "")

通过postcss，给组件中的所有dom添加了一个独一无二的动态属性，其中的id是唯一的
然后，给CSS选择器额外添加一个对应的属性选择器来选择该组件中dom
