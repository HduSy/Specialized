Created Date：2023-09-19 17:28:14  
Last Modified：2023-09-19 17:28:14

# Tags

#bnpm #bilibili

# Content

## 加载一段脚本

## 自定义 Javascript 操作 cookie

[[../Roval/dev/浏览器/Cookie|Cookie]]

## 跨域解决方案

`document.domain`+`iframe` 应用

## ES6 - class

### static 类静态方法

### \_func 内部方法

## 监听 storage 事件

浏览器不同 tab 页面，`localStorage` 变化时的监听。

```js
window.addEventListener('storage', e => console.log(e))
```

多 tab 下登录状态同步：

```js
/**

* 监听bili-login-state storage 变化时触发refresh 刷新其它标签窗口状态

* @内部方法

*/
const biliLoginStateStorage = 'bili-login-state'
_onStorage() {
	window.addEventListener('storage', e => {
		if(e.key === biliLoginStateStorage){
			this.refresh()
		}
	})
}
```

# Reference

[Package - @bilibili/bili-user](http://npm.bilibili.co/package/@bilibili/bili-user)
