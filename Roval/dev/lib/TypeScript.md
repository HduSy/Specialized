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

# 参考文献

[编译选项](https://www.tslang.cn/docs/handbook/compiler-options.html)  
[工具类型](https://www.typescriptlang.org/docs/handbook/utility-types.html)
