Created Date：2023-09-19 19:06:26  
Last Modified：2023-09-19 19:06:26

# Tags

#bnpm #bilibili

# Content

## tslint vs eslint

`tslint`：类型检查、语言转换 ts->js、语法检查  
`eslint`：代码风格、语法检查

## tsconfig.json

### config

```json
{
	"compileOnSave": true,
	"compilerOptions": {
		"strict": false, // 开启所有严格的类型检查
		"sourceMap": false, // 是否生成sourcemap文件
		"preserveConstEnums": false, // 是否生成枚举enum的映射代码
		"jsx": "react",
		"lib": ["dom", "es2015", "es2016", "es2017", "esnext"],
		"target": "es5", // 生成代码使用的语言版本
		"module": "commonjs", // 使用的模块系统: 'none', 'commonjs', 'umd', 'es6', 'es2015', 'esnext', 'amd', 'system'
		"moduleResolution": "node",
		"experimentalDecorators": true,
		"allowSyntheticDefaultImports": true,
		"removeComments": true, // 移除注释
		"noUnusedLocals": true, // [warning]:仅声明未使用的变量
		"noUnusedParameters": true,
		"noImplicitAny": true, // [error]:禁止隐式any推导
		"strictNullChecks": true, // [error]:禁止null类型变量赋值给非null类型变量
		"baseUrl": ".",
		"outDir": "./dist",
		"paths": {
		"workspace/*": ["."]
		}
	},
	"include": [
		"src/**/*",
	],
	"exclude": ["node_modules/**/*"]
}
```

### extends

[tslint-config-prettier - npm](https://www.npmjs.com/package/tslint-config-prettier) - 解决 `tslint` 与 `prettier` 的冲突；  

[tslint-eslint-rules - npm](https://www.npmjs.com/package/tslint-eslint-rules) - 补全 `tslint` 中缺失的 `eslint` 规则，【已废弃】- 可由 [Getting Started | typescript-eslint](https://typescript-eslint.io/getting-started) 替代；

# Reference

[Package - @bilibili/bili-act-events](http://npm.bilibili.co/package/@bilibili/bili-act-events)  

[代码检查工具！从 TSLint 到 ESLint - 掘金](https://juejin.cn/post/6955025103507849223)  
[TSLint](https://palantir.github.io/tslint/) - 2019 年【已废弃】- 由 [typescript-eslint](https://typescript-eslint.io/) 替代  
[快速上手，tsconfig（文件选项） - 掘金](https://juejin.cn/post/6953553286657998879/)  
[快速上手，tsconfig （编译选项） - 掘金](https://juejin.cn/post/6953554051879403534)  
