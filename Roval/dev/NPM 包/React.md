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

### useRef

[useRef – React](https://react.dev/reference/react/useRef)

- 值的改变不会触发 re-render
- 可用来操作 DOM 元素
  
 ### useMemo 性能优化

#### useMemo(calculateValue, dependencies)

`dependencies`: Reactive values include props, state, and all the variables and functions declared directly inside your component body.  

`calculateValue` 大概长这样 `() => any`，是一个无参数可以返回任意类型的函数，根据 `dependencies` 缓存多次 re-render 间的计算结果，初始化时调用一次，之后只有依赖项发生变化时，才重新调用 `calculateValue` 计算值，否则直接返回上一次的计算结果。

[useMemo – React](https://react.dev/reference/react/useMemo)

### useCallback 性能优化

#### useCallback(fn, dependencies)  

`dependencies`: Reactive values include props, state, and all the variables and functions declared directly inside your component body.

`fn` 是想要缓存的函数，接受任意参数，返回任意类型。一般情况下，组件的 re-render 也会递归 re-render 它的子组件，如果不想子组件重复渲染，可以把子组件用 `memo` 包裹，`memo` 组件的特性就是只要组件 props 不变，那么就会 skip re-render 过程。传递给子组件的函数用 `useCallback` 包裹，这样的话，`dependencies` 不变，函数还是同一函数，子组件可避免重渲染。  

[useCallback – React](https://react.dev/reference/react/useCallback)

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

路由管理

### react-redux

全局状态管理

### react-loadable

基于组件而非路由的代码分割（基于路由缺点，页面中尚处于隐藏的组件也不分情况的加载了）  
[GitHub - jamiebuilds/react-loadable: :hourglass\_flowing\_sand: A higher order component for loading components with promises.](https://github.com/jamiebuilds/react-loadable)

### react-dnd & react-dnd-html5-backend

元素拖拽  
[React DnD](https://react-dnd.github.io/react-dnd/about)

### react-hot-loader

不修改状态的前提下，局部刷新（与 webpack 的 HMR 不同），能提升开发构建速度。  
[react-hot-loader原理 - react等等我](https://sekin.gitbook.io/react/chapter1/react-hot-loaderyuan-li)

### classnames

条件表达式拼接元素 css `class` 样式  
[classnames - npm](https://www.npmjs.com/package/classnames)

### styled-components

`css in js` 方案之一  
[styled-components - npm](https://www.npmjs.com/package/styled-components)

# Reference

[《React 学习之道》](https://github.com/the-road-to-learn-react/the-road-to-learn-react-chinese)  
[尚硅谷课程笔记](https://github.com/xzlaptt/React)
