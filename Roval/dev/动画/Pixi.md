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

[Pixi官网教程中译](https://github.com/Zainking/LearningPixi)
