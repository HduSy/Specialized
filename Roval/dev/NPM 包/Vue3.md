Created Date：2022-12-17 22:26:41  
Last Modified：2022-12-17 22:26:40

# Tags

#Vue

# Content

## 简介

### 什么是 Vue

是一款用于构建用户界面的 `JavaScript` 框架，基于标准 `HTML、CSS、JavaScript` 构建，提供一套**声明式、组件化**的编程模型  
**核心功能**：

- 声明式渲染：基于 `HTML` 拓展了一套**模板语法**，**声明式**地描述最终输出的 `HTML` 与 `JavaScript` 状态之间的关系
- 响应性：自动跟踪 `JavaScript` 状态，变化时响应式更新 `DOM`

### API 风格

#### Options API

由多个选项**对象**来描述组件实例，选项所定义的属性会暴露在函数内部 `this` 上，指向组件实例

#### Composition API

导入（`import`）`API` 函数描述组件逻辑，`<script setup>` 标识会让 `Vue` 编译时进行一些处理方便 `API` 在组件内的使用

## 快速上手

## 基础

### 创建一个应用

#### 应用配置

应用实例 `app` 暴露 `.config` 对象，允许开发者配置一些应用级选项；  
暴露 `component` 方法，支持注册组件。

#### 多应用实例

### 模版语法

底层机制中，`Vue` 将模版转为高度优化的 `js` 代码，状态变化时，智能推导出需重新渲染的最少数量组件，并应用最少的 `DOM` 操作

#### 插值

`{{}}`

#### 指令

![[Pasted image 20231109224050.png]]

`v-attribute`：指令的任务是其表达式发生变化时更新 `DOM`  
`v-html`  
`v-bind` 简写 `:`  
`v-if`  
`v-show`  
`v-for`  
`v-on` 简写 `@`  
`v-slot`

##### 参数类型

元素属性：`<a :href="url"> … </a>` 表达式 `url` 的值绑定到元素的 `href` 属性上；  
事件名称：`<a @click="doSomething"> … </a>` 将 `click` 事件绑定到元素上

##### 参数也可以是动态的

`<a :[attributeName]="attributeName"> … </a>`  
`<a @[eventName]="eventName"> … </a>`  

动态参数**值限制**为字符串或 `null`，后者时表示移除绑定

##### 修饰符

告诉指令以特殊方式绑定，`.prevent`

### 响应式基础

#### 声明响应状态

##### ref

```js
import { ref } from 'vue'
let count = ref<number|string>(0) // 为refs标注类型
count.value = '0' // 成功!
```

如果 `ref` 属性不是插值表达式最终计算值，则只有**顶层声明的 ref**才会被正常解包，否则也能正常解包

```html
<script setup>
	import {ref} from 'vue'
	const count = ref(0)
	const obj = {id: ref(0)}
	const {id} = obj
</script>
<div>{{ count+1 }}</div> // 渲染正常 <div>1</div>
<div>{{ obj.id+1 }}</div> // 渲染不正常 <div>[Object Object]1</div>
<div>{{id}}</div> // 渲染正常
<div>{{obj.id}}</div> // 渲染正常
```

##### 为什么使用 ref

当你在模板中使用了一个 ref，然后改变了这个 ref 的值时，Vue 会自动检测到这个变化，并且相应地更新 DOM。这是通过一个**基于依赖追踪的响应式系统**实现的。当一个组件首次渲染时，Vue 会**追踪**在渲染过程中使用的每一个 ref。然后，当一个 ref 被修改时，它会**触发**追踪它的组件的一次**重新渲染**。

#### DOM 更新时机

更改响应式状态，DOM 会自动更新，但不是实时的，VUE 会缓冲直至下一次“更新时机”。

#### 响应式代理 Vs 原始对象

`reactive` 返回原始对象的 `proxy` 版本，始终访问声明对象的代理版本，才是 VUE 响应式系统的最佳实践。

#### reactive() 局限性

1) 仅对对象类型有效，对基础类型无效；

2) `vue` 的响应式系统通过 **属性访问** 进行追踪，必须始终保持对响应式对象的引用。解构属性、属性赋值、属性传递都会失去响应式。

#### Ref

创建任何类型值的响应式 `ref` 对象

##### Ref 在模板中的解包

ref 作为顶层属性被访问时，会被自动解包

### 计算属性

最佳实践一：不应有副作用  
最佳实践二：只读不写

### 条件渲染

#### V-show Vs V-if

1、不支持在 `template` 上使用；  
2、`v-if` 确保切换时，事件和子组件真实地被销毁与重建，`v-show` 只会切换 `css display` 属性；  
3、`v-if` 惰性渲染，初次渲染时只有为 true 才会被渲染，而 `v-show` 始终会被渲染；

# Reference

[Vue3 入门指南与实战案例](https://vue3.chengpeiquan.com/)
