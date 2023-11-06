Created Date：2023-02-06 22:59:29  
Last Modified：2023-02-06 22:59:29

# Tags

#HTML

# Content

## DOMContentLoaded

### 触发时机

HTML 加载并解析完成，DOM 构建完成

```js
// 作用在 document 上
document.addEventListener("DOMContentLoaded", ready);
```

### 阻塞情况

- **受 \<script\> 标签脚本阻塞**，因为脚本可能修改 DOM，须停止 DOM 的构建；
- **不直接** 受样式、img、iframe 资源文件阻塞；
- **间接** 受样式文件阻塞，因为脚本可能获取元素坐标以及其他样式相关的属性，必须等待样式加载完毕；

## window.onload

### 触发时机

当整个页面，包括样式、图片和其他资源被加载完成时

## window.unload

### 触发时机

最终离开页面后

### 用途

不耗时操作，如发送分析数据 (也可用 `navigator.sendBeacon`)

## window.beforeunload

### 触发时机

离开页面前，询问是否离开

## document.readyState

### 值

- loading - 还在加载中
- interactive - 文档解析完成，与 DomContentLoaded 几乎同时，但在其之前
- complete - 文档和资源加载完成，与 load 几乎同时，但在 load 之前

## pageshow

- Initially loading the page
- Navigating to the page from another page in the same window or tab
- Restoring a frozen page on mobile OSes
- Returning to the page using the browser's forward or back buttons  

`pageshow` 和 `pagehide` 事件发生时，判断页面是否从浏览器缓存加载，还是从服务器请求加载

# Reference

[页面生命周期：DOMContentLoaded，load，beforeunload，unload](https://zh.javascript.info/onload-ondomcontentloaded)  
[Document: DOMContentLoaded event - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event)  
[Window: pageshow event - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/pageshow_event)
