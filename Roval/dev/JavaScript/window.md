Created Date：2022-12-17 22:05:16  
Last Modified：2022-12-17 22:05:16

# Tags

# Content

1) 窗口关系  
`window.top`：指向最上层窗口，即浏览器窗口本身。  
`window.self`：window 终极属性，始终指向 window，`window.self === window` 为 `true`。  
`window.parent`：当前窗口的父窗口。

2) 窗口位置及像素比  
`window.moveTo(x,y)`：将窗口移动到相对于屏幕的一个绝对位置 `(x,y)`  
`window.moveBy(x,y)`：相对于当前位置进行移动  
`windos.screenTop`：返回值为 CSS 像素，窗口相对于屏幕顶部的位置。  
`windos.screenLeft`：返回值为 CSS 像素，窗口相对于屏幕左侧的位置。

3) 像素比  
首先知晓两个概念：

- 物理像素：物理设备实际像素。
- 逻辑像素：即 CSS 像素，浏览器报告的虚拟像素。  
`window.devicePixelRatio = 物理像素/逻辑像素 ` 对应屏幕密度（DPI, dots per inch）。

1) 窗口大小  
`window.outerWidth/window.outerHeight`：浏览器窗口自身宽高（即便在 `iframe` 中调用）。  
`window.innerWidth/window.innerHeight`：可视区宽高。  
`document.documentElement.clientWidth/clientHeight`：可视区宽高。  
`window.resizeTo(x,y)`：缩放窗口到 `x*y`。  
`window.resizeBy(x,y)`：窗口宽高缩放 `x`、`y` 的量。

2) 窗口位置  
`window.pageXoffset/scrollX/pageYoffset/scrollY`：衡量文档相对于视口滚动距离。  
`window.scroll(x,y)/scrollTo(x,y)/scrollBy(x,y)`：操纵文档滚动。接受一个 `ScrollTpOptions` 参数，通过 `behavior` 属性告诉浏览器是否平滑滚动。

```js
window.scrollTo({
	left: 100,
	top: 100,
	behavior: 'smooth', // auto 正常滚动
})
```

1) 导航与打开新窗口  
`window.open(url: 链接,target: 目标窗口,特性字符串，当前打开时是否替换当前页浏览记录)`，target 也可以是 `_self、_top、_parent、_blank`。

# Reference
