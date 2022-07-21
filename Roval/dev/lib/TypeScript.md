创建日期：2022-05-28 22:29:03  
最后修改：2022-05-28 22:29:03

- - -
> Neatness begets order; but from order to taste there is the same difference as from taste to genius, or from love to friendship.  
>—<cite>Johann Kaspar Lavater</cite>

# 正文

仅记录部分点。

## 基础类型

### 元组 Tuple

可以描述已知元素数量和类型的**数组**。

```ts
const x: [number, string] = [1, '1']
```

访问越界元素时，类型会自动推导为联合类型：

```ts
x[3] = '1' // ok. x[3] 类型为 string|number
```

### 枚举 Enum

为一组**数值**赋予友好名字，默认从 0 开始。

```ts
enum EActInfoType {  
  NormalSave,  
  Publish,  
  PublishOnSchedule,  
  Rollback,  
}
```

也可以由索引拿到名字：

```ts
const type: string = EActInfoType[1] // type = 'Publish'
```

### 任意类型 Any

编程阶段不确定类型，可能来自动态内容（如用户输入）或第三方代码库，为了通过编译阶段类型检查，可以用 `any` 类型标记这些变量。`Object` 达不到同等需求，调用方法即便是存在的也会报错。

```ts
let notSure: any = 4;  
notSure.ifItExists(); // ok.
notSure.toFixed(); // ok.
  
let prettySure: Object = 4;  
prettySure.toFixed(); // notok.
```

### 无类型 Void

没有任何类型，声明为 `void` 的变量只能被 `null & undefined` 赋值。

```ts
const n: void = null
const u: void = undefined
```

### 特殊类型 Undefined Null Never

`undefined`、`null`、`never` 均是任何类型的子类型，可以赋值给其他类型的值。没有类型是 `never` 的子类型或可以赋值给 `never`（`any` 也不行，`never` 本身除外）。

```ts
let n: never
const a: any = '1'
n = a // notok. Type 'any' is not assignable to type 'never'.
console.log(n)
```

### 对象类型 Object

除以上基本类型外。

### 类型断言

只在编译阶段起作用，告诉编译器指定值的类型应该是什么。两种写法：

```ts
const someValue: any = 'this is a string';  
  
// let strLength: number = (<string>someValue).length;  
const strLength: number = (someValue as string).length; // 语法支持 jsx
```

# 参考文献

[编译选项](https://www.tslang.cn/docs/handbook/compiler-options.html)  
[工具类型](https://www.typescriptlang.org/docs/handbook/utility-types.html)
