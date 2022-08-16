# 目录

- [[#正文|正文]]
	- [[#生命周期|生命周期]]
	- [[#动态参数|动态参数]]
- [[#参考文献|参考文献]]

创建日期：2022-02-19 00:34:20  
最后修改：2022-02-19 00:34:19  

#前端 #npm包 #vue

- - -
> Gratitude is not only the greatest of virtues, but the parent of all the others.  
>—<cite>Cicero</cite>

# 正文

## 生命周期

![[Pasted image 20220211003207.png]]

## 动态参数

指令绑定动态参数时要注意最好小写，因为模版会将其全部小写化，这样的话，当 data 中的动态参数是大写时，就绑定不到了。

```vue
<a v-on:[eventName]="doSomething"> ... </a> ❌
<a v-on:[eventname]="doSomething"> ... </a> ✅
data: {
	eventName: 'href',
}
```

## 插槽

Vue 提供一组插槽 API`<slot></slot>` 来实现内容分发，之间可以是任何模版或 HTML，也可以是其他组件。

### 后备内容（default）

提供默认值，在不提供任何插槽内容时以后备内容显示：

```html
submit-button: <button type="submit"> <slot>Submit</slot> </button>
```

```html
<submit-button></submit-button>
👇
<button type="submit"> Submit </button>
```

### 具名插槽（name）

当同时存在多个插槽时，`slot` 利用 attribute`name` 区分，不提供时 `name` 为 `default`，同时 `template` 元素上使用指令 `v-slot:` 指定分发到哪个插槽的 `name`。

```html
base-layout：
<div class="container">
	<header>
		<slot name="header"></slot>
	</header>
</div>
使用：
<base-layout>
	<template v-slot:header>
		<h1>Here might be a page title</h1>
	</template>
</base-layout>
👇
<div class="container">
	<header>
		<h1>Here might be a page title</h1>
	</header>
</div>
```

### 作用域插槽（插槽 prop）

**编译作用域** 决定父组件所在父级作用域与子模板所在子作用域不在同一作用域，想在父组件在插槽内容中使用子组件数据，通过在 `<slot>` 元素上绑定 attribute（**插槽 prop**），然后在父级作用域中，使用带值的 `v-slot` 来定义所有插槽 prop 的对象名字。

```html
current-user:
<span>
	<slot v-bind:user="user"> {{ user.lastName }} </slot>
</span>
使用：
<current-user>
	<template v-slot:default="slotProps">
		{{ slotProps.user.firstName }}
	</template>
</current-user>
```

### 独占默认插槽的简写

当组件 **只存在** 默认插槽时，组件本身可当作插槽模板来用，即 `v-slot` 指令可应用在标签上。

```html
<current-user v-slot="slotProps"> {{ slotProps.user.firstName }} </current-user>
```

### 插槽解构

插槽工作原理就是单参数函数，能够作为函数参数的 JavaScript 表达式都可作为 `v-slot` 的值。函数的参数可解构。

### 条件渲染

`v-if`：惰性渲染，只有条件为 truthy 时才会渲染；
`v-show`：不论条件真假都会渲染，更改 `display` 属性，始终存在于 `DOM` 中。
`v-if`：有切换开销，因为切换过程会使条件块内的事件监听器与子组件销毁与重建；
`v-show`：有初始渲染开销。

### 列表渲染

`v-for`

```html
0、item in items
1、(item,index) in items
2、item of items
3、value in obj
4、(value,name) in obj
5、n in 100 // 重复100次
```

注：`v-for` 与 `v-if` 同时使用时，前者优先级大，前者循环的情况下，再判断条件满足与否，满足条件的才会显示。

#### 数组更新检测

##### 变更方法（非替换

修改原数组本身的方法，如 `push、pop、shift、splicce` 等。

##### 非变更方法（替换方法

新数组替换原有数组，`slice、concat` 等不修改数组本身的方法。**所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。**

### 组件基础

1、vue 通过 `component` 元素 `is` 属性动态切换不同组件，属性值可以是 **已注册** 组件名字或组件的选项对象。

```html
<component v-bind:is="currentTab.component" class="tab"></component>
```

组件名：

```html
<component v-bind:is="currentTabComponent" class="tab"></component>
```

```js
Vue.component("tab-home", {

template: "<div>Home component</div>"

});
currentTab = 'Home'
currentTabComponent = "tab-" + currentTab.toLowerCase();
```

选项对象：

```js
currentTab = {
	name: "Home",
	component: {template: "<div>Home component</div>"}
}
```

2、property vs attribute  
property 其实还是 dom 对象的属性，随时可更改，值也会变，也不受限可扩展，初始值或许部分依赖于 html attribute 但不完全是这样，html attribute 值的范围是有限的，遵循 html 标准。`v-bind` 的 `.prop` 修饰符可指定“属性”作为一个 `DOM property` 绑定而不是作为 `HTML attribute` 绑定。
3、一些 html 元素如 `ul、table、form` 等天生不认自定义组件标签如 `<blog-info/>`，这时用其支持的标签，同时 `is` 指定自定义标签即可。当有如下情况时，当我没说：

- 字符串 (例如：`template: '…'`)
- [单文件组件 (`.vue`)](https://cn.vuejs.org/v2/guide/single-file-components.html)
- [`<script type="text/x-template">`](https://cn.vuejs.org/v2/guide/components-edge-cases.html#X-Templates)

### Prop

- 一次性传入一个对象所有属性作为组件 prop 的写法：v-bind=obj；
- vue 的 prop 为单向数据流不建议子组件修改，需要修改的两种场景：
	- 以初始值传入作为子组件本地数据使用；
	- 以初始值传入需经转换，作为子组件计算属性。
- prop 验证：数组和对象的默认值必须通过一个函数来返回；prop 验证在实例创建之前就开始了，因此选项里的 data、computed 等在 `default`、`validator`（自定义验证函数）中是不可用的；
- prop 合并/替换：多数情况 prop 会发生替换，class/style 特殊点，会合并 class="a"=>class="a b"；
- inheritAttrs:false 与 $attrs 打组合拳，实现更细粒度的 prop 传递（手动决定这些 attribute 会被赋予哪个元素）。

### 自定义事件

`.native` 修饰符可以直接监听子组件根元素上的原生方法。
`$listeners`property 提供作用在组件上的所有监听器，通 `v-on="$listeners"`，就可以使组件内所有元素获取所有事件监听器了。
v-bind.sync="doc"

#### 插槽

1、组件设计时留好槽位，以便在组件开始结束标签之间插入内容。
2、作用域插槽的目的是在父级作用域提供访问子级作用域的能力，这样就能在父组件访问子组件实例上的数据项了。用法：

```html
// 子组件 Child
<div>
	<slot :txt=txt>{{txt}}</slot>
</div>
// 父组件 Father
<div>
	<child v-slot='slotProps'>{{slotProps.txt}}+'-'+{{slotProps.txt}}</child> //独占默认插槽可直接写在组件标签上
</div>
// or
<div>
	<child>
		<template v-slot='slotProps'>{{slotProps.txt}}+'-'+{{slotProps.txt}}</template>
	</child> //独占默认插槽可直接写在组件标签上
</div>
// 插槽缩写 & 插槽解构
<div>
	<child>
		<template #default='{txt}'>{{txt}}+'-'+{{txt}}</template>
	</child>
</div>

```

## 参考文献

[[juejin-list#property vs attribute https juejin cn post 6844904114065768462|property vs attribute]]
