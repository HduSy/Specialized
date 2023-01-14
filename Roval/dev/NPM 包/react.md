Created Date：2022-12-17 22:19:24  
Last Modified：2022-12-17 22:19:23

# Tags

 #React

# Content

## 语言本身

- Fragments 避免添加额外 DOM 元素以防标签失效 ->[Fragment](https://zh-hans.reactjs.org/docs/fragments.html)

短写法：`<></>`，无属性可加；常规写法：`<React.Fragment key={}></React.Fragment>`

## Hooks

- useEffect 为函数式组件提供副效应，支持第二个参数填依赖项，条件执行  
[阮一峰 useEffect](https://www.ruanyifeng.com/blog/2020/09/react-hooks-useeffect-tutorial.html)
- 产生 memorization 函数，以空间换时间，缓存纯函数计算结果，只有指定依赖项发生变更时才重新计算结果。  
[CSDN useCallback](https://blog.csdn.net/milk_0126/article/details/103635225)
- more

## UI 呈现所需最小状态原则 - state

![[Pasted image 20230114165200.png]]  

说人话就是：

1. 随时间改变；
2. 非 prop 传递；
4. 可计算得来；

## 相关生态库

- react-loadable

# Reference

[《React 学习之道》](https://github.com/the-road-to-learn-react/the-road-to-learn-react-chinese)  
[尚硅谷课程笔记](https://github.com/xzlaptt/React)
