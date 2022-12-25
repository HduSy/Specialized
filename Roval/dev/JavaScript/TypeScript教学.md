# Tags

#TypeScript

# Content

## 定义

js 类型超集，浏览器不识别 ts 代码，最终将编译为 js 代码。

## 与 JS 区别

ts 是强类型静态类型的语言，js 是弱类型动态类型的语言。[[强类型、弱类型、静态类型、动态类型]]

| TS                 |    JS    |
| ------------------ |:--------:|
| 强类型             |  弱类型  |
| 支持动态、静态类型 | 动态类型 |
| JS 超集            |          |

## 目标

对于生命周期长、较为复杂的 SPA、为保证一定开发效率，最重要的是提升代码可维护性和运行质量，Typescript 很有必要。

- 开发效率上，不论是 WebStorm 还是 VSCode，Typescript 为 IDE 增加智能代码提示与静态检测能力，在团队协作中整体开发效率；
- 可维护性上，长期维护的项目有==“熵”==的特质，随着人员增多，水平不一，可维护性大大下降，通过 **强类型约束** 与 **静态类型检查**，可提高可维护性，甚至 **重构** 的频率也因此增加；
- 线上代码质量上，实际开发过程中，大多 bug 因调用方与被调用方的数据格式不一致引起，TS 有强制类型检查，提前静态检测，发现问题，减少线上 bug 量。

## TS 能干点什么

### 静态检查

错误尽早在编译期检查出来，而非运行时或线上（不容易发现），只有影响程序正常使用时才会发现。  
1、低级错误，如字符敲错、调用没有的属性或方法（属性、方法判断）；

```ts
const peoples = [{
  name: 'tim',
  age: 20
}, {
  name: 'alex',
  age: 22
}];
const sortedPeoples = peoples.sort((a, b) => a.name.localCompare(b.name));
```

```console
error TS2339: Property 'localCompare' does not exist on type 'string'.

```

2、非空判断，前端处理后端嵌套较深的接口，层层取值，不做空值判断的话，就是一颗随机炸弹；

```ts
let data = { list: null, success: true }; const value = data.list.length;
```

```console
error TS2532: Object is possibly 'null'.
```

### 面向对象编程增强

1、访问权限控制，提供 `static`、`protected`、`public`、`private` 访问控制符；

```ts
class Person {
  protected name: string;
  constructor(name: string) { this.name = name; }
}

class Employee extends Person {
  private department: string;
  constructor(name: string, department: string) {
    super(name);
    this.department = department;
  }
}
let howard = new Employee("Howard", "Sales");
console.log(howard.name);

```

```console
error TS2445: Property 'name' is protected and only accessible within class 'Person' and its subclasses.
```

2、接口 `interface`，增强了可扩展性；  
3、范型 T；

```ts
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

4、类型系统  
被调用方负责写好自己对外类型显示，调用方不必关心被调用方内部细节，只需知道什么类型。  
5、模块系统，`namespace`

### 成本

学习成本中等，因为兼容 JS 代码，可渐进式增加类型、接口定义以增强代码健壮性。

## TypeScript 入门

### 数据类型

#### number、string、boolean

需要注意的是 `void` 与 `undefined&null` 区别，后者是任意类型的子类型，也就是说可以赋值给其他类型，`void` 的常用场景就是定义函数无返回值，声明一个变量为 `void` 类型没有多大意义，因为只能被赋值为 `undefined&null`

#### any

表示任意类型。

- 声明变量时未指定类型，则为 any 类型
- 可以在 any 类型上访问任意方法与属性
- any 类型允许被任意类型的值赋值
- 对 any 类型进行操作，其返回值仍为 any 类型，属性类型也都是 any 类型【属性污染】  
不建议使用 any，会失去 ts 的意义。

#### unknown

也表示任意类型，但是相比于 any 是类型安全的 ，定义为 unknown 类型的变量，不允许执行任意操作。  

#### void

与 any 类型相反，表示无类型。

- 声明为 void 的变量只能被 null & undefined 赋值；

```ts
const n: void = null
const u: void = undefined
```

- 无返回值的函数，返回值类型可声明为 void；

```ts
function add(a: number, b: number):void {
	console.log(a + b)
}
```

#### never

**永不** 存在值的类型，如抛出异常的函数、死循环。

```ts
function print(str: string): never {
	throw new Error('TTT')
}
function print(str: string): never {
	while(true) {
		console.log(str)
	}
}
```

没有类型是 never 的子类型或可以赋值给 never（any 也不行，never 本身除外）

#### undefined null

`undefined`、`null`、`never` 均是任何类型的子类型，可以赋值给其他类型的值。

```ts
let n: never
const a: any = '1'
n = a // no. Type 'any' is not assignable to type 'never'.
```

#### 数组类型

##### 表示方法

- `类型 +[]` 表示，如 `let arr: number[] = [1, 2, 3]`
- 数组范型 `Array<element>` 表示，如 `let arr:Array<number> = [1, 2, 3]`
- 接口表示（不推荐）

```typescript
    interface NumberArray {
		[index: number]: any;  
    }
    let fibonacci: NumberArray = [1, '1', 2, 3, 5];
```

但是在表示 **类数组** 方面很 nice，如 `IArguments`, `NodeList`, `HTMLCollection`

```typescript
    function sum() {
		let args: IArguments = arguments;  
    }
```

##### 特点

- 越界访问、赋值不报错；
- 调用数组方法，push 元素时，类型必须一致。

#### 元组 tuple

##### 表示方法

可以描述已知元素 **指定数量** 和 **指定类型** 的数组。

```ts
const x: [number, string] = [1, '1']
```

##### 特点

- 越界访问、赋值报错；
- 调用数组方法，push 一个 **已定义类型** 的元素时，不会报错。  

```ts
x.push(1) // ok
x.push('1') // ok
x.push(true) // no
```

#### interface (Duck Typing)

##### 描述对象

- 确定属性
- 只读属性
- 可选属性
- 任意类型
1. 一个接口中只能定义一个任意属性，且其余属性的类型必须是任意属性的子类型
2. 任意属性可定义为联合类型

```typescript
interface Person {
    name: string;
    readonly id: number;
    age?: number;  
    [propName: string]: any;  
}
```

##### 描述函数

```typescript
interface ISumFunc {
	(x: number, y: number) : number
}
const sumNumber: ISumFunc = (x, y) => x + y
```

- 只读属性：属性前加 `readonly`, 只读的约束只在第一次给对象赋值而非第一次给只读属性赋值  
[[type vs interface]]

#### 函数类型

- 可选参数

与可选属性相似，注意的是，得放在最后，保证后面不能再有确定参数，写法为 `?:`

```ts
function buildName(firstName: string, lastName?: string): string {
	return firstName + ' ' + lastName;  
}  
let tomcat = buildName('Tom', 'Cat');  
let tom = buildName('Tom');
```

- 参数默认值

```typescript
function buildName(firstName: string = 'Tom', lastName: string): string {
	return firstName + ' ' + lastName;  
}  
let tomcat = buildName(undefined, 'Cat');  
let tom = buildName('Tom');
```

- 函数重载（）

```typescript
function reverse(x: number): number; // 定义  
function reverse(x: string): string; // 定义  
// 实现  
function reverse(x: number | string): number | string | void {  
	if (typeof x === 'number') {  
		return Number(x.toString().split('').reverse().join(''));  
	} else if (typeof x === 'string') {  
		return x.split('').reverse().join('');  
	}  
}
```

#### enum

用来定义常量。数值递增。

```ts
enum EActInfoType {  
  NormalSave,  // 0
  Publish,  // 1
  PublishOnSchedule,  // 2
  Rollback,  // 3
}
```

反向映射，由枚举值拿到枚举名。

```ts
const type: string = EActInfoType[1] // type = 'Publish'
```

const 常量枚举，编译后的代码简洁。

```ts
const enum Direction { UP = "UP", DOWN = "DOWN", LEFT = "LEFT", RIGHT = "RIGHT" }
```

#### 类型推断

1. 变量声明未赋值为 any 类型；
2. 变量赋值时；
3. 为函数指定默认参数时；
4. 未明确指定函数返回值类型时；

#### 内置类型

##### JS 内置

`number` `boolean` `string` `null` `undefined` `object` `bigint` `symbol`

##### ECMA

`Array` `Error` `RegExp` `Date`

##### DOM BOM

`HTMLElement` `NodeList` `MouseEvent` 等等等

##### TS 核心库

定义了浏览器环境需要用到的类型

## TS 高阶

### 联合类型 |

表示一个变量支持多种类型。当 ts 不确定变量类型时，只能访问联合类型共有属性或方法。

```ts
let num: number|string;
num = 6
num = '6'
```

### 交叉类型 &

类似接口继承，实现对对象形状的组合和扩展。[[type vs interface]]

```ts
type A = {
	a: string
}
type B = A & {b: string}
const C:B = {
	a: 'hehe',
	b: 'haha'
}
```

### 类型别名 type

类型起个别名，使得 ts 代码写起来简洁、清晰。[[type vs interface]]

### 类型断言

值 as 类型

### 字面量类型

定义一些常量，只能从已定义常量中取值

```ts
type ButtonSize = 'mini' | 'small' | 'normal' | 'large'
```

### 声明文件

声明语句：声明语句中只能定义类型，不能定义具体实现

声明文件：声明语句放在一个 `.d.ts` 结尾的文件中即是声明文件，typescript 解析所有.ts 文件，因而包含 `.d.ts` 的声明文件，在其他 ts 文件中就可以获得声明文件中的定义了

## 拥抱 TS 之代码中的实践

### 摘要

`TypeScript` 增强了 IDE 功能，更好的代码提示、定义跳转、接口提示、代码重构，静态类型检查，在代码编写阶段发现语法错误。[[TypeScript教学]]

#### 1、善用类型注释

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

#### 2、善用 *.d.ts 声明文件

`*.ts` 文件会获取 `*.d.ts` 声明文件中的类型定义。在 `tsconfig.json` 中配置全局自定义类型声明文件，则声明文件中的类型定义都能被项目中的 `*.ts` 文件获取到，不需要 `import` 就可以直接使用。

```
common-types.d.ts
```

#### 3、第三方库声明文件

当在 `TypeScript` 项目中使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。针对多数第三方库，社区已经帮我们定义好了它们的声明文件，我们可以直接下载下来使用。一般推荐使用 `@types` 统一管理第三方库的声明文件，`@types` 的使用方式很简单，直接用 `npm` 或 `yarn` 安装对应的声明模块即可：

```bash
yarn add @types/lodash
```

可在此查找 [第三方库声明文件查找地址](https://www.typescriptlang.org/dt/search?search=)

#### 4、自定义声明文件

当第三方库未提供声明文件时，编辑器会报错找不到库的声明文件，可通过以下方式解决编辑器报错：

```ts
declare module '@bilibili/h5-utils'  
declare module 'fontmin'  
declare module 'animejs/lib/anime.es.js'  
declare module '@bilibili/share-h5-next'  
declare module 'qrcode'  
declare module '@bilibili/sakura/lib/v-slidingdrawer'
```

#### 5、命名空间 - Namespace

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

#### 6、善用 JS 新特性 - 可选链

可选链是先检查属性是否存在（非 `null` 或 `undefined`），存在再访问该属性的运算符，不存在返回 `undefined`。

```ts
const age = user&&user.info&&user.info.getAge&&user.info.getAge()  
```

否则直接访问 `user.info.getAge()` 很容易命中 `Uncaught TypeError: Cannot read property…`，以可选链改写后：

```ts
const list = user?.info?.getAge?.()
```

#### 7、善用 JS 新特性 - 空值合并运算符

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

#### 8、善用使用访问限定修饰符 - `private`、`protected` 和 `public`

TypeScript 为类定义提供了三种访问修饰符：

- `public` : 公有类型，在类里面、子类、类外面都可以访问到，如果不加任何修饰符，默认为此访问级别，默认；
- `protected` : 保护类型，在类里面、子类里面可以访问，在类外部不能访问；
- `private` : 私有类型，只能在当前类内部访问。
- `static`：声明为类的静态属性和方法。  
【⚠️注：】转义后的代码在 `JS` 环境中完全可以正确执行，不会受限。`TypeScript` 仅仅是扩展了更为严格的语法，借助 LSP 和编译器来帮助开发者在开发环境中尽早发现并解决存在或替在的问题。

#### 9、善用类型收窄

类型断言、类型守卫、双重断言  
类型守卫：

- typeof：用于判断 `number`，`string`，`boolean` 或 `symbol` 四种基本类型；
- instanceof：用于判断一个实例是否属于某个类
- in：用于判断一个属性/方法是否属于某个对象

#### 10、常量枚举 Vs 普通枚举

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

#### 11、高级类型

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

# Reference

[详解tsconfig.json文件](https://www.pengfeixc.com/blogs/javascript/tsconfig)  
[接口interface](https://ts.xcatliu.com/advanced/class-and-interfaces.html#%E7%B1%BB%E5%AE%9E%E7%8E%B0%E6%8E%A5%E5%8F%A3)  
[类型别名type](https://ts.xcatliu.com/advanced/type-aliases.html)  
[如何在项目中用好 TypeScript 🤔](https:juejin.cn/post/7058868160706904078)  
[类型系统 top/bottom type](https://mp.weixin.qq.com/s/rZ96wy8xUrx4T1qG5OKS0w)  
[编译选项](https://www.tslang.cn/docs/handbook/compiler-options.html)  
[工具类型](https://www.typescriptlang.org/docs/handbook/utility-types.html)  
[1.9W字总结一份通俗易懂的 TS 教程，入门 + 实战！](https://juejin.cn/post/7068081327857205261)
