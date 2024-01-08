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

- B 站赛事抽奖组件，奖品轮播图、中奖用户信息轮播展示

## Code

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

# Reference

[性能优化之通俗易懂学习requestAnimationFrame和使用场景举例 - 掘金](https://juejin.cn/post/7190728064458817591)  
[requestAnimationFrame -- JavaScript 标准参考教程（alpha）](https://javascript.ruanyifeng.com/htmlapi/requestanimationframe.html)
