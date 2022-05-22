
创建日期：2022-05-22 21:33:51
最后修改：2022-05-22 21:33:51
- - -
> Real magic in relationships means an absence of judgement of others.
> — <cite>Wayne Dyer</cite>

## 正文
#### root
[定义指定目录下Eslint规则](https://eslint.org/docs/user-guide/configuring/configuration-files#cascading-and-hierarchy)
默认情况下，ESLint从父级目录里寻找配置文件，一直到根目录，如果你想要整个项目都遵循统（同）一约定时，这将会很有用。但另一种情况是把 ESLint 限制到一个特定的项目，这就需要在特定项目目录下的 `package.json` 文件或者 `.eslintrc.*` 文件里的 `eslintConfig` 字段下设置 `"root": true`。ESLint 一旦发现配置文件中有 `"root": true`，它就会停止在父级目录中寻找。
```js
{
	"root": true,
}
```
#### env
[定义支持的环境变量](https://eslint.org/docs/user-guide/configuring/language-options#specifying-environments)
```js
{
	"env": {
		"browser": true,
		"node": true
    }
}
```
#### extend
[拓展"第三方"Eslint配置](https://eslint.org/docs/user-guide/configuring/configuration-files#extending-configuration-files)
值可以是，相对/绝对路径下的配置文件、npm包中的配置或plugin形式的配置
```js
extends: [  
	'plugin:vue/base',  
	'@bilibili/recommend', // bilibili代码规范  
	'@vue/typescript/recommended',  
],
```
#### parserOptions
[指定支持的JavaScript版本](https://eslint.org/docs/user-guide/configuring/language-options#specifying-parser-options)
```js
parserOptions: {  
  ecmaVersion: 2020,  
},
```
#### tsconfig.json
1、is declared but its value is never read.
```json
"compilerOptions": {
	"noUnusedLocals": false,
}
```
2、
#### 代码文件里的临时规则设定
[当前文件关闭、当前行关闭等](https://eslint.bootcss.com/docs/user-guide/configuring#disabling-rules-with-inline-comments)
```
/* eslint-disable */
/* eslint-disable-line */
/* eslint-disable-next-line */
```
## 参考文献
- [Configuring Eslint](https://eslint.bootcss.com/docs/user-guide/configuring)
- 