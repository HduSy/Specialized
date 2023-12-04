# Func

一些封装好的函数

## JS 判断字符串长度

```js
// 中文字符占2字节
function strlen(str){
    var len = 0;
    for (var i=0; i<str.length; i++) { 
     var c = str.charCodeAt(i); 
    // 单字节加1 
     if ((c >= 0x0001 && c <= 0x007e) || (0xff60<=c && c<=0xff9f)) { 
       len++; 
     } else { 
      len+=2; 
     } 
    } 
    return len;
}
```

## 大数加单位

利用 Math 方法给大数加单位

```ts
export function magnitude(value: number) {
  const param = { value: 0, unit: '' };
  const k = 10000;
  const sizes = ['', '万', '亿', '万亿'];
  let i;
  let result;
  if (value < k) { // 小于1w不额外加单位
    param.value = value;
    param.unit = '';
  } else {
    i = Math.floor(Math.log(value) / Math.log(k)); // 确定单位：对数运算法则之换底公式，相当于以k为低value求对数
    result = value / Math.pow(k, i); // 根据i求结果
    param.value = result.toFixed(2); // 只保留两位小数
    param.unit = sizes[i];
  }
  return param.value + param.unit;
}
```

## 文件导出实现

```ts
// 文件导出a标签实现
export const exportFile = (data: any) => {
  if (!data) return;
  data = data.replace('http', 'https');
  const link = document.createElement('a');
  link.style.display = 'none';
  link.href = data;
  document.body.appendChild(link);
  link.click();
};
```

## 拷贝至剪切板

```ts
import { message } from 'antd';
let bodyInput: any;
/**
 * copy到剪切板函数
 * @param src 要拷贝的值
 */
export default function (src: string) {
  if (!bodyInput) {
    const input = document.createElement('textarea');
    input.value = src;
    input.style.opacity = '0'; // 隐藏
    document.body.appendChild(input);
    bodyInput = input;
  } else {
    bodyInput.style.display = 'block'; // 显示
    bodyInput.value = src;
  }

  bodyInput.select(); // 选中.value内容
  document.execCommand('copy'); // 执行浏览器copy方法
  message.success({
    content: '已成功复制到剪贴板',
    duration: 1
  });
  bodyInput.blur(); // 失去焦点
  bodyInput.style.display = 'none'; // 隐藏
}
```

## 图片相关

```ts
/**
 * 提供图片url获取图片宽高方法
 * @param imgUrl 图片url
 * @param callback 图片成功加载回调
 * @param errorCb 图片加载错误回调
 */
export const getImageSize = (imgUrl, callback, errorCb = null) => {
  const image = new Image();
  image.src = imgUrl;
  image.onload = async function () {
    callback(image.width || 0, image.height || 0);
    image.onload = null; // 清除事件回调函数
  };
  image.onerror = function () {
    errorCb && errorCb();
    image.onerror = null; // 清除事件回调函数
  };
};
export const isValidImage = (url: string) =>
  /.(jpeg|jpg|gif|webp|png|avif)/.test(url) || /^data:image/.test(url);
```

## 等几秒

```ts
export const wait = (time) => {
  return new Promise<void>((resolve) => {
    setTimeout(() => {
      resolve();
    }, time);
  });
};
```
