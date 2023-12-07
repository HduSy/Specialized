Created Date：2023-12-07 14:07:54  
Last Modified：2023-12-07 14:07:54

# Tags

# Content

## 创建 Redux State Slice

为 `slice` 定义名称 (`name`)，编写一个包含 `reducer` 函数的对象 (`reducers`) ，自动生成相应的 `action` 代码，`name` 选项的字符串用作每个 `action` 类型的第一部分，每个 `reducer` 函数的**键名**用作第二部分。因此，`"counter"` 名称 + `"increment"` reducer 函数生成了一个 `action` 类型 `{type: "counter/increment"}`  
`createSlice` 自动生成与 `reducer` 函数同名的 `action creator`  

```ts
// features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit'

export const counterSlice = createSlice({
  name: 'counter', // 切片标识
  initialState: { // 初始state
    value: 0
  },
  reducers: { // 定义一个或多个更新state的函数
    increment: state => {
      // Redux Toolkit 允许我们在 reducers 写 "可变" 逻辑。它
      // 并不是真正的改变状态值，因为它使用了 Immer 库
      // 可以检测到“草稿状态“ 的变化并且基于这些变化生产全新的
      // 不可变的状态
      state.value += 1
    },
    decrement: state => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})
// 每个 case reducer 函数会生成对应的 Action creators
export const { increment, decrement, incrementByAmount } = counterSlice.actions // 导出 action creators

export default counterSlice.reducer // 导出 reducer 函数
```

## 将 Slice Reducers 添加到 Store 中

app/store.js

```ts
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export default configureStore({
  // 告诉 store 使用这个 slice reducer 函数来处理对counter的所有更新
  reducer: {
    counter: counterReducer
  }
})
```

## 在 React 组件中使用 Redux 状态和操作

features/counter/Counter.js

```ts
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment } from './counterSlice'
import styles from './Counter.module.css'

export function Counter() {
  const count = useSelector(state => state.counter.value) // 从 store 中读取数据
  const dispatch = useDispatch() // dispatch actions

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  )
}
```

## 用 Thunk 编写异步逻辑

**thunk** 是一种特定类型的 Redux 函数，可以包含异步逻辑，它以 `dispatch` 和 `getState` 作为参数

```ts
// 下面这个函数就是一个 thunk ，它使我们可以执行异步逻辑
// 你可以 dispatched 异步 action `dispatch(incrementAsync(10))` 就像一个常规的 action
// 调用 thunk 时接受 `dispatch` 函数作为第一个参数
// 当异步代码执行完毕时，可以 dispatched actions
export const incrementAsync = amount => dispatch => {
  setTimeout(() => {
    dispatch(incrementByAmount(amount))
  }, 1000)
}
```

# Reference

[[react-redux]]
