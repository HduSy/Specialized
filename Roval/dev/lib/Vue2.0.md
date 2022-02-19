## 目录

- [[#正文|正文]]
	- [[#生命周期|生命周期]]
	- [[#动态参数|动态参数]]
- [[#参考文献|参考文献]]

创建日期：2022-02-19 00:34:20
最后修改：2022-02-19 00:34:19
- - -
> Gratitude is not only the greatest of virtues, but the parent of all the others.
> — <cite>Cicero</cite>

## 正文
### 生命周期
![[Pasted image 20220211003207.png]]

### 动态参数
指令绑定动态参数时要注意最好小写，因为模版会将其全部小写化，这样的话，当data中的动态参数是大写时，就绑定不到了。

```vue
<a v-on:[eventName]="doSomething"> ... </a> ❌
<a v-on:[eventname]="doSomething"> ... </a> ✅
data: {
	eventName: 'href',
}
```

## 参考文献
