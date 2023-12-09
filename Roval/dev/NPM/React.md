Created Date：2022-12-17 22:19:24  
Last Modified：2022-12-17 22:19:23

# Tags

 #React #前端性能优化

# Content

## 语言本身

- Fragments 避免添加额外 DOM 元素以防标签失效 ->[Fragment](https://zh-hans.reactjs.org/docs/fragments.html)

短写法：`<></>`，无属性可加；常规写法：`<React.Fragment key={}></React.Fragment>`

### PureComponent vs Component

[无状态组件 | Roundtable Coders](https://johninch.github.io/Roundtable/Question-Bank/react/SFC.html#purecomponent-vs-component)

## Hooks

### createContext

类似于 Vue 里面 `provide & inject` 跨组件通信方式。创建上下文  
[createContext – React](https://react.dev/reference/react/createContext)

### useEffect

- useEffect 为函数式组件提供副效应，支持第二个参数填依赖项，条件执行  
[阮一峰 useEffect](https://www.ruanyifeng.com/blog/2020/09/react-hooks-useeffect-tutorial.html)
- 产生 memorization 函数，以空间换时间，缓存纯函数计算结果，只有指定依赖项发生变更时才重新计算结果。  
[CSDN useCallback](https://blog.csdn.net/milk_0126/article/details/103635225)
- more

### useRef(initialValue)

[useRef – React](https://react.dev/reference/react/useRef) [[#useImperativeHandle]]

#### 使用情景

- 创造一个引用值，该值不用来渲染；
- 作为 `DOM` 元素的引用；
- 能避免重复创建 `ref` 值，后续渲染将返回同一对象；

#### API

参数： `initialValue`：current 属性的初始值，该参数在首次渲染之后被忽略；

返回值：返回一个只有 `current` 属性的对象，初始值为 `initialValue`。  
若作为 `jsx` 节点属性用，**当 `React` 创建 `DOM` 节点并将其渲染到屏幕时**，`React` 将会把 `DOM` 节点设置为 ref 对象的 `current` 属性。  
若节点从屏幕上移除时，`React` 将把 `current` 属性设置回 `null`。

#### 注意

- 修改 `ref` 不会引起组件重渲染；
- 组件多次渲染之间存储信息；
- 对组件而言是 `local` 本地的，外部变量则是共享的；
- 与视图无关，不适合用于存储期望显示在屏幕上的信息；
- **不要在渲染期间写入或者读取 `ref.current`**，`useEffect` 里是 ok 的；
- **不要滥用 ref**  
- **如果可以通过 prop 实现，那就不应该使用 ref**

```ts
function MyComponent() {
  // ...
  // 🚩 不要在渲染期间写入 ref
  myRef.current = 123;
  // ...
  // 🚩 不要在渲染期间读取 ref
  return <h1>{myOtherRef.current}</h1>;
}
```

### React.memo 性能优化

创建（memoized）记忆化组件，`prop` 变化或其自身 `state`、`context` 变化时，才会重新渲染。  
[memo – React 中文文档](https://zh-hans.react.dev/reference/react/memo)

### useMemo 性能优化

#### useMemo(calculateValue, dependencies)

`calculateValue` 大概长这样 `() => any`，是一个无参数可以返回任意类型的函数，根据 `dependencies` 缓存多次 `re-render` 间的计算结果，**初始化时调用一次**，之后只有依赖项发生变化时才重新调用 `calculateValue` 计算值，否则直接返回上一次的计算结果

`dependencies`: reactive values include **props, state, and all the variables and functions** declared directly inside your component body.React will compare each dependency with its previous value using the [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) comparison.  

[useMemo – React](https://react.dev/reference/react/useMemo)

### useCallback 性能优化

#### useCallback(fn, dependencies)  

`dependencies`: Reactive values include props, state, and all the variables and functions declared directly inside your component body.

`fn` 是想要缓存的函数，接受任意参数，返回任意类型。一般情况下，组件的 re-render 也会递归 re-render 它的子组件，如果不想子组件重复渲染，可以把子组件用 `memo` 包裹，`memo` 组件的特性就是只要组件 props 不变，那么就会 skip re-render 过程。传递给子组件的函数用 `useCallback` 包裹，这样的话，`dependencies` 不变，函数还是同一函数，子组件可避免重渲染。 [useCallBack你真的知道怎么用吗。 - 掘金](https://juejin.cn/post/7107943235099557896)

[useCallback – React](https://react.dev/reference/react/useCallback)

### useImperativeHandle(ref, createHandle, dependencies?)

自定义由 `ref` 暴露出来的 `handle` 句柄，**受限的**而非暴露整个 `DOM` 节点，这样就无法访问该 `DOM` 节点其他的属性和方法了 [useImperativeHandle – React 中文文档](https://zh-hans.react.dev/reference/react/useImperativeHandle) [[#useRef(initialValue)]] [[#forwardRef]]  

#### 参数

`dependencies`：如果一次重新渲染导致某些依赖项发生了改变，或你没有提供这个参数列表，你的函数 `createHandle` 将会被重新执行，而新生成的句柄则会被分配给 `ref`。

## API

### forwardRef

`forwardRef` 允许组件使用 [ref](https://zh-hans.react.dev/learn/manipulating-the-dom-with-refs) 将 `DOM` 节点暴露给父组件 [forwardRef – React 中文文档](https://zh-hans.react.dev/reference/react/forwardRef)

## UI 呈现所需最小状态原则 - state

![[Pasted image 20230114165200.png]]  

说人话就是：

1. 随时间改变；
2. 非 prop 传递；
3. 可计算得来；

## prop 使用默认值的情况

![[Pasted image 20230115231217.png]]

1. 不传值
2. 传 `undefined`，不可传 `null` 或 `0`

## React 技术栈

### react-dom

包含一些仅在浏览器 DOM 环境下运行的方法，不支持在 `React Native` 中使用  
[React DOM API – React 中文文档](https://zh-hans.react.dev/reference/react-dom)

#### createPortal

允许在 `DOM` 的不同位置渲染组件

#### flushSync

强制 `React` 同步刷新状态并更新 `DOM`

### react-router-dom

路由管理 [Home v6.17.0 | React Router](https://reactrouter.com/en/main)

### react-redux

全局状态管理 [[react-redux]]

### @reduxjs/toolkit

#### createSelector

【React 性能优化】的一种写法，If the selector is called again with the same arguments, the previously **cached result** is returned instead of recalculating a new result.

```js
	import { createSelector } from '@reduxjs/toolkit'
```

[GitHub - reduxjs/reselect: Selector library for Redux](https://github.com/reduxjs/reselect#createselectorinputselectors--inputselectors-resultfunc-selectoroptions)  
[GitHub - reselect](https://github.com/reduxjs/reselect)

### react-loadable

基于组件而非路由的代码分割（基于路由缺点，页面中尚处于隐藏的组件也不分情况的加载了）  
[GitHub - jamiebuilds/react-loadable: :hourglass\_flowing\_sand: A higher order component for loading components with promises.](https://github.com/jamiebuilds/react-loadable)

### react-dnd & react-dnd-html5-backend

React 元素拖拽库，开发低代码平台拖拽功能  
[GitHub - react-dnd/react-dnd: Drag and Drop for React](https://github.com/react-dnd/react-dnd/)  
[关于react-dnd，看这一篇就够了 - 掘金](https://juejin.cn/post/7155046917028708359)  
[GitHub - bokuweb/react-rnd: 🖱 A resizable and draggable component for React.](https://github.com/bokuweb/react-rnd)

### react-hot-loader

不修改状态的前提下，局部刷新（与 webpack 的 HMR 不同），能提升开发构建速度。  
[react-hot-loader原理 - react等等我](https://sekin.gitbook.io/react/chapter1/react-hot-loaderyuan-li)

### classnames

条件表达式拼接元素 css `class` 样式  
[classnames - npm](https://www.npmjs.com/package/classnames)

### styled-components

`css in js` 方案之一  
[styled-components - npm](https://www.npmjs.com/package/styled-components)

### recharts

图表  
[recharts.org](https://recharts.org/en-US)

### prop-types

`React Component Prop` 类型检查 [GitHub - facebook/prop-types: Runtime type checking for React props and similar objects](https://github.com/facebook/prop-types)

### react-highlight-words

做到文字高亮效果  
[react-highlight-words - npm](https://www.npmjs.com/package/react-highlight-words)

### antd

antDesign-UI 框架  
[Ant Design of React - Ant Design](https://ant.design/docs/react/introduce)

### @ant-design/charts

antDesign Charts 图表库  
[Ant Design Charts - 简单好用的 React 图表库](https://v0-charts.ant.design/)

# Reference

[《React 学习之道》](https://github.com/the-road-to-learn-react/the-road-to-learn-react-chinese)  
[尚硅谷课程笔记](https://github.com/xzlaptt/React)
