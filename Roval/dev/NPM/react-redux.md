Created Date：2022-12-17 22:20:18  
Last Modified：2022-12-17 22:20:18

# Tags

 #Npm包 #React

# Content

## 按需使用

- 组件状态共享
- 全局状态
- 组件改变全局状态
- A 组件改变 B 组件状态

## 设计思想

- Web 视作状态机，视图状态一一对应
- 所有状态存在一个对象中

## 官方推荐最佳实践

**如果今天你要写任何的 Redux 逻辑，你都应该使用 `@reduxjs/toolkit` 来编写代码**

### redux vs RTK

`redux` 弊端：`redux` 要写很多额外的于 `redux API` 无关的样板代码，且需手动编写对象的展开和数组操作，这是 `redux bug` 的主要原因 ；

`RTK` 优势：消除样板代码，大大简化书写，防止常见错误。

- `createSlice` 基于 [Immer 库](https://immerjs.github.io/immer/) 编写 `reducer`，从而支持 `"mutating" JS` 语法，比如 `state.value = 123`，无需使用拓展运算符。 同时以 `reducer` 名称生成 `action type` 字符串；
- `configureStore` 通过**单个函数调用**设置一个配置完善的 `redux store`，包括合并 `reducer`、添加 `thunk` 中间件以及设置 `redux DevTools` 集成。与 `createStore` 相比更容易配置，因为它接受命名选项参数；
- 友好支持 `TypeScript`；

## 基本概念&API

### Store

#### 说明

应用的整体全局状态以对象树的方式存放于单个 `store`。唯一改变状态树（`state tree`）的方法是创建 `action`，一个描述发生了什么的对象，并将其 `dispatch` 给 `store`。 要指定状态树如何响应 `action` 来进行更新，你可以编写纯 `reducer` 函数，这些函数根据旧 `state` 和 `action` 来计算新 `state`。

#### 示例

### State

#### 说明

获取某时点 `store` 快照，相同的 `state` 对应相同的视图。

#### 示例

### Action

#### 说明

在交互的页面（`View`）中，用户无法直接接触 `state`，只能接触 `View`，`View` 的变化触发 `state` 变化，`action` 就是 `View` 发出的通知。  
`action` 是改变 `state` 的唯一方式，携带数据到 `state`。

##### Action Creator

函数方式生成 `action`

##### store.dispatch()

`store.dispatch()` 是 `View` 发出 `action` 的唯一方法。

#### 示例

### Reducer

#### 说明

`store` 收到 `action` 后，经计算过程返回一个全新的 `state` 以更新 `View`，这个中间的计算过程就叫做 `reducer`。

##### store.subscribe()

设置监听 `state` 的函数，每当 `dispatch action` 的时候就会执行，`state` 树中的一部分可能已经变化，执行相应函数进行处理。你可以在回调函数里调用 [`getState()`](https://www.reduxjs.cn/api/store/#getstate) 来拿到当前 `state`

#### 示例

将 `render` 或 `setState` 方法放入 `listener` 即可：

```javascript
import { createStore } from 'redux';
const store = createStore(reducer);

store.subscribe(listener);
```

返回解除监听函数 `unsubscribe`：

```javascript
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

unsubscribe();
```

## 中间件与异步操作

# Reference

[Redux 入门教程（一）：基本用法 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)
