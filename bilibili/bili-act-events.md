Created Date：2023-09-19 19:06:26  
Last Modified：2023-09-19 19:06:26

# Tags

#bnpm #bilibili

# Content

## vue 组件间通信

## tslint.json

### tslint vs eslint

`tslint`：类型检查、语言转换 ts->js、语法检查  
`eslint`：代码风格、语法检查

```json
extends: 内设配置项名称
rules:  规则
  {
    //ts专用
    adjacent-overload-signatures ： true,  //  Enforces function overloads to be consecutive.
    ban-comma-operator：true, //禁止逗号运算符。
    ban-type: [true, ["object","User {} instead."],["string"]] //禁止类型
    member-access： [true , "no-public"||"check-accessor"|| "check-constructor" || "check-parameter-property"  ] ,  //类成员必须声明 private public ....
    member-order: [true, {order:....}],  //类声明排序
    no-any: true,//不需使用any类型
    no-empty-interface:true //禁止空接口 {}
    no-import-side-effect: [true, {"ignore-module": "(\\.html|\\.css)$"}], //禁止导入带有副作用的语句
    no-inferrable-types：[true, "ignore-params", "ignore-properties"]， //不允许将变量或参数初始化为数字，字符串或布尔值的显式类型声明。
    no-internal-module:true， //不允许内部模块
    no-magic-numbers: [true,1,2,3], //不允许在变量赋值之外使用常量数值。当没有指定允许值列表时，默认允许-1,0和1
    no-namespace: [ true,"allpw-declarations"], //不允许使用内部modules和命名空间
    no-non-null-assertion: true , //不允许使用!后缀操作符的非空断言。
    no-parameter-reassignment: true, //不允许重新分配参数
    no-reference: true, // 禁止使用/// <reference path=> 导入 ，使用import代替
    no-unnecessary-type-assertion： true, //如果类型断言没有改变表达式的类型就发出警告
    no-var-requires： true, //不允许使用var module = require("module"),用 import foo = require('foo')导入
    only-arrow-functions：[true，"allow-declarations"，"allow-named-functions"], //允许箭头表达式，不需要传统表达式 ； 允许独立的函数声明  ；允许表达，function foo() {}但不是function() {}
    prefer-for-of:true,  //建议使用for(..of)
    promise-function-async: true, 要求异步函数返回promise
    typedef: [true, "call-signature", "parameter", "member-variable-declaration"], //需要定义的类型存在
    typedef-whitespace： true, //类型声明的冒号之前是否需要空格
    unified-signatures： true, //重载可以被统一联合成一个

    //function 专用
    await-promise： true,  //警告不是一个promise的await
    ban: [
          true,
          "eval",
          {"name": "$", "message": "please don't"},
          ["describe", "only"],
          {"name": ["it", "only"], "message": "don't focus tests"},
          {
            "name": ["chai", "assert", "equal"],
            "message": "Use 'strictEqual' instead."
          },
          {"name": ["*", "forEach"], "message": "Use a regular for loop instead."}
    ],
    curly: true, //for if do while 要有括号
    forin:true, //用for in 必须用if进行过滤
    import-blacklist:true, //允许使用import require导入具体的模块
    label-postion: true, //允许在do/for/while/swith中使用label
    no-arg:true, //不允许使用 argument.callee
    no-bitwise:true, //不允许使用按位运算符
    no-conditional-assignmen: true, //不允许在do-while/for/if/while判断语句中使用赋值语句
    no-console：true, //不能使用console
    no-construct: true, //不允许使用 String/Number/Boolean的构造函数
    no-debugger： true, //不允许使用debugger
    no-duplicate-super: true, //构造函数两次用super会发出警告
    no-empty:true, //不允许空的块
    no-eval: true, //不允许使用eval
    no-floating-promises: true, //必须正确处理promise的返回函数
    no-for-in-array: true, //不允许使用for in 遍历数组
    no-implicit-dependencies: true, //不允许在项目的package.json中导入未列为依赖项的模块
    no-inferred-empty-object-type： true， //不允许在函数和构造函数中使用{}的类型推断
    no-invalid-template-strings： true, //警告在非模板字符中使用${
    no-invalid-this：true, //不允许在非class中使用 this关键字
    no-misused-new: true, //禁止定义构造函数或new class
    no-null-keyword: true, //不允许使用null关键字
    no-object-literal-type-assertion：true, //禁止objext出现在类型断言表达式中
    no-return-await：true, //不允许return await
    arrow-parens： true, //箭头函数定义的参数需要括号
  }
```

### key-extends

[tslint-config-prettier - npm](https://www.npmjs.com/package/tslint-config-prettier) - 解决 `tslint` 与 `prettier` 的冲突；  

[tslint-eslint-rules - npm](https://www.npmjs.com/package/tslint-eslint-rules) - 补全 `tslint` 中缺失的 `eslint` 规则，【已废弃】- 可由 [Getting Started | typescript-eslint](https://typescript-eslint.io/getting-started) 替代；

### rules

## tsconfig.json

[[../Roval/工程化/tsconfig.json|tsconfig.json]]

# Reference

[Package - @bilibili/bili-act-events](http://npm.bilibili.co/package/@bilibili/bili-act-events)  

[GitHub - developit/mitt: 🥊 Tiny 200 byte functional event emitter / pubsub.](https://github.com/developit/mitt)

[代码检查工具！从 TSLint 到 ESLint - 掘金](https://juejin.cn/post/6955025103507849223)  
[TSLint](https://palantir.github.io/tslint/) - 2019 年【已废弃】- 由 [typescript-eslint](https://typescript-eslint.io/) 替代  
