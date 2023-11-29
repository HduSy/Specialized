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
