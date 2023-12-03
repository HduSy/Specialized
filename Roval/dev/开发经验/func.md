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
