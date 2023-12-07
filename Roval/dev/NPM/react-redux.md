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
- 消除意外的 `mutations`，这一直是 Redux bug 的首要原因；
- 消除了手写任何 `action creator` 或 `action type` 的需求；
- 自动配置 Redux DevTools 扩展；
- 友好支持 `TypeScript`；

### 变更

- `createStore` 替换为 `configureStore`
- `reducer` 切换到 `createSlice`

## 基本概念

**Redux 是一个使用叫做 “action” 的事件来管理和更新应用状态的模式和工具库** 它以**集中式 Store**（centralized store）的方式对整个应用中使用的状态进行集中管理，其规则确保状态只能以**可预测**的方式更新。

### 使用情形

- 应用中有很多 state 在多个组件中需要使用
- 应用 state 会随着时间的推移而频繁更新
- 更新 state 的逻辑很复杂
- 中型和大型代码量的应用，很多人协同开发

### 核心思想

应用中使用集中式的全局状态来管理，并明确更新状态的模式，以便让代码具有可预测性。**Redux 期望所有状态更新都是使用不可变的方式**

### 放入全局 store 还是组件 state

- 应用程序的其他部分是否关心这些数据？
- 你是否需要能够基于这些原始数据创建进一步的派生数据？
- 是否使用相同的数据来驱动多个组件？
- 能够将这种状态恢复到给定的时间点（即时间旅行调试）对你是否有价值？
- 是否要缓存数据（即，如果数据已经存在，则使用它的状态而不是重新请求它）？
- 你是否希望在热重载视图组件（交换时可能会丢失其内部状态）时保持此数据一致？

## API

### store

应用的整体全局状态以对象树的方式存放于单个 `store`。唯一改变状态树（`state tree`）的方法是创建 `action`，一个描述发生了什么的对象，并将其 `dispatch` 给 `store`。 要指定状态树如何响应 `action` 来进行更新，你可以编写纯 `reducer` 函数，这些函数根据旧 `state` 和 `action` 来计算新 `state`。

#### store.getState()

返回当前状态值

#### store.dispatch()

- **更新 state 的唯一方法是调用 `store.dispatch()` 并传入一个 action 对象**。
- 是 `View` 发出 `action` 的唯一方法。
- `dispatch` 一个 `action` 可以形象的理解为 " 触发一个事件 "。

#### store.subscribe()

设置监听 `state` 的函数，每当 `dispatch action` 的时候就会执行，`state` 树中的一部分可能已经变化，执行相应函数进行处理。你可以在回调函数里调用 [`getState()`](https://www.reduxjs.cn/api/store/#getstate) 来拿到当前 `state`

### 示例

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

### state

初始启动：

- 获取应用程序特定时刻的 `store` 快照
- 基于 `state` 来渲染视图
- 当发生某些事情时（例如用户单击按钮），`state` 会根据发生的事情进行更新
- 基于新的 `state` 重新渲染视图  

更新环节：

- 应用程序中发生了某些事情，例如用户单击按钮
- dispatch 一个 action 到 Redux store，例如 `dispatch({type: 'counter/increment'})`
- store 用之前的 `state` 和当前的 `action` 再次运行 reducer 函数，并将返回值保存为新的 `state`
- store 通知所有订阅过的视图，通知它们 store 发生更新
- 每个订阅过 store 数据的视图 组件都会检查它们需要的 state 部分是否被更新。
- 发现数据被更新的每个组件都强制使用新数据重新渲染，紧接着更新网页

### action

`action` 是一个具有 `type` 字段的普通 JavaScript 对象。**你可以将 `action` 视为描述应用程序中发生了什么的事件**，`action` 对象可以有其他字段，其中包含有关发生的事情的附加信息。按照惯例，我们将该信息放在名为 `payload` 的字段中。  
在交互的页面（`View`）中，用户无法直接接触 `state`，只能接触 `View`，`View` 的变化触发 `state` 变化，`action` 就是 `View` 发出的通知。  
`action` 是改变 `state` 的唯一方式，携带数据到 `state`。

#### action creator

是一个创建并返回一个 `action` 对象的函数，避免每次手动编写 `action` 对象

### reducer

`reducer` 是一个函数，接收当前的 `state` 和一个 `action` 对象，必要时决定如何更新状态，并返回新状态。函数签名是：`(state, action) => newState`。 **你可以将 reducer 视为一个事件监听器，它根据接收到的 action（事件）类型处理事件。**  
`store` 收到 `action` 后，经计算过程返回一个全新的 `state` 以更新 `View`，这个中间的计算过程就叫做 `reducer`。

#### 规则

- 仅使用 `state` 和 `action` 参数计算新的状态值
- 禁止直接修改 `state`。必须通过复制现有的 `state` 并对复制的值进行更改的方式来做 *不可变更新（immutable updates）*。
- 禁止任何异步逻辑、依赖随机值或导致其他“副作用”的代码

#### 原因

- Redux 的目标之一是使你的**代码可预测**。当函数的输出仅根据输入参数计算时，更容易理解该代码的工作原理并对其进行测试；
- 另一方面，如果一个函数依赖于自身之外的变量，或者行为随机，你永远不知道运行它时会发生什么；
- 如果一个函数 mutate 了其他对象，比如它的参数，这可能会**意外地改变应用程序的工作方式**。这可能是错误的常见来源，例如“我更新了我的状态，但现在我的视图没有在应该更新的时候更新！”
- Redux DevTools 的一些功能取决于你的 reducer 是否正确遵循这些规则；

### slice

**`slice` 是应用中单个功能的 Redux reducer 逻辑和 action 的集合**，该名称来自于将根 Redux 状态对象拆分为多个状态 “slice”，**根 `state` 的切片**

### selector

`selector` 函数可以从 `store` 状态树中提取指定的片段。

## 中间件与异步操作

# Reference

[Redux 入门教程（一）：基本用法 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)
