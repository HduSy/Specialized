## 目录

- [[#正文|正文]]
	- [[#定义|定义]]
	- [[#目标|目标]]
	- [[#TS能干点什么|TS能干点什么]]
		- [[#静态检查|静态检查]]
		- [[#面向对象编程增强|面向对象编程增强]]
		- [[#成本|成本]]
- [[#参考文献|参考文献]]

创建日期：2022-02-19 20:43:12
最后修改：2022-02-19 20:43:11
- - -
> Let us resolve to be masters, not the victims, of our history, controlling our own destiny without giving way to blind suspicions and emotions.
> — <cite>John F. Kennedy</cite>

## 正文
### 定义
JavaScript类型超集，代码最终将编译为纯JavaScript代码。
### 目标
>编译期可以做静态检查，为大规模代码提供结构化装置，编译出符合习惯、易读的JS代码，和ECMAScript标准对齐，使用一贯的、可删除的、结构化的类型系统，保护编译后的JS代码的运行时行为等等。

对于生命周期长、较为复杂的SPA、为保证一定开发效率，最重要的是提升代码可维护性和运行质量，Typescript很有必要。
- 开发效率上，不论是WebStorm还是VsCode，Typescript为IDE增加智能代码提示与静态检测能力，在团队协作中整体开发效率；
- 可维护性上，长期维护的项目有==“熵”==的特质，随着人员增多，水平不一，可维护性大大下降，通过**强类型约束**与**静态类型检查**，可提高可维护性，甚至**重构**的频率也因此增加；
- 线上代码质量上，实际开发过程中，大多bug因调用方与被调用方的数据格式不一致引起，TS有强制类型检查，提前静态检测，发现问题，减少线上bug量。
### TS能干点什么
#### 静态检查
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

#### 面向对象编程增强
1、访问权限控制，提供`static`、`protected`、`public`、`private`访问控制符；
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

2、接口`interface`，增强了可扩展性；
3、范型T；
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

#### 成本
学习成本中等，因为兼容JS代码，可渐进式增加类型、接口定义以增强代码健壮性。

## 参考文献
