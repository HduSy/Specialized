# Tags

#TypeScript

# Content

## 定义

JavaScript 类型超集，代码最终将编译为纯 JavaScript 代码。

## 目标

>编译期可以做静态检查，为大规模代码提供结构化装置，编译出符合习惯、易读的 JS 代码，和 ECMAScript 标准对齐，使用一贯的、可删除的、结构化的类型系统，保护编译后的 JS 代码的运行时行为等等。

对于生命周期长、较为复杂的 SPA、为保证一定开发效率，最重要的是提升代码可维护性和运行质量，Typescript 很有必要。

- 开发效率上，不论是 WebStorm 还是 VsCode，Typescript 为 IDE 增加智能代码提示与静态检测能力，在团队协作中整体开发效率；
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

## TypeScript 基础语法

## 入门到放弃

### 基础

#### 基础数据类型

`number、string、boolean、undefined、null、void`

需要注意的是 void 与 undefined&null 区别，后者是任意类型的子类型，也就是说可以赋值给其他类型，void 的常用场景就是定义函数无返回值，声明一个变量为 void 类型没有多大意义，因为只能被赋值为 undefined&null

#### 任意值

- 声明变量时未指定类型，则为 any 类型
		
- 可以在 any 类型上访问任意方法与属性
		
- any 类型允许被任意类型的值赋值
		
- 对 any 类型进行操作，其返回值仍为 any 类型，属性类型也都是 any 类型【属性污染】

#### 类型推断

- 未明确指定一个变量的类型的的时候会推断该变量的类型
		
- 当定义变量未赋值时，之后该变量都会成为 any 类型

#### 联合类型

- 联合类型通过 `|` 分隔
		
- 当 ts 并不知道联合类型的变量属于哪个类型时，只能访问联合类型共有的属性&方法

#### 元组 Tuple

可以描述已知元素数量和类型的 **数组**。

```ts
const x: [number, string] = [1, '1']
```

访问越界元素时，类型会自动推导为联合类型：

```ts
x[3] = '1' // ok. x[3] 类型为 string|number
```

#### 枚举 Enum

为一组 **数值** 赋予友好名字，默认从 0 开始。

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

#### 对象类型——接口 Interface

接口一方面是对行为的抽象，具体实现则由类去实现，另一方面是对对象形状进行描述

- 确定属性 少一个属性&多一个属性都是不允许的
- 可选属性

```typescript
interface Person {
    name: string;
    age?: number;  
}
```

- 任意属性

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;  
}
```

1) 一个接口中只能定义一个任意属性，且其余属性的类型必须是任意属性的子类型

2) 任意属性可定义为联合类型

```typescript
    interface Person {
		readonly id: number;
		name: string;
		age?: number;
		[propName: string]: any;  
    }
```

- 只读属性：属性前加 `readonly`, 只读的约束只在第一次给对象赋值而非第一次给只读属性赋值  
[[Type VS Interface]]

#### 数组类型

- 类型 +[] 表示，如 `let arr: number[] = [1, 2, 3]`
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

#### 函数的类型

- 函数声明，参数个数确定，不可多不可少

```typescript
    function sum(x: number, y: number): number {
		return x + y;  
    }
```

- 函数表达式，TS 中的 `=>` 用来表示函数定义，左边是输入类型，右边是输出类型，不同于 ES6 中的箭头函数

```typescript
    let mySum: (x: number, y: number) => number = 
	function (x: number, y: number):number {
		return x + y;  
    };
```

- 接口定义函数

```typescript
    interface SearchFunc {
		(source: string, subString: string): boolean;  
    }  
    let mySearch: SearchFunc;  
    mySearch = function(source: string, subString: string) {
		return source.search(subString) !== -1;  
    }
```

- 可选参数

		与可选属性相似，注意的是后面不能有确定参数

- 参数默认值，**TypeScript 会将添加了默认值的参数识别为可选参数**, 此时就不受「可选参数必须接在必需参数后面」的限制了

```typescript
    function buildName(firstName: string, lastName: string = 'Cat') {
		return firstName + ' ' + lastName;  
    }  
    let tomcat = buildName('Tom', 'Cat');  
    let tom = buildName('Tom');
```

- 剩余参数，只能是最后一个参数

```typescript
    function push(array: any[], ...items: any[]) {
		items.forEach(function(item) {
			array.push(item);
		});  
    }
    let a = [];  
    push(a, 1, 2, 3);
```

- 函数重载

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

#### 无类型 Void

没有任何类型，声明为 `void` 的变量只能被 `null & undefined` 赋值。

```ts
const n: void = null
const u: void = undefined
```

#### 特殊类型 Undefined Null Never

`undefined`、`null`、`never` 均是任何类型的子类型，可以赋值给其他类型的值。没有类型是 `never` 的子类型或可以赋值给 `never`（`any` 也不行，`never` 本身除外）。

```ts
let n: never
const a: any = '1'
n = a // notok. Type 'any' is not assignable to type 'never'.
console.log(n)
```

#### Unknown & Never

##### Never 妙用

- unreachable code detect
- exhaustive check

#### 类型断言

手动指定值的类型

- 用途一：将联合类型断言为具体类型，骗过 TS 编译器使得编译不报错（因为联合类型在使用时，只能调用共用的属性或方法，否则编译报错），但不能避免运行时报错；
		
- 用途二：将父类断言为子类，从而访问父类上没有的方法
		
- 用途三：将类型断言为 any，如 `(window as any).foo = 1;`
		
- 用途四：将 " 烂代码 " 中的 any 类型断言为任意具体类型
		
		- TS是结构类型系统，类型之间的对比只会比较它们最终的结构，而会忽略定义时的关系。要使得 `A` 能够被断言为 `B`，只需要 `A` 兼容 `B` 或 `B` 兼容 `A` 即可，这里的`兼容`指的就是`涵盖`
				
- 除非迫不得已，千万别用双重断言
		
- 类型断言 VS 类型转换：类型断言并不会改变变量本身类型，编译后都会去掉
		
- 类型断言 VS 类型声明：类型声明比类型断言更加严格，
		
		1. `animal` 断言为 `Cat`，只需要满足 `Animal` 兼容 `Cat` 或 `Cat` 兼容 `Animal` 即可
				
		2. `animal` 赋值给 `tom`，需要满足 `Cat` 兼容 `Animal` 才行
				
- 类型断言 VS 范型：最优解决方案

#### 声明文件

声明语句：声明语句中只能定义类型，不能定义具体实现

声明文件：声明语句放在一个 `.d.ts` 结尾的文件中即是声明文件，typescript 解析所有.ts 文件，因而包含 `.d.ts` 的声明文件，在其他 ts 文件中就可以获得声明文件中的定义了

### Tsc 配置文件 tsconfig.json

## 接口 Interface

### 可选属性

```ts
interface Itest {
	x?:number;
}
```

### 只读属性

只能在对象创建时修改其属性值。

```ts
interface Itest {
	readonly x:number;
}
let ro = R
```

### 函数类型

```
interface SearchFunc {
  (source: string, subString: string): boolean;
}
```

### 索引类型

数字索引的返回值必须是字符串索引返回值类型的子类型，因为 `javascript` 会把数字类型索引转为字符串类型索引去取值。

```
// 数字索引
interface StringArray {
  [index: number]: string;
}
// 字符串索引
interface StringArray {
  [index: string]: string;
}
// 设置只读
interface StringArray {
  readonly [index: string]: string;
}
```

### 接口继承类

当接口继承了一个类类型时，同样会继承到类的 private 和 protected 成员，这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现（implement）。

```ts
class Control {
    private state: any;
}
interface SelectableControl extends Control {
    select(): void;
}
// ok
class Button extends Control implements SelectableControl {
    select() { }
}
class TextBox extends Control {

}
// not ok
// Error: Property 'state' is missing in type 'Image'.
class Image implements SelectableControl {
    select() { }
}

class Location {

}
```

## 类型 Type

## Type VS Interface

声明对象的类型——interface  
声明类型别名——type

### 相同点

对 **接口定义** 的两种不同形式，目的都是一样的，都是用来定义 **对象** 或者 **函数** 的形状

### 不同点

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
[掘金详细TS](https://juejin.cn/post/7068081327857205261)  
[接口interface](https://ts.xcatliu.com/advanced/class-and-interfaces.html#%E7%B1%BB%E5%AE%9E%E7%8E%B0%E6%8E%A5%E5%8F%A3)  
[类型别名type](https://ts.xcatliu.com/advanced/type-aliases.html)  
[如何在项目中用好 TypeScript 🤔](https:juejin.cn/post/7058868160706904078)  
[类型系统 top/bottom type](https://mp.weixin.qq.com/s/rZ96wy8xUrx4T1qG5OKS0w)  
[编译选项](https://www.tslang.cn/docs/handbook/compiler-options.html)  
[工具类型](https://www.typescriptlang.org/docs/handbook/utility-types.html)  
[1.9W字总结一份通俗易懂的 TS 教程，入门 + 实战！](https://juejin.cn/post/7068081327857205261)
