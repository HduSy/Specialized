1. Pixi的`Application`对象默认使用`WebGL`引擎渲染模式，如果强制使用`Canvas`引擎绘制，则须配置：
```ts
let app = new PIXI.Application({
    width: 256,         // default: 800
    height: 256,        // default: 600
    antialias: true,    // default: false
    transparent: false, // default: false
    resolution: 1       // default: 1

  }
);
```