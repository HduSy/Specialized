# promise

#《JS 高程》中译讲解不是很好，转阮一峰 ES6

## 两个特点

1. 状态一旦发生改变就固定了
		
2. 对象状态私有，不受外界影响
		

## 缺点

1. 一旦开始，无法中途取消
		
2. pending 状态时无法得知刚开始执行还是即将完成
		
3. 异常捕获，内部抛出的异常外部无法捕获
		

**方法**

Promise.all ,将多个 promise 实例作为参数，包装成一个新的 promise 实例。只有当所有实例全部 fulfilled 后，p 状态才会变成 fulfilled；只要其中一个实例变成 rejected，p 状态就会变成 rejected

const p = Promise.all([p1, p2, p3])

Promise.race，只要其中一个实例状态发生改变，p 的状态也会跟着改变，率先状态改变的返回值传给 p 的回调函数

const p = Promise.race([p1, p2, p3])

Promise.any，其中一个实例状态 fulfilled 就会导致 p 状态 fulfilled，所有实例都 rejected，才会导致 p 状态 rejected

const p = Promise.any([p1, p2, p3])

Promise.allSettled，所有实例状态均定下后，将所有实例结果作为 p 回调，一旦结束状态总是 **fulfilled** 的

const p = Promise.allSettled([p1, p2, p3])

**Promise.resolve**，将对象转为 Promise 对象，分四种情况：

- 参数是 promise 对象，原封不动返回
		
- 参数是 thenable 对象，将其转为 promise 对象并立即执行其 then 方法
		

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

- 参数是原始值，且不具有 then 方法，Promise.resolve 将返回一个新的状态 resolved 的 promise 对象，其回调函数将立即执行
		
- 参数为空，返回一个 resolved 的状态的 Promise 对象
		

Promise.reject，返回一个 rejected 状态的 promise 实例，参数会作为原因，原封不动地传下去
