---
title: react高阶组件
date: 2018-12-26 21:53:06
tags:
summary:
---
### 定义
高阶组件是一个函数，这个函数的功能就是，接收一个组件作为参数，返回一个新的包装好的组件

### 从实例中理解高阶组件


![20181225202345.png | center | 562x417](https://cdn.nlark.com/yuque/0/2018/png/115449/1545740664646-88e7a742-3ab8-44d9-bd63-3b8e590d57ce.png "")


状态为publish时，操作按钮为 `应用下架`
状态为unpublish时，操作按钮为 `应用上架`
id为0，则为管理员，有权限操作，其他id无权限

代码实现
```javascript
import Button from 'antd';

<Table.Column title="id" dataIndex="admin" />
<Table.Column title="状态" dataIndex="status" />
<Table.Column 
	title="操作"
	cell={ (value, index, record) =>
		<Button 
			disabled={record.admin !== 0}
			shape={record.status === 'publish'?'warning':'ghost'}
		> {record.status === 'publish'?'应用下架':'应用上架'}</Button>
	} 
/>
```

按钮这块，过多的业务判断逻辑写在一起，显得非常的混乱，不优雅

### 使用高阶组件实现
```javascript
import Button from 'antd';
import operBtn from './compoments/OperBtn.jsx'
// 传入组件参数
const OperButton = operBtn(Button);

<Table.Column title="id" dataIndex="admin" />
<Table.Column title="状态" dataIndex="status" />
<Table.Column 
	title="操作"
	cell={ (value, index, record) =>
		<OperButton admin={record.admin} status={record.status} />
	} 
/>
```

我们把逻辑判断放到幕后去操作，前面尽量让HTML简洁

OperBtn.jsx
```javascript
import React, { Component } from 'react';

// 接收一个组件参数
export default function (Button) {
  // 然后封装返回另外一个组件
	return class OperBtn extends Component {
		render() {
			const {admin, status} = this.props;
			return (
				<Button 
					disabled={admin !== 0}
					shape={status === 'publish'?'warning':'ghost'}
				>{status === 'publish'?'应用下架':'应用上架'}</Button>
			);
		}
	}
}
```

### 使用装饰器实现
```javascript
import Btn from './Button';

<Table.Column title="id" dataIndex="admin" />
<Table.Column title="状态" dataIndex="status" />
<Table.Column 
	title="操作"
	cell={ (value, index, record) =>
		<Btn admin={record.admin} status={record.status}/>
	} 
/>
```

Button.jsx
```javascript
import React, { Component } from 'react';
import {Button} from 'antd';
import OperBtn from './OperBtn'

// 装饰器实现
@OperBtn
export default class Btn extends Component {
	render() {
    // 从 OperBtn.jsx 过来的props
		const {admin, status} = this.props;
		return (
			<Button 
				disabled={admin !== 0}
				shape={status === 'publish'?'warning':'ghost'}
			>{status === 'publish'?'应用下架':'应用上架'}</Button>
		);
	}
}
```

OperBtn.jsx
```javascript
import React, { Component } from 'react';

// 接收Button.jsx导出的类
export default function (Button) {
	return class OperBtn extends Component {
		render() {
			// <Btn admin={record.admin} status={record.status}/>
			// 通过this.props接收值
			return (
				<Button {...this.props} />
			);
		}
	}
}
```