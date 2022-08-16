# 目录

- [[#摘要|摘要]]
- [[#正文|正文]]
	- [[#相同点|相同点]]
	- [[#不同点|不同点]]
- [[#参考文献|参考文献]]

创建日期：2022-02-22 15:07:25  
最后修改：2022-02-22 15:07:25

- - -
> You need chaos in your soul to give birth to a dancing star.  
>—<cite>Friedrich Nietzsche</cite>

# 摘要

声明对象的类型——interface  
声明类型别名——type

# 正文

## 相同点

对 **接口定义** 的两种不同形式，目的都是一样的，都是用来定义 **对象** 或者 **函数** 的形状

## 不同点

type 能做到，interface 不能做到：

- type 可以定义 **基本类型的别名**，如 `type myString = string`
- type 可以通过 **typeof** 操作符来定义，如 `type myType = typeof someObj`
- type 可以声明 **联合类型**，如 `type unionType = myType1 | myType2`
- type 可以声明 **元组类型**，如 `type yuanzu = [myType1, myType2]`  
interface 能做到，type 不能做到：
interface 可以 **声明合并**，type 会覆盖只保留最后一个声明：

```typescript
	interface test {
		name: string
	}
	interface test {
		age: number 
	} 
	/* 
	test实际为 
	{
		name: string;
		age: number 
	}
	*/
```

- interface 新建了一个类型，type 不会新建一个类型而只是创建了一个别名来引用类型；
- 继承写法

```ts
// 接口继承接口extends
interface Alarm {
    alert(): void;
}

interface LightableAlarm extends Alarm {
    lightOn(): void;
    lightOff(): void;
}
// type不能extends\implement
type num = {
  num:number;
}
interface IStrNum extends num {
  str:string;
}
// 与上面等价
type TStrNum = A & {
  str:string;
}
```

## 参考文献

[接口interface](https://ts.xcatliu.com/advanced/class-and-interfaces.html#%E7%B1%BB%E5%AE%9E%E7%8E%B0%E6%8E%A5%E5%8F%A3)  
[类型别名type](https://ts.xcatliu.com/advanced/type-aliases.html)
