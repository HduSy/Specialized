创建日期：2022-05-07 20:48:34  
最后修改：2022-05-07 20:48:34

#前端 #npm包 #vue

- - -
> Things do not change; we change.  
>—<cite>Henry David Thoreau</cite>

# 正文

## 基础

### 响应式基础

#### DOM 更新时机

更改响应式状态，DOM 会自动更新，但不是实时的，VUE 会缓冲直至下一次“更新时机”。

#### 响应式代理 Vs 原始对象

`reactive` 返回原始对象的 `proxy` 版本，始终shi yong

#### reactive() 局限性

1. 仅对对象类型有效，对基础类型无效；
2. `vue` 的响应式系统通过 **属性访问** 进行追踪，必须始终保持对响应式对象的引用。解构属性、属性赋值、属性传递都会失去响应式。

# 参考文献

[Vue3 入门指南与实战案例](https://vue3.chengpeiquan.com/)
