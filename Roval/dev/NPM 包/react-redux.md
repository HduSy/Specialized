Created Date：2022-12-17 22:20:18  
Last Modified：2022-12-17 22:20:18

# Tags

 #npm包 #react

# Content

## 按需使用

- 组件状态共享
- 全局状态
- 组件改变全局状态
- A 组件改变 B 组件状态

## 设计思想

- Web 视作状态机，视图状态一一对应
- 所有状态存在一个对象中

## 基本概念&API

### Store

#### 说明

保存数据的地方，整个应用只能有一个。redux 提供 `createStore()` 方法啊生成。

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

设置监听 `state` 的函数，一旦发生变化，便执行相应函数处理。

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
