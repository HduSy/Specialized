
创建日期：2022-02-22 15:07:25
最后修改：2022-02-22 15:07:25
- - -
> You need chaos in your soul to give birth to a dancing star.
> — <cite>Friedrich Nietzsche</cite>

## 正文
### 相同点
对 **接口定义** 的两种不同形式，目的都是一样的，都是用来定义 **对象** 或者 **函数** 的形状
### 不同点
type能做到，interface不能做到：
-   type可以定义 **基本类型的别名**，如 `type myString = string`
-   type可以通过 **typeof** 操作符来定义，如 `type myType = typeof someObj`
-   type可以申明 **联合类型**，如 `type unionType = myType1 | myType2`
-   type可以申明 **元组类型**，如 `type yuanzu = [myType1, myType2]`
interface能做到，type不能做到：
interface可以 **声明合并**，type会覆盖只保留最后一个声明：
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
### 继承写法
```ts
// 接口继承接口extends
interface Alarm {
    alert(): void;
}

interface LightableAlarm extends Alarm {
    lightOn(): void;
    lightOff(): void;
}

```
## 参考文献
[类与接口](https://ts.xcatliu.com/advanced/class-and-interfaces.html#%E7%B1%BB%E5%AE%9E%E7%8E%B0%E6%8E%A5%E5%8F%A3)
