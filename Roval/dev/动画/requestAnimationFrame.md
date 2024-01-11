Created Date：2023-11-28 16:54:26  
Last Modified：2023-11-28 16:54:25

# Tags

[[../杂/MDN|MDN]]

# Content

## 简介

## 优势

1. 充分利用显示器的刷新机制，随显示器刷新频率而刷新，不会卡顿；
2. 离开页面，自动停止刷新，节省 GPU、CPU、电力⚡️；

## 动画原理

- 配合 css style 每一帧修改相关样式实现动画；
- 配合 canvas 修改对象相关属性；

## 应用案例

- **节流**；
- 赛事抽奖组件，奖品轮播图；
- 中奖用户信息轮播展示；
- 抽奖 9 宫格轮播动画速度控制；

## Show Me The Code

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>HTML + CSS</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <div class="wrapper">
      <div class="box">
        <div class="child" style="background-color: red">这</div>
        <div class="child" style="background-color: gold">里</div>
        <div class="child" style="background-color: yellowgreen">是</div>
        <div class="child" style="background-color: purple">三</div>
        <div class="child" style="background-color: brown">土</div>
        <div class="child" style="background-color: red">这</div>
        <div class="child" style="background-color: gold">里</div>
        <div class="child" style="background-color: yellowgreen">是</div>
        <div class="child" style="background-color: purple">三</div>
        <div class="child" style="background-color: brown">土</div>
      </div>
    </div>
    <script>
      const box = document.querySelector(".box");
      let anime = null;
      let positionX = 0;
      const animate = () => {
        if (anime) cancelAnimationFrame(anime);
        positionX -= 1;
        if (positionX < -(5 * 100 + 5 * 20)) positionX = 0;
        box.style.transform = `translateX(${positionX}px)`;
        anime = requestAnimationFrame(animate);
      };
      animate();
    </script>
  </body>
</html>
```

[codesandbox: requestAnimationFrame动画实践](https://codesandbox.io/p/sandbox/requestanimationframedong-hua-shi-jian-yhwrdr?file=%2Findex.html)  
[my-vite-app/src/components/9Lottery/index.vue at main · HduSy/my-vite-app · GitHub](https://github.com/HduSy/my-vite-app/blob/main/src/components/9Lottery/index.vue) 9 宫格抽奖降速  
[my-vite-app/src/components/MatchLottery.vue at main · HduSy/my-vite-app · GitHub](https://github.com/HduSy/my-vite-app/blob/main/src/components/MatchLottery.vue) 奖品轮播展示

### 节流

```js
const current = ref(0)
const containerScrollTop = () => {
  let now = Date.now()
  // 屏幕刷新频率大概率为60hz 16.6ms，保证渲染间隔在两次刷新周期之内
  if(now - current.value > 30) {
    console.log('computing!')
    current.value = Date.now()
    currentScollTop.value = containerRef.value?.scrollTop || containerRef.value?.scrollTop || 0
    updateVisiableItems()
  }
  requestAnimationFrame(containerScrollTop)
}
```

# Reference

[性能优化之通俗易懂学习requestAnimationFrame和使用场景举例 - 掘金](https://juejin.cn/post/7190728064458817591)  
[requestAnimationFrame -- JavaScript 标准参考教程（alpha）](https://javascript.ruanyifeng.com/htmlapi/requestanimationframe.html)
