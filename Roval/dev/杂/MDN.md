Created Date：2022-12-17 21:18:02  
Last Modified：2022-12-17 21:18:02

# Tags

#CSS [[技术博客与网站]]

# Content

## CSS

[关系选择器 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Combinators)  
[:focus-within - CSS：层叠样式表 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus-within) 元素获取焦点时的伪类  
[display - CSS：层叠样式表 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display)  
[perspective() - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/perspective) 透视效果，应用在 `css` 视差滚动

## JavaScript

[window.postMessage - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage) on('message', ()=>{})  
[MessageEvent - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent) `message` 事件回调参数 `e`  
[event.stopImmediatePropagation - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopImmediatePropagation)  
[URLSearchParams - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)  
[Intersection Observer API - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Intersection_Observer_API)  
[Element.getBoundingClientRect() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect)  
[Window：resize 事件 - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/resize_event)  
[Window - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)  
[Document - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)  
[EventTarget.addEventListener() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)  
[Window：requestAnimationFrame() 方法 - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/window/requestAnimationFrame)  
[Document: visibilitychange event - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document/visibilitychange_event) 页面初始加载并不会执行 visibilitychange 回调  
[Element: scrollIntoView() method - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView) 滚动元素父容器使调用该方法的元素可见

## HTML

[tabindex - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/tabindex) 元素焦点  
[script type importmap - HTML | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap)

## 响应式图片

[响应式图片 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)  

针对设计问题&分辨率切换问题，srcset 设置几组图片及固有宽度，sizes 依媒体查询设置槽宽

### 响应检测顺序

1. 浏览器检测屏幕宽度
2. sizes 定槽宽
3. 根据 srcset 固有宽度取合适图片资源
4. 加载并显示

# Reference

[[../开发经验/front-end|front-end]]
