# 标签

#掘金

# 目录

- [[#正文|正文]]
	- [[#摘要|摘要]]
		- [[#1、善用类型注释|1、善用类型注释]]
		- [[#2、善用*.d.ts声明文件|2、善用*.d.ts声明文件]]
		- [[#3、第三方库声明文件|3、第三方库声明文件]]
		- [[#4、自定义声明文件|4、自定义声明文件]]
		- [[#5、命名空间 - namespace|5、命名空间 - namespace]]
		- [[#6、善用JS新特性 - 可选链|6、善用JS新特性 - 可选链]]
		- [[#7、善用JS新特性 - 空值合并运算符|7、善用JS新特性 - 空值合并运算符]]
		- [[#8、善用使用访问限定修饰符 - `private`、`protected` 和 `public`|8、善用使用访问限定修饰符 - `private`、`protected` 和 `public`]]
		- [[#9、善用类型收窄|9、善用类型收窄]]
		- [[#10、常量枚举vs普通枚举|10、常量枚举vs普通枚举]]
		- [[#11、高级类型|11、高级类型]]
- [[#参考文献|参考文献]]

创建日期：2022-02-19 21:52:07  
最后修改：2022-02-19 21:52:07

- - -
> Beware of false knowledge; it is more dangerous than ignorance.  
>—<cite>Bernard Shaw</cite>

# 正文

## 摘要

`TypeScript` 增强了 IDE 功能，更好的代码提示、定义跳转、接口提示、代码重构，静态类型检查，在代码编写阶段发现语法错误。[[为什么要用TypeScript]]

### 1、善用类型注释

 当鼠标悬浮在使用到该类型的地方时，编辑器会有更好的提示

 
 ```ts
/** 奖品信息 */
interface IAwardItem {  
	 gift_name: string;  
	 gift_pic: string;  
	 gift_description: string;  
	 gift_type: number; // 奖励类型,1:默认类型, 2:碎片类型, 3:祝福类型  
	 splinter_have: number; // 用户持有的碎片个数, 只有gift_type=2时有效  
	 splinter_count: number; // 碎片上限, 只有gift_type=2时有效  
}
 ```
 

### 2、善用 *.d.ts 声明文件

`*.ts` 文件会获取 `*.d.ts` 声明文件中的类型定义。在 `tsconfig.json` 中配置全局自定义类型声明文件，则声明文件中的类型定义都能被项目中的 `*.ts` 文件获取到，不需要 `import` 就可以直接使用。

```
common-types.d.ts
```

### 3、第三方库声明文件

当在 `TypeScript` 项目中使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。针对多数第三方库，社区已经帮我们定义好了它们的声明文件，我们可以直接下载下来使用。一般推荐使用 `@types` 统一管理第三方库的声明文件，`@types` 的使用方式很简单，直接用 `npm` 或 `yarn` 安装对应的声明模块即可：

```bash
yarn add @types/lodash
```

可在此查找 [第三方库声明文件查找地址](https://www.typescriptlang.org/dt/search?search=)

### 4、自定义声明文件

当第三方库未提供声明文件时，编辑器会报错找不到库的声明文件，可通过以下方式解决编辑器报错：

```ts
declare module '@bilibili/h5-utils'  
declare module 'fontmin'  
declare module 'animejs/lib/anime.es.js'  
declare module '@bilibili/share-h5-next'  
declare module 'qrcode'  
declare module '@bilibili/sakura/lib/v-slidingdrawer'
```

### 5、命名空间 - Namespace

在不同命名空间内定义类型：

```ts
declare namespace Space01 {  
    interface ICourse {
		name: string;
		teacher: string;  
		rank: number;  
	}  
}  
declare namespace Space02 {  
    interface ICourse {  
		name: string;  
		teacher: string;  
		score: number;  
	}  
}  
const courseUS: Space01.ICourse = {  
	name: 'Art',  
	teacher: 'Tomas',  
	rank: 1,  
}  
const courseZH: Space02.ICourse = {  
	name: '艺术',  
	teacher: '小白',  
	rank: 1,  
}
```

### 6、善用 JS 新特性 - 可选链

可选链是先检查属性是否存在（非 `null` 或 `undefined`），存在再访问该属性的运算符，不存在返回 `undefined`。

```ts
const age = user&&user.info&&user.info.getAge&&user.info.getAge()  
```

否则直接访问 `user.info.getAge()` 很容易命中 `Uncaught TypeError: Cannot read property…`，以可选链改写后：

```ts
const list = user?.info?.getAge?.()
```

### 7、善用 JS 新特性 - 空值合并运算符

当左侧的操作数为 `null` 或者 `undefined` 时，返回其右侧操作数，否则返回左侧操作数。

```ts
const course = {  
  level: 0,  
}  
const level = course.level ?? '暂无等级' // 0    
const level = course.rank ?? '暂无排名' // 暂无排名
const level = course.level || '暂无等级' // 暂无等级
```

【⚠️注：】与 `||` 操作符表现不同，判断左侧操作数为 `falsy` 即取右侧操作数。

### 8、善用使用访问限定修饰符 - `private`、`protected` 和 `public`

TypeScript 为类定义提供了三种访问修饰符：

- `public` : 公有类型，在类里面、子类、类外面都可以访问到，如果不加任何修饰符，默认为此访问级别，默认；
- `protected` : 保护类型，在类里面、子类里面可以访问，在类外部不能访问；
- `private` : 私有类型，只能在当前类内部访问。
- `static`：声明为类的静态属性和方法。
【⚠️注：】转义后的代码在 `JS` 环境中完全可以正确执行，不会受限。`TypeScript` 仅仅是扩展了更为严格的语法，借助 LSP 和编译器来帮助开发者在开发环境中尽早发现并解决存在或替在的问题。

### 9、善用类型收窄

类型断言、类型守卫、双重断言  
类型守卫：

- typeof：用于判断 `number`，`string`，`boolean` 或 `symbol` 四种基本类型；
- instanceof：用于判断一个实例是否属于某个类
- in：用于判断一个属性/方法是否属于某个对象

### 10、常量枚举 Vs 普通枚举

```ts
const enum Direction {  
	UP,  
	Right,  
	Down,  
	Left  
}
const directions = [ Direction.UP, Direction.Right ]
// 编译结果
const directions = [ 0, 1 ]
```

常量 (const) 枚举，不允许有计算值，且在编译阶段被移除。

```ts
enum Direction {  
	a = 1 + 1,  
	b = 'abc'.length,  
}
```

允许有计算值，且在编译阶段不会被移除。

### 11、高级类型

- 类型索引 `keyof`  
`keyof` 类似于 `Object.keys`

```ts
interface TaskActionNames {  
 // 签到  
 checkin: string;
 // 观看视频  
 watchVideo: string;
 // 观看限免影片  
 watchMovie: string;
 // 投稿/发视频  
 post: string;
}  
type TaskActionName = keyof TaskActionNames
// 等效于
type TaskActionName = 'checkin'|'watchVideo'|'watchMovie'|'post'
```

- 类型约束 `extend`  
常与 `keyof` 一起使用

```ts
function getValue<T, K extends keyof T>(obj: T, key: K) {  
  return obj[key]  
}  
const obj = { a: 1 }  
const a = getValue(obj, 'a') // 1  
const b = getValue(obj, 'b') // 传入对象无key时IDE报错
```

- 类型映射 `in`  
`in` 关键词的作用主要是做类型的映射，配合 `keyof` 遍历已有接口的 `key` 或者是遍历联合类型。
- ts 内置类型  
`Exclude`：排除相同属性  
`Partial`：可选属性  
`Required`：必须属性  
`Pick`：选取属性  
`Omit`：剔除属性

## 参考文献

[如何在项目中用好 TypeScript 🤔](https:juejin.cn/post/7058868160706904078)
