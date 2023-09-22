Created Date：2023-07-18 14:36:02  
Last Modified：2023-07-18 14:36:01

# Tags

#babel #前端工程化 #工程化

# Content

## 功能

1、语法转换；  
2、为目标环境添加 `polyfill` 提供不支持的功能；

## 使用方式

### @babel/cli 命令行

### 打包工具

`webpack`：`babel-loader` [[webpack]]  
`rollup`：`@rollup/plugin-babel` [[rollup]]  
`@rollup/register`  

无论使用哪种方式，`@babel/core` 和相应 `配置文件` 都是必须的

## 使用指南

### plugin

小型 `Javascript` 程序，指导 `Babel` 如何转换代码。

#### @babel/plugin-transform-runtime

移除辅助函数，将其替换为 `@babel/runtime/helpers` 中的函数引用

### presets

不用一个一个设置 `plugin` 而是同时设置一组组合。

#### @babel/preset-env

名为 `env` 的预设，指定支持的浏览器及版本范围，为目标浏览器没有的功能，进行语法转换。

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        "useBuiltIns": "usage"
      }
    ]
  ]
}
```

##### browserlist

[图形化显示浏览器支持范围](https://browsersl.ist/)

### polyfill

`7.4.0` 后不建议使用 `@babel/polyfill`，将在全局模拟完整的 ECMA2015+ 功能，造成污染。  
相应的，`@babel/preset-env` 的 `"useBuiltIns": "usage"` 配置，只对使用到的，目标环境缺失的功能进行代码转换和添加 `polyfill`

## core-js

- `JavaScript` 标准化库的 `polyfill`
- 模块化，按需引入
- 与 `babel` 高度集成，最大程度优化引入而非全量引入

# Reference

[Babel相关内容串联 | Congzhou's Blog](https://congzhou09.github.io/knowledge/Babel%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9%E4%B8%B2%E8%81%94.html)  
[前端工程化（7）：你所需要知道的最新的babel兼容性实现方案 - 掘金](https://juejin.cn/post/6976501655302832159)  
[一文搞懂 core-js@3、@babel/polyfill、@babel/runtime、@babel/runtime-corejs3 的作用与区别 - 掘金](https://juejin.cn/post/7062621128229355528)

[一文彻底读懂Babel - 谢小飞的博客](https://xieyufei.com/2020/11/18/Babel-Practice.html)

[一文搞懂Babel配置 - 掘金](https://juejin.cn/post/7116698494827495454)  
[babel配置指南 · Issue #16 · zyl1314/blog · GitHub](https://github.com/zyl1314/blog/issues/16)  

[深入浅出 Babel 上篇：架构和原理 + 实战 - 掘金](https://juejin.cn/post/6844903956905197576?searchId=20230919153112D7757C501CCE1BA2EF5F)
