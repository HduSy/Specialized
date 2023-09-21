Created Date：2023-09-20 08:52:52  
Last Modified：2023-09-20 08:52:52

# Tags

# Content

## 简介

代码模块 `ESM` 打包工具，将小的代码片段打包成更大、更复杂的代码，如库或应用程序。

## 优点

输出结果扁平；  
`tree-shaking`；  
代码可读；

## 缺点

加载 `非ESM` 第三方模块时比较复杂，需要配置一堆插件；  
模块最终打包到一个函数中，不支持 `HMR`；  
浏览器中，代码拆分功能依赖 `AMD`；

## webpack vs rollup

开发复杂前端应用程序：引入第三方模块 +HMR 功能提高开发效率 + 拆包  
开发类库：优点

新版本的 `webpack` 的功能几乎覆盖包含了 `rollup` 的优势点。

## parcel

# Reference

[「前端工程化」之 Rollup 上手与基本原理\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1w84y1z77V/?vd_source=032760beb957fcfec470635ca2ed9cef)
