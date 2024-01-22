Created Date：2022-12-25 11:06:39  
Last Modified：2022-12-25 11:06:39

# Tags

# Content

## 相同点

1. 都可以用来描述对象和函数；
2. 都有继承的概念，只不过写法不同，能相互继承；  
接口继承

```ts
// interface extends interface
interface Name {
	name: string;
}
interface User extends Name {
	age: number;
}
const person: User = {name:'Tom',age:18}
```

交叉类型

```ts
// type & type
type Name = {
	name: string;
}
type User = Name & {
	age: number;
}
const person: User = {name:'Tom',age:18}
```

## 不同点

- 目的不同，`interface` 用来描述对象形状不能重命名原始类型，`type` 用来给类型起个别名，方便代码书写简洁；
- `type` 可以声明 **基本类型的别名**、**联合类型**、**元组类型**；可通过 `typeof` 获取实例类型进行赋值；

```ts
type Name = string
type ID = string|number
type Address = [string, string]
type DomType = typeof xxx
```

- `interface` 允许 **声明合并**，而 `type` 不允许重复声明会报 `Error: Duplicate identifier`；

```typescript
	interface test {
		name: string
	}
	interface test {
		age: number 
	} 
	/* ✅ 
	test实际为 
	{
		name: string;
		age: number 
	}
	*/
	type ID = string|number
	type ID = string
	/* ❌
	Error: Duplicate identifier 'Window'.
	*/
```

- `interface` 新建了一个类型，`type` 不会新建一个类型而只是创建了一个别名来引用类型；

# Reference

[Typescript 中的 interface 和 type 到底有什么区别 - 掘金](https://juejin.cn/post/6844903749501059085)
