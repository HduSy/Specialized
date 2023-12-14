Created Date：2023-12-14 14:36:39  
Last Modified：2023-12-14 14:36:39

# Tags

# Content

## Function.prototype.call

[Function.prototype.call() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)  

修改函数内部 `this` 指向，非严格模式下，不穿参时 `this` 指向 `window`  
参数以参数列表的形式一个个传入 `call` 方法剩余参数

## Function.prototype.apply

[Function.prototype.apply() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)  

修改函数内部 `this` 指向，非严格模式下，不穿参时 `this` 指向 `window`  
`apply` 第二个参数为数组或类数组，`null undefined` 表明无参数传递

## Function.prototype.bind

[Function.prototype.bind() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)  

修改函数内部 `this` 指向，并返回一个新的函数，该函数的 `this` 指向 `bind` 方法的第一个参数。  
`bind` 其余参数以参数列表形式一个个接收

# Reference

[通过例子来理解 Javascript 中的 Call、Apply和 Bind](https://www.freecodecamp.org/chinese/news/understand-call-apply-and-bind-in-javascript-with-examples)
