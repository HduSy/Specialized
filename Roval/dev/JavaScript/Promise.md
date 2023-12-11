Created Date：2022-12-17 22:08:48  
Last Modified：2022-12-17 22:08:47

# Tags

#JavaScript

# Content

《JS 高程》中译讲解不是很好，转《阮一峰 ES6》

## 两个特点

- 状态一旦发生改变就**固定**了，**任何时候**都可以得到这个结果。与事件（`Event`）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的；
- 对象状态**私有**，不受外界影响。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态；

## 缺点

1. 一旦开始，**无法**中途取消；
2. `pending` 状态时**无法**得知进展，刚开始执行还是即将完成；
3. 如果不设置异常捕获，内部抛出的异常外部**无法**捕获；

## 方法

### Promise.prototype.then()

`Promise` 状态改变时的回调函数，接收两个***可选参数***，第一个参数是 `resolved` 状态的回调，第二个参数是 `rejected` 状态的回调

### Promise.prototype.catch()

`Promise.prototype.catch()` 方法是 `.then(null, rejection)` 或 `.then(undefined, rejection)` 的别名，用于指定发生错误时的回调函数，而一般来说不要在 `then()` 方法里面定义 `rejected` 状态的回调函数（即 `then` 的第二个参数），而总是使用 `catch` 方法

### Promise.prototype.finally()

指定不管 `Promise` 对象最后状态如何，都会执行的操作

### Promise.all

将多个 `promise` 实例作为参数，包装成一个新的 `promise` 实例 `p`。只有当所有实例全部 `fulfilled` 后，`p` 状态才会变成 `fulfilled`；只要其中一个实例变成 `rejected`，`p` 状态就会变成 `rejected`

```js
const p = Promise.all([p1, p2, p3])
p.then(([p1data, p2data, p3data])=>{}, ()=>{})
```

### Promise.race

只要其中一个实例状态发生改变，`p` 的状态也会跟着改变，率先状态改变的返回值传给 `p` 的回调函数

```js
const p = Promise.race([p1, p2, p3])
```

### Promise.any

其中一个实例状态 `fulfilled` 就会导致 `p` 状态 `fulfilled`，所有实例都 `rejected`，才会导致 `p` 状态 `rejected`

```js
const p = Promise.any([p1, p2, p3])
```

### Promise.allSettled

所有实例状态均定下后，不管是 `fulfilled` 还是 `rejected`，才将所有实例结果作为 `p` 回调

```js
const p = Promise.allSettled([p1, p2, p3])
```

### Promise.resolve

将对象转为 `Promise` 对象，分四种情况：

- 参数是 `promise` 对象，原封不动返回
- 参数是 `thenable` 对象，将其转为 `promise` 对象并立即执行其 `then` 方法

```js
let thenable = {
    then: function(resolve, reject) {
        resolve(42);
    }
};
​  
let p1 = Promise.resolve(thenable);  
p1.then(function (value) {
    console.log(value);  // 42  
});
```

- 参数是原始值，且不具有 `then` 方法，`Promise.resolve` 将返回一个新的状态 `resolved` 的 `Promise` 对象，其回调函数将立即执行
- 参数为空，返回一个 `resolved` 的状态的 `Promise` 对象

### Promise.reject

返回一个 `rejected` 状态的 `Promise` 实例，参数会作为原因，原封不动地传下去

# Reference

[ES6 Promise](https://es6.ruanyifeng.com/#docs/promise)
