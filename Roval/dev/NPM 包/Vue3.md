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

#### 深层响应性

`ref` 使任何类型的值具有深层响应性，包括数组、对象、`js` 内置数据结构，其变化会被检测到。当 `ref` 值为非原始值时，`ref()` 内部调用 [`reactive()`](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html#reactive) 转换为 `JavaScript` 响应式代理，**拦截响应式对象所有属性的访问与修改，进行依赖追踪与触发更新**

#### DOM 更新时机

**非实时**的，`vue` 会在 `next tick` 更新周期中缓存所有状态的修改，保证不论做了多少次状态修改，组件只被渲染一次。

```js
import { nextTick } from 'vue'

async function increment() {
  count.value++
  await nextTick()
  // 现在 DOM 已经更新了
}
```

### reactive

`reactive()` 将深层地转换对象：当访问嵌套对象时，它们也会被 `reactive()` 包装。当 ref 的值是一个对象时，`ref()` 也会在内部调用它。

#### 响应式代理 Vs 原始对象

`reactive` 返回原始对象的 `proxy` 版本，始终访问声明对象的代理版本，才是 `vue` 响应式系统的最佳实践。

#### reactive() 局限性

1) **值类型要求**：仅对对象类型（对象、数组、`js` 内置数据结构 `Map、Set`）有效，对 `number、string、boolean` 等原始类型无效；

2) **不可替换整个对象**：`vue` 的响应式系统通过 **属性访问** 进行追踪，必须始终保持对响应式对象的引用。解构属性、属性赋值、属性传递都会失去响应式。

3) **解构赋值不友好**：从 `reactive` 声明的响应式对象解构赋值出来的变量将**失去响应式**  

由于存在限制，官方建议使用 `ref` 作为声明响应状态的主要 `API`

#### 额外解包细节

`ref` 作为 `reative` 对象属性时，会自动解包  

```js
const count = ref(0)
const state = reactive({
  count // 作为响应式对象属性值
})
console.log(state.count) // 0 自动解包
state.count = 1 // 自动解包
console.log(count.value) // 1
```

`ref` 作为响应式数组、原生集合类型（`Map、Set`）时，不会自动解包

```js
const books = reactive([ref('Vue 3 Guide')])
console.log(books[0].value) // 这里需要 .value，不会自动解包

const map = reactive(new Map([['count', ref(0)]]))
console.log(map.get('count').value) // 这里需要 .value，不会自动解包
```

### 计算属性

`computed()` 方法期望接收一个 `getter` 函数，返回值为一个**计算属性 ref**  

```js
import { computed } from 'vue'
const arr = ref([1,2,3])
const arrSum = computed(() => {
	const result = 0
	return arr.reduce((pre, cur) => pre + cur, 0)
})
// arrSum 为 ref 响应性值
```

基于响应式依赖更新

### 条件渲染

#### v-show VS v-if

[v-if-vs-v-show](https://cn.vuejs.org/guide/essentials/conditional.html#v-if-vs-v-show)  
1、`v-show` 不支持在 `template` 上使用；  
2、`v-if` “真实的”条件渲染，确保切换时，条件区块内的事件监听器和子组件被销毁与重建，`v-show` 只会切换 `css display` 属性；  
3、`v-if` 惰性渲染，初次渲染时只有为 true 才会被渲染，而 `v-show` 始终会被渲染始终出现在 `DOM` 中；

### 列表渲染

### 事件修饰符

[vue修饰符-东半球最详细的文档](https://segmentfault.com/a/1190000016786254)

`.stop` stopPropagation 阻止冒泡  
`.prevent` preventDefault 阻止默认事件  
`.self` 只有事件是从事件绑定元素**自身触发**时才会执行回调，不会执行冒泡传递的事件，也不会阻止冒泡事件的传递  
`.capture` 事件捕获阶段执行  
`.once` 只触发一次  
`.passive` 不阻止事件的默认行为，不用等待回调事件的完成  
`.native` 组件上绑定的事件触发，要用其修饰

### 按键修饰符

`@keyup.xxx`  

`.enter`  
`.tab`  
`.delete` (捕获“Delete”和“Backspace”两个按键)  
`.esc`  
`.space`  
`.up`  
`.down`  
`.left`  
`.right`  

`.exact` 修饰符：精确的按键组合才触发事件

### 鼠标按键修饰符

`.left`  
`.right`  
`.middle`

### 表单输入绑定

`v-model='xxx'` 针对不同表单项封装了属性和方法，可省略手动 `v-bind` 绑定属性和 `v-on` 绑定事件  

#### v-model 修饰符

`.lazy` 表单输入事件，`change` 事件后触发而不是 `input` 事件  
`.number` 表单输入值自动 `parseFloat` 转化，无法转化则返回原始值  
`.trim` 去除表单输入值前后空格

#### 组件 v-model

### 侦听器 watch & watchEffect

#### watch

第一个参数为侦听的依赖，第二个参数为回调，第三个参数为配置对象，支持配置**深层侦听与及时回调** `deep、immediate、flush`  
接受参数 `ref、getter func、前两者组成的数组`

```js
const x = ref(0)
const y = ref(0)
// 单个 ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})
// getter 函数
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)
// 多个来源组成的数组
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})
```

#### watchEffect

当侦听器的源与回调中使用的值是同一个时，可通过 `watchEffect` 简化写法

```js
const todoId = ref(1)
const data = ref(null)

watch(todoId, async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
}, { immediate: true })
```

简化为

```js
// 会立即执行
watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
})
```

#### 区别

追踪响应式依赖的方式：

- `watch` 只追踪**明确指定**侦听的数据源，它不会追踪任何在回调中访问到的东西，仅支持**单个依赖项**
- `watchEffect` 同步执行过程中自动追踪**所有访问**到的响应式属性，支持**多个依赖项、嵌套结构中属性**

#### 回调触发时机

组件更新之前

#### 侦听器的停止

建议同步创建，会自动停止；  
异步创建的，需手动调用，`unwatch`（创建后的一个返回值）

```js
const unwatch = watchEffect(() => {})
// …当该侦听器不再需要时
unwatch()
```

### 引用 ref

用在元素  
用在组件（选项式 `API` 与非 `script setup` 时，`ref` 会拿到组件实例相当于子组件 `this`）,`script setup` 组件默认是**私有的**，父组件无法通过 `ref` 访问到，除非另外调用 `defineExpose` 宏显式暴露

### 组件基础

#### defineProps 宏 - 自定义组件 props

```js
<script setup>
const props = defineProps(['foo'])
// 类型校验
defineProps({
  title: String,
  likes: Number
})
// ts校验
defineProps<{
  title?: string
  likes?: number
}>()
// props校验
defineProps({
  // 基础类型检查
  // （给出 `null` 和 `undefined` 值则会跳过任何类型检查）
  propA: Number,
  // 多种可能的类型
  propB: [String, Number],
  // 必传，且为 String 类型
  propC: {
    type: String,
    required: true
  },
  // Number 类型的默认值
  propD: {
    type: Number,
    default: 100
  },
  // 对象类型的默认值
  propE: {
    type: Object,
    // 对象或数组的默认值
    // 必须从一个工厂函数返回。
    // 该函数接收组件所接收到的原始 prop 作为参数。
    default(rawProps) {
      return { message: 'hello' }
    }
  },
  // 自定义类型校验函数
  propF: {
    validator(value) {
      // The value must match one of these strings
      return ['success', 'warning', 'danger'].includes(value)
    }
  },
  // 函数类型的默认值
  propG: {
    type: Function,
    // 不像对象或数组的默认，这不是一个
    // 工厂函数。这会是一个用来作为默认值的函数
    default() {
      return 'Default function'
    }
  }
})
</script>
```

细节补充：

- 所有 `prop` 默认都是可选的（默认 `require:false`），除非声明了 `required: true`
- 除 `Boolean` 外（默认 `false`）的未传递的可选 prop 将会有一个默认值 `undefined`
- `Boolean` 类型的未传递 prop 将被转换为 `false`。这可以通过为它设置 `default` 来更改——例如：设置为 `default: undefined` 将与非布尔类型的 prop 的行为保持一致
- 如果声明了 `default` 值，那么在 prop 的值被解析为 `undefined` 时，无论 prop 是未被传递还是显式指明的 `undefined`，都会改为 `default` 值

#### defineEmits 宏 - 自定义组件事件 emits

```js
// 传入数组
<script setup>
const emit = defineEmits(['inFocus', 'submit'])
function buttonClick() {
  emit('submit')
}
</script>
```

```js
// 传入对象，并做校验，返回true通过，返回false事件不合法
<script setup>
const emit = defineEmits({
  // 没有校验
  click: null,

  // 校验 submit 事件
  submit: ({ email, password }) => {
    if (email && password) {
      return true
    } else {
      console.warn('Invalid submit event payload!')
      return false
    }
  }
})

function submitForm(email, password) {
  emit('submit', { email, password })
}
</script>
```

#### slot 组件插槽

#### 动态组件 component

`<component :is='xxx'></component>`

`xxx` 可以是**组件名或导入的组件**

#### DOM 中模板解析

- 组件名、prop 名、方法名均转为 `kebab-case`
- 必须写闭合标签
- `select、table、ul、ol` 等元素对放置其中的元素类型有限制，相应的，某些元素仅在放置于特定元素中时才会显示

## 组件注册

和全局注册相比，局部注册有 `tree shaking` 且依赖关系明确

## props

### 绑定对象属性值为多个 prop

将对象所有属性都当作 props 传入，可以使用 `无参数的v-bind`

```js
const post = {
  id: 1,
  title: 'My Journey with Vue'
}
<BlogPost v-bind="post" />
// 相当于
<BlogPost :id="post.id" :title="post.title" />
```

### 单向数据流

传递给子组件的 `props` 是**只读**的，子组件可通过：  
**局部化为本地变量**：

```js
const props = defineProps(['initialCounter'])
// 计数器只是将 props.initialCounter 作为初始值
// 像下面这样做就使 prop 和后续更新无关了
const counter = ref(props.initialCounter)
```

利用**计算属性**，对 `props` 进行一次转换：

```js
const props = defineProps(['size'])
// 该 prop 变更时计算属性也会自动更新
const normalizedSize = computed(() => props.size.trim().toLowerCase())
```

## 组件上的 v-model 用法

默认情况下：

```html
<CustomInput v-model="searchText" />
/* to */
<CustomInput :model-value="searchText" @update:model-value="newVal => searchText=newVal" />
```

```vue
<CustomInput v-model="searchText" />
```

CustomInput.vue 实现

```vue
<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

### 自定义 prop 名

```vue
<!-- MyComponent.vue -->
<script setup>
defineProps(['title'])
defineEmits(['update:title'])
</script>

<template>
  <input
    type="text"
    :value="title"
    @input="$emit('update:title', $event.target.value)"
  />
</template>
```

### 自定义 modelModifiers 修饰符

`vue` 除了支持 `.lazy`、`.number`、`.trim`，还支持自定义 `v-model` 修饰符

```vue
<script setup>
const props = defineProps({
  modelValue: String,
  modelModifiers: { default: () => ({}) }
})

const emit = defineEmits(['update:modelValue'])

function emitValue(e) {
  let value = e.target.value
  if (props.modelModifiers.capitalize) {
    value = value.charAt(0).toUpperCase() + value.slice(1)
  }
  emit('update:modelValue', value)
}
</script>

<template>
  <input type="text" :value="modelValue" @input="emitValue" />
</template>
```

### 自定义 prop 加 modifiers

对于又有参数又有修饰符的 `v-model` 绑定，生成的 prop 名将是 `arg + "Modifiers"`

```html
<UserName
  v-model:first-name.capitalize="first"
  v-model:last-name.uppercase="last"
/>
```

```js
<script setup>
const props = defineProps({
  firstName: String,
  lastName: String,
  firstNameModifiers: { default: () => ({}) },
  lastNameModifiers: { default: () => ({}) }
})
defineEmits(['update:firstName', 'update:lastName'])

console.log(props.firstNameModifiers) // { capitalize: true }
console.log(props.lastNameModifiers) // { uppercase: true}
</script>
```

## attributes 透传

组件上没有被内部 `props、emits` 声明的 `attributes` 将会透传给组件（声明过的被组件自己“消费”了），如 `class style id`  

当组件以单个元素作为根元素编写时，透传的 `attributes` 将会自动加到根元素上；  
当组件以多个元素作为根元素编写时，透传的 `attributes` 必须显式绑定到某个根元素上；`v-bind="$attrs"`

### class、style 透传

会发生合并

### 事件透传

父、子事件将同时触发

### 禁用透传

`defineOptions` 宏，`inheritAttrs:false`，使用场景为不想让透传到根元素，而是想透传到其他元素上的时候，通过 `$attrs` 访问

### 实际用法

```js
// setup 写法
<script setup>
import { useAttrs } from 'vue'

const attrs = useAttrs()
</script>
// 非 setup 写法
export default {
  setup(props, ctx) {
    // 透传 attribute 被暴露为 ctx.attrs
    console.log(ctx.attrs)
  }
}
```

## 插槽

# Reference

[Vue.js官方](https://cn.vuejs.org/guide/introduction.html)  
[Vue3 入门指南与实战案例](https://vue3.chengpeiquan.com/)
