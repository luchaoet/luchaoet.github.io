---
title: css的伪类与伪元素
date: 2018-10-19 17:49:05
tags: css
summary: 总结css伪类与伪元素
---
| 伪类 | 伪类通过冒号来定义，它定义了元素的状态，如点击按下，点击完成等，通过伪类可以为元素的状态修改样式 | :link |  |  |
| :---: | --- | --- | --- | --- |
|  |  | :visited |  |  |
|  |  | :hover |  |  |
|  |  | :active |  |  |
|  |  | :focus |  |  |
|  |  | :not(S) |  |  |
|  |  | :first-child |  |  |
|  |  | :last-child |  |  |
|  |  | :only-child |  |  |
|  |  | :empty |  |  |
|  |  | :checked |  |  |
|  |  | :nth-child(n) |  |  |
| 伪元素<br /> |  | :first-letter | 向文本的第一个字母添加特殊样式 | css1 |
|  |  | :first-line | 向文本的首行添加特殊样式 | css1 |
|  |  | :after | 在元素之后添加内容 | css2 |
|  |  | :before | 在元素之前添加内容 | css2 |
|  |  | ::selection<br />(仅支持双冒号) | 改变用户所选取部分的样式 | css3 |