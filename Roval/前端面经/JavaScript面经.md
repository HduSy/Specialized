Created Date：2023-12-19 17:32:49  
Last Modified：2023-12-19 17:32:49

# Tags

# Content

## 是什么

`JSON with padding`，解决浏览器**同源安全策略**的一种手段，利用的是 `script` 标签 `src` 属性不受跨域限制。用户发出请求时（**GET**）传递一个 `cb` 参数，服务端将数据包裹在 `cb` 返回，这样客户端自定 `cb` 回调函数拿到服务端返回的数据。

### 要点

- 只支持 `GET` 请求
- 服务端响应数据不是 `JSON` 而是 `JavaScript`，`Content-Type: application/javascript`，内容格式为 `callbackFunction(JSON)`

## 实现

### 要点

- 请求结束移除本次请求产生的 `script` 标签和 `window` 上的回调函数；
- 回调须挂载在 `window` 上，因为 `script` 加载后的执行作用域为 `window`；
- 考虑多 `jsonp` 请求，`window` 上挂载的回调函数属性名要唯一；

### 代码

```ts
(function (window,document) {
    "use strict";
    var jsonp = function (url,data,callback) {

        // 1.将传入的data数据转化为url字符串形式
        // {id:1,name:'jack'} => id=1&name=jack
        var dataString = url.indexof('?') == -1? '?': '&';
        for(var key in data){
            dataString += key + '=' + data[key] + '&';
        };

        // 2 处理url中的回调函数
        // cbFuncName回调函数的名字 ：my_json_cb_名字的前缀 + 随机数（把小数点去掉）
        var cbFuncName = 'my_json_cb_' + Math.random().toString().replace('.','');
        dataString += 'callback=' + cbFuncName;

        // 3.创建一个script标签并插入到页面中
        var scriptEle = document.createElement('script');
        scriptEle.src = url + dataString;

        // 4.挂载回调函数
        window[cbFuncName] = function (data) {
            callback(data);
            // 处理完回调函数的数据之后，删除jsonp的script标签
            document.body.removeChild(scriptEle);
            delete window[cbFuncName]
        }

        document.body.appendChild(scriptEle);
    }

    window.$jsonp = jsonp;

})(window,document)

```

# Reference

[JSONP详细实现 - 掘金](https://juejin.cn/post/7034473926319144968) 部分内容参考即可  
[手动实现JSONP | awesome-coding-js](https://www.conardli.top/docs/JavaScript/%E6%89%8B%E5%8A%A8%E5%AE%9E%E7%8E%B0JSONP.html)
