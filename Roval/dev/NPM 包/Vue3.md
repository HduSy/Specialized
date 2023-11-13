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

### 作用域插槽

应用：封装逻辑，组合视图

## 依赖注入

```js
// keys.js
export const myInjectionKey = Symbol()
```

```js
<!-- 在供给方组件内 -->
<script setup>
import { provide, ref } from 'vue'
import { myInjectionKey } from './keys.js'
const location = ref('North Pole')
function updateLocation() {
  location.value = 'South Pole'
}
provide('myInjectionKey', {
  location,
  updateLocation
})
</script>
```

响应式的依赖，使得后代组件与提供者建立依赖关系

```js
<!-- 在注入方组件 -->
<script setup>
import { inject } from 'vue'
import { myInjectionKey } from './keys.js'
const { location, updateLocation } = inject(myInjectionKey)
</script>
<template>
  <button @click="updateLocation">{{ location }}</button>
</template>
```

使用 `symbol` 作注入的依赖名，可有效避免冲突

## 异步加载组件

```js
const AsyncComp = defineAsyncComponent({
  // 待加载组件
  loader: () => import('./Foo.vue'),
  // 异步组件加载出来前使用的组件
  loadingComponent: LoadingComponent,
  // 防止与待加载组件之间的切换闪烁，设置的延迟时间，默认为 200ms
  delay: 200,
  // 加载失败后展示的组件
  errorComponent: ErrorComponent,
  // 如果提供了一个 timeout 时间限制，并超时了
  // 也会显示这里配置的报错组件，默认值是：Infinity
  timeout: 3000
})
```

## 逻辑复用

### 组合式函数 composition-funcion

利用 `Vue composition-API`**封装和复用有状态逻辑**。灵感来源 `React Hooks`

```js
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'
// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)
  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }
  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))
  // 通过返回值暴露所管理的状态
  return { x, y }
}
```

组合式函数复用

```vue
<script setup>
import { useMouse } from './mouse.js'
const { x, y } = useMouse()
</script>
<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

一个组合式函数可以调用一个或多个其他的组合式函数。这使得我们可以像使用多个组件组合成整个应用一样，用多个较小且逻辑独立的单元来组合形成复杂的逻辑。

### 组合式函数 VS 无渲染组件

**无渲染组件**是指仅复用逻辑，通过作用域插槽将状态交给消费者组件去做视图渲染

组合函数**无组件实例、无组件嵌套**开销，更加高效。

### 组合式函数 VS Vue2-mixin

[混入mixin — Vue2.js](https://v2.cn.vuejs.org/v2/guide/mixins.html)  
`mixin` 短板：

- 数据来源不清晰
- 多个 `mixin` 命名空间冲突

### 自定义指令

### 插件 - 官方文档没怎么懂

## 内置组件

### Transition

触发条件：

- `v-if`
- `v-show`
- `component` 动态组件
- 特殊 `key` 改变

#### 基于 CSS 动画效果

`name prop` 指定动画名

1. `name-enter-from`：进入动画的起始状态。在元素插入之前添加，在元素插入完成后的下一帧移除。
2. `name-enter-active`：进入动画的生效状态。应用于整个进入动画阶段。在元素被插入之前添加，在过渡或动画完成之后移除。这个 class 可以被用来定义进入动画的持续时间、延迟与速度曲线类型。
3. `v-enter-to`：进入动画的结束状态。在元素插入完成后的下一帧被添加 (也就是 `v-enter-from` 被移除的同时)，在过渡或动画完成之后移除。
4. `v-leave-from`：离开动画的起始状态。在离开过渡效果被触发时立即添加，在一帧后被移除。
5. `v-leave-active`：离开动画的生效状态。应用于整个离开动画阶段。在离开过渡效果被触发时立即添加，在过渡或动画完成之后移除。这个 class 可以被用来定义离开动画的持续时间、延迟与速度曲线类型。
6. `v-leave-to`：离开动画的结束状态。在一个离开动画被触发后的下一帧被添加 (也就是 `v-leave-from` 被移除的同时)，在过渡或动画完成之后移除。

为 `Transiton` 组件 `props` 传入特定值，在 `Vue` 动画机制下，结合其他**第三方 CSS 动画库**如 `animate.css` 时，很有用  

`动画 prop` 指定动画类名

- `enter-from-class`
- `enter-active-class`
- `enter-to-class`
- `leave-from-class`
- `leave-active-class`
- `leave-to-class`

```html
<!-- 假设你已经在页面中引入了 Animate.css -->
<Transition
  name="custom-classes"
  enter-active-class="animate__animated animate__tada"
  leave-active-class="animate__animated animate__bounceOutRight"
>
  <p v-if="show">hello</p>
</Transition>
```

`duration prop` 指定过渡总时长  
`Transition` 支持 `css` 子选择器，控制嵌套子元素单独修改过渡效果，随之带来的问题是，`Transition` 是以根元素上的第一个 `transitionend` 或者 `animationend` 事件来自动判断过渡何时结束，而期望的行为应该是**等待**所有内容元素的过渡完成，因此 `duration prop` 指定过渡时间要与所有内容过渡时间一致 [Vue SFC Playground](https://play.vuejs.org/#eNqVVd9v0zAQ/leO8LAfrE3HNKSFbgKmSYMHQNAHkPLiOtfEm2NHttN2mvq/c7bTNi1jgFop9t13d9995ziPyfumGc5bTLJkbLkRjQOLrm2uciXqRhsHj2BwBiuYGV3DAUEPcpUrrpUlaKUXcOkBh860eJSrcRqzUDxtHNaNZA5pBzCets5pBe+4FPz+Mk+66Bf+mSdXE12WEsdphMWQiWHKCicorGgN8wsKPD8f5QkoViNtFFqHBcX7AAopxBzmAzHrChCQS2YtbXXr0GyAHXTtFEptnbnzv1uUUm/AKaHXNTbrcbplSIZx2uuYttY9SL8chtInMAxV4JGcbsr4fWl0q4oMXiLiW5+vYUUhVJnB2ahZBkst1KBCUVYug9NRtK68g7J22WgJsJOOc96DRYUGqIjDgHEn5khUOqtENsfOGom5TUcZMClhNDyzgMzigKhQIzFzegwFSvYAIQHoGXE3VAJQYu2fx+m29H4RgG2RQUiTUZnX5zbm3ufsj97Jfjane5lm2tRZXHr1fx56/Y6CgLphXDhfYJ2cqC8QOFPUntU9KhALrFuw0FoaxtpqycydNnantb6q3Xx/o7rj7eb1nyqHMl7lXYp/o7I/0X8U+0+NkObbvE9L/6MnfXocTqC7pbMJLBxOYHBdUQWEizcwbUsQCiqmCtkTuptYTyI7jC/epBIW6K+0IzAWBKZ47SpiNDV6YZGmo82mxMUrWJAPgRC+WsxSMQtTRAUzscQipPYy9o/KcDQ6jfLQhRZe4uQkcZZuvZkoh3dWK7owgwZ5wnXdCInmSxOo5kkW1fE+mqxefAo2f0GSoNHOK+T3T9jv7NLb8uSrQWpmjnmy8TlmSnTRffP9My5pvXHWumgloZ9xfkOrZes5RtgHGgfR7uEC24/h2qdxTOzN0iFNoGvKE/XIVcDnCX0Krp9pfUv3bHgW4kjPZPULQX004w==)

`appear prop` 使得组件初次渲染时有动画效果  
`mode prop` 过渡模式，官方推荐 `out-in`

#### 基于 JavaScript 动画

`Transition` 暴露了 8 个钩子，结合**第三方 JS 动画库**如 `GSAP`、`Animate.js`

#### 过渡可复用

做法：基于 `Transition` 开发组件，通过 `slot` 插槽传入其他组件

### TransitionGroup

用于对 `v-for` 列表中的元素或组件的**插入、移除和顺序改变**添加动画效果

#### VS Transition

相同点：`props`、`class name`、`js hook`  
不同点：

- 默认情况下，它不会渲染一个容器元素。但你可以通过传入 `tag` prop 来指定一个元素作为容器元素来渲染。
- [过渡模式 mode](https://cn.vuejs.org/guide/built-ins/transition.html#transition-modes) 在这里不可用，因为我们不再是在互斥的元素之间进行切换。
- 列表中的每个元素都**必须**有一个独一无二的 `key` attribute。
- CSS 过渡 class 会被应用在列表内的元素上，**而不是**容器元素上。

#### 渐进延迟列表动画

列表项元素添加 `data attributes`，`js hook` 中调用第三方 JS 动画库如 `GSAP` 实现不同项渐进延迟效果

### KeepAlive

动态组件缓存状态

### Teleport

`to prop`  
把组件内的指定模板，“传送”到 `DOM` 上指定位置，`to` 的值可以为 `CSS` 选择器字符串或 `DOM` 元素对象

### Suspense

实验性功能，暂时跳过

## 应用规范化

### SFC

#### why

- 使用熟悉的 HTML、CSS 和 JavaScript 语法编写模块化的组件
- [放便编写组件作用域的 CSS](https://cn.vuejs.org/api/sfc-css-features.html)
- [在使用组合式 API 时语法更简单](https://cn.vuejs.org/api/sfc-script-setup.html)
- 预编译模板，避免运行时的编译开销
- [让本来就强相关的关注点自然内聚](https://cn.vuejs.org/guide/scaling-up/sfc.html#what-about-separation-of-concerns)
- 通过交叉分析模板和逻辑代码能进行更多编译时优化
- [更好的 IDE 支持](https://cn.vuejs.org/guide/scaling-up/tooling.html#ide-support)，提供自动补全和对模板中表达式的类型检查
- 开箱即用的模块热更新 (HMR) 支持

#### how

`SFC` 将被编译为标准的 `ES6` 模块，方便 `import` 导入

#### what

**前端开发的关注点不是完全基于文件类型分离的，仅仅这样并不够帮我们在日益复杂的前端应用的背景下提高开发效率。前端工程化的最终目的都是为了能够更好地维护代码，提高开发效率**  

相较于分离相互交织，组件的逻辑、样式、模板三者耦合内聚，有利于可维护性

### 工具链

### 路由

前端路由：不需要重载整个页面来更新，更好的用户体验。利用 `history API` 拦截 `hash` 变化，渲染对应路由组件

### 状态管理

- **状态**：驱动整个应用的数据源；
- **视图**：对**状态**的一种声明式映射；
- **交互**：状态根据用户在**视图**中的输入而作出相应变更的可能方式。

[Pinia | The intuitive store for Vue.js](https://pinia.vuejs.org/zh/)

### 测试

#### 类型

- **单元测试**：检查给定函数、类或组合式函数的输入是否产生预期的输出或副作用。
- **组件测试**：检查你的组件是否正常挂载和渲染、是否可以与之互动，以及表现是否符合预期。这些测试比单元测试导入了更多的代码，更复杂，需要更多时间来执行。
- **端到端（E2E）测试**：检查跨越多个页面的功能，并对生产构建的 Vue 应用进行实际的网络请求。这些测试通常涉及到建立一个数据库或其他后端。  

单元测试：单元测试通常适用于独立的业务逻辑、组件、类、模块或函数，不涉及 UI 渲染、网络请求或其他环境问题  
组件测试：组件测试主要需要关心组件的公开接口而不是内部实现细节，测试这个组件做了什么，而不是测试它是怎么做到的  
E2E 测试：前端代码和静态服务器，还包括所有相关的后端服务和基础设施，主打一个生产环境，真实性使用

### 服务端渲染 SSR

#### what

服务端将组件渲染成 HTML，返回给浏览器，浏览器将静态 HTML 激活为可交互的客户端应用。

#### why

优点：

- **更好的 SEO**：搜索引擎爬虫可以直接看到完全渲染的页面
- **更快的首屏加载（js 加载和请求）**：服务端渲染的 HTML 无需等到所有的 `JavaScript` 都下载并执行完成之后才显示，数据获取过程在首次访问时在服务端完成，相比于从客户端获取，可能有更快的数据库连接
- **统一的心智模型**：你可以使用相同的语言以及相同的声明式、面向组件的心智模型来开发整个应用，而不需要在后端模板系统和前端框架之间来回切换。  
缺点：
- **开发中的限制**：浏览器端特定的代码只能在某些生命周期钩子中使用；一些外部库可能需要特殊处理才能在服务端渲染的应用中运行
- **更高的服务端负载**：在 Node.js 中渲染一个完整的应用要比仅仅托管静态文件更加占用 CPU 资源，因此如果你预期有高流量，请为相应的服务器负载做好准备，并采用合理的缓存策略
- **更多的与构建配置和部署相关的要求**：服务端渲染的应用需要一个能让 Node.js 服务器运行的环境，不像完全静态的 SPA 那样可以部署在任意的静态文件服务器上

#### SSR vs SSG

**静态站点生成** (Static-Site Generation，缩写为 SSG)，也被称为预渲染，是另一种流行的构建快速网站的技术。如果用服务端渲染一个页面所需的数据对每个用户来说都是相同的，那么我们可以**只渲染一次**，提前在构建过程中完成，而不是每次请求进来都重新渲染页面。**预渲染的页面生成后作为静态 HTML 文件被服务器托管**。

SSG 保留了和 SSR 应用相同的性能表现：它带来了优秀的首屏加载性能。同时，它比 SSR 应用的花销更小，也更容易部署，因为它输出的是静态 HTML 和资源文件。这里的关键词是**静态**：SSG 仅可以用于消费静态数据的页面，即数据在构建期间就是已知的，并且在多次部署期间不会改变。每当数据变化时，都需要重新部署。

如果你调研 SSR 只是为了优化为数不多的营销页面的 SEO (例如 `/`、`/about` 和 `/contact` 等)，那么你可能需要 SSG 而不是 SSR。SSG 也非常适合构建基于内容的网站，比如文档站点或者博客。事实上，你现在正在阅读的这个网站就是使用 [VitePress](https://vitepress.dev/) 静态生成的，它是一个由 Vue 驱动的静态站点生成器。

# Reference

[Vue.js官方](https://cn.vuejs.org/guide/introduction.html)  
[Vue3 入门指南与实战案例](https://vue3.chengpeiquan.com/)
