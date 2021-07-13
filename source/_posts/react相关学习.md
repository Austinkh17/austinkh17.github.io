---
title: react相关学习
date: 2021-06-16 17:52:03
tags: react
categories: react
---

[Vue 转 React 指南，看这篇文章就够了](https://markdowner.net/article/166272088981004288)
[轻松学会 React 钩子：以 useEffect() 为例](https://www.ruanyifeng.com/blog/2020/09/react-hooks-useeffect-tutorial.html)
[React Hooks 入门教程](http://www.ruanyifeng.com/blog/2019/09/react-hooks.html)
1.useState()
2.useContext()-组件间共享状态
3.useEffect()-副作用钩子
4.useReducer()
5.自定义hook
[react Hook之useMemo、useCallback及memo](https://juejin.cn/post/6844903954539626510)
[Hook API 索引](https://zh-hans.reactjs.org/docs/hooks-reference.html)
[react-redux性能优化之reselect](https://www.jianshu.com/p/1fcef4c892ba)
[redux-saga](https://redux-saga-in-chinese.js.org/)
[React调试利器：React DevTools](https://juejin.cn/post/6877546408925200391)
[redux devTools使用指南](https://zhuanlan.zhihu.com/p/30784749)
[Redux入门教程（快速上手）](https://segmentfault.com/a/1190000011474522)
[Redux-saga框架使用详解及Demo教程](https://juejin.cn/post/6844903669305966599)
[Redux 入门教程（一）：基本用法](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)
[Redux 入门教程（二）：中间件与异步操作](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html)
[Redux 入门教程（三）：React-Redux 的用法](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)

<!--more-->

### 1.redux管理状态，react-redux连接react和redux
index.js
```
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import ReactDOM from 'react-dom'
import { createStore } from 'redux'
import { Provider, connect } from 'react-redux'

// React component  UI组件
class Counter extends Component {
  render() {
    const { value, onIncreaseClick } = this.props
    return (
      <div>
        <span>{value}</span>
        <button onClick={onIncreaseClick}>Increase</button>
      </div>
    )
  }
}

Counter.propTypes = {
  value: PropTypes.number.isRequired,
  onIncreaseClick: PropTypes.func.isRequired
}

// Action
const increaseAction = { type: 'increase' }

// Reducer
function counter(state = { count: 0 }, action) {
  const count = state.count
  switch (action.type) {
    case 'increase':
      return { count: count + 1 }
    default:
      return state
  }
}

// Store
const store = createStore(counter)

// Map Redux state to component props
function mapStateToProps(state) {
  return {
    value: state.count
  }
}

// Map Redux actions to component props
function mapDispatchToProps(dispatch) {
  return {
    onIncreaseClick: () => dispatch(increaseAction)
  }
}

// Connected Component  连接redux和react
const App = connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter)

//  容器组件
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

### 2.redux-saga处理异步action
index.js
```
import React from 'react';
import ReactDOM from 'react-dom';
import {createStore, applyMiddleware} from 'redux'
import createSagaMiddleware from 'redux-saga'

import rootSaga from './sagas'
import Counter from './Counter'
import rootReducer from './reducers'

const sagaMiddleware = createSagaMiddleware()
let middlewares = []
middlewares.push(sagaMiddleware)

const createStoreWithMiddleware = applyMiddleware(...middlewares)(createStore)
const store = createStoreWithMiddleware(rootReducer)

sagaMiddleware.run(rootSaga)

const action = type => store.dispatch({ type })

function render() {
  ReactDOM.render(
    <Counter
      value={store.getState()}
      onIncrement={() => action('INCREMENT')}
      onDecrement={() => action('DECREMENT')}
      onIncrementAsync={() => action('INCREMENT_ASYNC')} />,
    document.getElementById('root')
  )
}

render()

store.subscribe(render)
```

sagas.js
```
import { put, call, take,fork } from 'redux-saga/effects';
import { takeEvery, takeLatest } from 'redux-saga'

export const delay = ms => new Promise(resolve => setTimeout(resolve, ms));

function* incrementAsync() {
  // 延迟 1s 在执行 + 1操作
  yield call(delay, 1000);
  yield put({ type: 'INCREMENT' });
}

export default function* rootSaga() {
  // while(true){
  //   yield take('INCREMENT_ASYNC');
  //   yield fork(incrementAsync);
  // }

  // 下面的写法与上面的写法上等效
  yield* takeEvery("INCREMENT_ASYNC", incrementAsync)
}
```

reducer.js
```
export default function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    case 'INCREMENT_ASYNC':
      return state
    default:
      return state
  }
}
```

### 3.
- reselect是配合 redux 使用的一款轻量型的状态选择库，目的在于当 store 中的 state 重新改变之后，使得局部未改变的状态不会因为整体的 state 变化而全部重新渲染，功能有点类似于组件中的生命周期函数 shouldComponentDidUpdate ，但是它们并不是一个东西。
- React Helmet 是一个为React 打造的 head 管理工具。 它可以动态更新渲染在服务端的 meta 标签，就像在客户端上一样轻松愉快。 可以说是做SEO、社交平台优化的最佳选择。

### 4. css(react)局部作用域与global
#### 局部作用域
css的规则都是全局的，任何一个组件的样式规则，都对整个页面有效。产生局部作用域的唯一方法，就是使用一个独一无二的class的名字，不会与其他选择器重名，但是当我们与其他人共同开发的时候，无法保证一定与其他人不同，这时候就要用到css modules。

下面是一个React组件App.js

```
import React from 'react';
import style from './App.css';
 
export default () => {
  return (
    <h1 className={style.title}>
      Hello World
    </h1>
  );
};
```
上面代码中，我们将样式文件App.css输入到style对象，然后引用style.title代表一个class，构建工具会将类名style.title编译成一个哈希字符串。这样一来，这个类名就变成独一无二了，只对App组件有效。

#### global
使用global声明的class，都不会被编译成哈希字符串。

:global {
...
}
可作用于修改antd等ui组建的样式

### 访问类型
public 允许我在类的内外被调用
private 允许在类内被调用
protected 允许在类内及继承的子类中使用
如果不写的话,默认是public

### react事件绑定的三种方式
[推荐阅读](https://www.cnblogs.com/chengeping/p/10517690.html)
在 React 组件中，每个方法的上下文都会指向该组件的实例，即自动绑定 this 为当前组件。 而且 React 还会对这种引用进行缓存，以达到 CPU 和内存的优化。在使用 ES6 classes 或者纯 函数时，这种自动绑定就不复存在了，我们需要手动实现 this 的绑定。

### react中类似watch的方法
1.函数式
```
useEffect(() => {
    console.log('counter发生了变化，最新值：', counter);
}, [counter]);
```
2.class
```
public componentDidUpdate (prevProps, prevState) {
    if(!isEqual(prevProps.editingViewInfo.variable, this.props.editingViewInfo.variable)){
      this.variableChangeExecuteParams()
    }
}
```