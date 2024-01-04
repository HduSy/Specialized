Created Date：2022-12-17 21:03:24  
Last Modified：2022-12-17 21:03:23

# Tags

#图形 #动画 [[../杂/MDN|MDN]]

# Content

## Property

### canvas size

#### width height 属性

The `width` attribute specifies the width of the `<canvas>` element, **in pixels**.  
The `height` attribute specifies the height of the `<canvas>` element, **in pixels**.

**画布尺寸**，默认 `300px 150px` 大小

#### css style 的 width height

**画板尺寸**，`css` 样式，可通过内联样式、内部样式表或外部样式表设置

#### 最佳实践

- **画板的优先级要高一些，画布与画板尺寸不一时，画布会进行同比缩放**，画布上已经绘制好的图形也会随之缩放，随之导致变形失真；
- 画布 `100*100`，画板 `200*200` 时，实际渲染尺寸为 `200*200`，**以画板为准**；
- 画布 `200*200`，画板 `100*100` 时，实际渲染尺寸为 `100*100`，**以画板为准**；

## API

### HTMLCanvasElement

#### getContext(contextType)

获取 `canvas` 绘制区域上下文

### CanvasRenderingContext2D

#### clearRect(x, y, width, height)

绘制一个矩形区域，将其内部像素透明度设为 0，来达到擦除一个矩形区域的目的

#### beginPath()

starts a new path by emptying the list of sub-paths

#### closePath()

add a straight line from the current point to the start of the current sub-path. 如果已闭合什么也不做

#### save()

入 `stack` 存放当前绘画设置的状态

#### restore()

出 `stack` 弹出一次绘画状态应用，来恢复最近一次 `save` 过的绘画状态！  
[codesandbox.io/p/devbox/canvas-save-restore-gp9j9h](https://codesandbox.io/p/devbox/canvas-save-restore-gp9j9h)

#### scale(x, y)

[CanvasRenderingContext2D: scale() method - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/scale)  
Adds a scaling transformation to the **canvas units** horizontally and/or vertically.**By default, one unit on the canvas is exactly one pixel.** A scaling transformation modifies this behavior. For instance, a scaling factor of **0.5 results in a unit size of 0.5 pixels**; shapes are thus **drawn at half the normal size**. Similarly, a scaling factor of **2.0 increases the unit size so that one unit becomes two pixels**; shapes are thus **drawn at twice the normal size**.

缩放的是 `canvas units:pixels` 的比例，改了 `1 unit` 所对应的 `pixels` 数

所以对于 `dpr=2、dpr=3` 的屏幕，为防止 `canvas` 变模糊，常做以下处理：

```js
const ctx = canvas.getContext('2d')
const dpr = window.devicePixelRatio || 1 // dpr=物理像素/逻辑像素
// 画布大小
canvas.width = Math.round(oldWidth * dpr);
canvas.height = Math.round(oldHeight * dpr);
// 画板大小
canvas.style.width = `${oldWidth}px`;
canvas.style.height = `${oldHeight}px`;
ctx.scale(dpr, dpr); // 改比例
```

##### scale 为负数时  

[【Canvas杂谈：第一季】scale · hongru/Canvas-Tattle · GitHub](https://github.com/hongru/Canvas-Tattle/issues/14)  
[MDN] 说法：A negative value flips pixels across the horizontal/vertical axis（镜像反转）

#### translate()

水平/垂直方向平移画板坐标系

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

[PixiJs学前篇（一）：Canvas基础【绘制篇】🔥🔥 - 掘金](https://juejin.cn/post/7161696893695688740)  

[CodePen上效果炸裂的Canvas动画合集](https://codepen.io/collection/nZQqEM/3/?cursor=ZD0wJm89MCZwPTEmdj00)  
[理解Canvas Context 的save() 和 restore() - 掘金](https://juejin.cn/post/6844903879599996942)  
[画布尺寸 - Visualization Guidebook](https://tsejx.github.io/visualization-guidebook/canvas/basic/scale)
