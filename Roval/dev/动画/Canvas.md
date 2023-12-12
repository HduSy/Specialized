Created Date：2022-12-17 21:03:24  
Last Modified：2022-12-17 21:03:23

# Tags

#图形 #动画 [[../杂/MDN|MDN]]

# Content

## Property

### width height

**画布尺寸**，默认 `300px 150px` 大小

### style.width height

**画板尺寸**，`css` 样式，可通过内联样式、内部样式表或外部样式表设置

#### 两者不一致时

存在一个问题：渲染时，画布要通过缩放来使其与画板尺寸一样，画布上已经绘制好的图形也会随之缩放，随之导致变形失真

## API

### HTMLCanvasElement: getContext(contextType)

获取 `canvas` 绘制区域上下文

### CanvasRenderingContext2D.clearRect(x, y, width, height)

绘制一个矩形区域，将其内部像素透明度设为 0，来达到擦除一个矩形区域的目的

### CanvasRenderingContext2D.scale(x, y)

缩放 `canvas` 元素大小

## Demo

### 仪表盘

[codesandbox.io/p/sandbox/canvasshi-xian-lei-si-zhi-fu-bao-xin-yong-ji-fen-forked-zf5qpz?file=%2Findex.html](https://codesandbox.io/p/sandbox/canvasshi-xian-lei-si-zhi-fu-bao-xin-yong-ji-fen-forked-zf5qpz?file=%2Findex.html)

## 实践

### 高清分辨率

上面说过，避免图形变形失真，要保持画布尺寸和画板尺寸一致。  
这只是针对分辨率不高的设备而言，其 `window.devicePixelRatio` 为 1。而高分辨率屏幕，它的 `window.devicePixelRatio` 大于 1。  
Canvas 绘制的图形是位图，即 **栅格图像** 或 **点阵图像**，当将它渲染到高清屏时，会被放大，每个像素点会用 `window.devicePixelRatio` 平方个物理像素点来渲染，因此图片会变得模糊。  

**解决方法：**

1. 通过 `window.devicePixelRatio` 获取当前设备屏幕的 `DPR`
2. 获取或设置 `Canvas` 容器的画板尺寸（`width height`）根据 `DPR` 重新设置 `width*dpr height*dpr`
4. 通过 `context.scale(dpr, dpr)` 缩放 `Canvas` 画布的**坐标系**，在 `DPR` 为 2 时相当于把 `Canvas` 坐标系也扩大了两倍，这样绘制比例放大了两倍，之后 `Canvas` 的实际绘制像素就可以按原先的像素值处理

```js
const dpr = window.devicePixelRatio || 1;
canvas.width = dpr * width;
canvas.height = dpr * height;
ctx.scale(dpr, dpr);
```

# Reference

[CodePen上效果炸裂的Canvas动画合集](https://codepen.io/collection/nZQqEM/3/?cursor=ZD0wJm89MCZwPTEmdj00)  

[画布尺寸 - Visualization Guidebook](https://tsejx.github.io/visualization-guidebook/canvas/basic/scale)
