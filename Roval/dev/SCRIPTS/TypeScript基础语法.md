# 入门到放弃

## 基础

### 基础数据类型

`number、string、boolean、undefined、null、void`

需要注意的是 void 与 undefined&null 区别，后者是任意类型的子类型，也就是说可以赋值给其他类型，void 的常用场景就是定义函数无返回值，声明一个变量为 void 类型没有多大意义，因为只能被赋值为 undefined&null

### 任意值

- 声明变量时未指定类型，则为 any 类型
		
- 可以在 any 类型上访问任意方法与属性
		
- any 类型允许被任意类型的值赋值
		
- 对 any 类型进行操作，其返回值仍为 any 类型，属性类型也都是 any 类型【属性污染】
		

### 类型推断

- 未明确指定一个变量的类型的的时候会推断该变量的类型
		
- 当定义变量未赋值时，之后该变量都会成为 any 类型
		

### 联合类型

- 联合类型通过 `|` 分隔
		
- 当 ts 并不知道联合类型的变量属于哪个类型时，只能访问联合类型共有的属性&方法
		

### 对象类型——接口 interface

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

1. 一个接口中只能定义一个任意属性，且其余属性的类型必须是任意属性的子类型
2. 任意属性可定义为联合类型
				
- 只读属性
		
```typescript
    interface Person {
		readonly id: number;
		name: string;
		age?: number;
		[propName: string]: any;  
    }
```
			

属性前加 `readonly`, 只读的约束只在第一次给对象赋值而非第一次给只读属性赋值  
[[Type VS Interface]]

### 数组类型

- 类型 +[] 表示，如 `let arr: number[] = [1, 2, 3]`
		
- 数组范型 `Array<element>` 表示，如 `let arr:Array<number> = [1, 2, 3]`
		
- 接口表示（不推荐）
		
```typescript
    interface NumberArray {
		[index: number]: any;  
    }
    let fibonacci: NumberArray = [1, '1', 2, 3, 5];
```
		

但是在表示**类数组**方面很 nice，如 `IArguments`, `NodeList`, `HTMLCollection`

```typescript
    function sum() {
		let args: IArguments = arguments;  
    }

```
		

### 函数的类型

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
		

### 类型断言

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
		

### 声明文件

声明语句：声明语句中只能定义类型，不能定义具体实现

声明文件：声明语句放在一个 `.d.ts` 结尾的文件中即是声明文件，typescript 解析所有.ts 文件，因而包含 `.d.ts` 的声明文件，在其他 ts 文件中就可以获得声明文件中的定义了
