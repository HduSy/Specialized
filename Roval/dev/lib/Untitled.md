##### 生命周期
![[Pasted image 20220211003207.png]]

##### 动态参数
指令绑定动态参数时要注意最好小写，因为模版会将其全部小写化，这样的话，当data中的动态参数是大写时，就绑定不到了。

```vue
<a v-on:[eventName]="doSomething"> ... </a> ❌
<a v-on:[eventname]="doSomething"> ... </a> ✅
data: {
	eventName: 'href',
}
```

##### test