Created Date：2023-09-19 19:06:26  
Last Modified：2023-09-19 19:06:26

# Tags

#bnpm #bilibili

# Content

## tsconfig.json

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

# Reference

[Package - @bilibili/bili-act-events](http://npm.bilibili.co/package/@bilibili/bili-act-events)
