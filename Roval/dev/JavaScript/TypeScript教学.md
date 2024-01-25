Created Date：2022-12-22 16:19:19  
Last Modified：2022-12-26 12:45:48

# Tags

#TypeScript [[../../Project/tsconfig.json|tsconfig.json]]

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

### 静态类型检查

错误尽早在编译期检查出来，而非运行时或线上（不容易发现），只有影响程序正常使用时才会发现

- 低级错误，如拼写错误、调用没有的属性或方法（属性、方法判断）；

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

- 非空判断，前端处理后端嵌套较深的接口，层层取值，不做空值判断的话，就是一颗随机炸弹；

```ts
let data = { list: null, success: true }; const value = data.list.length;
```

```console
error TS2532: Object is possibly 'null'.
```

- 自动补全与快速修复

### 非异常失败

ECMA 规范明确了的异常之外的错误提示

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

### 基础

#### 类型检查器

#### tsc 编译器

##### 显式类型（Explict Types）  

显式指定类型，也叫类型注解，不总是需要注解类型，`ts` 某些情况会根据上下文类型，推断出类型

##### 类型抹除（Erased Types）

编译输出的 `js` 文件不再具有 `ts` 特有代码，如类型注解会被抹除，只剩下 `js` 代码

##### 降级（Down Leveling）

将高版本 ECMA 语法转为低版本语法的过程。默认 `ES3`，不过目前浏览器均已支持 `ES5`，设置 `target:"ES5"` 即可

##### 严格模式（strict）

#### 类型收窄（narrowing）

沿着代码可能的执行路径，分析值在给定位置上的具体类型

##### `typeof` 收窄

也叫类型保护，在**类型上下文**（`type context`）中使用，用于获取一个变量或者属性的类型，和其他的类型操作符搭配使用才能发挥它的作用，使用示例：

```ts
type Predicate = (x: unknown) => boolean;
type K = ReturnType<Predicate>;
/// type K = boolean
```

直接传入函数名：

```ts
function f() {
  return { x: 10, y: 3 };
}
type P = ReturnType<f>;

// 'f' refers to a value, but is being used as a type here. Did you mean 'typeof f'?
```

因为值（`values`）和类型（`types`）并不是一种东西，解决：

```ts
function f() {
  return { x: 10, y: 3 };
}
type P = ReturnType<typeof f>;
                    
// type P = {
//    x: number;
//    y: number;
// }
```

```ad-tip
在 `ts` 中，只有对**标识符（比如变量名）或者他们的属性**使用 `typeof` 才是合法的
```

```ts
let shouldContinue: typeof msgbox("Are you sure you want to continue?"); // ❌
// Meant to use = ReturnType<typeof msgbox> ✅
```

```ts
/* 对象 */
const person = { name: "kevin", age: "18" }
type Kevin = typeof person;
// type Kevin = {
// 		name: string;
// 		age: string;
// }

/*函数*/
function identity<Type>(arg: Type): Type {
  return arg;
}
type result = typeof identity;
// type result = <Type>(arg: Type) => Type

/*enum类型*/
enum UserResponse {
  No = 0,
  Yes = 1,
}
type result = typeof UserResponse;
// result 类型类似于：
// {
//	"No": number,
//  "YES": number
// }

// ok
const a: result = {
  "No": 2,
  "Yes": 3
}
```

`typeof` 通常还会搭配 `keyof` 操作符用于获取属性名的联合字符串

```ts
type result = keyof typeof UserResponse;
// type result = "No" | "Yes"
```

##### 真值收窄

`&&`、`||`、`!`

```ts
function printAll(strs: string | string[] | null) {
  if (strs && typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}
```

##### 等值收窄

`===`、`==`、`!==`、`!=`

```ts
function example(x: string|number, y:string|boolean) {
	if(x ==== y) {
		// x、y收窄为string
		x.toUpperCase()
		y.toUpperCase()
	} else {
		// x、y没有被收窄
	}
}
```

##### in 收窄

```ts
type Fish = { swim: () => void };
type Bird = { fly: () => void };
type Human = { swim?: () => void; fly?: () => void };
 
function move(animal: Fish | Bird | Human) {
  if ("swim" in animal) {
    animal; // (parameter) animal: Fish | Human
  } else {
    animal; // (parameter) animal: Bird | Human
  }
}
```

##### `instanceof` 收窄

也是一种类型保护

```ts
function example(date: Date|string) {
	if(date instanceof Date) {
		console.log(date.toUTCString()) // date: Date
	} else {
		console.log(date.toUpperString()) // date: string
	}
}
```

##### 赋值语句收窄

```ts
let a = Math.random() > 0.5 ? 1 : false // a:number|boolean

a = 2 // a:number
```

##### 控制流分析（基于 if 条件可达性）

### 数据类型

#### number、string、boolean

需要注意的是 `void` 与 `undefined&null` 区别，后者是任意类型的子类型，也就是说可以赋值给其他类型，`void` 的常用场景就是定义函数无返回值，声明一个变量为 `void` 类型没有多大意义，因为只能被赋值为 `undefined&null`

##### 类型判断式

`pet is Fish` 就是我们的类型判断式

```ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}
// Both calls to 'swim' and 'fly' are now okay.
let pet = getSmallPet();
 
if (isFish(pet)) {
  pet.swim(); // let pet: Fish
} else {
  pet.fly(); // let pet: Bird
}
```

##### 可判断联合式

```ts
interface Circle {
  kind: "circle";
  radius: number;
}
 
interface Square {
  kind: "square";
  sideLength: number;
}
 
type Shape = Circle | Square;

function getArea(shape: Shape) {
  if (shape.kind === "circle") {
    return Math.PI * shape.radius! ** 2 // shape: Circle
  } else {
	return shape.sideLength ** 2 // // shape: Square
  }
}
```

##### 穷尽检查

任何除 `never` 类型以外的类型都不可赋值给 `never` 类型，可用作 `switch` 语句穷尽检查（确实穷尽时，报错）

```ts
interface Triangle {
  kind: "triangle";
  sideLength: number;
}
 
type Shape = Circle | Square | Triangle;
 
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape;
      // Type 'Triangle' is not assignable to type 'never'.
      return _exhaustiveCheck;
  }
}
```

#### `any`

表示任意类型

- 声明变量时未指定类型，则为 `any` 类型
- 可以在 `any` 类型上访问任意方法与属性
- `any` 类型允许被任意类型的值赋值
- 对 `any` 类型进行操作，其返回值仍为 `any` 类型，属性类型也都是 `any` 类型【属性污染】  

不建议使用 any，会失去 ts 的意义。

#### `unknown`

也表示任意类型，但是相比于 any 是类型安全的 ，定义为 unknown 类型的变量，不允许执行任意操作。  

#### `void`

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

#### `never`

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

#### `undefined` `null`

`undefined`、`null`、`never` 均是任何类型的子类型，可以赋值给其他类型的值。

```ts
let n: never
const a: any = '1'
n = a // no. Type 'any' is not assignable to type 'never'.
```

##### `strictNullChecks` 开启（建议

如果一个值==可能是== `null` 或者 `undefined`，在用它的方法或者属性之前，**先检查这些值**，就像用可选的属性之前，先检查一下 是否是 `undefined

```ts
function doSomething(x: string | null) {
  if (x === null) {
    // do nothing
  } else {
    console.log("Hello, " + x.toUpperCase());
  }
}
```

##### `strictNullChecks` 关闭

如果一个值==可能是== `null` 或者 `undefined` 依然可以被正确的访问，或者被赋值给任意类型的属性  

##### 非空断言操作符!

不做任何检查的情况下，从类型中移除 `null` 和 `undefined`，表示它的值不可能是 `null` 或者 `undefined`

```ts
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
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

##### `ReadonlyArray<T>` 或 `readonly T[]`

是一种特殊类型作为意图声明，明确告知数组内容不可更改

```ts
const arr: ReadonlyArray<number> = [1, 2, 3];
// arr[0] = 0;
// Index signature in type 'readonly number[]' only permits reading.
arr.push(4);
// Property 'push' does not exist on type 'readonly number[]'.
```

#### 元组类型

明确知道数组包含多少个元素，并且每个位置元素的类型都明确知道的时候

##### 表示方法

可以描述已知元素 **指定数量** 和 **指定类型** 的数组。

```ts
const arr: [number, string] = [1, '1']
type Either2dOr3d = [number, number, number?] // 可选类型
const brr:Either2dOr3d = [1, 2] // ok
```

##### 支持剩余元素语法

必须是 `array/tuple` 类型

```ts
type typea = [string, number, …boolean[]];
type typeb = [string, number];
const arr: typea = ['', 1, true];
const crr: typea = ['', 1, true, true, true];
const brr: typeb = ['', 1];
console.log(arr.length); // (property) length: number 有剩余元素的元组不设length，因为不清楚还有多少元素呐
console.log(brr.length); // (property) length: 2
```

##### 支持 `readonly`

只读元组，大部分情况下，元组用来被创建，后续不会修改，声明为 `readonly` 是好的编程习惯

##### 特点

- 越界访问、赋值报错；
- 调用数组方法，`push` 一个 **已定义类型** 的元素时，不会报错。  

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

- 返回值类型  
也不总是需要手动注解，`ts` 会根据 `return` 语句自动推断

#### 枚举类型

`enum` 数据类型，在运行时会被编译成对象添加到语言。用来定义常量，数值递增

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

#### 对象类型

简单的列出对象的属性和对应的类型来描述对象，三种方式：

```ts
// 匿名
function greet(person: { name: string; age: number }) {
  return "Hello " + person.name;
}
// interface
interface Person {
  name: string;
  age: number;
} 
function greet(person: Person) {
  return "Hello " + person.name;
}
// 类型别名
type Person = {
  name: string;
  age: number;
};
function greet(person: Person) {
  return "Hello " + person.name;
}

interface SomeType {
  x?: number; // 可选类型
  readonly prop: string; // 只读类型，属性本身只读，对于引用类型值来说地址不可变
}
```

`ts` 在检查两个类型是否兼容的时候，并不会考虑两个类型里的属性是否是 `readonly`，这就意味着，`readonly` 的值是可以通过别名修改的。

```ts
interface Person {
  name: string;
  age: number;
}
 
interface ReadonlyPerson {
  readonly name: string;
  readonly age: number;
}
 
let writablePerson: Person = {
  name: "Person McPersonface",
  age: 42,
};
 
// works，类型是兼容的
let readonlyPerson: ReadonlyPerson = writablePerson;
 
console.log(readonlyPerson.age); // prints '42'
writablePerson.age++;
console.log(readonlyPerson.age); // prints '43'
```

##### 索引签名

不确定属性的名字，只知道属性的类型时使用。

```ts
interface StringArray {  
  [index: number]: string;
}
// 表示当一个 StringArray 类型的值使用 number 类型的值进行索引的时候，会返回一个 string 类型的值
```

一个索引签名的属性类型必须是 `string` 或者是 `number`

```ts
interface ReadonlyStringArray {
  readonly [index: number]: string|number;
  ength: number; // ok, length is a number
  name: string; // ok, name is a string
}
```

##### 属性继承 `extends`

^b3b2fb

方便从其他声明过的类型中拷贝成员，并且随意添加新成员

```ts
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}
// 同时继承多个类型
interface ColorfulCircle extends Colorful, Circle {}
const cc: ColorfulCircle = {
  color: "red",
  radius: 42,
};
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

## TS 进阶

### 高级类型（一）

#### 联合类型 `|`

表示一个变量支持多种类型，`ts` 要求进行的操作只能是访问联合类型**共有的属性或方法**

```ts
let num: number|string;
num = 6
num = '6'
```

**类型收窄**：`ts` 根据代码结构推断出一个更加具体的类型，如 `if` 条件中 `typeof`、`Array.isArray` 判断

```ad-faq
你可能很奇怪，为什么联合类型只能使用这些类型属性的交集？
让我们举个例子，现在有两个房间，一个房间都是身高八尺戴帽子的人，另外一个房间则是会讲西班牙语戴帽子的人，合并这两个房间后，我们唯一知道的事情是：每一个人都戴着帽子
```

#### 交叉类型 `&`

类似接口继承，实现对对象形状的组合和扩展

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

##### 与接口继承的不同 [[#^b3b2fb]]

最原则性的不同就是在于冲突怎么处理

```ts
interface Colorful {
  color: string;
}
// 重写类型会导致编译错误
interface ColorfulSub extends Colorful {
  color: number
}

// Interface 'ColorfulSub' incorrectly extends interface 'Colorful'.
// Types of property 'color' are incompatible.
// Type 'number' is not assignable to type 'string'.

// 不会报错
type ColorfulSub = Colorful & {
  color: number
}
// 最终color类型为never,取得是string和number的交集
type ColorfulSub = {
  color: never
}
```

#### 类型别名 `type`

顾名思义，一个可以指代任意类型的名字。多次使用的类型起个别名方便使用，使得 `ts` 代码写起来简洁、清晰 [[type vs interface]]

```ts
type Point = {
	x: number;
	y: number;
}
```

#### 接口 `interface`

命名对象类型的另一种方式

```ts
interface Point {
	x: number;
	y: number;
}
```

与 `type` 区别，`type` 通过交叉类型 `&` 扩展，`interface` 通过 `extends` 扩展；`type` 本身无法添加新的属性 [[type vs interface]]

#### 类型断言 `as`

你知道明确的类型但 `ts` 不知道时，可手动推断为具体类型

```ts
// ts只知道这是个HTMLElement，手动告诉ts这是个HTMLCanvasElement
const canvasEl = document.getElementById("myCanvas") as HTMLCanvasElement
// 另种写法
const canvasEl = <HTMLCanvasElement>document.getElementById('myCanvas')
```

```ad-warning
因为类型断言会在编译的时候被移除，所以运行时并不会有类型断言的检查，即使类型断言是错误的，也不会有异常或者 `null` 产生
```

#### 字面量类型（Literal Types）

定义一些常量，只能从已定义常量中取值，🉑结合联合类型发挥作用

```ts
type ButtonSize = 'mini' | 'small' | 'normal' | 'large'
```

##### 字面量推断

`req.method` 被推断为 `string` ，而不是 `"GET"`

```ts
declare function handleRequest(url: string, method: "GET" | "POST"): void;
const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);
// Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.
```

解决 1：添加类型断言改变推断结果

```ts
const req = { url: "https://example.com", method: "GET" as "GET" }
```

解决 2：使用 `as const` 把整个对象转为一个类型字面量：

```ts
const req = { url: "https://example.com", method: "GET" } as const
```

#### 声明文件

声明语句：声明语句中只能定义类型，不能定义具体实现  
声明文件：声明语句放在一个 `.d.ts` 结尾的文件中即是声明文件，ts 解析所有 .ts 文件，因而包含 `.d.ts` 的声明文件，在其他 ts 文件中就可以获得声明文件中的定义了

#### 泛型

定义函数、类、接口时不必预先指定类型，而是在使用时指定具体类型。 [泛型(generic) - TypeScript 中文手册](https://www.tsdev.cn/generics.html)

```ad-note
可以把范型想象成一个实际类型的模板，`T` 就是一个占位符，可以被替代为具体的类型，重复利用这个模板
```

##### 基本使用

###### 约束函数

像 **占位符** 一样，在定义类型时作为参数传入。

```ts
function print<T = number>(param: T):T {
	console.log(param)
	return param
}

function swap<T, U>(tuple: [T, U]):[U, T] {
	return [tuple[1], tuple[0]]
}
```

###### 约束泛型

泛型通过 `extends` 继承接口，可对泛型进行一定的约束，如，要求传入的类型一定要有 `length` 属性，否则报错

```ts
interface ILength {
	length: number
}
function printLength<T extends ILength>(param: T):number {
	return param.length
}

printLength('ttt') // ok
printLength([1,2,3]) // ok
printLength({length: 1}) // ok
printLength(666) // no
```

##### 应用

###### 泛型约束类

```ts
class stack <T> {
	private data: T[] = []
	push(item: T) {
		return this.data.push(item)
	}

	pop(): T| undefined {
		return this.data.pop()
	}
}

const numStack = new stack<number>()
numStack.push(1) // ok
numStack.push('1') // no
const strStack = new stack<string>()
strStack.push('1') // ok
strStack.push(1) // no
```

###### 泛型约束接口

接口定义更加灵活

```ts
interface IParam<T, U> {
	key: T;
	value: U;
}

const param1:IParam<string, number> = {key: '1', value: 1}
const param2:IParam<number, string> = {key: 1, value: '1'}
```

###### 泛型定义数组

```ts
const param3: Array<number> = [1,2,3]
```

###### 泛型帮助类型

```ts
type OrNull<Type> = Type | null;
type OneOrMany<Type> = Type | Type[];
type OneOrManyOrNull<Type> = OrNull<OneOrMany<Type>>; // 逐渐开始做体操
type OneOrManyOrNull<Type> = OneOrMany<Type> | null
type OneOrManyOrNullStrings = OneOrManyOrNull<string>;
type OneOrManyOrNullStrings = OneOrMany<string> | null
```

### 高级类型（二）

#### 索引类型

##### `keyof`

获取某种类型的所有键，返回类型是**字符串或者数字字面量的联合类型**

```ts
// 字符串
type Point = { x: number; y: number };
type P = keyof Point;
// type P = "x" | "y"

type Arrayish = { [n: number]: unknown };  
type A = keyof Arrayish;  
// type A = number

type Mapish = { [k: string]: boolean };  
type M = keyof Mapish;  
// type M = string | number

// 数字字面量
const NumericObject = {
  [1]: "冴羽一号",
  [2]: "冴羽二号",
  [3]: "冴羽三号"
};
type result = keyof typeof NumericObject
// typeof NumbericObject 的结果为：
// {
//   1: string;
//   2: string;
//   3: string;
// }
// 所以最终的结果为：
// type result = 1 | 2 | 3

// symbol
const sym1 = Symbol();
const sym2 = Symbol();
const sym3 = Symbol();
const symbolToNumberMap = {
  [sym1]: 1,
  [sym2]: 2,
  [sym3]: 3,
};
type KS = keyof typeof symbolToNumberMap; // typeof sym1 | typeof sym2 | typeof sym3

// 类
class Person {
  name: "冴羽"
}
type result = keyof Person;
// type result = "name"
class Person {
  [1]: string = "冴羽";
}
type result = keyof Person;
// type result = 1

// 接口
interface Person {
  name: "string";
}
type result = keyof Person;
// type result = "name"
```

##### `T[K]`

获取接口 `T` 的 `K` 属性代表的类型

#### 映射类型

##### `in`

遍历联合类型

```ts
type Person = "name"|"age"|"school"
type Obj = {
	[k in Person]: string
}
// 实际上，
type Obj = {  
	name: string;  
	age: string;  
	school: string;  
}
```

##### `Partial<T>`

将类型 T 的所有属性映射为可选

```ts
type Partial<T> = {
	[P in keyof T]?: T[P]
}
```

##### `Readonly<T>`

将类型 T 的所有属性映射为只读

```ts
type Readonly<T> = {
	readonly [P in keyof T]: T[P]
}
```

##### `Pick<T, K>`

从类型 T 中挑选属性 K 组成新的类型。第一个参数为目标类型，第二个参数必须取自目标类型属性字面量联合类型。

```ts
type Pick<T, K extends keyof T> = {
	[P in K]: T[P]
}
```

##### `Record<K, T>`

创建一个新的类型

```ts
type Record<K extends keyof any, T> = {
	[P in K]: T
}
```

##### 条件类型

```ts
T extends U ? X : Y 
//若类型 T 可被赋值给类型 U,那么结果类型就是 X 类型,否则就是 Y 类型
```

##### `Exclude<T, U>`

联合类型 T 中剔除联合类型 U 剩下的部分

```ts
type Test = Exclude<'a'|'b'|'c', 'b'>
// type Test = "a" | "c"
```

原理

```ts
type Exclude<T, U> = T extends U ? never : T
```

never 与其他类型联合结果为其他类型

##### `Extract<T, U>`

提取联合类型 T 与联合类型 U 交集

```ts
type Test = Extract<'key1' | 'key2', 'key1'>
// type Test = "key1"
```

原理

```ts
type Extract<T, U> = T extends U ? T : never
```

##### `Omit<T, U>`

从类型 T 中剔除类型 U 中的所有属性

```ts
interface IPer {
	name: string;
	age: number;
}
type TT = Omit<IPer, 'name'>
// type TT = {age: number;}
```

##### `NonNullable<T>`

过滤类型 T 中的 null、undefined 类型

```ts
type NonNullable<T> = T extends null | undefined ? never : T
```

##### `Parameters<T>`

获取函数参数类型，T 为函数类型

```ts
type T3 = Parameters<(arg1: string, arg2: number) => void> // [arg1: string, arg2: number]

const p: T3 = ['', 0]
```

##### `ReturnType<T>`

获取函数返回值类型，T 为函数类型

```ts
type T1 = ReturnType<(s: string) => void> // void
```

##### `InstanceType<T>`

用于构造一个由所有 `T` 的构造函数的实例类型组成的类型。

### 类型体操

ts 高级类型会根据传入的类型参数如 T、U 得出新的类型，这个过程涉及类型计算逻辑，该逻辑就为 **类型体操**（社区对一些复杂类型计算逻辑的戏称）[[类型体操]]

### 声明文件

#### declare

定义类型

#### `*.d.ts`

将声明集中存放在\*.d.ts 文件中构成声明文件，项目中其余 .ts 文件都可以获得类型定义了。

#### 第三方库

对应类型文件为 `@types/xxx`

#### 自己写声明文件

1. 一个方法是放在 `@node_modules/@types/xxx/index.d.ts` 下面；
2. 一个方法是新建 `types` 文件夹，在其中添加 `xxx/index.d.ts` 声明文件，并在 [[../../Project/tsconfig.json|tsconfig.json]] 配置 `paths` 和 `baseUrl` 选项。

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

`*.ts` 文件会获取 `*.d.ts` 声明文件中的类型定义。在 [[../../Project/tsconfig.json|tsconfig.json]] 中配置全局自定义类型声明文件，则声明文件中的类型定义都能被项目中的 `*.ts` 文件获取到，不需要 `import` 就可以直接使用。

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

## tsconfig.json 配置

[[../../Project/tsconfig.json|tsconfig.json]]

# Reference

[TypeScript中文文档\_入门进阶必备](https://ts.yayujs.com/)  
[深入理解 TypeScript | 深入理解 TypeScript](https://jkchao.github.io/typescript-book-chinese/)  
[会写 TypeScript 但你真的会 TS 编译配置吗？ - 掘金](https://juejin.cn/post/7039583726375796749)  
[接口interface](https://ts.xcatliu.com/advanced/class-and-interfaces.html#%E7%B1%BB%E5%AE%9E%E7%8E%B0%E6%8E%A5%E5%8F%A3)  
[类型别名type](https://ts.xcatliu.com/advanced/type-aliases.html)  
[如何在项目中用好 TypeScript 🤔](https:juejin.cn/post/7058868160706904078)  
[类型系统 top/bottom type](https://mp.weixin.qq.com/s/rZ96wy8xUrx4T1qG5OKS0w)  
[编译选项](https://www.tslang.cn/docs/handbook/compiler-options.html)  
[工具类型](https://www.typescriptlang.org/docs/handbook/utility-types.html)  
[1.9W字总结一份通俗易懂的 TS 教程，入门 + 实战！](https://juejin.cn/post/7068081327857205261)
