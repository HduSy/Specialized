Created Date：2022-12-17 22:12:47  
Last Modified：2022-12-17 22:12:46

# Tags

#Npm包 #动画

# Content

## Fundamentals

## Plugins

### ScrollTrigger

创建随页面滚动**指定元素**产生的动画效果 [ScrollTrigger | GSAP | Docs & Learning](https://gsap.com/docs/v3/Plugins/ScrollTrigger)

#### scrollTrigger.trigger

`target` 进入视口时执行动画；

#### scrollTrigger.toggleActions

`${onEnter} ${onLeave} ${onEnterBack} ${onLeaveBack}` 值可以是 `"play", "pause", "resume", "reset", "restart", "complete", "reverse", and "none"`，分别控制进入视口、离开视口、再次进入、再次离开时的动画播放

#### scrollTrigger.start/end

`(el start) (viewport start)`/`(el end) (viewport end)` 值用来指定元素动画触发开始/结束时机

#### scrollTrigger.scrub

动画播放进程 `link` 到滚动条。`true` 无延时即刻，数字时代表 `duration`

#### scrollTrigger.pin

`sticky` 效果，`pin` 在某个位置

#### scrollTrigger.snap

指定进度完成剩余动画

#### scrollTrigger.onEnter/onLeave/onEnterBack/onLeaveBack callback

进入/退出动画播放区回调

#### scrollTrigger.onToggle callback

进入/退出动画播放区回调

#### scrollTrigger.toggleClass

进入/退出动画播放区切换类

### ScrollSmoother

创建顺滑的**页面**滚动动画 [ScrollSmoother | GSAP | Docs & Learning](https://gsap.com/docs/v3/Plugins/ScrollSmoother)

# Reference

[GSAP 中文教程 中文文档 ｜官方文档 官方教程翻译 ｜好奇代码出品](https://gsap.framer.wiki/)  
[官网Get Started](https://greensock.com/get-started/)  
[Easing | GSAP | Docs & Learning](https://gsap.com/docs/v3/Eases/)
