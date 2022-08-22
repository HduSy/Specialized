创建日期：2022-05-07 20:48:34  
最后修改：2022-05-07 20:48:34

#前端 #npm包 #vue

- - -
> Things do not change; we change.  
>—<cite>Henry David Thoreau</cite>

# 正文

## 基础

### 响应式基础

#### reactive() 局限性

1. 仅对对象类型有效，对基础类型无效；
2. `vue` 的响应式系统通过 **属性访问** 进行追踪，必须始终保持对响应式对象的引用。解构属性、属性赋值、属性传递都会失去响应式。

# 参考文献

[Vue3 入门指南与实战案例](https://vue3.chengpeiquan.com/)
