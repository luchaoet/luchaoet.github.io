---
title: flex布局
date: 2018-10-19 16:43:11
tags: ["flex", "css3"]
summary: flex弹性布局
---

Flexible Box 弹性布局
<span data-type="color" style="color:#F5222D"><strong>display: flex／inline-flex;</strong></span>

注意，设为 Flex 布局以后，子元素的<span data-type="background" style="background-color:#FFFB8F">float</span>、<span data-type="background" style="background-color:#FFFB8F">clear</span>和<span data-type="background" style="background-color:#FFFB8F">vertical-align</span>属性将失效。

---

## 1.容器的属性
* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content

### 1.1 flex-direction: <span data-type="background" style="background-color:#FFFB8F">row</span> | <span data-type="background" style="background-color:#FFFB8F">row-reverse</span> | <span data-type="background" style="background-color:#FFFB8F">column</span> | <span data-type="background" style="background-color:#FFFB8F">column-reverse</span>; 主轴的方向，即项目的排列方向

| row(默认) 主轴为水平方向，起点在左端 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536820139098-749c149e-8503-4772-9043-ac0c3cae2932.png" > |
| --- | --- |
| row-reverse 主轴为水平方向，起点在右端 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536820304582-66c87a00-b7b1-46e9-bca4-6f9cb72b4bd4.png" /> |
| coumn 主轴为垂直方向，起点在上沿 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536820375125-674aa130-f8c5-469a-bb0c-6198ed5a1ebc.png" /> |
| column-reverse 主轴为垂直方向，起点在下沿 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536820410702-2c91e9bb-f8a5-4688-af66-8b0c7dae01da.png" /> |

### 1.2 flex-wrap: <span data-type="background" style="background-color:#FFFB8F">nowrap</span> | <span data-type="background" style="background-color:#FFFB8F">wrap</span> | <span data-type="background" style="background-color:#FFFB8F">wrap-reverse</span>; 换行

| nowrap(默认) 不换行 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536820919453-eaa96d42-e988-4932-ac3d-d0ea807d726e.png" /> |
| --- | --- |
| wrap 换行 第一行在上方 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536821160803-4167dffe-0a14-447a-9103-e147a9011546.png" /> |
| wrap-reverse 换行 第一行在下方 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536821128497-30ed6de2-43b6-4196-b02b-1affd545364c.png" /> |

### 1.3 flex-flow
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
flex-flow: <flex-direction> || <flex-wrap>;
### 1.4 justify-content: <span data-type="background" style="background-color:#FFFB8F">flex-start</span> | <span data-type="background" style="background-color:#FFFB8F">flex-end</span> | <span data-type="background" style="background-color:#FFFB8F">center</span> | <span data-type="background" style="background-color:#FFFB8F">space-between</span> | <span data-type="background" style="background-color:#FFFB8F">space-around</span>;


| flex-start | 默认值 左对齐 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536822576088-293bcbcb-ebc7-4b8f-ac2c-15311769f4f1.png"/> |
| --- | --- | --- |
| flex-end | 右对齐 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536822501567-b0c69e00-85ef-4ea8-82e2-9d715d5a2098.png"/> |
| center | 居中 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536822609702-043a708a-2220-4e65-ad01-1afed0c8c0b9.png"/> |
| space-between | 两端对齐，项目之间的间隔都相等 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536822645773-41136a16-69fb-4ee1-abe0-d3fa8d8486fa.png"/> |
| space-around | 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536822679830-1fe3e151-0863-4175-9a82-4e1ef4b7e81a.png"/> |

### 1.5 align-items: <span data-type="background" style="background-color:#FFFB8F">flex-start</span> | <span data-type="background" style="background-color:#FFFB8F">flex-end</span> | <span data-type="background" style="background-color:#FFFB8F">center</span> | <span data-type="background" style="background-color:#FFFB8F">baseline</span> | <span data-type="background" style="background-color:#FFFB8F">stretch</span>;


| flex-start | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536823353807-0dcfaa51-3509-42c4-93df-3584dfcb3311.png" /> |
| --- | --- |
| flex-end | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536823391774-a58849a7-fe1b-4a26-9784-9d6d4b913af6.png" /> |
| center | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536823413352-12f034ed-ed0c-4b34-9cf2-ae47660ee1b6.png"/> |
| baseline | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536823546804-e752afe1-518e-4760-a497-a730d3af9e31.png" /> |
| stretch | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536823629323-845ef374-2c90-417f-93ae-5f39a6220441.png" /> |

### 1.6 align-content: <span data-type="background" style="background-color:#FFFB8F">flex-start </span>| <span data-type="background" style="background-color:#FFFB8F">flex-end</span> | <span data-type="background" style="background-color:#FFFB8F">center</span> | <span data-type="background" style="background-color:#FFFB8F">space-between</span> | <span data-type="background" style="background-color:#FFFB8F">space-around</span> | <span data-type="background" style="background-color:#FFFB8F">stretch</span>; 容器下所有项目的堆叠方式

| flex-start | 从上到下堆叠 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536824290734-c5bb2cbd-1476-4f6d-9db9-454090f54147.png" /> |
| --- | --- | --- |
| flex-end | 从下到上堆叠 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536824504217-65a13958-ab3e-4059-88dd-59bff9e8718c.png"/> |
| center | 堆叠在中间 | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536824584914-ada15f66-b973-482f-9d81-94a220b8ed1f.png"/> |
| space-between |  | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536824817313-533c7637-9b12-48ff-8405-acd15e949970.png"/> |
| space-around |  | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536824864230-c1289f7c-9fcd-4c34-add7-d855d4edd603.png"/> |
| stretch |  | <img src="https://cdn.nlark.com/yuque/0/2018/png/115449/1536824925218-8bc522f9-5ebc-4f5a-897c-aee1a551d143.png" /> |


---

## 2.项目的属性
* order
* flex-grow
* flex-shrink
* flex-basis
* flex
* align-self
### 2.1 order 
项目的排列顺序。数值越小，排列越靠前，默认为0
### 2.2 flex-grow
项目的占据比例


![image.png | left | 400x59.130434782608695](https://cdn.nlark.com/yuque/0/2018/png/115449/1536825508342-f317f43c-d6ae-4dfa-8a20-66be455b1f3a.png "")

### 2.3 flex-shrink
### 2.4 flex-basis
项目的占据宽度 flex-basis:300px;
### 2.5 flex
flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
### 2.6 align-self
align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。


![image.png | left | 400x137.69063180827888](https://cdn.nlark.com/yuque/0/2018/png/115449/1536826020004-f01a09f4-1b58-4194-aa1c-3acf9fee1ee9.png "")

该属性可能取6个值，除了auto，其他都与align-items属性完全一致
