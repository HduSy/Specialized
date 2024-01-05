Created Date：2024-01-05 10:48:47  
Last Modified：2024-01-05 10:48:47

# Tags

[[video iPhone低电量模式]]

# Content

## 春节许愿代码实践

```html
<video
      ref="video"
      src="https://activity.hdslb.com/blackboard/static/20220117/6a806a6a097a741ff3b5beac907646e3/hqduMu4NwC.mp4"
      @touchmove.stop.prevent
      :class="['ParticleVideo', {showOpacity: showVideoFrame}]"
      autoplay
      loop
      muted
      preload
      @timeupdate="onVideoProgress"
      poster="https://i0.hdslb.com/bfs/activity-plat/static/20220120/6a806a6a097a741ff3b5beac907646e3/16rt5g5qjb.png"
      webkit-playsinline="true"
      playsinline="true"
      x-webkit-airplay="disallow"
      x5-video-player-type="h5"
      x5-video-player-fullscreen="true"
      x5-video-orientation="portraint"
    ></video>
```

## 关键属性

### autoplay

#### 背景

因为页面加载完成后自动播放音频或带音频的视频对用户来说并不友好，在实现自动播放功能时需谨慎。以下两种自动播放方案受到浏览器策略影响：

```html
<!--autoplay 属性-->
<audio src="/music.mp3" autoplay></audio>
<video src="/video.mp4" autoplay></video>
```

```js
// play 方法
audioElement.play();
```

#### 浏览器限制条件

从用户角度来说，在用户未请求播放的情况下，网页或 App 打开时突然自发播放音频/视频，并不和谐、自然。因此浏览器通常只在以下条件下允许自动播放：

- 如果 `audio` 音量为 0 或者设置为 `muted`；
- 如果 `video` 没有音轨或被设置 `muted` 为静音，则不会阻止 `video` 自动播放；
- 用户与网站有手势交互，如点击、触摸、滑动、按下某个键等；
- 网站在白名单内，浏览器认为用户与媒体经常交互；
- 自动播放 [Permissions Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Permissions_Policy) 授予给 `iframe`；  

以上的处理仅作为指导，具体阻塞 `autoplay` 的情况又**因不同浏览器而异**。

#### 应对

当 `autoplay` 被浏览器阻塞时，我们的应对方案：

##### 1. autoplay attribute

这是最简单的方法。在满足：

- 页面允许自动播放；
- 页面加载后元素成功创建；
- 网络正常，已加载足够播放的视频资源；

条件时，会自动播放

##### 2. Detecting whether autoplay is allowed

适用于支持 [Navigator: getAutoplayPolicy() method - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getAutoplayPolicy) 的浏览器

```js
// type 检测（始终推荐）
if (navigator.getAutoplayPolicy("mediaelement") === "allowed") {
  // The video element will autoplay with audio.
} else if (navigator.getAutoplayPolicy("mediaelement") === "allowed-muted") {
  // Mute audio on video
  video.muted = true;
} else if (navigator.getAutoplayPolicy("mediaelement") === "disallowed") {
  // Set a default placeholder image.
  video.poster = "http://example.com/poster_image_url";
}
```

or

```js
if (navigator.getAutoplayPolicy(video) === "allowed") {
  // The video element will autoplay with audio.
} else if (navigator.getAutoplayPolicy(video) === "allowed-muted") {
  // Mute audio on video
  video.muted = true;
} else if (navigator.getAutoplayPolicy(video) === "disallowed") {
  // Set a default placeholder image.
  video.poster = "http://example.com/poster_image_url";
}
```

网站、网页或指定元素自动播放策略可能因用户交互发生改变，这个无从得知，因此始终推荐使用第一种类型检测法

##### 3. listen play event - 监听 play 事件

==【兼容性差】==：`Safari` 上用户和 `video` 标签交互后，触发 `play` 方法，`Chrome` 并不会触发 `play` 方法。  
对于不支持 `navigator.getAutoplayPolicy` 方法的浏览器

```js
const video = document.getElementById("video");
video.addEventListener("play", handleFirstPlay, false);

let hasPlayed = false;
function handleFirstPlay(event) {
  if (!hasPlayed) {
    hasPlayed = true;

    // Remove listener so this only gets called once.
    const vid = event.target;
    vid.removeEventListener("play", handleFirstPlay);

    // Start whatever you need to do after first playback has started
  }
}
```

#### call play() method - 执行 play 方法

```ad-tip
强烈建议通过`autoplay`属性达成自动播放目的，支持更广泛且浏览器有相关播放优化
```

##### 1. Playing video

```js
document.querySelector("video").play();
```

##### 2. Handling play() failures

`play` 方法返回一个 `Promise`，成功开始播放时状态变为 `resolved`，播放失败时状态变为 `rejected`

```js
let startPlayPromise = videoElem.play();

if (startPlayPromise !== undefined) { // undefined 检测兼容旧版本浏览器
  startPlayPromise
    .then(() => {
      // Start whatever you need to do only after playback
      // has begun.
    })
    .catch((error) => {
      if (error.name === "NotAllowedError") {
        showPlayButton(videoElem);
      } else {
        // Handle a load or playback error
      }
    });
}
```

用户与网页发生交互后才开始自动播放实现：

```js
let playAttempt = setInterval(() => {
  videoElem
    .play()
    .then(() => {
      clearInterval(playAttempt);
    })
    .catch((error) => {
      console.log("Unable to play the video, User has not interacted yet.");
    });
}, 3000);

```

#### Permision Policy

`self`: 同域名下；`*`: 不限制；`https://example.media`: 指定域；

```http
Permissions-Policy: autoplay=(self|*|https://example.media), fullscreen=(self)
```

```html
<iframe src="mediaplayer.html" allow="autoplay; fullscreen"> </iframe>
```

禁止

```http
Permissions-Policy: autoplay=()
```

```html
<iframe src="mediaplayer.html" allow="autoplay 'none'"> </iframe>
```

### preload

### playsinline

## X5 内核

```ad-tip
X5 是腾讯基于 Webkit 开发的**浏览器内核**，iOS 端微信、QQ WebView 内核为 Webkit，Android 端为 X5
```

很多 case 在 iOS 端微信/QQ 已经解决，因此针对 Android 端需要 X5 属性解决

### x5-video-player-type

> 早期无论是在 iOS 还是 Android 的浏览器中，它都位于页面的最顶层，无法被遮挡。后来，这个问题在 iOS 下得到了解决。但是对 Android 的大部分浏览器来说，问题仍然存在。

X5 内核提供了一种名叫「**同层播放器**」的特殊 video 元素以解决遮挡问题。

### x5-video-player-fullscreen

### x5-video-orientation

## 关键事件

### timeupdate

## 真实实践

```vue
<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { base64toBlob } from '../utils';

const kvLink = 'https://activity.hdslb.com/blackboard/static/20220117/6a806a6a097a741ff3b5beac907646e3/hqduMu4NwC.mp4'
const kvLocal = '/src/assets/video/KV.f.mp4'
const poster = '/src/assets/p5.jpg'
const poster2 = '/src/assets/16rt5g5qjb.png'

const videoRef = ref<HTMLVideoElement>()
const played = ref(false)
let showVideoFrame = ref(false)

const onVideoProgress = (e: Event) => {
  const target = e.target as HTMLVideoElement
  if(target.currentTime > 0.1) {
    console.log('播放进度:', target.currentTime)
    // showVideoFrame.value = true
  }
  if (target.duration - target.currentTime < 0.05) {
    console.log('timeupdate: 播放结束')
  }
}
// 监听play事件（浏览器间处理不太兼容
const videoPlayEventDetect = () => {
  // Chrome无play事件触发，Safari上用户触摸video交互后触发play事件执行回调
  const videoEl = videoRef.value
  const handleFirstPlay = (e: Event) => {
      console.log('Event: play')
      if(!played.value) {
        played.value = true
        showVideoFrame.value = true
        console.log('Event: 正常播放')
        videoEl.removeEventListener('play', handleFirstPlay)
      } else {
        console.log('Event: 未正常播放')
        videoEl.muted = true
        videoEl.poster = poster
      }
    }
    videoEl.addEventListener('play', handleFirstPlay, false)
}
// 执行play方法（浏览器间兼容良好
const videoPromiseDetect = () => {
  // 调用video play()方法返回一个Promise
  const videoEl = videoRef.value
  let playPromise = videoEl?.play()
  if(playPromise!==undefined) {
    playPromise.then(() => {
      console.log('Promise: 正常播放')
      showVideoFrame.value = true
    }).catch((err) => {
      console.error('Promise: 未正常播放\n', err.message)
      videoEl.poster = poster2
      videoEl.muted = true
      videoPlayEventDetect()
    })
  }
}
// 事件&浏览器支持度
const detectVideoAutoPlay = () => {
  const videoEl = videoRef.value
  // caniuse支持度差，几乎都不支持，只有firefox支持
  if(navigator.getAutoplayPolicy) {
    // type 检测（始终推荐）
    if (navigator.getAutoplayPolicy("mediaelement") === "allowed") {
      console.log('The video element will autoplay with audio.')
      showVideoFrame.value = true
    } else if (navigator.getAutoplayPolicy("mediaelement") === "allowed-muted") {
      console.log('Mute audio on video')
      videoEl.muted = true;
    } else if (navigator.getAutoplayPolicy("mediaelement") === "disallowed") {
      // Set a default placeholder image.
      videoEl.poster = poster;
    }
  } else if(videoEl) {
    console.log('不支持 navigator.getAutoplayPolicy 检测法')
    videoPromiseDetect()
    // videoPlayEventDetect()
  }
}

const kvBlob = ref('')
const loadVideoSource =() => {
  // 体积还是太大
  // 加载视频 不可放在mounted里面
  import('../assets/video/video-to-b64/video').then(data => {
    const videoB64Data = data.videoB64Data
    const blob = base64toBlob(videoB64Data, 'video/mp4')
    const blobUrl = URL.createObjectURL(blob)
    kvBlob.value = blobUrl
  }).catch(e=>console.error('base64 视频加载失败'))
}
onMounted(()=>{
  detectVideoAutoPlay()
  // loadVideoSource()
})
</script>

<template>
  <!-- muted -->
  <video
    width="375"
    :class="['ParticleVideo', {showOpacity: showVideoFrame}]"
    ref="videoRef"
    :src="kvLink"
    @touchmove.stop.prevent
    autoplay
    loop
    preload="auto"
    @timeupdate="onVideoProgress"
    :poster="poster2"
    playsinline="true"
    webkit-playsinline="true"
    x-webkit-airplay="disallowed"
    x5-video-player-type="h5"
    x5-video-orientation="portraint"
    x5-video-player-fullscreen="true"
></video>
</template>

<style lang="scss">
.ParticleVideo {
  object-fit: cover;
  //width: 7.5rem;
  //height: 13.14rem;
  //width: 100vw;
  //height: 100%;
  //position: relative;
  //transform: translateY(1185px - 1314px);
  position: fixed;
  left: 0;
  top: 0;
  opacity: 0;
  //transform: translateY(-160px);
  &.showOpacity {
    opacity: 1;
  }
}
*::-webkit-media-controls-panel {
  display: none!important;
  opacity: 0;
  -webkit-appearance: none;
}

/* Old shadow dom for play button */

*::-webkit-media-controls-play-button {
  display: none!important;
  opacity: 0;
  -webkit-appearance: none;
}

/* New shadow dom for play button */

/* This one works! */

*::-webkit-media-controls-start-playback-button {
  display: none!important;
  opacity: 0;
  -webkit-appearance: none;
}
</style>
```

## 总结

# Reference

[Autoplay guide for media and Web Audio APIs - Web media technologies | MDN](https://developer.mozilla.org/en-US/docs/Web/Media/Autoplay_guide) 🌟🌟🌟  
[视频H5 video标签最佳实践 SegmentFault 思否](https://segmentfault.com/a/1190000009395289) 🌟🌟🌟  
[X5同层播放器应用实践 | 前端开发 | Luo's Blog](https://mrluo.life/article/detail/136/x5-video-practice) 🌟🌟🌟  
[探索“自闭”标签之video](https://mp.weixin.qq.com/s/Yi6yITCAA4UR_8tjF-WjOw)  
[玩转前端 Video 播放器](https://mp.weixin.qq.com/s/IBBB2o2imnRSnsv7QUBBgg)  
[大前端播放器 VideoX 如何回应业务诉求](https://mp.weixin.qq.com/s/h0TZXtJBoLf1ynFDDHo6Nw)
