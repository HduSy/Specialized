Created Date：2023-11-23 11:13:18  
Last Modified：2023-11-23 11:13:17

# Tags

# Content

跨平台开源动画格式，文件以 `.svga` 为后缀，同时兼容 `ios android web flutter`

## 优势

### 跨平台性

轻松在 `ios android web flutter` 中引入 `svga` 播放器并播放资源文件  

### 性能优化

动画文件体积小，播放资源占用更优，动画还原效果好

### 合作效率

动画设计师专注于动画设计，通过工具输出 `.svga` 文件给到开发者，开发者集成 `svga player` 直接在代码中使用。大大减少动画交互沟通成本，提升开发效率

## 代码实践

直播魔法奇遇活动：`live-live/nuwa/packages/magical_adventure/src/views/full/FullRoot.vue`

```js
import SVGA from 'svgaplayerweb'

// svga 动画文件
const svgaMap = {
  success:
    'https://i0.hdslb.com/bfs/activity-plat/static/20230313/914c3fa24b42b66ec286f08e3d89a3b7/reZDqJfiTw.svga',
  normal:
    'https://i0.hdslb.com/bfs/activity-plat/static/20230313/914c3fa24b42b66ec286f08e3d89a3b7/4klbkrH7qP.svga'
}
const player = new SVGA.Player('#animate-canvas') // 动画播放相关
player.loops = 1
player.setContentMode('AspectFill')
player.onFinished(() => {
	// 动画播放完成回调
})

const parser = new SVGA.Parser() // 动画加载解析
parser.load(
    isSuccess ? svgaMap.success : svgaMap.normal,
    (videoItem) => {
      player.setVideoItem(videoItem)
      player.startAnimation()
    }
)
```

# Reference

[SVGA官网](https://svga.io/index.html)  
[GitHub - svga/SVGAPlayer-Web-Lite: Lightweight SVGA Web Player](https://github.com/svga/SVGAPlayer-Web-Lite)  
[GitHub - svga/SVGAPlayer-Web](https://github.com/svga/SVGAPlayer-Web)
