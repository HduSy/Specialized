创建日期：2022-03-11 11:57:30  
最后修改：2022-03-11 11:57:30

- - -
> We shall never know all the good that a simple smile can do.  
>—<cite>Mother Teresa</cite>

# 正文

```js
const defaults = {  
  scale: 'x',  
}  
  
const mapApi = {  
  pc: {  
    x: pc_x,  
 y: pc_y,  
 xy: pc_xy,  
 },  
 h5: {  
    x: h5_x,  
 y: h5_y,  
 xy: h5_xy,  
 },  
}  
  
export default function resizeConfig(options={}) {  
  const myOptions = { ...defaults, ...options }  
  let { type, scale } = myOptions  
  
  type = type.toLowerCase() // 支持大小写  
 scale = scale.toLowerCase()  
  
  // window.addEventListener('resize', mapApi[type][scale])  
 mapApi[type][scale]() // 初始化时先执行一遍  
}  
  
// h5 对应h5的适配 宽度750 高度1624 按比例缩放 比例最大不超过1  
// pc 对应pc的适配 宽度1920 高度1080 按比例缩放 比例最大不超过1  
  
// x 按宽度为标准缩放  
function h5_x() {  
  const html = document.querySelector('html')  
  let width = html.clientWidth / 750  
  
 width = width > 1 ? 1 : width  
  
  html.style.fontSize=`${100 * width}px`  
}  
  
function pc_x() {  
  const html = document.querySelector('html')  
  let width = html.clientWidth / 1920  
  
 width = width > 1 ? 1 : width  
  
  html.style.fontSize=`${100 * width}px`  
}  
  
// y 按高度为标准缩放  
function h5_y() {  
  const html = document.querySelector('html')  
  let height = html.clientHeight / 1624  
  
 height = height > 1 ? 1 : height  
  
  html.style.fontSize=`${100 * height}px`  
}  
  
function pc_y() {  
  const html = document.querySelector('html')  
  let height = html.clientHeight / 1080  
  
 height = height > 1 ? 1 : height  
  
  html.style.fontSize=`${100 * height}px`  
}  
  
// xy 按两者比例最小值缩放  
function h5_xy() {  
  const html = document.querySelector('html')  
  const width = html.clientWidth / 750  
 const height = html.clientHeight / 1000  
  
 let scale = width < height ? width : height  
  scale = scale > 1 ? 1 : scale  
  
  html.style.fontSize=`${100 * scale}px`  
}  
  
function pc_xy() {  
  const html = document.querySelector('html')  
  const width = html.clientWidth / 1920  
 const height = html.clientHeight / 1080  
  
 let scale = width < height ? width : height  
  scale = scale > 1 ? 1 : scale  
  
  html.style.fontSize=`${100 * scale}px`  
}
```

```js
import resize from '@/common/js/resizeConfig'  
resize({ type: 'h5', scale: 'xy' })
```

# 参考文献
