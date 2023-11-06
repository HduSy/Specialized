Created Date：2023-11-06 14:54:37  
Last Modified：2023-11-06 14:54:37

# Tags

#前端面试

# Content

## 如何指定插件执行顺序

[插件 API | Vite 官方中文文档](https://cn.vitejs.dev/guide/api-plugin.html#plugin-ordering)

## Vite 插件常见 Hook

[插件 API | Vite 官方中文文档](https://cn.vitejs.dev/guide/api-plugin.html#vite-specific-hooks)

### config(config, {mode, command})

修改或返回 `config`。第一个参数：用户原始配置；第二个参数：描述配置环境的变量。

### configResolved

读取并存储最终解析到的 `config`

### configServer

配置开发服务器，拿到开发服务器实例、注入 `middleware`

### transfromIndexHtml

转换 `HTML`

### handleHotUpdate

执行自定义 `HMR`

## Vite 比 Webpack 快在哪里

1. 不需要全量构建
2. `esbuild` 打包器快
3. 动态引入，按需加载，请求相应路由时才处理 `Vite load resolve transform parse`
4. 充分利用缓存，依赖强缓存，源码协商缓存，减少请求

## Vite 相比 Webpack 缺点

1. 首次启动时首屏加载
2. 热更新慢
3. 生态没有 `Webpack` 好，且仅支持 `ESM` 模块规范

# Reference

[面试必备：常见的webpack / Vite面试题汇总 - 掘金](https://juejin.cn/post/7207659644487893051)  
[谈谈你对vite的了解](https://blog.51cto.com/u_14627797/6316959)
