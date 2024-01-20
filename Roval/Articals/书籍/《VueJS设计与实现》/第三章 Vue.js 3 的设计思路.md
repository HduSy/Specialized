Created Date：2024-01-03 17:59:56  
Last Modified：2024-01-03 17:59:56

# Tags

# Content

## 3.1 声明式的描述 UI

编写前端页面涉及到的内容：

- DOM 元素
- 属性
- 事件
- 元素的层级结构  

对于 Vue3 来说分别对应的解决方案：

- 使用与 HTML 标签 一致的方式来描述 DOM 元素（相同
- 使用与 HTML 标签 一致的方式来描述属性（相同
- 使用 `:` 或 `v-bind` 来描述动态绑定的属性（不同
- 使用 `@` 或 `v-on` 来描述事件（不同
- 使用与 HTML 标签一致的方式来描述层级结构（相同  

相比于声明式描述，还可以**使用 JavaScript 对象来描述 UI，更加灵活（虚拟 DOM）**

### Vue 组件里的渲染函数

```js
import { h } from 'vue'
export default {
  render() {
    return h('h1', { onClick: handler }) // 虚拟 DOM
  }
}
```

  一个组件要渲染的内容，是通过渲染函数来描述的，也就是上面代码中的 **render 函数**，`Vue.js` 会根据组件的 `render` 函数的返回值拿到虚拟 DOM，然后就可以把组件的内容渲染出来了，**h 函数就是一个辅助创建虚拟 `DOM` 的工具函数**，`h` 函数作用如下：

```js
render() {
  // 为了效果更加直观，这里没有使用 h 函数，而是直接采用了虚拟 DOM 对象
  // 下面的代码等价于：
  // return h('div', { id: 'foo', class: cls })
  return {
    tag: 'div',
    props: {
      id: 'foo',
      class: cls
    }
  }
}
```

## 3.2 初识渲染器

### 本节代码

```js
function renderer(vnode, container) {
  if (typeof vnode.tag === 'string') {
    // 说明 vnode 描述的是标签元素
    mountElement(vnode, container)
  } else if (typeof vnode.tag === 'object') {
    // 对象式组件
    mountComponent(type, vnode, container)
  } else if (typeof vnode.tag === 'function') {
	// 函数式组件
	mountComponent(type, vnode, container)
  }
}
// 挂载标签元素
function mountElement(vnode, container) {
  // 使用 vnode.tag 作为标签名称创建 DOM 元素
  const el = document.createElement(vnode.tag)
  // 遍历 vnode.props，将属性、事件添加到 DOM 元素
  for (const key in vnode.props) {
    if (/^on/.test(key)) {
      // 如果 key 以字符串 on 开头，说明它是事件
      el.addEventListener(
        key.substr(2).toLowerCase(), // 事件名称 onClick ---> click
        vnode.props[key] // 事件处理函数
      )
    } else {
      el.setAttribute(key, vnode.props[key])
    }
  }

  // 处理 children
  if (typeof vnode.children === 'string') {
    // 如果 children 是字符串，说明它是元素的文本子节点
    el.appendChild(document.createTextNode(vnode.children))
  } else if (Array.isArray(vnode.children)) {
    // 递归地调用 renderer 函数渲染子节点，使用当前元素 el 作为挂载点
    vnode.children.forEach(child => renderer(child, el))
  }

  // 将元素添加到挂载点下
  container.appendChild(el)
}
// 挂载组件
function mountComponent(type, vnode, container) {
  // 调用组件函数，获取组件要渲染的内容（虚拟 DOM）
  const subtree = type === 'function' ? vnode.tag() : vnode.tag.render()
  // 递归地调用 renderer 渲染 subtree
  renderer(subtree, container)
}
```

### 渲染器的作用

将虚拟 DOM 渲染为真实 DOM：  
![[Pasted image 20240120144916.png]]  

#### 工作原理

递归地遍历虚拟 DOM 对象，并**调用原生 DOM API 来完成真实 DOM 的创建**。渲染器的精髓在于后续的**更新**，它会通过 `Diff 算法` 找出变更点，并且只会更新需要更新的内容，**而非**重走一遍创建流程

## 3.3 组件本质

**组件就是一组 DOM 元素的封装**，可以用一个函数来代表组件，**组件的返回值也是虚拟 DOM，它代表组件要渲染的内容**

```js
// MyComponent 是一个返回虚拟DOM的函数
const MyComponent = function () {
  return {
    tag: 'div',
    props: {
      onClick: () => alert('hello')
    },
    children: 'click me'
  }
}
// MyComponent 是一个对象，存在一个渲染函数属性，返回虚拟DOM
const MyComponent = {
  render() {
    return {
      tag: 'div',
      props: {
        onClick: () => alert('hello')
      },
      children: 'click me'
    }
  }
}
```

## 3.4 模板的工作原理——编译器

编译器和渲染器一样，只是一段程序而已，不过它们的工作内容不同。编译器的作用其实就是**将模板编译为渲染函数**  
以 vue 组件为例：

```vue
// 模板，编译器处理的部分
<template>
  <div @click="handler">
    click me
  </div>
</template>

<script>
export default {
  data() {/* ... */},
  methods: {
    handler: () => {/* ... */}
  }
}
</script>
```

`template` 模板内容编译成渲染函数，添加到 `script` 标签块的**组件对象**上，最终在浏览器里运行的代码就是：

```js
export default {
  data() {
    return {

    }
  },
  methods: {
    handler: () => {}
  },
  render() {
    return h('div', { onClick: handler}, 'click me')
  }
}
```

无论是 `template` 写法还是 `render` 写法都属于==声明式描述 UI==，对于一个组件来说，它要渲染的内容最终都是通过**渲染函数**（由**编译器**处理模板内容生成）产生的，然后渲染器再把渲染函数返回的虚拟 DOM 渲染为真实 DOM，这就是模板的工作原理，也是 Vue.js 渲染页面的流程。

## 3.5 Vue.js 是各个模块组成的有机整体（学到了

### 渲染器与编译器之间存在信息交流

寻找并且只更新变化的内容是渲染器的作用之一，但它不善于“寻找”的过程。而编译器在编译阶段，分析动态内容，将信息提取出来交给渲染器，相互配合使得性能得到进一步提升，而它们之间交流的媒介就是虚拟 DOM

# Reference
