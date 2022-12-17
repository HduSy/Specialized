Created Date：2022-12-17 22:09:16  
Last Modified：2022-12-17 22:09:15

# Tags

#JavaScript

# Content

1. 概述  
Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。[[zhihu-list#怎么理解元编程？https www zhihu com question 23856985]]

```javascript
const target = {
	name: '',
	age: 0,
}
const handler = {
	get: function(target, propKey) {
		console.log(`捕获到对象获取${key}属性的值操作`)
		return target[propKey]
	}
}
/*
	target: 被代理对象;
	handler: 配置对象，定义拦截操作的方法。对于每一个被代理的操作，需要提供一个对应的处理函数，该函数将拦截对应的操作。
*/
var proxy = new Proxy(target, handler);
proxy.name
// 捕获到对象获取name属性的值操作
proxy.age
// 捕获到对象获取age属性的值操作
```

能拦截的操作：

- **get(target, propKey, receiver)**：拦截对象属性的读取，比如 `proxy.foo` 和 `proxy['foo']`。
- **set(target, propKey, value, receiver)**：拦截对象属性的设置，比如 `proxy.foo = v` 或 `proxy['foo'] = v`，返回一个布尔值。
- **has(target, propKey)**：拦截 `propKey in proxy` 的操作，返回一个布尔值。
- **deleteProperty(target, propKey)**：拦截 `delete proxy[propKey]` 的操作，返回一个布尔值。
- **ownKeys(target)**：拦截 `Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for…in` 循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而 `Object.keys()` 的返回结果仅包括目标对象自身的可遍历属性。
- **getOwnPropertyDescriptor(target, propKey)**：拦截 `Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。
- **defineProperty(target, propKey, propDesc)**：拦截 `Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。
- **preventExtensions(target)**：拦截 `Object.preventExtensions(proxy)`，返回一个布尔值。
- **getPrototypeOf(target)**：拦截 `Object.getPrototypeOf(proxy)`，返回一个对象。
- **isExtensible(target)**：拦截 `Object.isExtensible(proxy)`，返回一个布尔值。
- **setPrototypeOf(target, proto)**：拦截 `Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
- **apply(target, object, args)**：拦截 Proxy 实例作为函数调用的操作，比如 `proxy(…args)`、`proxy.call(object,…args)`、`proxy.apply(…)`。
- **construct(target, args)**：拦截 Proxy 实例作为构造函数调用的操作，比如 `new proxy(…args)`。

# Reference