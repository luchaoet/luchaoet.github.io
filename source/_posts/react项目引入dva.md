---
title: react项目引入dva
date: 2018-12-14 22:11:01
tags: ['react', 'dva']
summary:
---
我们项目并不是使用dva-cli创建的react脚手架，而是在开发中期发现项目中有太多的状态需要维护，非常的麻烦，所以想单独把state状态抽离出来，于是便引入了dva
dva是基于react+redux最佳实践上实现的封装方案，简化了redux和redux-saga使用上的诸多繁琐操作
dva = React-Router + Redux + Redux-saga

### 数据流向
<span data-type="color" style="color:rgb(44, 62, 80)">数据的改变发生通常是通过用户交互行为或者浏览器行为（如路由跳转等）触发的，当此类行为会改变数据的时候可以通过 </span>`dispatch`<span data-type="color" style="color:rgb(44, 62, 80)"> </span>发起一个 action，如果是同步行为会直接通过 `Reducers`<span data-type="color" style="color:rgb(44, 62, 80)"> </span>改变 `State`<span data-type="color" style="color:rgb(44, 62, 80)">，如果是异步行为（副作用）会先触发 </span>`Effects`<span data-type="color" style="color:rgb(44, 62, 80)"> </span>然后流向 `Reducers`<span data-type="color" style="color:rgb(44, 62, 80)"> </span>最终改变 `State`<span data-type="color" style="color:rgb(44, 62, 80)">，所以在 dva 中，数据流向非常清晰简明，并且思路基本跟开源社区保持一致</span>


![PPrerEAKbIoDZYr.png | center | 747x235](https://cdn.nlark.com/yuque/0/2018/png/115449/1544672504156-29383284-11ad-4629-8510-ac6e74ebb483.png "")


### Router
routes.js
```javascript
import { Router } from 'dva/router';

const customRoutes = [
    {
        path: '/index',
        childRoutes: [],
        component: IndexPage
    }
];
export default ({history}) => {
  return (
    <Router
      history={history}
      routes={[...customRoutes]}
    />
  );
};
```

index.js
```javascript
import routes from './routes';

app.router(routes);
```

### 定义model
```javascript
namespace: 'indexModel',
state: {
    list: []
},
reducers: {},
effects: {},
subscriptions: {}
```

### <span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">关联Model中的数据源</span></span>
将需要的state和需要绑定的响应事件注入到组件
```javascript
import { connect } from 'dva';
const mapStateToProps = state => state.indexModel;
const mapDispatchToProps = dispatch => {
  return { dispatch, };
};
@connect(mapStateToProps, mapDispatchToProps)
export default class Index extends Component {
    constructor(props) {
    super(props);
    this.state = {}
    render(){
        return <div>indexPage</div>
    }    
}
```

#### 获取state数据
```javascript
const { list } = this.props;
```

#### dispatch函数
dispatching function 是一个用于触发 action 的函数，action 是改变 State 的唯一途径，但是它只描述了一个行为，而 dipatch 可以看作是触发这个行为的方式，而 Reducer 则是描述如何改变数据的
在 dva 中，connect Model 的组件通过 props 可以访问到 dispatch，可以调用 Model 中的 Reducer 或者 Effects
```javascript
this.props.dispatch({
    type: 'indexModel/fetchList',
    payload:{
        currentPage: 1,
        pageSize: 10
    }
})
```
dispatch 函数，既可以调用reducers更新数据，又可以调用effects进行异步操作
通过 type 属性指定对应的 actions 类型，而这个类型名在 reducers（effects）会一一对应，从而知道该去调用哪一个 reducers（effects），除了 type 以外，其它对象中的参数随意定义，都可以在对应的 reducers（effects）中获取，从而实现消息传递，将最新的数据传递过去更新 model 的数据（state）
<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Action在model自身模型以外定义时需要加model的namespace前缀, 在model中定义不需要加</span></span>

### namespace
namespace是model state在全局state中的key

### state
state是整个应用的数据层。应用的state被存储在一个object tree中。应用的初始state在model中定义，也就是说，由model state组成全局state。操作的时候每次都要当作不可变数据（immutable data）来对待，保证每次都是全新对象，没有引用关系，这样才能保证 State 的独立性，便于测试和追踪变化

### reducer
reducer是唯一可以更新state的地方，这个唯一性让我们的 App 更具可预测性，所有的数据修改都有据可查。reducer接收参数 state 和 action，返回新的 state，通过语句表达即 (state, action) => newState。该函数把一个集合归并成一个单值
```javascript
namespace: 'indexModel',
state: {
    list: []
},
reducers: {
    updateList(state, {payload}){
        return {
            ...state,
            list: payload
        }
    }
},
effects: {},
subscriptions: {}
```

### effect
异步处理
```javascript
import { createService } from '@/utils/ajax';

export default {
  namespace: 'indexModel',
  state: {
    list: []
  },
  reducers: {
    updateList(state, {payload}){
        return {
            ...state,
            list: payload
        }
    }
  },
  effects: {
    * fetchList({ payload }, { call, put }) {
      const pager = yield select(state => state.historyModel.pager);
      const res = yield call(createService, {
        url: '/task/list',
        data: payload
      });
      if(res && res.success){
        yield put({
          type: 'updateList',
          payload: res.data || []
        });
      }
    }
  },
  subscriptions: {}
}
```
当数据需要从服务器获取时，需要发起异步请求，请求到数据之后，通过调用 Reducers更新数据到全局state。dva 通过对 model 增加 effects 属性来处理 side effect(异步任务)，这是基于 redux-saga 实现的，语法为 generator。Generator 返回的是迭代器，通过 yield 关键字实现暂停功能

`fetchList(action, {call, put, select}){}`表示一个worker Saga，监听所有的query action，并触发一个Api调用以获取服务器数据。当每个query action被发起时调用 call 和 put 都是 redux-saga 的 effects，call 表示调用异步函数，put 表示 dispatch action，其他的还有 select, take, fork, cancel 等，select 则可以用来访问其它 model。格式：`*(action, effects) => void`

<span data-type="color" style="color:rgb(47, 47, 47)"><span data-type="background" style="background-color:rgb(255, 255, 255)">在Effects里，Generator函数通过yield命令将异步操作同步化，无论是yield 亦或是 async 目的只有一个： 让异步编写跟同步一样 ，从而能够很好的控制执行流程</span></span>

### subscription
```javascript
subscriptions: {
    setup(( dispatch, history )){
        history.listen(({ pathname, query }) => {
            if(pathToRegexp(`/userAuthManager/list`).test(pathname)) {
                dispatch({
                    type:`fetchList`,
                    payload: ....
                })
            }
        })
    }
}
```
Subscriptions 表示订阅，用于订阅一个数据源，然后按需 dispatch action。格式为 ({ dispatch, history }) => unsubscribe 。比如：当用户进入 /userAuthManager/list 页面时，触发 action fetchList加载数据。

如果 url 规则比较复杂，比如 /users/:userId/search，那么匹配和 userId 的获取都会比较麻烦。这是推荐用 path-to-regexp 简化这部分逻辑

### call 和 put
dva 提供多个 effect 函数内部的处理函数，比较常用的是 call 和 put。
call：执行异步函数
put：发出一个 Action，类似于 dispatch

### 多任务调度
#### 任务的并行执行
<span data-type="color" style="color:rgb(44, 62, 80)">把多个要并行执行的东西放在一个数组里，就可以并行执行，等所有的都结束之后，进入下个环节，类似promise.all的操作。一般有一些集成界面，比如dashboard，其中各组件之间业务关联较小，就可以用这种方式去分别加载数据，此时，整体加载时间只取决于时间最长的那个</span>
```javascript
const [result1, result2]  = yield [
  call(service1, param1),
  call(service2, param2)
]

// 实例
if(res && res.success){
    yield [
        put({type: 'saveSummaries', payload: res.data.summaries }),
        put({type: 'savePager', payload: res.pager }),
        put({type: 'saveScanTask', payload: res.data.scanTask })
    ]
}
```

#### 任务竞争
<span data-type="color" style="color:rgb(44, 62, 80)">若干个任务之间，只要有一个执行完成，就进入下一个环节，类似于Promise.race的作用</span>
```javascript
const { data, timeout } = yield race({
  data: call(service, 'some data'),
  timeout: call(delay, 1000)
});

if (data)
  put({type: 'DATA_RECEIVED', data});
else
  put({type: 'TIMEOUT_ERROR'});
```
<span data-type="color" style="color:rgb(44, 62, 80)">这个例子比较巧妙地用一个延时一秒的空操作来跟一个网络请求竞争，如果到了一秒，请求还没结束，就让它超时</span>

### 子任务
若干个任务，并行执行，但必须全部做完之后，下一个环节才继续执行
<span data-type="color" style="color:rgb(44, 62, 80)">这个类似于Promise.all的作用</span>


### model的复用
<span data-type="color" style="color:rgb(44, 62, 80)">有时候，业务上可能遇到期望把一些与外部关联较少的model拆出来的需求，我们可能会拆出这样的一个model，然后用不同的视图容器分别connect同一个model</span>

### 总结
dva将所有与数据操作相关的逻辑集中放在一个地方处理和维护，在数据跟业务状态交互比较紧密的场景下，会使我们的代码更加清晰可控。尤其适用于数据跟业务状态关联性极强的企业级后台信息管理系统
对于一个企业级后台管理系统，由于要进行大量的数据操作，在设计model时将不同类型的业务需求数据操作分开处理，便于维护
项目的开发流程一般是从设计model state开始进行抽象数据，完成component后，将组件和model建立关联，通过dispatch一个action，在reducer中更新数据完成数据同步处理；当需要从服务器获取数据时，通过Effects数据异步处理，然后调用Reducer更新全局state。是一个单向的数据流动过程。解决了redux中代码分散和重写问题
