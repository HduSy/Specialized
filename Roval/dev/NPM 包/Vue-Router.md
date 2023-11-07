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

同级展示多个视图

# Reference

[Vue Router | Vue.js 的官方路由](https://router.vuejs.org/zh/)
