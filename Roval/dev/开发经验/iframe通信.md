Created Date：2023-12-07 17:27:57  
Last Modified：2023-12-07 17:27:56

# Tags

[[../杂/技术博客与网站|技术博客与网站]] [[../杂/MDN|MDN]]

# Content

## message 事件

```ts
/**
* type: 消息类型
* origin: 消息发送者源 协议域名端口
* source: 发送消息窗口对象的引用，你可以使用此来在具有不同 origin 的两个窗口之间建立双向通信。
*/
window.addEventListener('message', e => {
    console.log(e.type)
    console.log(e.origin)
    console.log(e.source)
}, false)
```

```ts
/**
* message: 传递的数据
* targetOrigin: 目标源
* source: 消息发送者
*/
window.parent.postMessage(message, targetOrigin, transfer)
```

# Reference

[window.postMessage - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)
