Created Date：2023-12-19 17:32:49  
Last Modified：2023-12-19 17:32:49

# Tags

# Content

## JSONP

### 是什么

`JSON with padding`，解决浏览器**同源安全策略**的一种手段，利用的是 `script` 标签 `src` 属性不受跨域限制。用户发出请求时（**GET**）传递一个 `cb` 参数，服务端将数据包裹在 `cb` 返回，这样客户端自定 `cb` 回调函数拿到服务端返回的数据。

#### 要点

- 只支持 `GET` 请求
- 服务端响应数据不是 `JSON` 而是 `JavaScript`，`Content-Type: application/javascript`，内容格式为 `callbackFunction(JSON)`

### 实现

#### 要点

- 请求结束移除本次请求产生的 `script` 标签和 `window` 上的回调函数；
- 回调须挂载在 `window` 上，因为 `script` 加载后的执行作用域为 `window`；
- 考虑多 `jsonp` 请求，`window` 上挂载的回调函数属性名要唯一；

#### 代码

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

## 构造函数与 new

### 对象是什么

1. 对象是单个实物的抽象  
   实物与实物之间的关系就变成的对象与对象之间的关系，对象之间分工合作，从而模拟现实情况，面向对象编程
2. 对象是一个拥有属性和方法的“容器”  
   属性是对象的状态，方法是对象的行为

### 构造函数

`JavaScript` 语言的对象体系，不是基于“类”的，而是基于构造函数（`constructor`）和原型链（`prototype`），是创建对象的**模板**。是一个普通函数但有着特殊用法：

1. 函数体内 `this` 指向即将创建的对象；
2. 作为构造函数使用时，使用 `new` 命令；

### new

执行构造函数，返回一个实例对象

#### 原理

```ad-note
1、创建一个空对象，作为将要返回的对象实例；
2、将这个空对象的原型，指向构造函数原型，以能够访问构造函数原型上的属性和方法；
3、将构造函数内部this指向修改为该对象并执行，以便该对象可访问到构造函数中的属性和方法；
5、返回值处理，若构造函数返回引用类型将作为返回值返回，否则将返回该新建的对象
```

```js
// new 实现
function myNew(Construtor, ...args) {
  const newObj = new Object() // 创建一个新对象
  // newObj.__proto__ = Construtor.prototype
  Object.setPrototypeOf(newObj, Construtor.prototype) // 新对象的原型指向构造函数的原型
  const result = Construtor.apply(newObj, args) // 修改构造函数的this为newObj并执行
  return typeof result === 'object' ? result : newObj // 返回值处理
}

// 测试
function Otaku(name, age) {
  this.name = name;
  this.age = age;
  this.habit = "Games";
}
Otaku.prototype.strength = 60;
Otaku.prototype.sayYourName = function () {
  console.log("I am " + this.name);
};
var person = myNew(Otaku, "Kevin", "18");
console.log(person.name); // Kevin
console.log(person.habit); // Games
console.log(person.strength); // 60
person.sayYourName(); // I am Kevin
```

# Reference

[JSONP详细实现 - 掘金](https://juejin.cn/post/7034473926319144968) 部分内容参考即可  
[手动实现JSONP | awesome-coding-js](https://www.conardli.top/docs/JavaScript/%E6%89%8B%E5%8A%A8%E5%AE%9E%E7%8E%B0JSONP.html)  
[阮一峰 - 构造函数与new命令](https://javascript.ruanyifeng.com/oop/basic.html)  
[new 一个对象时发生了什么](https://jonny-wei.github.io/blog/base/javascript/prototype.html#%E9%97%AE%E9%A2%98)
