Created Date：2023-12-07 17:27:57  
Last Modified：2023-12-07 17:27:56

# Tags

[[../杂/技术博客与网站|技术博客与网站]] [[../杂/MDN|MDN]]

# Content

## iframe element

### property

#### src

绝对路径或相对路径。`React` 中 `src` 可直接指定一个 `Router path` 直接渲染相应 `component`

```tsx
import { FunctionComponent } from 'react'
// iframe src 指定为react-route-dom路由，是完全ok的
const PreviewPage: FunctionComponent = () => {
  // 渲染 /preview 对应 component: <Preview/>
  return <iframe src='/preview'></iframe>
}
export default PreviewPage
```

```tsx
<Routes>
	<Route path='/preview' element={<Preview />}></Route>
</Routes>
```

#### contentDocument

`iframe document`

#### contentWindow

`iframe window`

#### loading

`lazy load or eagar`

#### 略…

### events

```js
// For a new iframe
const iframe = document.createElement("iframe");

iframe.onload = function() {
  console.log("The iframe is loaded");
};
iframe.onerror = function() {
  console.log("Something wrong happened");
};

iframe.src = "https://logrocket.com/";
document.body.appendChild(iframe);

// For an existing iframe
const iframe = document.querySelector('.my-iframe');

iframe.onload = function() {
  console.log("The iframe is loaded");
}
iframe.onerror = function() {
  console.log("Something wrong happened");
}
```

## iframe communication

### 父传子

```js
// 发送
const myiframe = document.getElementById('myIframe')
myIframe.contentWindow.postMessage('message', '*');
// 接收
window.onmessage = function(event){
    if (event.data == 'message') {
        console('Message received!');
    }
};
```

### 子传父

```js
// 发送
window.top.postMessage('reply', '*')
// 接收
window.onmessage = function(event){
    if (event.data == 'reply') {
        console('Reply received!');
    }
};
```

### `message` 事件

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
[iframe | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)
