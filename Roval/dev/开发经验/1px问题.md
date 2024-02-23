Created Date：2024-02-22 17:15:22  
Last Modified：2024-02-22 17:15:22

# Tags

# Content

`dpr > 1` 的设备 `1px` 看起来不止 `1px` 比较粗

## 解决

### 伪元素

```css
.hairline{
  position: relative;
  &::after{
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 1px;
    width: 100%;
    transform: scaleY(0.5);
    transform-origin: 0 0;
    background-color: #EDEDED;
  }
}
```

### js 改 viewport

```js
let viewport = document.querySelector('meta[name=viewport]')
//下面是根据设备像素设置viewport
if (window.devicePixelRatio == 1) {
    viewport.setAttribute('content', 'width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no')
}
if (window.devicePixelRatio == 2) {
    viewport.setAttribute('content', 'width=device-width,initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no')
}
if (window.devicePixelRatio == 3) {
    viewport.setAttribute('content', 'width=device-width,initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no')
}
function resize() {
    let width = screen.width > 750 ? '75px' : screen.width / 10 + 'px'
    document.getElementsByTagName('html')[0].style.fontSize = width
}
window.onresize = resize
```

# Reference

[[移动端适配]]
