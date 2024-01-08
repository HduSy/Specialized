Created Date：2024-01-08 14:26:24  
Last Modified：2024-01-08 14:26:24

# Tags

#动画

# Content

## 摘要

> Lottie is a library for Android, iOS, Web, and Windows that parses [Adobe After Effects](http://www.adobe.com/products/aftereffects.html) animations exported as JSON with [Bodymovin](https://github.com/airbnb/lottie-web) and renders them natively on mobile and on the web!

`Adobe AE + Bodymovin` 输出 `.json` 动画文件，传给 `lottie-web`，返回实例暴露控制方法（`play`、`pause`、`stop` 等）

## Show Me The Code

```vue
<script lang="ts" setup>
import lottieWeb, { LottiePlayer } from 'lottie-web';
import { onMounted, ref } from 'vue';
import loading from '../assets/loading/loading.json';

const el = ref(null)
let animation: LottiePlayer
const played = ref(false)
const playorstopLottieAnime = () => {
  if(animation && !played.value) {
    animation.play()
    played.value = true
  } else {
    animation.pause()
    played.value = false
  }
}
onMounted(() => {
  animation = lottieWeb.loadAnimation({
    container: el.value,
    renderer: 'svg',
    loop: true,
    autoplay: false,
    animationData: loading
  })
})
</script>

<template>
  <div id="container" ref="el"></div>
  <div class="btn" @click="playorstopLottieAnime">点击切换Lottie动画播放状态</div>
</template>


<style scoped lang="scss">
#container {
  width: 200px;
  height: 200px;
  background-color: #06aeec;
}
.btn {
  padding: 10px;
  border-radius: 15px;
  border: 1px solid black;
  cursor: pointer;
  margin: 10px auto 0;
}
</style>
```

# Reference

[Airbnb动效工具Lottie](https://airbnb.io/lottie/#/web?id=html-player-installation)
