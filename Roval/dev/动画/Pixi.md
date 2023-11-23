Created Date：2022-12-17 21:05:49  
Last Modified：2022-12-17 21:05:48

# Tags

#动画

# Content

Pixi 的 `Application` 对象默认使用 `WebGL` 引擎渲染模式，如果强制使用 `Canvas` 引擎绘制，则须配置：

```ts
let app = new PIXI.Application({
    width: 256,         // default: 800
    height: 256,        // default: 600
    antialias: true,    // default: false
    transparent: false, // default: false
    resolution: 1       // default: 1
	forceCanvas: true, // default: false
  }
);
```

# Reference

[教你用PixiJs实现复杂动画 - 掘金](https://juejin.cn/post/6917849020341682189)  
[骨骼动画初体验 | fx-team](https://fx-team.github.io/2018/02/11/%E9%AA%A8%E9%AA%BC%E5%8A%A8%E7%94%BB%E5%88%9D%E4%BD%93%E9%AA%8C/)
