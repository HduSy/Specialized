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

### config

```json
{
   // 指定需要编译文件 否则默认当前目录下除了exclude之外的所有.ts, .d.ts,.tsx 文件
   "include": ["./test.ts"],
   // 指定需要编译文件 否则默认当前目录下除了exclude之外的所有.ts, .d.ts,.tsx 文件
   "files": ["./src/**/*"],
   // 不编译某些文件
   "exclude": ["test.ts"],
   "compilerOptions": {
       // 只编译修改过的文件,这个时候会生成tsconfig.tsbuildinfo,下次编译的时候会进行对比只编译修改过的文件 
       "incremental": true,
       // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
       "target": "es5",
       // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
       "module": "commonjs",
       /* 注意：如果未指定--lib，则会注入默认的librares列表。注入的默认库为：
       对于 --target ES5: DOM,ES5,ScriptHost
       对于 --target ES6: DOM,ES6,DOM.Iterable,ScriptHost
       TS 绝不会在您的代码中注入polyfill,所以需要你自己制定编译lib */
       "lib": ["es5", "dom", "ScriptHost", "es2015.promise"],
       // 允许编译JS
       "allowJs": true,
       /* 是否检测JS的语法,例如下面的语法编辑器会报错
       let name = 'paul';
       console.log(name.a.b) */
       "checkJs": true,
       // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
       "jsx": preserve,
       /* 如果设为true，编译每个ts文件之后会生成一个js文件和一个声明文件,
       declaration和allowJs不能同时设为true */
       "declaration": true
       // 值为true或false，指定是否为声明文件.d.ts生成map文件
       "declarationMap": true
       // 用来指定编译时是否生成.map文件
       "sourceMap": true,
       // 当module设置为 'amd' and 'system'的时候可以使用此命令,这样可以将ts文件打包到一个目录下
       "outFile":"./",
       //  outDir 编译后的文件存到到哪个目录下,默认是每一个ts文件的当前目录,,下面的配置就是把ts编译到build目录下
       "outDir": './build',
       // 下面单独介绍
       "rootDir": "./src",
       // 是否编译构建引用项目,很复杂后面介绍
       "composite": true,
       // 指定文件用来存储增量编译信息,默认是tsconfig.tsbuildinfo
       "tsBuildInfoFile": "./",
       // 编译的时候删除注释
       "removeComments": true,
       // 不生成编译文件，这个一般比较少用,这个build目录下将没有任何文件,但是会进行编译,有错误会抛出
       "noEmit": true,
       // 是否引入npm包tslib中的辅助函数,__extends等 
       "importHelpers": true,
       // 当target为'ES5' or 'ES3'时，为'for-of', spread, and destructuring'中的迭代器提供完全支持
       "downlevelIteration": true,
       // isolatedModules的值为true或false，指定是否将每个文件作为单独的模块，默认为true，它不可以和declaration同时设定
       // 不是很理解,将每一个文件作为单独模块
       "isolatedModules": true,
       /* Strict Type-Checking Options */
       // 严格模式将会打开下面的几个选项
       "strict": false, 
       /* 不允许变量或函数参数具有隐式any类型,例如
       function(name) {
           return name;
       } */
       "noImplicitAny": true,
       // null类型检测,const teacher: string = null;会报错
       "strictNullChecks": true,
       // 对函数参数进行严格逆变比较
       "strictFunctionTypes": true,
       // 严格检查bind call apply
       "strictBindCallApply": true,
       // 此规则将验证构造函数内部初始化前后已定义的属性。
       "strictPropertyInitialization": true,
       // 检测this是否隐式指定
       "noImplicitThis": true,
       // 使用js的严格模式,在每一个文件上部声明 use strict
       "alwaysStrict": true,
       /* Additional Checks */
       // 默认false,是否检测定义了但是没使用的变量
       "noUnusedLocals": true,
       // 用于检查是否有在函数体中没有使用的参数
       "noUnusedParameters": true,
       // 用于检查函数是否有返回值，设为true后，如果函数没有返回值则会提示
       "noImplicitReturns": true,
       // 用于检查switch中是否有case没有使用break跳出switch
       "noFallthroughCasesInSwitch": true,
       /* Module Resolution Options */
       // 用于选择模块解析策略，有'node'和'classic'两种类型
       "moduleResolution": "node",
       // 复杂的很 下面单独介绍这三个模块
       "baseUrl": './'
       "paths": {},                   
       "rootDirs": [],
       /* typeRoots用来指定声明文件或文件夹的路径列表，如果指定了此项，则只有在这里列出的声明文件才会被加载 */
       typeRoots: [],
       // types用来指定需要包含的模块，只有在这里列出的模块的声明文件才会被加载进来
       types:[],
       // 用来指定允许从没有默认导出的模块中默认导入 
       "allowSyntheticDefaultImports": true, 
       // 通过为导入内容创建命名空间，实现CommonJS和ES模块之间的互操作性
       "esModuleInterop": true ,
       // 不把符号链接解析为真实路径，具体可以了解下webpack和node.js的symlink相关知识
       "preserveSymlinks": true,
       "allowUmdGlobalAccess": true,
       
       // sourceRoot用于指定调试器应该找到TypeScript文件而不是源文件的位置，这个值会被写进.map文件里
       "sourceRoot": '',
       // mapRoot用于指定调试器找到映射文件而非生成文件的位置，指定map文件的根路径，该选项会影响.map文件中的sources属性
       "mapRoot",
       // inlineSourceMap指定是否将map文件内容和js文件编译在一个同一个js文件中，如果设为true,则map的内容会以//#soureMappingURL=开头，然后接base64字符串的形式插入在js文件底部
       "inlineSourceMap": true,
       // inlineSources用于指定是否进一步将ts文件的内容也包含到输出文件中
       "inlineSources": true,
       
       // experimentalDecorators用于指定是否启用实验性的装饰器特性
       "experimentalDecorators": true,
       
       // emitDecoratorMetadata用于指定是否为装上去提供元数据支持，关于元数据，也是ES6的新标准，可以通过Reflect提供的静态方法获取元数据，如果需要使用Reflect的一些方法，需要引用ES2015.Reflect这个库
       "emitDecoratorMetadata": true,
       // compileOnSave的值是true或false，如果设为true，在我们编辑了项目中的文件保存的时候，编辑器会根据tsconfig.json中的配置重新生成文件，不过这个要编辑器支持
       "compileOnSave": true,
       // 很复杂 下面介绍
       "references":[]",
   }
}


```

# Reference

[Package - @bilibili/bili-act-events](http://npm.bilibili.co/package/@bilibili/bili-act-events)  

[GitHub - developit/mitt: 🥊 Tiny 200 byte functional event emitter / pubsub.](https://github.com/developit/mitt)

[代码检查工具！从 TSLint 到 ESLint - 掘金](https://juejin.cn/post/6955025103507849223)  
[TSLint](https://palantir.github.io/tslint/) - 2019 年【已废弃】- 由 [typescript-eslint](https://typescript-eslint.io/) 替代  
[快速上手，tsconfig（文件选项） - 掘金](https://juejin.cn/post/6953553286657998879/)  
[快速上手，tsconfig （编译选项） - 掘金](https://juejin.cn/post/6953554051879403534)  
