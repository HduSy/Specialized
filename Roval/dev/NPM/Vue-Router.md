Created Date：2023-11-07 08:23:24  
Last Modified：2023-11-07 08:23:23

# Tags

#Npm包 [[NPM]]

# Content

## 基础

### 动态路由匹配

相同的组件会重复使用

### 路由匹配语法

#### 路由中的正则配置

- 更精准匹配
- 利用正则 `+、*` 将参数标记为可重复  
- 利用正则 `?` 将参数标记为可选

#### strict、sensitive 路由配置

`strict`：`/a` 而非 `/a/`  
`sensitive`：`/a` 而非 `/A`

### 嵌套路由

`children` 声明子路由

### 编程式导航

`Browser History API`

### 命名路由

优点：

- 无硬编码的 URL
- 防止 `path` 打错字
- `params` 自动编解码
- 绕过路径排序

### 命名视图

不止嵌套，想同级展示多个视图时，可声明多个 `router-view` 作为多个出口  

#### router-view key 属性

指定 `key` 属性值为 `$route.fullPath` 保证 `Vue` 组件复用时强制执行组件生命周期函数，执行顺序为：`beforeRouteUpdate -> created -> mounted`，从而重新拉取最新数据

### 重定向和别名

#### 相对重定向

### 路由组件传参

通过配置 `props` 为 `true`，`route.params` 将被设置为组件的 `props`，把 `$route` 与组件解耦

### 不同的历史记录模式

#### hash 模式

`createWebHashHistory`  
`URL` 上多了一个 `#` 字符  
从未发送到服务器，不需要在服务器上做任何特殊处理，不过，不利于 `SEO`

#### h5 模式

`createWebHistory`  
`URL` 看着正常些，需要服务器添加**回退路由**，在 `URL` 拿不到任何匹配的资源时，应返回 `index.html`

## 进阶

### 导航守卫

#### 全局前置守卫

`beforeEach` 返回值如下：  
`false`：取消导航；  
路由地址：跳转到另一个路由

### 组合式 API

#### useRoute、useRouter

#### onBeforeRouteLeave、onBeforeRouteUpdate

导航守卫 `Hook`

#### useLink、RouterLink

### 滚动行为

`scrollBehavior` 路由切换后滚动到顶部、滚动到指定元素、保持原先滚动位置、或者不滚动，回调函数返回 `ScrollToOptions` 对象

#### 延迟滚动

`setTimeout`

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({ left: 0, top: 0 })
      }, 500)
    })
  },
})
```

# Reference

[Vue Router | Vue.js 的官方路由](https://router.vuejs.org/zh/)  
[＜router-view :key=“key“/＞中key的作用 - CodeAntenna](https://codeantenna.com/a/U6lnGWo9ur)  
