Created Date：2023-12-19 11:32:03  
Last Modified：2023-12-19 11:32:02

# Tags

[[../dev/开发经验/React|React]] [[../dev/NPM/React|React]]

# Content

## 使用 useMemo 和 useCallback 的场景

[我们应该在什么场景下使用 useMemo 和 useCallback ？- 题目详情 - 前端面试题宝典](https://fe.ecool.fun/topic/f10c4b00-cceb-4323-97dc-55b315b05024?orderBy=updateTime&order=desc&tagId=13)

### Why

- 防止不必要的副作用 `effect`
- 防止不必要的重渲染 `re-render`；（易误解
- 防止不必要的重复计算；（易误解

### 额外 effect

将作为 `useEffect` 依赖的变量或函数进行缓存，可避免重复 `effect`

### 重复 re-render

#### 什么情况下会触发

- 自身 `state`、`prop` 变化的组件；
- 父组件重新渲染时，所有子组件；（常常被开发者忽略而导致大量无效缓存，**增加应用程序初始化耗时**
- `Context Value` 变化时，使用该值的组件；

#### 如何防止子组件 re-render

同时满足以下两个条件，子组件才能有效避免 `re-render`：

- **组件自身被缓存；**
- **所有 `prop` 被缓存；  

同时也带来了：

- 开发者工作量增加，组件一旦选择缓存，后续新增 `prop` 都要保证缓存，徒增工作量；
- 代码可读性和复杂性增加，代码中充斥着缓存逻辑；
- 性能成本，初始化耗时增加，对于大型复杂多组件应用程序更为明显；

#### 如何判断是否需要缓存

- 人肉判断，直观感受到存在渲染性能问题
- 工具
	- [\<Profiler> – React](https://react.dev/reference/react/Profiler)
	- [Huse - useRenderTimes](https://ecomfe.github.io/react-hooks/#/hook/debug/use-render-times)

### 额外计算

现实中用到的计算量倒还好，实际上是 `组件` 渲染占大头，组件渲染才是性能的瓶颈，应该把 `useMemo` 用在程序里渲染昂贵的组件上，而不是数值计算上

### 为什么 React 没有默认缓存组件

- 缓存有成本，小的成本累计变的不可控
- 默认缓存无法保证准确性（`Object.is()` 浅比较）[[../dev/杂/MDN|MDN]]

![[Pasted image 20231219120537.png]]

# Reference

[面试题列表 - 前端面试题宝典](https://fe.ecool.fun/topic-list)
